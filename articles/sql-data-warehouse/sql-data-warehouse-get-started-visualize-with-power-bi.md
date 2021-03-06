<properties
   pageTitle="Visualizzare i dati di SQL Data Warehouse con Power BI in Microsoft Azure"
   description="Visualizzare i dati di SQL Data Warehouse con Power BI"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor="" />

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/16/2016"
   ms.author="lodipalm;barbkess;sonyama" />

# Visualizzare i dati con Power BI

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 

Questa esercitazione illustra come usare Power BI per connettersi a SQL Data Warehouse e creare alcune visualizzazioni di base.

> [AZURE.VIDEO azure-sql-data-warehouse-sample-data-and-powerbi]

## Prerequisiti

Per eseguire questa esercitazione, è necessario:

- Un'istanza di SQL Data Warehouse in cui sia precaricato il database AdventureWorksDW. Per effettuarne il provisioning, vedere [Creare un Azure SQL Data Warehouse][] e scegliere di caricare i dati di esempio. Se si ha già un data warehouse ma non i dati di esempio, è possibile [caricare manualmente i dati di esempio][].


## 1\. Connettersi al database

Per aprire Power BI e connettersi al database AdventureWorksDW:

1. Accedere al [portale di Azure][].
2. Fare clic su **Database SQL** e scegliere il database SQL Data Warehouse AdventureWorks.

    ![Trovare il database][1]

3. Fare clic sul pulsante Apri in Power BI.

    ![Pulsante Power BI][2]

4. Verrà visualizzata la pagina di connessione di SQL Data Warehouse che visualizza l'indirizzo Web del database. Fare clic su Avanti.

    ![Connessione a Power BI][3]

6. Immettere il nome utente e la password del server Azure SQL per connettersi al database SQL Data Warehouse.

    ![Accesso a Power BI][4]

7. Dopo aver eseguito l'accesso a Power BI, fare clic sul set di dati AdventureWorksDW nel pannello sinistro. Verrà aperto il database.

    ![Apertura di AdventureWorksDW in Power BI][5]



## 2\. Creare un report

È ora possibile usare Power BI per analizzare i dati di esempio di AdventureWorksDW. Per eseguire l'analisi, AdventureWorksDW dispone di una visualizzazione denominata VenditeAggregate. Questa visualizzazione contiene alcune metriche chiave per l'analisi delle vendite della società.

1. Per creare una mappa degli importi di vendita in base al codice postale, nel riquadro con i campi a destra fare clic su AggregateSales per espanderlo. Fare clic sulle colonne PostalCode e SalesAmount per selezionarle.

    ![Selezione di AggregateSales in Power BI][6]

    Power BI riconosce automaticamente tali informazioni come dati geografici e le inserisce direttamente in una mappa.

    ![Mappa di Power BI][7]

2. In questo passaggio viene creato un grafico a barre che mostra gli importi delle vendite per reddito del cliente. Per creare il grafico, passare alla visualizzazione AggregateSales espansa. Fare clic sul campo SalesAmount. Trascinare il campo Customer Income a sinistra e rilasciarlo sull'asse.

    ![Selezione asse in Power BI][8]

    Il grafico a barre è stato spostato a sinistra.

    ![Barra di Power BI][9]

3. In questo passaggio viene creato un grafico a linee che mostra gli importi delle vendite per data dell'ordine. Per creare il grafico, passare alla visualizzazione AggregateSales espansa. Fare clic su SalesAmount e OrderDate. Nella colonna Visualizzazioni fare clic sull'icona del grafico a linee. Si tratta della prima icona nella seconda riga in Visualizzazioni.

	![Selezione del grafico a linee in Power BI][10]

    Ora è disponibile un report che mostra tre diverse visualizzazioni dei dati.

    ![Riga di Power BI][11]

Per salvare lo stato in qualsiasi momento, fare clic su **File** e selezionare **Salva**.

## Passaggi successivi
Dopo essersi esercitati con i dati di esempio, si passerà ora alle operazioni di [sviluppo][], [caricamento][] o [migrazione][]. In alternativa, vedere il [sito Web di Power BI][].

<!--Image references-->
[1]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-find-database.png
[2]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-button.png
[3]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-connect-to-azure.png
[4]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-sign-in.png
[5]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-open-adventureworks.png
[6]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-aggregatesales.png
[7]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-map.png
[8]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-chooseaxis.png
[9]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-bar.png
[10]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-prepare-line.png
[11]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-line.png
[12]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-save.png

<!--Article references-->
[migrazione]: sql-data-warehouse-overview-migrate.md
[sviluppo]: sql-data-warehouse-overview-develop.md
[caricamento]: sql-data-warehouse-overview-load.md
[caricare manualmente i dati di esempio]: sql-data-warehouse-load-sample-databases.md
[connecting to SQL Data Warehouse]: sql-data-warehouse-integrate-power-bi.md
[Creare un Azure SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md

<!--Other-->
[portale di Azure]: https://portal.azure.com/
[sito Web di Power BI]: http://www.powerbi.com/

<!---HONumber=AcomDC_0622_2016-->