<properties
   pageTitle="Creare una funzione di elaborazione di eventi | Microsoft Azure"
   description="Usare Funzioni di Azure per creare una funzione C# che viene eseguita in base a un timer di eventi."
   services="functions"
   documentationCenter="na"
   authors="ggailey777"
   manager="erikre"
   editor=""
   tags=""
   />

<tags
   ms.service="functions"
   ms.devlang="multiple"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="09/25/2016"
   ms.author="glenga"/>
   
# Creare una funzione di Azure di elaborazione di eventi

Funzioni di Azure è un'esperienza di calcolo su richiesta basata su eventi che consente di creare unità pianificate o attivate del codice implementate in un'ampia gamma di linguaggi di programmazione. Per altre informazioni su Funzioni di Azure, vedere [Panoramica di Funzioni di Azure](functions-overview.md).

Questo argomento illustra come creare una nuova funzione in C#, che viene eseguita in base a un timer di eventi per aggiungere messaggi a una coda di archiviazione.

## Prerequisiti 

Prima di creare una funzione, è necessario avere un account Azure attivo. Se non si possiede un account di Azure, [sono disponibili account gratuiti](https://azure.microsoft.com/free/).

## Creare una funzione attivata tramite timer a partire dal modello

Un'app per le funzioni ospita l'esecuzione delle funzioni in Azure. Prima di creare una funzione, è necessario avere un account Azure attivo. Se non si possiede già un account Azure, [sono disponibili account gratuiti](https://azure.microsoft.com/free/).

1. Andare sul [portale di Funzioni di Azure](https://functions.azure.com/signin) e accedere con l'account Azure.

2. Se si dispone di un'app per le funzioni esistente da poter usare, selezionarla da **App per le funzioni personali** e fare clic su **Apri**. Per creare una nuova app per le funzioni, digitare un **nome** univoco da assegnare o accettare quello generato automaticamente, selezionare l'**area** desiderata, quindi fare clic su **Crea + Attività iniziali**.

3. Nell'app per le funzioni fare clic su **+ Nuova funzione** > **TimerTrigger - C#** > **Crea**. Verrà creata una funzione con un nome predefinito, che viene eseguita in base alla pianificazione predefinita, ovvero una volta al minuto.

	![Creare una nuova funzione attivata tramite timer](./media/functions-create-an-event-processing-function/functions-create-new-timer-trigger.png)

4. Nella nuova funzione fare clic sulla scheda **Integra** > **Nuovo output** > **Azure Storage Queue** (Coda di archiviazione di Azure) > **Seleziona**.

	![Creare una nuova funzione attivata tramite timer](./media/functions-create-an-event-processing-function/functions-create-storage-queue-output-binding.png)

5. In **Azure Storage Queue output** (Output della coda di archiviazione di Azure) selezionare una **Storage account connection** (Connessione all'account di archiviazione) esistente o crearne una nuova, quindi fare clic su **Salva**.

	![Creare una nuova funzione attivata tramite timer](./media/functions-create-an-event-processing-function/functions-create-storage-queue-output-binding-2.png)

6. Nella scheda **Sviluppo** sostituire lo script C# esistente nella finestra **Codice** con il codice seguente:

		using System;
		
		public static void Run(TimerInfo myTimer, out string outputQueueItem, TraceWriter log)
		{
		    // Add a new scheduled message to the queue.
		    outputQueueItem = $"Ping message added to the queue at: {DateTime.Now}.";
		    
		    // Also write the message to the logs.
		    log.Info(outputQueueItem);
		}

	Questo codice aggiunge un nuovo messaggio alla coda con la data e l'ora correnti al momento dell'esecuzione della funzione.

7. Fare clic su **Salva** e verificare la finestra **Log** per la successiva esecuzione della funzione.

8. (Facoltativo) Passare all'account di archiviazione e verificare che i messaggi vengano aggiunti alla coda.

9. Tornare alla scheda **Integra** e modificare il campo della pianificazione in `0 0 * * * *`. La funzione viene ora eseguita una volta all'ora.

Ecco un esempio estremamente semplificato di associazione tra trigger timer e output della coda di archiviazione. Per altre informazioni, vedere gli argomenti [Trigger timer in Funzioni di Azure](functions-bindings-timer.md) e [Trigger e associazioni di Archiviazione di Azure in Funzioni di Azure](functions-bindings-storage.md).

##Passaggi successivi

Vedere gli argomenti seguenti per altre informazioni su Funzioni di Azure.

+ [Guida di riferimento per gli sviluppatori a Funzioni di Azure](functions-reference.md) Informazioni di riferimento per programmatori per la codifica di funzioni e la definizione di trigger e associazioni.
+ [Test di Funzioni di Azure](functions-test-a-function.md) Descrive diversi strumenti e tecniche per il test delle funzioni.
+ [Come aumentare le prestazioni di Funzioni di Azure](functions-scale.md) Presenta i piani di servizio disponibili con Funzioni di Azure, tra cui il piano di servizio dinamico, e come scegliere quello più appropriato.

[AZURE.INCLUDE [Nota introduttiva](../../includes/functions-get-help.md)]

<!---HONumber=AcomDC_0928_2016-->