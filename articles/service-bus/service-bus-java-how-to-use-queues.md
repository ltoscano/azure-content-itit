<properties
	pageTitle="Come usare le code del bus di servizio con Java | Microsoft Azure"
	description="Informazioni su come usare le code di Bus di servizio in Azure. Gli esempi di codice sono scritti in Java."
	services="service-bus"
	documentationCenter="java"
	authors="sethmanheim"
	manager="timlt"
	/>

<tags
	ms.service="service-bus"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="Java"
	ms.topic="article"
	ms.date="05/06/2016"
	ms.author="sethm"/>

# Come usare le code del bus di servizio

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Questo articolo illustra come usare le code del bus di servizio. Gli esempi sono scritti in Java e utilizzano [Azure SDK for Java][]. Gli scenari presentati includono **creazione di code**, **invio e ricezione di messaggi**, nonché **eliminazione di code**.

## Informazioni sulle code del bus di servizio

Le code del bus di servizio supportano un modello di comunicazione con **messaggistica negoziata**. Quando si usano le code, i componenti di un'applicazione distribuita non comunicano direttamente l'uno con l'altro, ma scambiano messaggi tramite una coda, che agisce da intermediario (broker). Un producer di messaggi (mittente) invia un messaggio alla coda e quindi prosegue con la relativa elaborazione. In modo asincrono, il consumer di messaggi (ricevitore) recupera il messaggio dalla coda e lo elabora. Il producer non deve attendere la risposta del consumer per continuare a elaborare e inviare ulteriori messaggi. Le code consentono un recapito dei messaggi di tipo **FIFO (First In, First Out)** a uno o più consumer concorrenti. In base a questo metodo, in genere i messaggi vengono ricevuti ed elaborati nell'ordine temporale in cui sono stati aggiunti alla coda e ogni messaggio viene ricevuto ed elaborato da un solo consumer.

![Concetti relativi alle code](./media/service-bus-java-how-to-use-queues/sb-queues-08.png)

Le code del bus di servizio sono una tecnologia di carattere generale che può essere usata in numerosi scenari:

- Comunicazione tra ruoli Web e di lavoro in un'applicazione Azure multilivello.
- Comunicazione tra app locali e app ospitate in Azure in una soluzione ibrida.
- Comunicazione tra componenti di un'applicazione distribuita in esecuzione in locale in organizzazioni diverse o in reparti diversi della stessa organizzazione.

L'uso delle code consente di scalare orizzontalmente in modo più semplice le applicazioni e garantisce maggiore resilienza nell'architettura.

## Creare uno spazio dei nomi del servizio

Per iniziare a usare le code del bus di servizio in Azure, è innanzitutto necessario creare uno spazio dei nomi. Uno spazio dei nomi fornisce un contenitore di ambito per fare riferimento alle risorse del bus di servizio all'interno dell'applicazione.

Per creare uno spazio dei nomi:

1.  Accedere al [portale di Azure classico][].

2.  Nel pannello di navigazione sinistro del portale fare clic su **Bus di servizio**.

3.  Nel riquadro inferiore del portale fare clic su **Crea**. ![](./media/service-bus-java-how-to-use-queues/sb-queues-03.png)

4.  Nella finestra di dialogo **Add a new namespace** immettere un nome per lo spazio dei nomi. Verrà effettuato immediatamente un controllo sulla disponibilità del nome.![](./media/service-bus-java-how-to-use-queues/sb-queues-04.png)

5.  Dopo avere verificato che lo spazio dei nomi è disponibile, scegliere il paese o l'area in cui dovrà essere ospitato. Assicurarsi di usare lo stesso paese/area in cui verranno distribuite le risorse di calcolo.

	IMPORTANTE: selezionare la **stessa area** che si intende scegliere per la distribuzione dell'applicazione. In questo modo sarà possibile ottenere prestazioni ottimali.

6. 	Non modificare i valori predefiniti negli altri campi della finestra di dialogo (**Messaggistica** e **Livello Standard**), quindi fare clic sul segno di spunta. A questo punto, lo spazio dei nomi verrà creato e abilitato nel sistema. Potrebbero essere necessari alcuni minuti per consentire al sistema di effettuare il provisioning delle risorse per lo spazio dei nomi creato.

Sarà necessario attendere qualche istante affinché venga attivato lo spazio dei nomi creato che verrà quindi visualizzato nel portale di Azure. Prima di continuare, attendere che lo stato dello spazio dei nomi sia **Attivo**.

## Recuperare le credenziali di gestione predefinite per lo spazio dei nomi

Per poter eseguire le operazioni di gestione, ad esempio creare una coda, nel nuovo spazio dei nomi, è necessario ottenere le credenziali di gestione per lo spazio dei nomi. È possibile ottenere queste credenziali dal portale.

1.  Nel riquadro di navigazione sinistro fare clic sul nodo **Bus di servizio** per visualizzare l'elenco degli spazi dei nomi disponibili: ![](./media/service-bus-java-how-to-use-queues/sb-queues-13.png)

2.  Selezionare lo spazio dei nomi appena creato nell'elenco visualizzato.

3.  Fare clic su **Configura** per visualizzare i criteri di accesso condivisi per lo spazio dei nomi. ![](./media/service-bus-java-how-to-use-queues/sb-queues-14.png)

4.  Prendere nota del valore della chiave o copiarlo negli Appunti.

## Configurare l'applicazione per l'uso del bus di servizio

Assicurarsi di aver installato [Azure SDK per Java][] prima di compilare questo esempio. Se si utilizza Eclipse, è possibile installare [Azure Toolkit per Eclipse][] che include Azure SDK per Java. È quindi possibile aggiungere le **librerie di Microsoft Azure per Java** al progetto:

![](./media/service-bus-java-how-to-use-queues/eclipselibs.png)

Aggiungere le seguenti istruzioni `import` all'inizio del file Java:

```
// Include the following imports to use Service Bus APIs
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

## Creare una coda

Per eseguire operazioni di gestione per le code del bus di servizio, è possibile utilizzare la classe **ServiceBusContract** class. Un oggetto **ServiceBusContract** è costruito con una configurazione appropriata che incapsula il token di firma di accesso condiviso con le autorizzazioni necessarie a gestirlo. La classe **ServiceBusContract** rappresenta l'unico punto di comunicazione con Azure.

La classe **ServiceBusService** fornisce i metodi per creare, enumerare ed eliminare le code. Nell'esempio seguente viene illustrato il modo in cui è possibile utilizzare un oggetto **ServiceBusService** per creare una coda denominata "TestQueue" con uno spazio dei nomi denominato "HowToSample":

		Configuration config =
			ServiceBusConfiguration.configureWithSASAuthentication(
					"HowToSample",
					"RootManageSharedAccessKey",
					"SAS_key_value",
					".servicebus.windows.net"
					);

    ServiceBusContract service = ServiceBusService.create(config);
    QueueInfo queueInfo = new QueueInfo("TestQueue");
    try
    {
		CreateQueueResult result = service.createQueue(queueInfo);
    }
	catch (ServiceException e)
	{
	    System.out.print("ServiceException encountered: ");
        System.out.println(e.getMessage());
        System.exit(-1);
    }

In **QueueInfo** sono disponibili metodi che consentono di ottimizzare le proprietà della coda, ad esempio per impostare il valore di durata (TTL) predefinito da applicare ai messaggi inviati alla coda. Il seguente esempio illustra come creare una coda denominata `TestQueue` con una dimensione massima pari a 5 GB:

    long maxSizeInMegabytes = 5120;
    QueueInfo queueInfo = new QueueInfo("TestQueue");
    queueInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
    CreateQueueResult result = service.createQueue(queueInfo);

Si noti che è possibile utilizzare il metodo **listQueues** su oggetti **ServiceBusContract** per verificare se in uno spazio dei nomi servizio esiste già una coda con il nome specificato.

## Inviare messaggi a una coda

Per inviare un messaggio a una coda del bus di servizio, l'applicazione ottiene un oggetto **ServiceBusContract**. Il seguente codice illustra come inviare un messaggio per la coda `TestQueue` creata in precedenza all'interno dello spazio dei nomi `HowToSample`.

    try
    {
        BrokeredMessage message = new BrokeredMessage("MyMessage");
        service.sendQueueMessage("TestQueue", message);
    }
    catch (ServiceException e)
    {
        System.out.print("ServiceException encountered: ");
        System.out.println(e.getMessage());
        System.exit(-1);
    }

I messaggi inviati e ricevuti dalla coda del bus di servizio sono istanze della classe [BrokeredMessage][]. Gli oggetti [BrokeredMessage][] includono un insieme di proprietà standard, ad esempio [Label](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) e [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx), un dizionario utilizzato per contenere le proprietà personalizzate specifiche dell'applicazione e un corpo di dati arbitrari dell'applicazione. Per impostare il corpo del messaggio, un'applicazione può passare qualsiasi oggetto serializzabile nel costruttore di [BrokeredMessage][]. In tal caso, per serializzare l'oggetto verrà utilizzato il serializzatore appropriato. In alternativa, è possibile fornire un oggetto **java.IO.InputStream**.

L'esempio seguente illustra come inviare cinque messaggi di prova all'oggetto `TestQueue` **MessageSender** ottenuto nel frammento di codice precedente.

    for (int i=0; i<5; i++)
    {
         // Create message, passing a string message for the body.
         BrokeredMessage message = new BrokeredMessage("Test message " + i);
         // Set an additional app-specific property.
         message.setProperty("MyProperty", i);
         // Send message to the queue
         service.sendQueueMessage("TestQueue", message);
    }

Le code del bus di servizio supportano messaggi di dimensioni massime pari a 256 KB nel [livello Standard](service-bus-premium-messaging.md) e pari a 1 MB nel [livello Premium](service-bus-premium-messaging.md). Le dimensioni massime dell'intestazione, che include le proprietà standard e personalizzate dell'applicazione, non possono superare 64 KB. Non esiste alcun limite al numero di messaggi mantenuti in una coda, mentre è prevista una limitazione alla dimensione totale dei messaggi di una coda. Questa dimensione della coda viene definita al momento della creazione, con un limite massimo di 5 GB.

## Ricevere messaggi da una coda

Il modo principale per ricevere i messaggi da una coda consiste nell'utilizzare un oggetto **ServiceBusContract**. I messaggi ricevuti possono essere usati in due modalità diverse: **ReceiveAndDelete** e **PeekLock**.

Quando si utilizza la modalità **ReceiveAndDelete**, l'operazione di ricezione viene eseguita in un'unica fase. Quando infatti il bus di servizio riceve la richiesta di lettura relativa a un messaggio in una coda, lo contrassegna come utilizzato e lo restituisce all'applicazione. La modalità predefinita **ReceiveAndDelete** costituisce il modello più semplice ed è adatta per scenari in cui un'applicazione può tollerare la mancata elaborazione di un messaggio in caso di errore. Per comprendere meglio questo meccanismo, si consideri uno scenario in cui il consumer invia la richiesta di ricezione e viene arrestato in modo anomalo prima dell'elaborazione. Poiché il bus di servizio contrassegna il messaggio come utilizzato, quando l'applicazione viene riavviata e inizia a utilizzare nuovamente i messaggi, il messaggio utilizzato prima dell'arresto anomalo risulterà perso.

Nella modalità **PeekLock** l'operazione di ricezione viene suddivisa in due fasi, in modo da consentire il supporto di applicazioni che non possono tollerare messaggi mancanti. Quando il bus di servizio riceve una richiesta, individua il messaggio successivo da usare, lo blocca per impedirne la ricezione da parte di altri consumer e quindi lo restituisce all'applicazione. Dopo aver elaborato il messaggio, o averlo archiviato in modo affidabile per una successiva elaborazione, esegue la seconda fase del processo di ricezione chiamando **Delete** sul messaggio ricevuto. Quando il bus di servizio vede la chiamata **Delete**, contrassegna il messaggio come utilizzato e lo rimuove dalla coda.

L'esempio seguente illustra come ricevere ed elaborare messaggi usando la modalità **PeekLock** (non quella predefinita). Nel seguente esempio viene creato un ciclo infinito e i messaggi vengono elaborati non appena arrivano in "TestQueue":

    	try
	{
		ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
		opts.setReceiveMode(ReceiveMode.PEEK_LOCK);

		while(true)  {
	         ReceiveQueueMessageResult resultQM =
	     			service.receiveQueueMessage("TestQueue", opts);
		    BrokeredMessage message = resultQM.getValue();
		    if (message != null && message.getMessageId() != null)
		    {
			    System.out.println("MessageID: " + message.getMessageId());
			    // Display the queue message.
			    System.out.print("From queue: ");
			    byte[] b = new byte[200];
			    String s = null;
			    int numRead = message.getBody().read(b);
			    while (-1 != numRead)
	            {
	                s = new String(b);
	                s = s.trim();
	                System.out.print(s);
	                numRead = message.getBody().read(b);
			    }
	            System.out.println();
			    System.out.println("Custom Property: " +
			        message.getProperty("MyProperty"));
			    // Remove message from queue.
			    System.out.println("Deleting this message.");
			    //service.deleteMessage(message);
		    }  
		    else  
		    {
		        System.out.println("Finishing up - no more messages.");
		        break;
		        // Added to handle no more messages.
		        // Could instead wait for more messages to be added.
		    }
	    }
	}
	catch (ServiceException e) {
	    System.out.print("ServiceException encountered: ");
	    System.out.println(e.getMessage());
	    System.exit(-1);
	}
	catch (Exception e) {
	    System.out.print("Generic exception encountered: ");
	    System.out.println(e.getMessage());
	    System.exit(-1);
	}

## Come gestire arresti anomali e messaggi illeggibili dell'applicazione

Il bus di servizio fornisce funzionalità per il ripristino gestito automaticamente in caso di errori nell'applicazione o di problemi di elaborazione di un messaggio. Se un'applicazione ricevitore non è in grado di elaborare il messaggio per un qualsiasi motivo, può chiamare il metodo **unlockMessage**, anziché **deleteMessage**, sul messaggio ricevuto. In questo modo, il bus di servizio sbloccherà il messaggio nella coda rendendolo nuovamente disponibile per la ricezione da parte della stessa o da un'altra applicazione consumer.

Al messaggio bloccato nella coda è inoltre associato un timeout. Se l'applicazione non riesce a elaborare il messaggio prima della scadenza del timeout, ad esempio a causa di un arresto anomalo, il bus di servizio sbloccherà automaticamente il messaggio rendendolo nuovamente disponibile per la ricezione.

In caso di arresto anomalo dell'applicazione dopo l'elaborazione del messaggio ma prima dell'invio della richiesta **deleteMessage**, il messaggio verrà nuovamente recapitato all'applicazione al riavvio del sistema. Questo processo di elaborazione viene spesso definito di tipo **At-Least-Once**, per indicare che ogni messaggio verrà elaborato almeno una volta ma che in determinate situazioni potrà essere recapitato una seconda volta. Se lo scenario non tollera la doppia elaborazione, gli sviluppatori dovranno aggiungere logica aggiuntiva all'applicazione per gestire il secondo recapito del messaggio. A tale scopo viene spesso utilizzato il metodo **getMessageId** del messaggio, che rimane costante in tutti i tentativi di recapito.

## Passaggi successivi

A questo punto, dopo aver appreso le nozioni di base delle code del bus di servizio, vedere [Code, Argomenti e Sottoscrizioni][] per altre informazioni.

Per altre informazioni, vedere il [Centro per sviluppatori di Java](/develop/java/).


  [Azure SDK for Java]: http://azure.microsoft.com/develop/java/
  [Azure SDK per Java]: http://azure.microsoft.com/develop/java/
  [Azure Toolkit per Eclipse]: https://msdn.microsoft.com/library/azure/hh694271.aspx
  [Code, Argomenti e Sottoscrizioni]: service-bus-queues-topics-subscriptions.md
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
  [Gli oggetti ]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
  [portale di Azure classico]: http://manage.windowsazure.com

<!---HONumber=AcomDC_0525_2016-->