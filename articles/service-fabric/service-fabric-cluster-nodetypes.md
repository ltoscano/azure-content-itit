<properties
   pageTitle="Tipi di nodo di Service Fabric e set di scalabilità di macchine virtuali | Microsoft Azure"
   description="Descrive come i tipi di nodo di Service Fabric siano correlati ai set di scalabilità di macchine virtuali e come connettersi in remoto a un'istanza del set di scalabilità di macchine virtuali o a un nodo del cluster."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/> 

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/09/2016"
   ms.author="chackdan"/> 


# Relazione tra i tipi di nodo di Service Fabric e i set di scalabilità di macchine virtuali

I set di scalabilità di macchine virtuali sono una risorsa di calcolo di Azure che è possibile usare per distribuire e gestire una raccolta di macchine virtuali come set. Ogni tipo di nodo definito in un cluster di Service Fabric è configurato come un set di scalabilità di macchine virtuali separato. Ogni tipo di nodo può quindi essere aumentato o ridotto in modo indipendente, avere diversi set di porte aperte e avere metriche per la capacità diverse.

Lo screenshot seguente illustra un cluster con due tipi di nodo: front-end e back-end. Ogni tipo di nodo ha cinque nodi.

![Screenshot di un cluster con due tipi di nodo][NodeTypes]

## Mapping dei set di scalabilità di macchine virtuali ai nodi

Come si può notare dalla figura precedente, le istanze dei set di scalabilità di macchine virtuali iniziano con l'istanza 0 per poi aumentare. La numerazione viene rispecchiata dai nomi. Ad esempio, BackEnd_0 è l'istanza 0 del set di scalabilità di macchine virtuali BackEnd. Questo particolare set di scalabilità di macchine virtuali ha cinque istanze, denominate BackEnd_0, BackEnd_1, BackEnd_2, BackEnd_3 e BackEnd_4.

Quando si aumenta un set di scalabilità di macchine virtuali, viene creata una nuova istanza. La nuova istanza del set di scalabilità di macchine virtuali sarà in genere il nome del set di scalabilità di macchine virtuali + il successivo numero di istanza. Nell'esempio sarà BackEnd\_5.


## Mapping dei servizi di bilanciamento del carico dei set di scalabilità di macchine virtuali a ogni tipo di nodo/set di scalabilità di macchine virtuali

Se il cluster è stato distribuito dal portale o è stato usato il modello di Resource Manager di esempio fornito, quando si ottiene un elenco di tutte le risorse di un gruppo di risorse, verranno visualizzati i servizi di bilanciamento del carico per ogni set di scalabilità di macchine virtuali o tipo di nodo.

Il nome sarà simile a: **LB-&lt;nome tipo di nodo&gt;**, ad esempio, LB-sfcluster4doc-0, come in questo screenshot:


![Risorse][Resources] 


## Connessione remota a un'istanza di set di scalabilità di macchine virtuali o a un nodo del cluster
Ogni tipo di nodo definito in un cluster viene configurato come set di scalabilità di macchine virtuali separato. I tipi di nodo possono quindi essere aumentati o ridotti in modo indipendente ed essere costituiti da SKU di VM diverse. Diversamente dalle VM a istanza singola, le istanze dei set di scalabilità di macchine virtuali non ottengono un proprio indirizzo IP virtuale. Può quindi essere difficile cercare un indirizzo IP e una porta da usare per connettersi in remoto a un'istanza specifica.

Ecco i passaggi che è possibile seguire per trovarli.

### Passaggio 1: Trovare l'indirizzo IP virtuale per il tipo di nodo e quindi le regole NAT in ingresso per RDP

A questo scopo, è necessario ottenere i valori delle regole NAT in ingresso definiti nell'ambito della definizione di risorse per **Microsoft.Network/loadBalancers**.

Nel portale passare al pannello del servizio di bilanciamento del carico e quindi fare clic su **Impostazioni**.

![Pannello servizio di bilanciamento del carico][LBBlade] 


In **Impostazioni** fare clic su **Regole NAT in ingresso**. Si ottengono così l'indirizzo IP e la porta che è possibile usare per connettersi in remoto alla prima istanza del set di scalabilità di macchine virtuali. Nello screenshot seguente sono **104.42.106.156** e **3389**

![Regole NAT][NATRules] 

### Passaggio 2: Trovare la porta che è possibile usare per connettersi in remoto all'istanza del set di scalabilità di macchine virtuali/nodo specifico

Nella prima parte di questo documento si è parlato di come venga eseguito il mapping delle istanze dei set di scalabilità di macchine virtuali ai nodi. Questa operazione verrà usata per trovare la porta esatta.

Le porte vengono allocate in ordine crescente nell'istanza del set di scalabilità di macchine virtuali, quindi nell'esempio, per il tipo di nodo FrontEnd, le porte per ognuna delle cinque istanze sono le seguenti. Ora è necessario eseguire lo stesso mapping per l'istanza del set di scalabilità di macchine virtuali.

|**Istanza del set di scalabilità di macchine virtuali**|**Porta**|
|-----------------------|--------------------------|
|FrontEnd\_0|3389|
|FrontEnd\_1|3390|
|FrontEnd\_2|3391|
|FrontEnd\_3|3392|
|FrontEnd\_4|3393|
|FrontEnd\_5|3394|


### Passaggio 3: Connettersi in remoto all'istanza specifica del set di scalabilità di macchine virtuali

Nello screenshot seguente viene usata Connessione Desktop remoto per connettersi a FrontEnd\_1:

![RDP][RDP] 

## Come modificare i valori dell'intervallo di porte RDP

### Prima della distribuzione cluster

Quando si configura il cluster usando un modello di Resource Manager, è possibile specificare l'intervallo in **inboundNatPools**.

Passare alla definizione di risorse per **Microsoft.Network/loadBalancers** dove si troverà la descrizione per **inboundNatPools**. Sostituire i valori *frontendPortRangeStart* e *frontendPortRangeEnd*.

![Pool NAT in ingresso][InboundNatPools] 


### Dopo la distribuzione cluster
È una procedura un po' più complessa ed è possibile che le VM vengano riciclate. Sarà necessario impostare nuovi valori usando Azure PowerShell. Verificare che nel computer sia installato Azure PowerShell 1.0 o versione successiva. Se non è ancora installato, si consiglia vivamente di seguire la procedura descritta in [Come installare e configurare Azure PowerShell](../powershell-install-configure.md).

Accedere all'account Azure. Se per qualche motivo questo comando PowerShell genera un errore, verificare che Azure PowerShell sia installato correttamente.

```
Login-AzureRmAccount
```

Eseguire quanto segue per ottenere i dettagli sul servizio di bilanciamento del carico e visualizzare i valori della descrizione per **inboundNatPools**:

```
Get-AzureRmResource -ResourceGroupName <RGname> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load balancer name>
```

Impostare *frontendPortRangeEnd* e *frontendPortRangeStart* sui valori desiderati.

```
$PropertiesObject = @{
	#Property = value;
}
Set-AzureRmResource -PropertyObject $PropertiesObject -ResourceGroupName <RG name> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load Balancer name> -ApiVersion <use the API version that get returned> -Force
```


## Passaggi successivi

- [Panoramica della funzionalità "Distribuzione in qualsiasi ambiente" e confronto con i cluster gestiti da Azure](service-fabric-deploy-anywhere.md)
- [Sicurezza del cluster](service-fabric-cluster-security.md)
- [Service Fabric SDK e introduzione](service-fabric-get-started.md)


<!--Image references--> 
[NodeTypes]: ./media/service-fabric-cluster-nodetypes/NodeTypes.png
[Resources]: ./media/service-fabric-cluster-nodetypes/Resources.png
[InboundNatPools]: ./media/service-fabric-cluster-nodetypes/InboundNatPools.png
[LBBlade]: ./media/service-fabric-cluster-nodetypes/LBBlade.png
[NATRules]: ./media/service-fabric-cluster-nodetypes/NATRules.png
[RDP]: ./media/service-fabric-cluster-nodetypes/RDP.png

<!---HONumber=AcomDC_0921_2016-->