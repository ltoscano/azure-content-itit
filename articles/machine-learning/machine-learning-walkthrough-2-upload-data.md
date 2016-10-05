<properties
	pageTitle="Passaggio 2: Caricare dati in un esperimento di Machine Learning | Microsoft Azure"
	description="Passaggio 2 della procedura dettagliata Sviluppare una soluzione predittiva: Caricare dati pubblici archiviati in Azure Machine Learning Studio"
	services="machine-learning"
	documentationCenter=""
	authors="garyericson"
	manager="jhubbard"
	editor="cgronlun"/> 

<tags
	ms.service="machine-learning"
	ms.workload="tbd"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/16/2016" 
	ms.author="garye"/>


# Passaggio 2 della procedura dettagliata: Caricare dati esistenti in un esperimento di Azure Machine Learning

Questo è il secondo passaggio della procedura dettagliata [Sviluppare una soluzione di analisi predittiva in Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md).


1.	[Creare un'area di lavoro di Machine Learning](machine-learning-walkthrough-1-create-ml-workspace.md)
2.	**Caricare i dati esistenti**
3.	[Creare un nuovo esperimento](machine-learning-walkthrough-3-create-new-experiment.md)
4.	[Eseguire il training e valutare i modelli](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.	[Distribuire il servizio web](machine-learning-walkthrough-5-publish-web-service.md)
6.	[Accedere al servizio Web](machine-learning-walkthrough-6-access-web-service.md)

----------

Per sviluppare un modello predittivo per il rischio di credito, abbiamo bisogno di dati da usare per eseguire il training del modello e quindi testare il modello. Per questa procedura verrà usato il set di dati "UCI Statlog (German Credit Data) Data Set" da UCI Machine Learning Repository, disponibile all'indirizzo seguente: <a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)</a>

Verrà usato il file denominato **german.data**. Scaricare questo file nel disco rigido locale.

Questo set di dati contiene le righe di 20 variabili per 1000 clienti che in passato hanno fatto richiesta di un credito. Queste 20 variabili rappresentano il vettore della funzionalità del set di dati che fornisce le caratteristiche di identificazione di ogni richiedente. Una colonna aggiuntiva in ogni riga rappresenta il rischio di credito calcolato del richiedente. In questa colonna 700 richiedenti sono identificati come a basso rischio e 300 ad alto rischio.

Il sito Web UCI fornisce una descrizione degli attributi del vettore delle funzionalità che includono informazioni finanziarie, storico dei crediti, stato di occupazione e dati personali. Per ogni richiedente è stata assegnata una valutazione in formato binario per indicare se è a basso o ad alto rischio.

Questi dati verranno usati per creare un modello di analisi predittiva. Una volta completata questa operazione, il modello dovrebbe essere in grado di accettare informazioni per nuovi clienti e prevedere se sono a basso o ad alto rischio.

Ma ecco un'interessante svolta. La descrizione del set di dati spiega che classificare erroneamente un cliente come a basso rischio mentre è ad alto rischio implica costi cinque volte più alti per l'istituto di credito rispetto a classificare erroneamente un cliente come ad alto rischio mentre è a basso rischio. Un modo semplice per tenere conto di questo aspetto nell'esperimento consiste nel duplicare (5 volte) le voci che rappresentano un cliente con rischio di credito elevato. In tal modo, se il modello classifica erroneamente come basso un rischio alto, la classificazione errata sarà ripetuta 5 volte, una per ogni duplicato, e il costo di questo errore aumenterà nei risultati.

##Convertire il formato del set di dati
Nel set di dati originale viene usato un formato con valori delimitati da spazi vuoti. Per il funzionamento ottimale di Machine Learning Studio è preferibile usare un file con valori delimitati da virgole (CSV), di conseguenza il set di dati verrà convertito sostituendo gli spazi con le virgole.

Esistono diversi modi per convertire questi dati. Un'opzione consiste nell'usare il comando di Windows PowerShell seguente:

	cat german.data | %{$_ -replace " ",","} | sc german.csv  

Un'altra opzione consiste nell'usare il comando sed di Unix:

	sed 's/ /,/g' german.data > german.csv  

In entrambi i casi, è stata creata una versione delimitata da virgole dei dati del file **german.csv** che verrà usato nell'esperimento.

##Caricare il set di dati in Machine Learning Studio

Dopo aver convertito i dati in formato CSV, è necessario caricarli in Machine Learning Studio. Per altre informazioni introduttive su Machine Learning Studio, vedere la [home page di Microsoft Azure Machine Learning Studio ](https://studio.azureml.net/).

1.	Aprire Machine Learning Studio ([https://studio.azureml.net](https://studio.azureml.net)). Quando viene richiesto di effettuare l'accesso, usare l'account specificato come proprietario dell'area di lavoro.
1.  Fare clic sulla scheda **Studio** nella parte superiore della finestra.
1.	Fare clic su **+NEW** nella parte inferiore della finestra.
1.	Selezionare **DATASET**.
1.	Selezionare **FROM LOCAL FILE**.
1.	Nella **finestra di dialogo Carica un nuovo set di dati** fare clic su **Sfoglia** e individuare il file **german.csv** creato
1.	Immettere un nome per il set di dati. Per questa procedura, il nome sarà "UCI German Credit Card Data".
1.	Come tipo di dati selezionare **Generic CSV File With no header (.nh.csv)**.
1.	Aggiungere un'eventuale descrizione.
1.	Fare clic su **OK**.

![Caricare il set di dati][1]


I dati vengono caricati in un modulo del set di dati che è possibile usare in un esperimento.

> [AZURE.TIP] Per gestire i set di dati che sono stati caricati in Studio, fare clic sulla scheda **SET DI DATI** a sinistra della finestra di Studio.

Per altre informazioni sull'importazione di vari tipi di dati in un esperimento, vedere [Importare dati di training in Azure Machine Learning Studio](machine-learning-data-science-import-data.md).

**Passaggi successivi: [Creare un nuovo esperimento](machine-learning-walkthrough-3-create-new-experiment.md)**

[1]: ./media/machine-learning-walkthrough-2-upload-data/upload1.png

<!---HONumber=AcomDC_0921_2016-->