<properties
   pageTitle="Distribuire le risorse con PowerShell e modello | Microsoft Azure"
   description="Utilizzare Azure Resource Manager e Azure PowerShell per distribuire una risorsa in Azure. Le risorse sono definite in un modello di Resource Manager."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/> 

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tomfitz"/>

# Distribuire le risorse con i modelli di Azure Resource Manager e Azure PowerShell

> [AZURE.SELECTOR]
- [PowerShell](resource-group-template-deploy.md)
- [Interfaccia della riga di comando di Azure](resource-group-template-deploy-cli.md)
- [Portale](resource-group-template-deploy-portal.md)
- [API REST](resource-group-template-deploy-rest.md)

Questo articolo illustra come usare Azure PowerShell con modelli di Resource Manager per distribuire risorse in Azure.

> [AZURE.TIP] Per informazioni su come eseguire il debug di un errore durante la distribuzione, vedere:
>
> - [Visualizzare le operazioni di distribuzione con Azure PowerShell](resource-manager-troubleshoot-deployments-powershell.md), per ottenere informazioni su come risolvere l'errore
> - [Risolvere errori comuni durante la distribuzione di risorse in Azure con Azure Resource Manager](resource-manager-common-deployment-errors.md) per informazioni sulla risoluzione degli errori di distribuzione più comuni

Il modello può essere un file locale oppure un file esterno disponibile tramite un URI. Quando il modello si trova in un account di archiviazione, è possibile limitare l'accesso al modello e fornire un token di firma di accesso condiviso in fase di distribuzione.

## Azioni rapide per la distribuzione

Questo articolo descrive tutte le diverse opzioni disponibili durante la distribuzione. Tuttavia, spesso sono necessari solo due semplici comandi. Per iniziare a usare rapidamente la distribuzione, usare i comandi seguenti:

    New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "West US"
    New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -TemplateParameterFile <PathToParameterFile>

Per altre informazioni sulle opzioni di distribuzione più adatte allo scenario di riferimento, continuare a leggere questo articolo.

[AZURE.INCLUDE [resource-manager-deployments](../includes/resource-manager-deployments.md)]

## Distribuire con PowerShell

1. Accedere all'account Azure.

        Add-AzureRmAccount

     Viene restituito un riepilogo dell'account.

        Environment : AzureCloud
        Account     : someone@example.com
        ...

2. Se si hanno più sottoscrizioni, specificare l'ID sottoscrizione da usare per la distribuzione con il comando **Set-AzureRmContext**.

        Set-AzureRmContext -SubscriptionID <YourSubscriptionId>

3. In genere, quando si distribuisce un nuovo modello, è consigliabile creare un nuovo gruppo di risorse per contenere le risorse. Se è già presente un gruppo di risorse per la distribuzione, è possibile ignorare questo passaggio e usare semplicemente quel gruppo di risorse.

     Per creare un gruppo di risorse, specificare il nome e il percorso del gruppo di risorse. È necessario specificare un percorso per il gruppo di risorse perché nel gruppo di risorse vengono archiviati i metadati delle risorse. Per motivi di conformità può essere opportuno specificare dove vengono archiviati i metadati. In generale è consigliabile specificare un percorso in cui risiederà la maggior parte delle risorse. Usando lo stesso percorso è possibile semplificare il modello.

        New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "West US"
   
     Viene restituito un riepilogo del nuovo gruppo di risorse.
   
        ResourceGroupName : ExampleResourceGroup
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
             Actions  NotActions
             =======  ==========
             *
        ResourceId        : /subscriptions/######/resourceGroups/ExampleResourceGroup

4. Prima di eseguire la distribuzione, è possibile convalidare le impostazioni di distribuzione. Il cmdlet **Test AzureRmResourceGroupDeployment** consente di individuare eventuali problemi prima di creare le risorse effettive. L'esempio seguente mostra come convalidare una distribuzione.

        Test-AzureRmResourceGroupDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate>

5. Per distribuire le risorse al gruppo di risorse, eseguire il comando **New-AzureRmResourceGroupDeployment** e specificare i parametri necessari. I parametri includono un nome per la distribuzione, il nome del gruppo di risorse, il percorso o l'URL per il modello creato e qualsiasi altro parametro necessario per lo scenario. Se il parametro **Mode** non è specificato, viene usato il valore predefinito **Incremental**. Per eseguire una distribuzione completa, impostare **Mode** su **Complete**. Quando si utilizza la modalità di completamento, fare attenzione a non eliminare inavvertitamente le risorse non presenti nel modello.

     Per distribuire un modello locale, usare il parametro **TemplateFile**:

        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate>

     Per distribuire un modello esterno, usare il parametro **TemplateUri**:

        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri <LinkToTemplate>
   
     Per specificare i valori dei parametri sono disponibili le opzioni seguenti:
   
     1. Usare i parametri inline.

            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -myParameterName "parameterValue"

     2. Usare un oggetto parametro.

            $parameters = @{"<ParameterName>"="<Parameter Value>"}
            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -TemplateParameterObject $parameters

     3. Usare un file di parametri locali. Per informazioni sul file di modello, vedere [File di parametri](#parameter-file).

            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -TemplateParameterFile <PathToParameterFile>

     4. Usare un file di parametri esterni. Per informazioni sul file di modello, vedere [File di parametri](#parameter-file).

            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri <LinkToTemplate> -TemplateParameterUri <LinkToParameterFile>

        Quando si usa un file di parametri esterni non è possibile passare altri valori, né inline né da un file locale. Per altre informazioni, vedere la sezione [Precedenza dei parametri](#parameter-precendence).

     Una volta distribuite le risorse, verrà visualizzato un riepilogo della distribuzione.

        DeploymentName    : ExampleDeployment
        ResourceGroupName : ExampleResourceGroup
        ProvisioningState : Succeeded
        Timestamp         : 4/14/2015 7:00:27 PM
        Mode              : Incremental
        ...

     Se il modello include un parametro con lo stesso nome di uno dei parametri nel comando di PowerShell, viene richiesto di fornire un valore per il parametro. Il parametro del modello includerà il suffisso **FromTemplate**. Ad esempio, un parametro denominato **ResourceGroupName** nel modello sarà in conflitto con il parametro **ResourceGroupName** nel cmdlet [New-AzureRmResourceGroupDeployment](https://msdn.microsoft.com/library/azure/mt679003.aspx). Verrà quindi richiesto di fornire un valore per **ResourceGroupNameFromTemplate**. In generale, è consigliabile evitare questa confusione non attribuendo ai parametri lo stesso nome dei parametri usati per operazioni di distribuzione.

6. Per registrare informazioni aggiuntive sulla distribuzione che potrebbero contribuire a risolvere eventuali errori di distribuzione, usare il parametro **DeploymentDebugLogLevel**. È possibile specificare la registrazione del contenuto della richiesta, del contenuto della risposta o di entrambi con l'operazione di distribuzione.

        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -DeploymentDebugLogLevel All -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate>
        
     Per altre informazioni sulla risoluzione dei problemi relativi alle distribuzioni usando il contenuto di debug, vedere [Visualizzare le operazioni di distribuzione con Azure PowerShell](resource-manager-troubleshoot-deployments-powershell.md).

## Distribuire modelli dall'archiviazione con token SAS

È possibile aggiungere i modelli a un account di archiviazione e collegarli durante la distribuzione con un token SAS.

> [AZURE.IMPORTANT] Attenendosi alla seguente procedura, il BLOB contenente il modello sarà accessibile solo da parte del proprietario dell'account. Tuttavia, quando si crea un token di firma di accesso condiviso per il BLOB, quest'ultimo sarà accessibile a tutti gli utenti con quell'URI. Se l'URI viene intercettato da un altro utente, quest'ultimo sarà in grado di accedere al modello. Utilizzare un token di firma di accesso condiviso è un buon metodo per limitare l'accesso ai modelli, ma è necessario non includere direttamente nel modello dati sensibili come le password.

### Aggiungere un modello privato all'account di archiviazione

Configurare un account di archiviazione per i modelli nel modo seguente:

1. Creare un gruppo di risorse.

        New-AzureRmResourceGroup -Name ManageGroup -Location "West US"

2. Creare un account di archiviazione. Il nome dell'account di archiviazione deve essere univoco in tutto l'ambiente di Azure, pertanto assegnare all'account il proprio nome.

        New-AzureRmStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates -Type Standard_LRS -Location "West US"

3. Impostare l'account di archiviazione corrente.

        Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates

4. Creare un contenitore L'autorizzazione è impostata su **Off**, pertanto il contenitore è accessibile solo al proprietario.

        New-AzureStorageContainer -Name templates -Permission Off
        
5. Aggiungere il proprio modello al contenitore.

        Set-AzureStorageBlobContent -Container templates -File c:\Azure\Templates\azuredeploy.json
        
### Fornire il token SAS in fase di distribuzione

Per distribuire un modello privato in un account di archiviazione, recuperare un token SAS e includerlo nell'URI del modello.

1. Se l'account di archiviazione corrente è stato modificato, impostarlo su quello contenente i propri modelli.

        Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates

2. Creare un token di accesso condiviso con autorizzazioni di lettura e un'ora di scadenza per limitare l'accesso. Recuperare l'URI completo del modello che include il token SAS.

        $templateuri = New-AzureStorageBlobSASToken -Container templates -Blob azuredeploy.json -Permission r -ExpiryTime (Get-Date).AddHours(2.0) -FullUri

3. Distribuire il modello fornendo l'URI che include il token SAS.

        New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri $templateuri

Per un esempio sull'uso di un token di firma di accesso condiviso con modelli collegati, vedere [Uso di modelli collegati con Azure Resource Manager](resource-group-linked-templates.md).

[AZURE.INCLUDE [resource-manager-parameter-file](../includes/resource-manager-parameter-file.md)]

## Precedenza dei parametri

È possibile usare i parametri inline e un file di parametri locale nella stessa operazione di distribuzione. Ad esempio, è possibile specificare alcuni valori nel file di parametri locale e aggiungere altri valori inline durante la distribuzione. Se si specificano valori per un parametro sia nel file dei parametri locale che inline, il valore inline ha la precedenza.

Non è tuttavia possibile usare i parametri inline con un file di parametri esterno. Quando si specifica un file di parametri nel parametro **TemplateParameterUri**, tutti i parametri inline vengono ignorati. È necessario fornire tutti i valori dei parametri nel file esterno. Se il modello include un valore importante che non è possibile includere nel file di parametri, aggiungere tale valore a un insieme di credenziali delle chiavi e farvi riferimento nel file di parametri esterno oppure fornire inline tutti valori dei parametri in modo dinamico.

Per informazioni dettagliate sull'uso di un riferimento KeyVault per passare valori protetti, vedere [Passare valori protetti durante la distribuzione](resource-manager-keyvault-parameter.md).

## Passaggi successivi
- Per un esempio di distribuzione delle risorse con la libreria client .NET, vedere [Distribuire una macchina virtuale di Azure con C# e un modello di Azure Resource Manager](virtual-machines/virtual-machines-windows-csharp-template.md).
- Per definire i parametri nel modello, vedere [Creazione di modelli](resource-group-authoring-templates.md#parameters).
- Per indicazioni sulla distribuzione della soluzione in ambienti diversi, vedere [Ambienti di sviluppo e test in Microsoft Azure](solution-dev-test-environments.md).

<!---HONumber=AcomDC_0921_2016-->