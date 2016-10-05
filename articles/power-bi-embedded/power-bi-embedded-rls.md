<properties
   pageTitle="Sicurezza a livello di riga con Power BI Embedded"
   description="Informazioni dettagliate sulla sicurezza a livello di riga con Power BI Embedded"
   services="power-bi-embedded"
   documentationCenter=""
   authors="mgblythe"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="07/05/2016"
   ms.author="mblythe"/>

# Sicurezza a livello di riga con Power BI Embedded

La sicurezza a livello di riga può essere usata per limitare l'accesso utente a dati specifici in un report o a un set di dati, consentendo a più utenti diversi di usare lo stesso report visualizzando al tempo stesso dati diversi. Power BI Embedded supporta ora i set di dati configurati con la sicurezza a livello di riga.

![](media\power-bi-embedded-rls\pbi-embedded-rls-flow-1.png)

Per sfruttare i vantaggi della sicurezza a livello di riga, è importante comprendere tre concetti principali, ovvero utenti, ruoli e regole. Ecco informazioni più approfondite su ogni concetto:

**Utenti**: utenti finali effettivi che visualizzano i report. In Power BI Embedded gli utenti vengono identificati dalla proprietà username nel token dell'app.

**Ruoli**: utenti appartenenti ai ruoli. Un ruolo è un contenitore di regole e può essere denominato ad esempio "Responsabile vendite" o "Rappresentante". In Power BI Embedded gli utenti vengono identificati dalla proprietà roles nel token dell'app.

**Regole**: i ruoli hanno regole e tali regole sono i filtri effettivi che vengono applicati ai dati. È ad esempio possibile usare un filtro semplice come "Paese = USA" o un filtro più dinamico.

### Esempio

Per il resto di questo articolo verrà fornito un esempio di creazione di sicurezza a livello di riga e quindi di utilizzo di tale funzionalità in un'applicazione incorporata. Il nostro esempio usa il file PBIX [Retail Analysis Sample](http://go.microsoft.com/fwlink/?LinkID=780547).

![](media\power-bi-embedded-rls\pbi-embedded-rls-scenario-2.png)

L'esempio di analisi delle vendite al dettaglio mostra le vendite per tutti i punti vendita di una catena specifica. Senza sicurezza a livello di riga, ogni manager locale che effettua l'acceso e visualizza il report vedrà gli stessi dati. I dirigenti hanno deciso che ogni manager locale deve visualizzare solo le vendite relative ai punti vendita gestiti dal manager specifico e a questo scopo è possibile usare la sicurezza a livello di riga.

La sicurezza a livello di riga viene creata in Power BI Desktop. Quando il set di dati e il report vengono aperti, è possibile passare alla vista diagramma per visualizzare lo schema:

![](media\power-bi-embedded-rls\pbi-embedded-rls-diagram-view-3.png)

Ecco alcuni aspetti dello schema da notare:

-	Tutte le misure, ad esempio **Total Sales**, sono archiviate nella tabella dei fatti **Sales**.
-	Sono presenti quattro tabelle delle dimensioni aggiuntive correlate, ovvero **Item**, **Time**, **Store** e **District**.
-	Le frecce sulle linee relative alle relazioni indicano il modo in cui i filtri possono essere applicati da una tabella a un'altra. Se, ad esempio, un filtro viene posizionato su **Time[Date]**, nello schema corrente verrebbero filtrati verso il basso solo i valori della tabella **Sales**. Questo filtro non influirebbe su altre tabelle, perché tutte le frecce sulle linee relative alle relazioni fanno riferimento alla tabella Sales, non ad altre tabelle.
-	La tabella **District** indica il manager per ogni area:

    ![](media\power-bi-embedded-rls\pbi-embedded-rls-district-table-4.png)

In base a questo schema, se si applica un filtro alla colonna **District Manager** nella tabella District e se tale filtro corrisponde all'utente che visualizza il report, il filtro selezionerà verso il basso anche le tabelle **Store** e **Sales** per visualizzare solo i dati relativi al manager locale specifico.

Ecco come:

1.	Nella scheda Creazione di modelli fare clic su **Gestisci ruoli**. ![](media\power-bi-embedded-rls\pbi-embedded-rls-modeling-tab-5.png)

2.	Creare un nuovo ruolo denominato **Manager**. ![](media\power-bi-embedded-rls\pbi-embedded-rls-manager-role-6.png)

3.	Nella tabella **District** immettere l'espressione DAX seguente: **[District Manager] = USERNAME()** ![](media\power-bi-embedded-rls\pbi-embedded-rls-manager-role-7.png)

4.	Per assicurarsi che i ruoli funzionino correttamente, nella scheda **Creazione di modelli** fare clic su **Visualizza come ruoli**, quindi immettere i valori seguenti: ![](media\power-bi-embedded-rls\pbi-embedded-rls-view-as-roles-8.png)

    I report mostrano ora i dati che verrebbero visualizzati se effettuasse l'accesso l'utente **Andrew Ma**.

Se si applica il filtro come indicato in questa sezione, vengono filtrati verso il basso tutti i record delle tabelle **District**, **Store** e **Sales**. A causa tuttavia della direzione del filtro nelle relazioni tra le tabelle **Sales** e **Time**, **Sales** e **Item** e infine **Item** e **Time**, le tabelle non verranno filtrate verso il basso.

![](media\power-bi-embedded-rls\pbi-embedded-rls-diagram-view-9.png)

Questo approccio potrebbe essere adatto a questo requisito, ma se non si vuole che i manager visualizzino elementi per cui non presentano vendite, è possibile attivare il filtro incrociato bidirezionale per la relazione e applicare il filtro di sicurezza in entrambe le direzioni. A questo scopo, modificare come indicato di seguito la relazione tra **Sales** e **Item**:

![](media\power-bi-embedded-rls\pbi-embedded-rls-edit-relationship-10.png)

I filtri possono essere ora applicati anche dalla tabella Sales alla tabella **Item**:

![](media\power-bi-embedded-rls\pbi-embedded-rls-diagram-view-11.png)

**Nota** Se si usa la modalità DirectQuery per i dati, sarà necessario abilitare il filtro incrociato bidirezionale selezionando queste due opzioni:

1.	**File** -> **Opzioni e impostazioni** -> **Funzionalità in anteprima** -> **Enable cross filtering in both directions for DirectQuery** (Abilita il filtro incrociato bidirezionale per DirectQuery).
2.	**File** -> **Opzioni e impostazioni** -> **DirectQuery** -> **Consenti misure senza limitazioni in modalità DirectQuery**.


Per altre informazioni sui filtri incrociati bidirezionali, scaricare il white paper [Bidirectional cross-filtering in SQL Server Analysis Services 2016 and Power BI Desktop](http://download.microsoft.com/download/2/7/8/2782DF95-3E0D-40CD-BFC8-749A2882E109/Bidirectional cross-filtering in Analysis Services 2016 and Power BI.docx) \(Filtro incrociato bidirezionale in SQL Server Analysis Services 2016 e Power BI Desktop).

Queste sono tutte le operazioni da eseguire in Power BI Desktop, ma per consentire il funzionamento delle regole di sicurezza a livello di riga in Power BI Embedded è necessario ancora un passaggio. Gli utenti vengono autenticati e autorizzati dall'applicazione e i token dell'app vengono usati per concedere l'accesso a un report specifico di Power BI Embedded. Power BI Embedded non ha informazioni specifiche sull'identità dell'utente. Per consentire il funzionamento della sicurezza a livello di riga, sarà necessario passare contesto aggiuntivo come parte del token dell'app:
-	**username** (facoltativo): usata con la sicurezza a livello di riga, questa stringa può semplificare l'identificazione dell'utente quando si applicano le regole di sicurezza a livello di riga. Vedere l'articolo relativo all'uso della sicurezza a livello di riga con Power BI Embedded.
-	**roles**: stringa contenente i ruoli da selezionare quando si applicano le regole di sicurezza a livello di riga. Se si passa più di un ruolo, è consigliabile passarli come matrice di stringhe.

Se la proprietà username è presente, è necessario passare anche almeno un valore in roles.

Il token dell'app completo avrà un aspetto analogo al seguente:

![](media\power-bi-embedded-rls\pbi-embedded-rls-app-token-string-12.png)

Al termine di queste procedure, se un utente accede all'applicazione per visualizzare il report, potrà vedere solo i dati che è autorizzato a vedere, in base a quanto definito dalla sicurezza a livello di riga.

![](media\power-bi-embedded-rls\pbi-embedded-rls-dashboard-13.png)

## Vedere anche
[Row-level security (RLS) with Power BI](https://powerbi.microsoft.com/it-IT/documentation/powerbi-admin-rls/) (Sicurezza a livello di riga con Power BI)

<!---HONumber=AcomDC_0907_2016-->