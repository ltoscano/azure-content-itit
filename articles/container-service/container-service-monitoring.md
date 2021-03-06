<properties
   pageTitle="Monitorare un cluster del servizio contenitore di Azure con Datadog | Microsoft Azure"
   description="Monitorare un cluster del servizio contenitore di Azure con Datadog. Usare l'interfaccia utente Web del controller di dominio/sistema operativo per distribuire gli agenti Datadog al cluster."
   services="container-service"
   documentationCenter=""
   authors="rbitia"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Contenitori, controller di dominio/sistema operativo, Docker Swarm, Azure"/>  

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure"   
   ms.date="07/28/2016"
   ms.author="t-ribhat"/>  

# Monitorare un cluster del servizio contenitore di Azure con Datadog

In questo articolo verranno distribuiti agenti di Datadog in tutti i nodi agente nel cluster del servizio contenitore di Azure. Per questa configurazione, sarà necessario un account con Datadog.

## Prerequisiti 

[Distribuire](container-service-deployment.md) e [connettere](container-service-connect.md) un cluster configurato dal servizio contenitore di Azure. Esplorare l'[interfaccia utente](container-service-mesos-marathon-ui.md) di Marathon. Andare a [http://datadoghq.com](http://datadoghq.com) per configurare un account Datadog.

## Datadog 

Datadog è un servizio che raccoglie dati di monitoraggio dai contenitori all'interno del cluster del servizio contenitore di Azure. Datadog è dotato di un dashboard di integrazione Docker in cui è possibile visualizzare metriche specifiche all'interno dei propri contenitori. Le metriche raccolte dai contenitori sono organizzate per CPU, memoria, rete e I/O. Datadog suddivide le metriche in contenitori e immagini. Un esempio dell'aspetto dell'interfaccia utente per l'utilizzo della CPU è riportato di seguito.

![Interfaccia utente Datadog](./media/container-service-monitoring/datadog4.png)  

## Configurare una distribuzione Datadog con Marathon

Questi passaggi illustrano come configurare e distribuire le applicazioni Datadog nel cluster con Marathon.

Accedere all'interfaccia utente del controller di dominio/sistema operativo da [http://localhost:80/](http://localhost:80/). Dall'interfaccia utente del controller di dominio/sistema operativo passare a "Universe" ("Universo") in basso a sinistra e quindi cercare "Datadog" e fare clic su "Install".

![Pacchetto Datadog in Universe (Universo) per il controller di dominio/sistema operativo](./media/container-service-monitoring/datadog1.png)  

Per completare la configurazione, sarà necessario un account cloud Datadog o un account di valutazione gratuita. Dopo aver eseguito l'accesso al sito Web di Datadog, a sinistra passare a Integrations -> API.

![Chiave API di Datadog](./media/container-service-monitoring/datadog2.png)  

Immettere quindi la chiave API nella configurazione di Datadog in Universe (Universo) per il controller di dominio/sistema operativo.

![Configurazione di Datadog in Universe (Universo) per il controller di dominio/sistema operativo](./media/container-service-monitoring/datadog3.png)  

Nella configurazione precedente le istanze sono impostate su 10000000 in modo che quando viene aggiunto un nuovo nodo al cluster Datadog distribuirà automaticamente un agente in tale nodo. Si tratta di una soluzione temporanea. Dopo aver installato il pacchetto si dovrebbe tornare al sito Web Datadog e cercare "Dashboards". Da qui si vedrà Custom and Integration Dashboards. Il dashboard di integrazione Docker conterrà tutte le metriche del contenitore necessarie per il monitoraggio del cluster.

<!---HONumber=AcomDC_0810_2016-->