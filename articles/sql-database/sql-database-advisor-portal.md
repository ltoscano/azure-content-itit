<properties 
   pageTitle="Azure SQL Database Advisor tramite il portale di Azure | Microsoft Azure" 
   description="È possibile usare Azure SQL Database Advisor nel portale di Azure per rivedere e implementare le raccomandazioni per i database SQL esistenti che consentono di migliorare le prestazioni correnti delle query." 
   services="sql-database" 
   documentationCenter="" 
   authors="stevestein" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management" 
   ms.date="06/22/2016"
   ms.author="sstein"/>

# Advisor per database SQL

> [AZURE.SELECTOR]
- [Panoramica di SQL Database Advisor](sql-database-advisor.md)
- [Portale](sql-database-advisor-portal.md)

È possibile usare Azure SQL Database Advisor nel portale di Azure per rivedere e implementare le raccomandazioni per i database SQL esistenti che consentono di migliorare le prestazioni correnti delle query.

## Visualizzazione delle raccomandazioni

Nella pagina delle raccomandazioni sono visualizzate le raccomandazioni principali in base all'impatto potenziale sul miglioramento delle prestazioni. È anche possibile visualizzare lo stato delle ultime operazioni storiche. Selezionare un'indicazione o lo stato per visualizzarne i dettagli.

Per visualizzare e applicare le raccomandazioni, sono necessarie le autorizzazioni di [controllo degli accessi in base al ruolo](../active-directory/role-based-access-control-configure.md) corrette in Azure. Le autorizzazioni **Lettore** e **Collaboratore Database SQL** sono necessarie per visualizzare le raccomandazioni e le autorizzazioni **Proprietario** e **Collaboratore DB SQL** sono necessarie per eseguire qualunque azione, creare o eliminare indici e annullare la creazione di un indice.

1. Accedere al [portale di Azure](https://portal.azure.com/).
2. Fare clic su **SFOGLIA** > **Database SQL** e selezionare il database.
5. Fare clic su **Tutte le impostazioni** > **Raccomandazioni** per visualizzare le **Raccomandazioni** disponibili per il database selezionato.

> [AZURE.NOTE] Per ottenere le raccomandazioni è necessario che database sia stato in uso per circa una settimana e che in tale settimana si siano svolte attività. Inoltre devono essere presenti anche alcune attività coerenti. L'advisor del database SQL può ottimizzare più facilmente modelli di query coerenti anziché picchi irregolari casuali di attività. Se non sono disponibili raccomandazioni, nella pagina **Raccomandazioni** dovrebbe comparire un messaggio che ne spiega il motivo.

![Consigli](./media/sql-database-advisor-portal/recommendations.png)

Di seguito è riportato un esempio della raccomandazione relativa alla "correzione di problemi di schema"nel portale di Azure.

![Correggere problemi di schema](./media/sql-database-advisor-portal/sql-database-advisor-schema-issue.png)

Le indicazioni vengono ordinate nelle seguenti 4 categorie in base al potenziale impatto sulle prestazioni:

| Impatto | Descrizione |
| :--- | :--- |
| Alto | Le indicazioni ad alto impatto devono fornire l'impatto più significativo sulle prestazioni. |
| Medio | Le raccomandazioni a impatto medio devono migliorare le prestazioni, ma non sostanzialmente. |
| Basso | Le raccomandazioni a basso impatto devono offrire prestazioni migliori, ma i miglioramenti potrebbero non essere significativi. 


### Rimozione delle raccomandazioni dall'elenco

Se l'elenco di raccomandazioni contiene voci che si vuole rimuovere dall'elenco, ignorare la raccomandazione:

1. Selezionare una raccomandazione nell'elenco **Raccomandazioni**.
2. Fare clic su **Ignora** nel pannello **Dettagli**.


Se si vuole, è possibile aggiungere nuovamente gli elementi ignorati all'elenco **Raccomandazioni**:

1. Nel pannello **Raccomandazioni** fare clic su **Visualizza rimosse**.
1. Selezionare un elemento ignorato nell'elenco per visualizzarne i dettagli.
1. Facoltativamente, fare clic su **Annulla eliminazione** per aggiungere nuovamente l'indice all'elenco principale di **Raccomandazioni**.



## Applicazione delle raccomandazioni

L'advisor per database SQL offre controllo completo sull'attivazione delle raccomandazioni in uno dei 3 modi seguenti.

- Applicare le singole indicazioni una alla volta.
- Consentire all'advisor di applicare automaticamente le raccomandazioni (attualmente applicabile solo alle raccomandazioni relative agli indici).
- Eseguire manualmente lo script T-SQL consigliato nel database per implementare un'indicazione.

Selezionare una raccomandazione per visualizzarne i dettagli e quindi fare clic su **Visualizza script** per esaminare in dettaglio come verrà creata la raccomandazione.

Il database rimane online mentre l'advisor applica la raccomandazione: con l'advisor per database SQL un database non verrà mai portato offline.

### Applicare una singola indicazione

È possibile leggere e accettare le indicazioni una alla volta.

1. Nel pannello **Raccomandazioni** fare clic su una raccomandazione.
2. Nel pannello **Dettagli** fare clic su **Applica**.

    ![Applica suggerimento](./media/sql-database-advisor-portal/apply.png)

### Abilitare la gestione automatica degli indici

È possibile impostare l'advisor per database SQL in modo che implementi automaticamente le raccomandazioni. Man mano che le indicazioni vengono rese disponibili, verranno applicate automaticamente. Come per tutte le operazioni sugli indici gestite dal servizio, se l'impatto sulle prestazioni è negativo le indicazioni verranno ripristinate.

1. Nel pannello **Raccomandazioni** fare clic su **Automatizza**:

    ![Impostazioni di Advisor](./media/sql-database-advisor-portal/settings.png)

2. Impostare l'advisor in modo da **creare** o **eliminare** automaticamente gli indici:

    ![Indici consigliati](./media/sql-database-advisor-portal/automation.png)


### Eseguire manualmente lo script T-SQL consigliato

Selezionare qualsiasi raccomandazione e quindi fare clic su **Visualizza script**. Eseguire questo script nel database per applicare manualmente l'indicazione.

*Gli indici eseguiti manualmente non vengono monitorati e convalidati per l'impatto sulle prestazioni da parte del servizio*, quindi è consigliabile monitorarli dopo la creazione per verificare che offrano miglioramenti delle prestazioni e modificarli o eliminarli se necessario. Per informazioni dettagliate sulla creazione di indici, vedere [CREAZIONE INDICE (Transact-SQL)](https://msdn.microsoft.com/library/ms188783.aspx).


### Annullamento delle raccomandazioni

Le raccomandazioni con stato **In sospeso**, **Verifica** o **Operazione completata** possono essere annullate. Le raccomandazioni con stato **In esecuzione** non possono essere annullate.

1. Selezionare una raccomandazione nell'area **Cronologia ottimizzazione** per aprire il pannello **Dettagli raccomandazione**.
2. Fare clic su **Annulla** per interrompere il processo di applicazione della raccomandazione.



## Monitoraggio delle operazioni

L'applicazione di un'indicazione potrebbe non avvenire in tempo reale. Il portale fornisce dettagli sullo stato delle operazioni relative alle raccomandazioni. Di seguito sono indicati gli stati possibili di un indice:

| Stato | Descrizione |
| :--- | :--- |
| In sospeso | Il comando di applicazione della raccomandazione è stato ricevuto ed è pianificato per l'esecuzione. |
| In esecuzione | La raccomandazione viene applicata. |
| Success | È in corso l'applicazione della raccomandazione. |
| Errore | Si è verificato un errore durante il processo di applicazione della raccomandazione. Può trattarsi di un problema temporaneo o eventualmente di una modifica dello schema della tabella e lo script non è più valido. |
| Ripristino | La raccomandazione è stata applicata, ma è stata considerata non efficiente e verrà ripristinata automaticamente. |
| Ripristinato | La raccomandazione è stata ripristinata. |

Fare clic su un'indicazione in-process nell'elenco per visualizzarne i dettagli:

![Indici consigliati](./media/sql-database-advisor-portal/operations.png)


### Ripristino di una raccomandazione

Se è stato usato l'advisor per applicare la raccomandazione, ovvero non è stato eseguito manualmente lo script T-SQL, la raccomandazione verrà ripristinata automaticamente se viene rilevato un impatto negativo sulle prestazioni. Se per qualsiasi motivo si vuole semplicemente annullare una raccomandazione, procedere come segue:


1. Selezionare una raccomandazione applicata nell'area **Cronologia ottimizzazione**.
2. Fare clic su **Ripristina** nel pannello **Dettagli raccomandazione**.

![Indici consigliati](./media/sql-database-advisor-portal/details.png)


## Monitoraggio dell'impatto sulle prestazioni delle indicazioni relative agli indici

Dopo aver implementato correttamente le raccomandazioni (attualmente, solo raccomandazioni relative alle operazioni sugli indici e alle query con parametri), fare clic su **Informazioni dettagliate query** nel pannello dei dettagli delle raccomandazioni per aprire [Informazioni dettagliate prestazioni query](sql-database-query-performance.md) ed esaminare l'impatto sulle prestazioni delle query principali.

![Monitorare l'impatto sulle prestazioni](./media/sql-database-advisor-portal/query-insights.png)



## Riepilogo

L'advisor per database SQL fornisce raccomandazioni per migliorare le prestazioni del database SQL. Offrendo script T-SQL, nonché opzioni singole e completamente automatizzate (attualmente solo per gli indici), l'advisor risulta molto utile per ottimizzare il database e quindi per migliorare le prestazioni delle query.



## Passaggi successivi

Monitorare le raccomandazioni e continuare ad applicarle in modo da migliorare le prestazioni. I carichi di lavoro dei database sono dinamici e cambiano in modo continuo. L'advisor per database SQL continuerà a monitorare e fornire raccomandazioni potenzialmente utili per migliorare le prestazioni del database.

 - Per una panoramica, vedere [SQL Database Advisor](sql-database-advisor.md).
 - Vedere l'articolo [Informazioni dettagliate prestazioni query](sql-database-query-performance.md) per imparare a esaminare l'impatto sulle prestazioni delle query principali.

## Risorse aggiuntive

- [Archivio query](https://msdn.microsoft.com/library/dn817826.aspx)
- [CREATE INDEX](https://msdn.microsoft.com/library/ms188783.aspx)
- [Controllo degli accessi in base al ruolo](../active-directory/role-based-access-control-configure.md)

<!---HONumber=AcomDC_0629_2016-->