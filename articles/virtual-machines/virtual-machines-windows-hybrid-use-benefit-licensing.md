<properties
   pageTitle="Vantaggio Azure Hybrid Use per Windows Server | Microsoft Azure"
   description="Informazioni su come ottimizzare i vantaggi di Software Assurance per Windows Server per trasferire le licenze locali in Azure"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="07/13/2016"
   ms.author="georgem"/>

# Vantaggio Azure Hybrid Use per Windows Server

Per i clienti che usano Windows Server con Software Assurance è possibile trasferire le licenze locali di Windows Server in Azure ed eseguire macchine virtuali Windows Server in Azure a costi ridotti. Il vantaggio Azure Hybrid Use consente di eseguire macchine virtuali Windows Server in Azure alla sola tariffa di calcolo di base. Per altre informazioni, vedere la [pagina del vantaggio Azure Hybrid Use per la licenza](https://azure.microsoft.com/pricing/hybrid-use-benefit/). Questo articolo spiega come distribuire VM Windows Server in Azure per usare questo vantaggio di gestione delle licenze.

> [AZURE.NOTE] Il vantaggio Azure Hybrid Use non consente di usare immagini di Azure Marketplace per distribuire macchine virtuali Windows Server. È necessario distribuire le VM usando PowerShell o modelli di Resource Manager per registrare correttamente le VM come idonee per la tariffa di calcolo di base.

## Prerequisiti
Per usufruire del vantaggio Azure Hybrid Use per macchine virtuali Windows Server è necessario soddisfare alcuni requisiti:

- Disporre del modulo Azure PowerShell
- Disporre di un disco rigido virtuale di Windows Server da caricare in Archiviazione di Azure

### Installare Azure PowerShell
Verificare di aver prima [installato e configurato l'ultima versione di Azure PowerShell](../powershell-install-configure.md). Anche se si intende distribuire le VM usando modelli di Resource Manager, è comunque necessario aver installato Azure PowerShell per caricare il disco rigido virtuale di Windows Server (vedere il prossimo passaggio).

### Caricare un disco rigido virtuale di Windows Server

Per distribuire una VM Windows Server in Azure è prima necessario creare un disco rigido virtuale contenente la build di base di Windows Server. Questo disco rigido virtuale deve essere correttamente preparato con Sysprep prima di caricarlo in Azure. Sono disponibili [altre informazioni sui requisiti dei dischi rigidi virtuali e sul processo Sysprep](./virtual-machines-windows-upload-image.md) e [Supporto Sysprep per i ruoli server](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles). Eseguire il backup della VM prima di eseguire Sysprep. Dopo aver preparato il disco rigido virtuale, caricarlo nell'account di archiviazione di Azure usando il cmdlet `Add-AzureRmVhd` come segue:

```
Add-AzureRmVhd -ResourceGroupName MyResourceGroup -Destination "https://mystorageaccount.blob.core.windows.net/vhds/myvhd.vhd" -LocalFilePath 'C:\Path\To\myvhd.vhd'
```

> [AZURE.NOTE] Microsoft SQL Server, SharePoint Server e Dynamics possono usare anche le licenza di Software Assurance. È comunque necessario preparare l'immagine di Windows Server installando i componenti dell'applicazione e specificando i codici di licenza corrispondenti, quindi caricando l'immagine del disco in Azure. Esaminare la documentazione appropriata per l'esecuzione di SysPrep con l'applicazione, ad esempio [Considerazioni sull'installazione di SQL Server tramite SysPrep](https://msdn.microsoft.com/library/ee210754.aspx) o [Creare un'immagine di riferimento di SharePoint Server 2016 con Sysprep](http://social.technet.microsoft.com/wiki/contents/articles/33789.build-a-sharepoint-server-2016-reference-image-sysprep.aspx).

Altre informazioni sul [caricamento del disco rigido virtuale in Azure](./virtual-machines-windows-upload-image.md#upload-the-vm-image-to-your-storage-account).

> [AZURE.TIP] Questo articolo è incentrato sulla distribuzione di VM Windows Server. Anche le VM Windows Client possono essere distribuite nello stesso modo. Negli esempi seguenti, sostituire `Server` con `Client`.

## Avvio rapido: Distribuire una macchina virtuale con PowerShell
Quando si distribuisce la macchina virtuale Windows Server con PowerShell è presente un parametro aggiuntivo per `-LicenseType`. Dopo aver caricato il disco rigido virtuale in Azure, creare una nuova macchina virtuale usando `New-AzureRmVM` e specificare il tipo di licenza come segue:

```
New-AzureRmVM -ResourceGroupName MyResourceGroup -Location "West US" -VM $vm -LicenseType Windows_Server
```

È possibile [leggere una procedura più dettagliata sulla distribuzione di una VM in Azure con PowerShell](./virtual-machines-windows-hybrid-use-benefit-licensing.md#deploy-windows-server-vm-via-powershell-detailed-walkthrough) più avanti nel documento oppure vedere una guida più descrittiva con i diversi passaggi per [creare una VM Windows usando Resource Manager e PowerShell](./virtual-machines-windows-ps-create.md).

## Distribuire una macchina virtuale con Resource Manager
Nei modelli di Resource Manager è possibile specificare un parametro aggiuntivo per `licenseType`. Altre informazioni sulla [creazione di modelli di Azure Resource Manager](../resource-group-authoring-templates.md). Dopo aver caricato il disco rigido virtuale in Azure, modificare il modello di Resource Manager per includere il tipo di licenza come parte del provider di calcolo e distribuire il modello come di consueto:

```
"properties": {  
   "licenseType": "Windows_Server",
   "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
   },
```
 
## Verificare che la macchina virtuale usi il vantaggio della licenza
Dopo avere distribuito la VM con il metodo di distribuzione tramite PowerShell o Resource Manager, verificare il tipo di licenza con `Get-AzureRmVM` come indicato di seguito:
 
```
Get-AzureRmVM -ResourceGroup MyResourceGroup -Name MyVM
```

L'output è simile al seguente:

```
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : Windows_Server
```

L'output differisce da quello della macchina virtuale seguente distribuita senza vantaggio Azure Hybrid Use, ad esempio una macchina virtuale distribuita direttamente dalla raccolta di Azure:

```
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : 
```
 
## Procedura dettagliata per PowerShell

La procedura dettagliata per PowerShell seguente illustra la distribuzione completa di una macchina virtuale. Altre informazioni sui cmdlet effettivi e sui diversi componenti creati sono disponibili in [Creare una VM Windows con Resource Manager e PowerShell](./virtual-machines-windows-ps-create.md). Verranno creati il gruppo di risorse, l'account di archiviazione e la rete virtuale, quindi verrà definita e creata la VM.
 
Ottenere prima le credenziali in modo sicuro, specificare una località e definire un nome per il gruppo di risorse:

```
$cred = Get-Credential
$location = "West US"
$resourceGroupName = "TestLicensing"
```

Creare un IP pubblico:

```
$publicIPName = "testlicensingpublicip"
$publicIP = New-AzureRmPublicIpAddress -Name $publicIPName -ResourceGroupName $resourceGroupName -Location $location -AllocationMethod Dynamic
```

Definire la subnet, la scheda di interfaccia di rete e la rete virtuale:

```
$subnetName = "testlicensingsubnet"
$nicName = "testlicensingnic"
$vnetName = "testlicensingvnet"
$subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/8
$vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName -Location $location -AddressPrefix 10.0.0.0/8 -Subnet $subnetconfig
$nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $resourceGroupName -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $publicIP.Id
```

Assegnare un nome alla macchina virtuale e creare una configurazione:

```
$vmName = "testlicensing"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A1"
```

Definire il sistema operativo:

```
$computerName = "testlicensing"
$vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
```

Aggiungere la scheda di interfaccia di rete alla macchina virtuale:

```
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

Definire l'account di archiviazione da usare:

```
$storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -AccountName testlicensing
```

Caricare il disco rigido virtuale adeguatamente preparato e collegarlo alla macchina virtuale:

```
$osDiskName = "licensing.vhd"
$osDiskUri = '{0}vhds/{1}{2}.vhd' -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName
$urlOfUploadedImageVhd = "https://testlicensing.blob.core.windows.net/vhd/licensing.vhd"
$vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption FromImage -SourceImageUri $urlOfUploadedImageVhd -Windows
```

Creare infine la macchina virtuale e definire il tipo di licenza per l'uso del vantaggio Azure Hybrid Use:

```
New-AzureRmVM -ResourceGroupName $resourceGroupName -Location $location -VM $vm -LicenseType Windows_Server
```

## Passaggi successivi

Altre informazioni sul [Vantaggio Microsoft Azure Hybrid Use](https://azure.microsoft.com/pricing/hybrid-use-benefit/).

Altre informazioni sull'[uso dei modelli di Resource Manager](../resource-group-overview.md).

<!---HONumber=AcomDC_0831_2016-->