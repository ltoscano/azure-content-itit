<properties 
	pageTitle="Configurare le app Web nel servizio app di Azure" 
	description="Come configurare un'app Web nel servizio app di Azure" 
	services="app-service\web" 
	documentationCenter="" 
	authors="rmcmurray" 
	manager="wpickett" 
	editor=""/>

<tags 
	ms.service="app-service" 
	ms.workload="na" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="08/11/2016" 
	ms.author="robmcm"/>

# Configurazione delle app Web in Servizio app di Azure #

Questo argomento descrive come configurare un'app Web usando il [portale di Azure].

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## Impostazioni dell'applicazione

1. Nel [portale di Azure] aprire il pannello relativo all'app Web.
2. Fare clic su **Tutte le impostazioni**.
3. Fare clic su **Impostazioni applicazione**.

![Impostazioni dell'applicazione][configure01]

Nel pannello **Impostazioni applicazione** le impostazioni sono raggruppate in diverse categorie.

### Impostazioni generali

**Versioni del framework**. Impostare le opzioni seguenti se l'app usa uno dei seguenti framework:

- **.NET Framework**: consente di impostare la versione di .NET Framework.
- **PHP**: impostare la versione PHP oppure scegliere **disattivato** per disabilitare PHP.
- **Java**: consente di selezionare la versione di Java oppure **DISATTIVATO** per disabilitare Java. Utilizzare l'opzione **Contenitore Web** per scegliere tra le versioni Tomcat e Jetty.
- **Python**: consente di selezionare la versione Python oppure impostare **DISATTIVATO** per disabilitare Python.

Per motivi tecnici, l'abilitazione di Java per le proprie app disabilita le opzioni di .NET, PHP e Python.

<a name="platform"></a> **Piattaforma**. Scegliere se eseguire l'app Web in un ambiente a 32 bit o a 64 bit. L'ambiente a 64 bit richiede la modalità Basic o Standard. Le modalità Gratuito e Condiviso vengono eseguite sempre in un ambiente a 32 bit.

**Web Socket**. Impostare **ATTIVATO** per abilitare il protocollo WebSocket, ad esempio se nell'app Web viene utilizzato [ASP.NET SignalR] o [socket.io].

<a name="alwayson"></a> **Always On**. Per impostazione predefinita, le app Web vengono scaricate se restano inattive per un determinato periodo di tempo. Ciò consente al sistema di conservare le risorse. In modalità Basic o Standard è possibile abilitare **Always On** affinché l'app rimanga sempre caricata. Se nell'app vengono eseguiti processi Web continui, è necessario abilitare **Always On**, altrimenti l'esecuzione dei processi Web potrebbe non avvenire in modo affidabile.

**Versione pipeline gestita**. Consente di impostare la [modalità pipeline] IIS. Lasciare questa opzione impostata su Integrato (predefinita), tranne nel caso in cui un'app meno recente richieda una versione precedente di IIS.

**Scambio automatico**. Se si abilita l'opzione Scambio automatico per uno slot di distribuzione, il servizio app immette automaticamente l'app Web in produzione quando si esegue un aggiornamento di quello slot. Per altre informazioni, vedere [Eseguire la distribuzione negli slot di memorizzazione temporanea per le app Web nel servizio app di Azure](web-sites-staged-publishing.md).

### Debug

**Debug remoto**. Abilita il debug remoto. Quando viene abilitato, è possibile utilizzare il debugger remoto in Visual Studio per connettersi direttamente all'app Web. Il debug remoto resterà abilitato per 48 ore.

### Impostazioni app

In questa sezione vengono riportate coppie di nome/valore che verranno caricate all'avvio dell'app.

- Per le app.NET, queste impostazioni verranno inserite nella configurazione .NET `AppSettings` in fase di esecuzione, sostituendo le impostazioni esistenti.

- Le applicazioni PHP, Python, Java e Node possono accedere a queste impostazioni come variabili di ambiente durante il runtime. Per ciascuna impostazione dell'app vengono create due variabili di ambiente, una con il nome specificato dalla voce dell'impostazione dell'app e l'altra con il prefisso APPSETTING\_. Entrambe contengono lo stesso valore.

### Stringhe di connessione

Stringhe di connessione per le risorse collegate.

Per le app .NET, tali stringhe di connessione vengono inserite nelle impostazioni `connectionStrings` della configurazione .NET in fase di runtime, sostituendo le voci esistenti in cui la chiave è uguale al nome del database collegato.

Per le applicazioni PHP, Python, Java e Node queste impostazioni saranno disponibili come variabili di ambiente durante il runtime, con il tipo di connessione come prefisso. I prefissi delle variabili di ambiente sono i seguenti:

- SQL Server: `SQLCONNSTR_`
- MySQL: `MYSQLCONNSTR_`
- Database SQL: `SQLAZURECONNSTR_`
- Personalizzato: `CUSTOMCONNSTR_`

Ad esempio, se una stringa di connessione MySql venisse denominata `connectionstring1`, l'accesso avverrebbe attraverso la variabile di ambiente`MYSQLCONNSTR_connectionString1`.

### Documenti predefiniti

Il documento predefinito è rappresentato dalla pagina Web visualizzata nell'URL radice di un sito Web. Viene utilizzato il primo file corrispondente dell'elenco.

Le app Web potrebbero usare moduli che vengono instradati in base all'URL invece di visualizzare contenuto statico, nel caso in cui non sia presente alcun documento predefinito.

### Mapping dei gestori

Usare quest'area per aggiungere processori script personalizzati allo scopo di gestire le richieste di estensioni di file specifiche.

- **Estensione**. L'estensione file da gestire, ad esempio *.php o handler.fcgi.
- **Percorso processore script**. Il percorso assoluto del processore script. Le richieste di file che corrispondono a questo modello saranno elaborate dal processore script. Utilizzare il percorso `D:\home\site\wwwroot` per fare riferimento alla directory radice della propria app.
- **Argomenti aggiuntivi**. Argomenti facoltativi della riga dei comando per il processore script


### Applicazioni e directory virtuali 
 
Per configurare applicazioni e directory virtuali, specificare ogni directory virtuale e il corrispondente percorso fisico relativo alla radice del sito. È facoltativamente possibile selezionare la casella di controllo **Applicazione** in modo da contrassegnare una directory virtuale come applicazione.


## Abilitazione dei log di diagnostica

Per abilitare i log di diagnostica:

1. Nel pannello dell'app Web, fare clic su **Tutte le impostazioni**.
2. Fare clic su **Log diagnostici**.

Opzioni per la scrittura dei log di diagnostica da un'applicazione Web che supporta la registrazione:

- **Registrazione applicazioni**. Consente di scrivere i log delle applicazioni nel file system. La registrazione ha una durata di 12 ore.

**Livello**. Quando viene abilitata la registrazione, questa opzione consente di specificare la quantità di informazioni registrata (Errore, Avviso, Informazioni, Dettagliato).

**Registrazione del server Web**. I log vengono salvati nel formato file di log esteso W3C.

**Messaggi di errore dettagliati**. Consente di salvare messaggi di errore dettagliati in file htm. I file vengono salvati nel percorso /LogFiles/DetailedErrors.

**Traccia delle richieste non riuscite**. Consente di registrare le richieste non riuscite ai file XML. I file vengono salvati in /LogFiles/W3SVC*xxx*, dove xxx è un identificatore univoco. Questa cartella contiene un file XSL e uno o più file XML. Assicurarsi di scaricare il file XSL, perché fornisce la funzionalità di formattazione e filtro dei contenuti dei file XML.

Per visualizzare i file di log, è necessario creare le credenziali FTP, come descritto di seguito:

1. Nel pannello dell'app Web, fare clic su **Tutte le impostazioni**.
2. Fare clic su **Credenziali distribuzione**.
3. Immettere un nome utente e una password.
4. Fare clic su **Salva**.

![Reimpostare le credenziali di distribuzione][configure03]

Il nome utente completo FTP è "app\\nomeutente", dove *app* è il nome dell'app web. Il nome utente è elencato nel pannello dell'app Web in **Elementi essenziali**.

![Credenziali di distribuzione FTP][configure02]

## Altre attività di configurazione

### SSL 

In modalità Basic o Standard è possibile caricare certificati SSL per un dominio personalizzato. Per altre informazioni, vedere [Abilitare HTTPS per un'app Web].

Per visualizzare i certificati caricati, fare clic su **Tutte le impostazioni** > **Domini e SSL personalizzati**.

### Nomi di dominio

Aggiungere nomi di dominio personalizzati per la propria app Web. Per ulteriori informazioni, vedere [Configurare un nome di dominio personalizzato per un'app Web nel servizio app di Azure].

Per visualizzare i nomi di dominio, fare clic su **Tutte le impostazioni** > **Domini e SSL personalizzati**.

### Deployments

- Configurare la distribuzione continua. Vedere [Using Git to deploy Web Apps in Azure App Service] \(Uso di Git per distribuire app Web nel servizio app di Azure)
- Slot di distribuzione. Vedere [Configurare ambienti di staging per le app Web nel servizio app di Azure].

Per visualizzare gli slot di distribuzione, fare clic su **Tutte le impostazioni** > **Slot di distribuzione**.

### Monitoraggio

Nella modalità Basic o Standard, è possibile testare la disponibilità degli endpoint HTTP o HTTPS, da un numero massimo di tre posizioni geograficamente distribuite. Un test di monitoraggio ha esito negativo se il codice della risposta HTTP è un errore (4xx o 5xx) o se la risposta richiede più di 30 secondi. Un endpoint è considerato disponibile se il test di monitoraggio ha esito positivo da tutte le posizioni specificate.

Per ulteriori informazioni, vedere [Procedura: monitorare lo stato degli endpoint].

>[AZURE.NOTE] Per iniziare a usare il servizio app di Azure prima di registrarsi per ottenere un account Azure, andare a [Prova il servizio app], dove è possibile creare un'app Web iniziale temporanea nel servizio app. Non è necessario fornire una carta di credito né impegnarsi in alcun modo.

## Passaggi successivi

- [Configurare un nome di dominio personalizzato nel servizio app di Azure]
- [Abilitare HTTPS per un'app in Azure App Service]
- [Scalare un'app Web nel servizio app di Azure]
- [Informazioni di base sul monitoraggio di App Web nel servizio app di Azure]

<!-- URL List -->

[ASP.NET SignalR]: http://www.asp.net/signalr
[portale di Azure]: https://portal.azure.com/
[Configurare un nome di dominio personalizzato nel servizio app di Azure]: ./web-sites-custom-domain-name.md
[Configurare ambienti di staging per le app Web nel servizio app di Azure]: ./web-sites-staged-publishing.md
[Abilitare HTTPS per un'app in Azure App Service]: ./web-sites-configure-ssl-certificate.md
[Procedura: monitorare lo stato degli endpoint]: http://go.microsoft.com/fwLink/?LinkID=279906
[Informazioni di base sul monitoraggio di App Web nel servizio app di Azure]: ./web-sites-monitor.md
[modalità pipeline]: http://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture#Application
[Scalare un'app Web nel servizio app di Azure]: ./web-sites-scale.md
[socket.io]: ./web-sites-nodejs-chat-app-socketio.md
[Prova il servizio app]: http://go.microsoft.com/fwlink/?LinkId=523751

<!-- IMG List -->

[configure01]: ./media/web-sites-configure/configure01.png
[configure02]: ./media/web-sites-configure/configure02.png
[configure03]: ./media/web-sites-configure/configure03.png

<!---HONumber=AcomDC_0817_2016-->