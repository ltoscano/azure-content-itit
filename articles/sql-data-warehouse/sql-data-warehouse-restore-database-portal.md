<properties
   pageTitle="Ripristinare un'istanza di Azure SQL Data Warehouse (portale) | Microsoft Azure"
   description="Attività del portale di Azure per il ripristino di un'istanza di Azure SQL Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="Lakshmi1812"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/18/2016"
   ms.author="lakshmir;barbkess;sonyama"/>

# Ripristinare un'istanza di Azure SQL Data Warehouse (portale)

> [AZURE.SELECTOR]
- [Panoramica][]
- [Portale][]
- [PowerShell][]
- [REST][]

Questo articolo illustra come ripristinare un'istanza di Azure SQL Data Warehouse tramite il portale di Azure.

## Prima di iniziare

**Verificare la capacità in DTU.** Ogni SQL Data Warehouse è ospitato in un server SQL (ad esempio mioserver.database.windows.net), che ha una quota DTU predefinita. Per poter ripristinare un SQL Data Warehouse, verificare che la quota DTU rimanente nell'istanza del server SQL sia sufficiente per il database da ripristinare. Per informazioni su come calcolare la DTU necessaria o per richiedere maggiore DTU, vedere come [richiedere una modifica della quota DTU][].


## Ripristinare un database attivo o sospeso

Per ripristinare un database:

1. Accedere al [portale di Azure][].
2. Sul lato sinistro della schermata fare clic su **Sfoglia**, quindi su **Server SQL**.
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)
    
3. Individuare e selezionare il server.
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-select-server.png)

4. Individuare l'istanza di SQL Data Warehouse da ripristinare e selezionarla.
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-select-active-dw.png)
5. Nella parte superiore del pannello del data warehouse fare clic su **Ripristina**.
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-select-restore-from-active.png)

6. Specificare un nuovo **Nome database**.
7. Selezionare il **punto di ripristino** più recente.
    1. Assicurarsi di selezionare il punto di ripristino più recente. Poiché i punti di ripristino sono visualizzati secondo il formato UTC, l'opzione predefinita visualizzata potrebbe non essere il punto di ripristino più recente.
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-restore-blade-from-active.png)

8. Fare clic su **OK**.
9. Viene avviato il processo di ripristino del database che sarà possibile monitorare tramite **NOTIFICHE**.

>[AZURE.NOTE] Al termine del ripristino, sarà possibile configurare il database ripristinato seguendo la guida [Finalizzare un database ripristinato][].


## Ripristino di un database eliminato

Per ripristinare un database eliminato:

1. Accedere al [portale di Azure][].
2. Sul lato sinistro della schermata fare clic su **Sfoglia**, quindi su **Server SQL**.
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)

3. Individuare e selezionare il server.
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-select-server.png)

4. Scorrere fino alla sezione Operazioni nel pannello del server.
5. Fare clic sul riquadro **Database eliminati**.
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dws.png)

6. Fare clic sul database eliminato che si desidera ripristinare.
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dw.png)

7. Specificare un nuovo **Nome database**.
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-restore-blade-from-deleted.png)
    
8. Fare clic su **OK**.
9. Viene avviato il processo di ripristino del database che sarà possibile monitorare tramite **NOTIFICHE**.

>[AZURE.NOTE] Al termine del ripristino, sarà possibile configurare il database ripristinato seguendo la guida [Finalizzare un database ripristinato][].


## Passaggi successivi
Per altre informazioni sulle funzionalità di continuità aziendale delle edizioni del database SQL di Azure, leggere [Panoramica sulla continuità aziendale del database SQL][].

<!--Image references-->

<!--Article references-->
[Panoramica sulla continuità aziendale del database SQL]: ./sql-database-business-continuity.md
[Panoramica]: ./sql-data-warehouse-restore-database-overview.md
[Portale]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md
[Finalizzare un database ripristinato]: ./sql-database-recovered-finalize.md
[richiedere una modifica della quota DTU]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change

<!--MSDN references-->

<!--Blog references-->

<!--Other Web references-->
[portale di Azure]: https://portal.azure.com/

<!---HONumber=AcomDC_0824_2016-->