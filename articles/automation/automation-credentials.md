<properties 
   pageTitle="Asset di tipo credenziali in Automazione di Azure | Microsoft Azure"
   description="Gli asset credenziali in Automazione di Azure includono le credenziali di sicurezza che possono essere usate per l'autenticazione nelle risorse a cui accede il Runbook o la configurazione DSC. Questo articolo illustra come creare asset credenziali e usarli in un Runbook o in una configurazione DSC."
   services="automation"
   documentationCenter=""
   authors="mgoedtel"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="06/09/2016"
   ms.author="bwren" />

# Asset credenziali in Automazione di Azure

Un asset credenziali di Automazione contiene un oggetto [PSCredential](http://msdn.microsoft.com/library/system.management.automation.pscredential) che contiene le credenziali di sicurezza, ad esempio un nome utente e una password. I Runbook e le configurazioni DSC possono usare i cmdlet che accettano un oggetto PSCredential per l'autenticazione oppure possono estrarre il nome utente e la password dell'oggetto PSCredential per fornirli ad alcune applicazioni o servizi che richiedono l'autenticazione. Le proprietà delle credenziali sono archiviate in modo sicuro in Automazione di Azure ed è possibile accedervi nel Runbook o nella configurazione DSC con l'attività [Get-AutomationPSCredential](http://msdn.microsoft.com/library/system.management.automation.pscredential.aspx).

>[AZURE.NOTE] Gli asset sicuri in Automazione di Azure includono credenziali, certificati, connessioni e variabili crittografate. Questi asset vengono crittografati e archiviati in Automazione di Azure tramite una chiave univoca generata per ogni account di automazione. La chiave viene crittografata da un certificato master e archiviata in Automazione di Azure. Prima dell'archiviazione di un asset sicuro, la chiave per l'account di automazione viene decrittografata usando il certificato master e viene quindi usata per crittografare l'asset.

## Cmdlet di Windows PowerShell

I cmdlet della tabella seguente vengono usati per creare e gestire asset credenziali di automazione con Windows PowerShell. Sono inclusi nel [modulo Azure PowerShell](../powershell-install-configure.md), disponibile per l'uso nei runbook di Automazione e nelle configurazioni DSC.

|Cmdlet|Descrizione|
|:---|:---|
|[Get-AzureAutomationCredential](http://msdn.microsoft.com/library/dn913781.aspx)|Recupera informazioni riguardo un asset credenziali. Dall'attività **Get-AutomationPSCredential** è possibile recuperare soltanto le credenziali stesse.|
|[New-AzureAutomationCredential](http://msdn.microsoft.com/library/azure/jj554330.aspx)|Crea nuove credenziali di Automazione.|
|[Remove- AzureAutomationCredential](http://msdn.microsoft.com/library/azure/jj554330.aspx)|Rimuove le credenziali di Automazione.|
|[Set- AzureAutomationCredential](http://msdn.microsoft.com/library/azure/jj554330.aspx)|Imposta le proprietà per le credenziali di Automazione esistenti.|

## Attività del Runbook

Le attività incluse nella tabella seguente vengono usate per accedere alle credenziali in un Runbook o nelle configurazioni DSC.

|Attività|Descrizione|
|:---|:---|
|Get-AutomationPSCredential|Ottiene una credenziale da usare in un runbook o in una configurazione DSC. Restituisce un oggetto [System.Management.Automation.PSCredential](http://msdn.microsoft.com/library/system.management.automation.pscredential).|

>[AZURE.NOTE] È consigliabile evitare di usare le variabili nel parametro –Name di Get-AutomationPSCredential, poiché ciò può complicare l'individuazione delle dipendenze tra i Runbook o configurazioni DSC e gli asset credenziali durante la fase di progettazione.

## Creazione di un nuovo asset credenziali


### Per creare un nuovo asset credenziali con il portale di Azure classico

1. Dall'account di automazione fare clic su **Asset** nella parte superiore della finestra.
1. Nella parte inferiore della finestra, fare clic su **Aggiungi impostazione**.
1. Fare clic su **Aggiungi credenziali**.
2. Nell'elenco a discesa **Tipo di credenziali** selezionare **Credenziali PowerShell**.
1. Completare la procedura guidata e selezionare la casella di controllo per salvare le nuove credenziali.


### Per creare un nuovo asset credenziali con il portale di Azure

1. Dall'account di automazione fare clic sulla parte **Asset** per aprire il pannello **Asset**.
1. Fare clic sulla parte **Credenziali** per aprire il pannello **Credenziali**.
1. Fare clic su **Aggiungi credenziali** nella parte superiore del pannello.
1. Completare il modulo e fare clic su **Crea** per salvare le nuove credenziali.


### Per creare un nuovo asset credenziali con Windows PowerShell

I comandi di esempio seguenti mostrano come creare nuove credenziali di Automazione. Viene prima creato un oggetto PSCredential con il nome e la password, che verrà quindi usato per creare l'asset credenziali. In alternativa, usare il cmdlet **Get-Credential** per ricevere la richiesta di digitare nome e password.

	$user = "MyDomain\MyUser"
	$pw = ConvertTo-SecureString "PassWord!" -AsPlainText -Force
	$cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $user, $pw
	New-AzureAutomationCredential -AutomationAccountName "MyAutomationAccount" -Name "MyCredential" -Value $cred

## Uso di credenziali PowerShell

L'attività **Get-AutomationPSCredential** permette di recuperare un asset credenziali in un Runbook o configurazione DSC. Verrà restituito un [PSCredential object](http://msdn.microsoft.com/library/system.management.automation.pscredential.aspx) da usare con un'attività o un cmdlet che richiede un parametro PSCredential. È anche possibile recuperare le proprietà dell'oggetto credenziali da usare individualmente. L'oggetto ha una proprietà per il nome utente e la password protetta oppure è possibile usare il metodo **GetNetworkCredential** per restituire un oggetto [NetworkCredential](http://msdn.microsoft.com/library/system.net.networkcredential.aspx) che fornirà una versione non protetta della password.

### Esempio di Runbook testuale

I comandi di esempio seguenti mostrano come usare le credenziali PowerShell in un Runbook. In questo esempio, vengono recuperate le credenziali e il nome utente e la password vengono assegnati alle variabili.

	$myCredential = Get-AutomationPSCredential -Name 'MyCredential'
	$userName = $myCredential.UserName
	$securePassword = $myCredential.Password
	$password = $myCredential.GetNetworkCredential().Password


### Esempio di Runbook grafico

Per aggiungere un'attività **Get-AutomationPSCredential** a un Runbook grafico, fare clic con il pulsante destro del mouse sulle credenziali nel riquadro della libreria dell'editor grafico e scegliere **Aggiungi a area di disegno**.


![Aggiungere credenziali all'area di disegno](media/automation-credentials/credential-add-canvas.png)

La figura seguente mostra un esempio dell'uso di credenziali in un Runbook grafico. In questo caso vengono usate per fornire l'autenticazione per un runbook a risorse di Azure, come illustrato in [Autenticare i runbook con Azure Service Management e Azure Resource Manager](automation-sec-configure-aduser-account.md). La prima attività recupera le credenziali che possono accedere alla sottoscrizione di Azure. L'attività **Add-AzureAccount** usa quindi queste credenziali per fornire l'autenticazione per eventuali attività successive. Viene usato un [collegamento pipeline](automation-graphical-authoring-intro.md#links-and-workflow) poiché **Get-AutomationPSCredential** prevede un singolo oggetto.

![Aggiungere credenziali all'area di disegno](media/automation-credentials/get-credential.png)

## Uso di credenziali PowerShell in DSC
Anche se le configurazioni DSC in Automazione di Azure possono fare riferimento ad asset credenziali con **Get-AutomationPSCredential**, gli asset credenziali possono essere passati anche con i parametri, se necessario. Per ulteriori informazioni vedere [Compilazione di configurazioni in Azure Automation DSC](automation-dsc-compile.md#credential-assets).

## Passaggi successivi

- Per altre informazioni sui collegamenti nella creazione grafica, vedere [Creazione grafica in Automazione di Azure](automation-graphical-authoring-intro.md#links-and-workflow)
- Per comprendere i diversi metodi di autenticazione con Automazione, vedere [Sicurezza in Automazione di Azure](automation-security-overview.md)
- Per iniziare a usare runbook grafici, vedere [Il primo runbook grafico](automation-first-runbook-graphical.md)
- Per iniziare a usare runbook del flusso di lavoro PowerShell, vedere [Il primo runbook del flusso di lavoro PowerShell](automation-first-runbook-textual.md) 

 

<!---HONumber=AcomDC_0615_2016-->