<properties 
	pageTitle="Come identificare scenari e pianificare l'elaborazione dei dati di analisi avanzata | Microsoft Azure" 
	description="Pianificare l'analisi avanzata prendendo in considerazione una serie di domande chiave." 
	services="machine-learning" 
	documentationCenter="" 
	authors="bradsev"
	manager="jhubbard" 
	editor="cgronlun" /> 

<tags 
	ms.service="machine-learning" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="09/19/2016" 
	ms.author="bradsev" /> 


# Come identificare scenari e pianificare l'elaborazione dei dati di analisi avanzata

Quali risorse è necessario pianificare quando si configura un ambiente per l'elaborazione dell'analisi avanzata su un set di dati? Questo articolo suggerisce una serie di domande che contribuiscono a identificare le attività e le risorse attinenti allo scenario. L'ordine dei passaggi generali per l'analisi predittiva è illustrato in [Informazioni sul Processo di analisi scientifica dei dati per i team (TDSP)](data-science-process-overview.md). Ognuno di questi passaggi richiede risorse specifiche per le attività attinenti allo scenario specifico. Le domande principali per identificare lo scenario riguardano la logistica dei dati, le caratteristiche, la qualità dei set di dati e gli strumenti e linguaggi preferiti per eseguire l'analisi.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## Logistica: percorsi dei dati e spostamento
Le domande di logistica riguardano il percorso dell'**origine dati**, la **destinazione** in Azure e i requisiti per lo spostamento dei dati, incluse la pianificazione, la quantità e le risorse coinvolte. Potrebbe essere necessario spostare i dati più volte durante il processo di analisi. Uno scenario frequente consiste nello spostamento di dati locali in una qualche forma di archiviazione in Azure e quindi in Machine Learning Studio.

1. **Qual è l'origine dati?** È locale o nel cloud? ad esempio:
	- I dati sono disponibili a un indirizzo HTTP.
	- I dati si trovano in un percorso locale/di rete.
	- I dati si trovano in un database di SQL Server.
	- I dati sono archiviati in un contenitore di archiviazione di Azure.

2. **Qual è la destinazione di Azure?** Quale deve essere la destinazione per l'elaborazione o la modellazione? ad esempio:
	- Archiviazione BLOB di Azure
	- Database SQL Azure
	- Macchine virtuali SQL Server in Azure
	- Tabelle Hive o HDInsight (Hadoop in Azure)
	- Azure Machine Learning
	- Dischi rigidi virtuali di Azure montabili.

3. **Come verranno spostati i dati?** Gli argomenti seguenti illustrano le procedure e le risorse disponibili per inserire o caricare dati in vari ambienti di elaborazione e di archiviazione diversi.

	-  [Caricare i dati in ambienti di archiviazione per l'analisi](machine-learning-data-science-ingest-data.md)
	-  [Importare dati di training in Azure Machine Learning Studio da varie origini dati](machine-learning-data-science-import-data,md).

4. **I dati devono essere spostati a intervalli regolari o modificati durante la migrazione?** È consigliabile usare il Data factory di Azure (ADF) quando è necessario migrare continuamente i dati, soprattutto in uno scenario ibrido che accede a risorse locali e cloud o in presenza di dati transazionali, che devono essere modificati o ai quali va aggiunta una logica di business durante la migrazione. Per altre informazioni, vedere [Spostare i dati da un server SQL locale a SQL Azure con il Data factory di Azure](machine-learning-data-science-move-sql-azure-adf.md).

5. **Quanta parte dei dati deve essere spostata in Azure?** I set di dati molto grandi potrebbero superare la capacità di archiviazione di determinati ambienti. Per un esempio, vedere la discussione sui limiti di dimensione per Machine Learning Studio nella sezione successiva. In questi casi è possibile usare un campione dei dati durante l'analisi. Per informazioni dettagliate su come sottocampionare un set di dati in vari ambienti di Azure, vedere [Sample data in the Team Data Science Process](machine-learning-data-science-sample-data.md) (Campionare i dati nel Processo di analisi scientifica dei dati per i team).


## Domande sulle caratteristiche dei dati: tipo, formato e dimensione
Queste domande sono fondamentali per la pianificazione degli ambienti di elaborazione e archiviazione, ognuno dei quali è più adatto a vari tipi di dati e per ognuno dei quali sono previste determinate limitazioni.

1. **Quali sono i tipi di dati?** Ad esempio:
	- Numerico
	- Categorical
	- Stringhe
	- Binary

2. **Come sono formattati i dati?** Ad esempio:
    - File flat delimitati da virgole (CSV) o delimitati da tabulazioni (TSV)
    - Compressi o non compressi
	- BLOB di Azure
	- Tabelle Hive di Hadoop
	- Tabelle di SQL Server

2. **Quali sono le dimensioni dei dati?**
    - Piccole: meno di 2 GB
    - Medie: compresi tra 2 GB e 10 GB
	- Grandi: più di 10 GB

Si consideri ad esempio l'ambiente di Azure Machine Learning Studio:

- Per un elenco dei formati e dei tipi di dati supportati da Azure Machine Learning Studio, vedere la sezione [Tipi di dati e formati di dati supportati](machine-learning-data-science-import-data.md#data-formats-and-data-types-supported).
- Per informazioni sulle limitazioni dei dati per Azure Machine Learning Studio, vedere la sezione **Qual è la grandezza massima dei set di dati per i moduli?** in [Importazione ed esportazione dei dati per Machine Learning](machine-learning-faq.md#machine-learning-studio-questions).

Per informazioni sulle limitazioni di altri servizi Azure usati nel processo di analisi, vedere [Sottoscrizione ad Azure e limiti, quote e vincoli del servizio](../azure-subscription-service-limits.md).

## Domande sulla qualità dei dati: esplorazione e pre-elaborazione

1. **Quali sono le informazioni necessarie sui dati?** Esplorare i dati per comprenderne le caratteristiche di base, i modelli o le tendenze, gli outlier o gli eventuali valori mancanti. Questo passaggio è importante per determinare la quantità di pre-elaborazione necessaria, per formulare ipotesi che potrebbero suggerire le funzionalità o il tipo di analisi più appropriato e per formulare piani per la raccolta di dati aggiuntivi. Alcune tecniche utili per l'analisi dei dati consistono nel calcolare statistiche descrittive e tracciare visualizzazioni. Per informazioni dettagliate su come esplorare un set di dati in vari ambienti di Azure, vedere [Esplorare i dati nel Processo di analisi scientifica dei dati per i team](machine-learning-data-science-explore-data.md).

2. **I dati necessitano di pre-elaborazione o pulizia?** La pre-elaborazione e la pulizia dei dati rappresentano attività importanti che in genere devono essere eseguite prima di poter usare un set di dati in modo efficace per Machine Learning. I dati non elaborati sono fastidiosi, non affidabili e potrebbero non contenere alcuni valori. Utilizzare tali dati per la modellazione può produrre risultati fuorvianti. Per una descrizione, vedere [Attività per preparare i dati per operazioni avanzate con Machine Learning](machine-learning-data-science-prepare-data.md).

## Domande su strumenti e linguaggi
Sono disponibili numerose opzioni a seconda dei linguaggi e degli strumenti o ambienti di sviluppo usati.
 
1. **Quali linguaggi si preferisce usare per l'analisi?**
	- R
	- Python
	- SQL

2. **Quali strumenti usare per l'analisi dei dati?**
	- [Microsoft Azure Powershell](powershell-install-configure.md): un linguaggio di script usato per gestire le risorse di Azure in un linguaggio di script.
	- [Azure Machine Learning Studio](machine-learning-what-is-ml-studio/)
	- [Revolution Analytics](http://www.revolutionanalytics.com/revolution-r-open)
	- [RStudio](http://www.rstudio.com)
	- [Python Tools per Visual Studio](http://microsoft.github.io/PTVS/)
	- [Anaconda](https://www.continuum.io/why-anaconda)
	- [Jupyter Notebooks](http://jupyter.org/)
	- [Microsoft Power BI](http://powerbi.microsoft.com)


## Identificare lo scenario di analisi avanzata
Dopo aver risposto alle domande nella sezione precedente, si è pronti per determinare quale scenario è più adatto al caso specifico. Gli scenari di esempio sono descritti in [Scenari per l'analisi avanzata in Azure Machine Learning](machine-learning-data-science-plan-sample-scenarios.md).







 

<!---HONumber=AcomDC_0921_2016-->