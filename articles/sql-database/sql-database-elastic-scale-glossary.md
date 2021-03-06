<properties 
    pageTitle="Glossario sugli strumenti di database elastici | Microsoft Azure" 
    description="Spiegazione dei termini utilizzati per gli strumenti dei database elastici" 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove"/>

# Glossario sugli strumenti di database elastici
Di seguito sono elencate le definizioni dei termini usati per gli [strumenti di database elastici](sql-database-elastic-scale-introduction.md), una funzionalità del database SQL di Azure. Gli strumenti vengono usati per gestire le [mappe partizioni](sql-database-elastic-scale-shard-map-management.md), e includono la [libreria client](sql-database-elastic-database-client-library.md), lo [strumento di suddivisione-unione](sql-database-elastic-scale-overview-split-and-merge.md), i [pool elastici](sql-database-elastic-pool.md), e le [query](sql-database-elastic-query-overview.md).

Questi termini vengono usati in [Aggiungere una partizione mediante gli strumenti di Database elastico](sql-database-elastic-scale-add-a-shard.md) e [Usare la classe RecoveryManager per risolvere i problemi di partizionamento della mappa](sql-database-elastic-database-recovery-manager.md).

![Termini della scalabilità elastica][1]

**Database**: un database SQL di Azure.

**Routing dipendente dei dati**: la funzionalità che consente a un'applicazione di connettersi a una partizione in base a una specifica chiave di partizionamento orizzontale. Vedere [Routing dipendente dei dati](sql-database-elastic-scale-data-dependent-routing.md). Confrontare con **[Query su più partizioni](sql-database-elastic-scale-multishard-querying.md)**.

**Mappa globale partizioni**: il set di mapping tra le chiavi di partizionamento orizzontale e le rispettivi partizioni all'interno di un **set di partizioni**. La mappa partizioni globale viene archiviata nel **gestore mappe partizioni**. Confrontare con la **mappa partizioni locale **.

**Mappa partizioni di tipo elenco**: una mappa partizioni in cui le chiavi di partizionamento orizzontale vengono mappate singolarmente. Confrontare con la **mappa partizioni di tipo intervallo**.

**Mappa partizioni locale**: archiviata in una partizione, la mappa partizioni locale contiene i mapping per gli shardlet che risiedono nella partizione.

**Esecuzione di query su più partizioni**: la possibilità di eseguire una query su più partizioni; i set di risultati vengono restituiti usando la semantica di UNION ALL (nota anche come "query di tipo fan-out"). Confrontare con **Routing dipendente dai dati**.

**Multi-tenant** e a **tenant singolo**: l'immagine mostra un database a tenant singolo e un database multi-tenant:

![Database a tenant singolo e multi-tenant](./media/sql-database-elastic-scale-glossary/multi-single-simple.png)

Ecco una rappresentazione di database a tenant singolo e multi-tenant **partizionati**.

![Database a tenant singolo e multi-tenant](./media/sql-database-elastic-scale-glossary/shards-single-multi.png)

**Mappa partizioni di tipo intervallo**: una mappa partizioni in cui la strategia di distribuzione delle partizioni è basata su più intervalli di valori contigui.

**Tabelle di riferimento**: tabelle che non vengono partizionate, ma vengono replicate tra le partizioni. I codici di avviamento postale, ad esempio, possono essere archiviati in una tabella di riferimento.

**Partizione**: un database SQL di Azure che archivia i dati da un set di dati partizionato.

**Elasticità di partizionamento**: la possibilità di eseguire il **ridimensionamento orizzontale** e il **ridimensionamento verticale**.

**Tabelle partizionate**: tabelle che vengono partizionate, ovvero i cui dati vengono distribuiti tra le partizioni in base ai valori della chiave di partizionamento orizzontale.

**Chiave di partizionamento orizzontale**: un valore di colonna che determina la modalità di distribuzione dei dati tra le partizioni. Il tipo valore può essere uno dei seguenti: **int**, **bigint**, **varbinary** o **uniqueidentifier**.

**Set di partizioni**: la raccolta di partizioni attribuite alla stessa mappa partizioni nel gestore delle mappe partizioni.

**Shardlet**: il complesso dei dati associati a un singolo valore di una chiave di partizionamento orizzontale in una partizione. Uno shardlet è la più piccola unità di spostamento dei dati possibile quando si ridistribuiscono tabelle partizionate.

**Mappa partizioni**: il set di mapping tra le chiavi di partizionamento orizzontale e le rispettive partizioni.

**Gestore mappe partizioni**: un archivio di dati e oggetti di gestione che contiene le mappe partizioni, i percorsi delle partizioni e i mapping per uno o più set di partizioni.

![Mapping][2]


##Verbi

**Scalare orizzontalmente**: aumentare o ridurre le dimensioni di una raccolta di partizioni aggiungendo o rimuovendo partizioni a una mappa partizioni.

![Scalabilità orizzontale e verticale][3]

**Unire**: spostare shardlet da due partizioni a una partizione e aggiornare la mappa partizioni di conseguenza.

**Spostare shardlet**: spostare un singolo shardlet in una partizione diversa.

**Partizione**: eseguire il partizionamento orizzontale di dati strutturati in modo identico in più database in base a una chiave di partizionamento orizzontale.

**Dividere**: spostare più shardlet da una partizione a un'altra (in genere nuova). Come punto di divisione viene usata una chiave di partizionamento orizzontale fornita dall'utente.

**Scalare verticalmente**: aumentare o ridurre il livello di prestazioni di una singola partizione. Ad esempio, modifica di una partizione da Standard a Premium (che comporta più risorse di elaborazione).

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-glossary/glossary.png
[2]: ./media/sql-database-elastic-scale-glossary/mappings.png
[3]: ./media/sql-database-elastic-scale-glossary/h_versus_vert.png
 

<!---HONumber=AcomDC_0601_2016-->