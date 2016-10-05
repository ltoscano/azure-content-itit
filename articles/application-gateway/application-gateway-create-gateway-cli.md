<properties
   pageTitle="Creare un gateway applicazione con l'interfaccia della riga di comando di Azure | Microsoft Azure"
   description="Informazioni su come creare un gateway applicazione usando l'interfaccia della riga di comando di Azure in Resource Manager"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/> 
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/09/2016"
   ms.author="gwallace" /> 

# Creare un gateway applicazione con l'interfaccia della riga di comando di Azure

Il gateway applicazione di Azure è un dispositivo di bilanciamento del carico di livello 7. Fornisce richieste HTTP con routing delle prestazioni e failover tra server diversi, sia nel cloud che in locale. Il gateway applicazione offre le seguenti funzionalità di distribuzione delle applicazioni: bilanciamento del carico HTTP, affinità di sessione basata sui cookie, offload SSL (Secure Sockets Layer), probe di integrità personalizzati e supporto per più siti.

> [AZURE.SELECTOR]
- [Portale di Azure](application-gateway-create-gateway-portal.md)
- [PowerShell per Azure Resource Manager](application-gateway-create-gateway-arm.md)
- [PowerShell per Azure classico](application-gateway-create-gateway.md)
- [Modello di Azure Resource Manager](application-gateway-create-gateway-arm-template.md)
- [Interfaccia della riga di comando di Azure](application-gateway-create-gateway-cli.md)

## Prerequisito: installare l'interfaccia della riga di comando di Azure

Per eseguire i passaggi in questo articolo, è necessario [installare l'interfaccia della riga di comando di Azure per Mac, Linux e Windows (interfaccia della riga di comando di Azure CLI)](../xplat-cli-install.md) ed è necessario [accedere a Azure](../xplat-cli-connect.md).

> [AZURE.NOTE] Se non si dispone di un account Azure, è necessario procurarsene uno. Usare la [versione di valutazione gratuita](../active-directory/sign-up-organization.md).

## Scenario

Questo scenario mostra come creare un gateway applicazione usando il portale di Azure.

Questo scenario illustrerà come:

- Creare un gateway applicazione Medium con due istanze.
- Creare una rete virtuale denominata AdatumAppGatewayVNET con un blocco CIDR riservato 10.0.0.0/16.
- Creare una subnet denominata Appgatewaysubnet che usa 10.0.0.0/28 come blocco CIDR.
- Configurare un certificato per l'offload SSL.

![Esempio dello scenario][scenario] 

>[AZURE.NOTE] La configurazione aggiuntiva del gateway applicazione, che include i probe di integrità personalizzati, gli indirizzi del pool back-end e le regole aggiuntive, viene definita dopo la configurazione del gateway applicazione e non durante la distribuzione iniziale.

## Prima di iniziare

Il gateway applicazione di Azure richiede una propria subnet. Quando si crea una rete virtuale, assicurarsi di lasciare uno spazio indirizzi sufficiente per più subnet. Dopo che un gateway applicazione è stato distribuito in una subnet, alla subnet possono essere aggiunti solo altri gateway applicazione.

## Accedere ad Azure

Aprire il **prompt dei comandi di Microsoft Azure** ed effettuare l'accesso.

    azure login

Dopo avere digitato l'esempio precedente, viene fornito un codice. Passare a https://aka.ms/devicelogin in un browser per continuare il processo di accesso.

![Comando che illustra l'accesso al dispositivo][1] 

Nel browser immettere il codice ricevuto. Si verrà reindirizzati a una pagina di accesso.

![Browser in cui immettere il codice][2] 

Dopo avere immesso il codice ed effettuato l'accesso, chiudere il browser per continuare con lo scenario.

![Accesso eseguito][3] 

## Passare alla modalità Resource Manager

    azure config mode arm

## Creare il gruppo di risorse.

Prima di creare il gateway applicazione viene creato un gruppo di risorse che contenga il gateway applicazione. Di seguito è riportato il comando.

    azure group create -n AdatumAppGatewayRG -l eastus

## Crea rete virtuale

Dopo aver creato il gruppo di risorse, viene creata una rete virtuale per il gateway applicazione. Nell'esempio seguente, lo spazio degli indirizzi era 10.0.0.0/16 come definito nelle note sullo scenario precedente.

    azure network vnet create -n AdatumAppGatewayVNET -a 10.0.0.0/16 -g AdatumAppGatewayRG -l eastus

## Creare una subnet

Dopo aver creato la rete virtuale, viene aggiunta una subnet per il gateway applicazione. Se si intende usare il gateway applicazione con un'app Web ospitata nella stessa rete virtuale del gateway applicazione, assicurarsi di lasciare spazio sufficiente per un'altra subnet.

    azure network vnet subnet create -g AdatumAppGatewayRG -n Appgatewaysubnet -v AdatumAppGatewayVNET -a 10.0.0.0/28 

## Creare il gateway applicazione

Dopo aver creato la rete virtuale e la subnet, i prerequisiti per il gateway applicazione sono soddisfatti. Per il passaggio seguente sono necessari anche un certificato PFX esportato prima e la password del certificato. Gli indirizzi IP usati per il back-end sono gli indirizzi IP per il server back-end. Questi valori possono essere indirizzi IP privati nella rete virtuale, indirizzi IP pubblici o nomi di dominio completi per i server back-end.

    azure network application-gateway create -n AdatumAppGateway -l eastus -g AdatumAppGatewayRG -e AdatumAppGatewayVNET -m Appgatewaysubnet -r 134.170.185.46,134.170.188.221,134.170.185.50 -y c:\AdatumAppGateway\adatumcert.pfx -x P@ssw0rd -z 2 -a Standard_Medium -w Basic -j 443 -f Enabled -o 80 -i http -b https -u Standard



Questo esempio crea un gateway applicazione di base con le impostazioni predefinite per il listener, il pool back-end, le impostazioni HTTP back-end e le regole. Viene anche configurato l'offload SSL. Queste impostazioni possono essere modificate in base alla propria distribuzione dopo che è stato completato il provisioning. Se l'applicazione Web è già stata definita con il pool back-end nei passaggi precedenti, dopo la creazione, inizia il bilanciamento del carico.

## Passaggi successivi

Per informazioni su come creare probe di integrità personalizzati, vedere [Creare un probe personalizzato per un gateway applicazione con il portale](application-gateway-create-probe-portal.md)

Per informazioni su come configurare l'offload SSL ed evitare costose attività di decrittografia SSL nei server Web, vedere [Configurare un gateway applicazione per l'offload SSL con Azure Resource Manager](application-gateway-ssl-arm.md)

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli/scenario.png
[1]: ./media/application-gateway-create-gateway-cli/figure1.png
[2]: ./media/application-gateway-create-gateway-cli/figure2.png
[3]: ./media/application-gateway-create-gateway-cli/figure3.png

<!---HONumber=AcomDC_0921_2016-->