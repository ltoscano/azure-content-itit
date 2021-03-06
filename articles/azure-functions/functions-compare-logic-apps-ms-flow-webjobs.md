<properties
	pageTitle="Scegliere tra Flow, App per la logica, Funzioni e Processi Web | Microsoft Azure"
	description="Mettere a confronto i servizi di integrazione cloud di Microsoft e decidere quali è consigliabile usare."
	services="functions,app-service\logic"
	documentationCenter="na"
	authors="cephalin"
	manager="wpickett"
	tags=""
	keywords="microsoft flow, flow, app per la logica, funzioni di azure, funzioni, processi web di azure, processi web, elaborazione di eventi, calcolo dinamico, architettura senza server"/>

<tags
	ms.service="functions"
	ms.devlang="multiple"
	ms.topic="article"
	ms.tgt_pltfrm="multiple"
	ms.workload="na"
	ms.date="09/08/2016"
	ms.author="chrande; glenga"/>

# Scegliere tra Flow, App per la logica, Funzioni e Processi Web

Questo articolo mette a confronto i servizi seguenti, disponibili in Microsoft Cloud, ognuno dei quali consente di risolvere i problemi di integrazione e automazione dei processi aziendali:

- [Microsoft Flow](https://flow.microsoft.com/)
- [App per la logica di Azure](https://azure.microsoft.com/services/logic-apps/)
- [Funzioni di Azure](https://azure.microsoft.com/services/functions/)
- [Processi Web del servizio app di Azure](../app-service-web/web-sites-create-web-jobs.md)

Tutti questi servizi risultano utili quando si "mettono insieme" sistemi diversi. Possono definire input, azioni, condizioni e output e ognuno può essere eseguito in base a una pianificazione o un trigger. Tuttavia, ogni servizio offre un valore aggiunto univoco e il confronto non è finalizzato a stabilire quale sia "il servizio migliore", ma qual è "il servizio più adatto per una determinata situazione". Una combinazione di questi servizi è spesso il modo migliore per creare rapidamente una soluzione di integrazione scalabile e con funzionalità complete.

<a name="flow"></a>
## Flow rispetto ad App per la logica

Microsoft Flow e App per la logica di Azure possono essere esaminati insieme, essendo entrambi servizi di integrazione di tipo *configuration first*, un aspetto che facilita la creazione di processi e flussi di lavoro e l'integrazione con diverse applicazioni aziendali e SaaS.

- Flow si basa sulle app per la logica
- Hanno la stessa funzionalità di progettazione flussi di lavoro
- I [connettori](../connectors/apis-list.md) che funzionano in un servizio possono funzionare anche nell'altro

Flow consente a qualsiasi impiegato di eseguire semplici integrazioni, ad esempio ricevere SMS per i messaggi di posta elettronica importanti, senza ricorrere a sviluppatori o professionisti IT. D'altra parte, App per la logica consente di abilitare integrazioni avanzate o cruciali, come i processi B2B, in cui sono richieste procedure di sicurezza e la metodologia DevOps a livello di organizzazione. È normale che la complessità di un flusso di lavoro aziendale aumenti nel tempo. Di conseguenza, è possibile iniziare con un flusso e quindi convertirlo in un'app per la logica in base alle esigenze.

La tabella seguente consente di determinare se per una data integrazione è più appropriato usare Flow o App per la logica.

| | Flusso | App per la logica |
|---------------|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| Destinatari | Impiegati, utenti aziendali | Professionisti IT, sviluppatori |
| Scenari | Self-service | Cruciale |
| Strumento di progettazione | Nel browser, solo interfaccia utente | Nel browser e [Visual Studio](../app-service/logic/app-service-logic-deploy-from-vs.md), è disponibile la [visualizzazione Codice](../app-service-logic/app-service-logic-author-definitions.md) |
| DevOps | Ad hoc, sviluppo nell'ambiente di produzione | Controllo del codice sorgente, test, supporto, oltre ad automazione e gestibilità in [Azure Resource Manager](../app-service-logic/app-service-logic-arm-provision.md)|
| Esperienza di amministrazione| [https://flow.microsoft.com](https://flow.microsoft.com) | [https://portal.azure.com](https://portal.azure.com) |
| Sicurezza | Procedure standard: [sovranità dei dati](https://wikipedia.org/wiki/Technological_Sovereignty), [crittografia dati inattivi](https://wikipedia.org/wiki/Data_at_rest#Encryption) per i dati sensibili e così via. | Garanzie di sicurezza di Azure: [Sicurezza di Azure](https://www.microsoft.com/trustcenter/Security/AzureSecurity), [Centro sicurezza](https://azure.microsoft.com/services/security-center/) [log di controllo](https://azure.microsoft.com/blog/azure-audit-logs-ux-refresh/) e altro. |

<a name="function"></a>
## Funzioni e WebJobs

Funzioni di Azure e Processi Web del servizio app di Azure possono essere esaminati insieme, essendo entrambi servizi di integrazione di tipo *code first* e progettati per gli sviluppatori. Consentono di eseguire uno script o un frammento di codice in risposta a eventi diversi, ad esempio [nuovi BLOB di archiviazione ](functions-bindings-storage.md) o [una richiesta di webhook](functions-bindings-http-webhook.md). Ecco le analogie:

- Entrambi si basano sul [servizio app di Azure](../app-service/app-service-value-prop-what-is.md) e usano funzionalità come [controllo del codice sorgente](../app-service-web/app-service-continuous-deployment.md), [autenticazione](../app-service/app-service-authentication-overview.md), e [monitoraggio](../app-service-web/web-sites-monitor.md).
- Sono entrambi servizi rivolti agli sviluppatori.
- Supportano entrambi linguaggi di scripting e programmazione standard.
- Per entrambi è disponibile il supporto di NuGet e NPM.

Funzioni è l'evoluzione naturale di Processi Web, nel senso che usa gli elementi ottimali di Processi Web come base per il miglioramento. I miglioramenti includono:

- Sviluppo, test ed esecuzione di codice più semplici, direttamente nel browser.
- Integrazione predefinita con più servizi di Azure e di terze parti come i [webhook GitHub](https://developer.github.com/webhooks/creating/).
- Pagamento a consumo, non è necessario pagare per un [piano di servizio app](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
- [Ridimensionamento dinamico](functions-scale.md) automatico.
- I clienti del servizio app esistenti possono comunque scegliere l'esecuzione con un piano di servizio app per sfruttare i vantaggi delle risorse sottoutilizzate.
- Integrazione con App per la logica.

La tabella seguente riepiloga le differenze tra Funzioni e Processi Web:

| | Funzioni | WebJobs |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------|
| Ridimensionamento | Ridimensionamento senza configurazione | Ridimensionamento con il piano di servizio app |
| Prezzi | Pagamento a consumo o parte del piano di servizio app | Parte del piano di servizio app |
| Tipo di esecuzione | Attivata, pianificata (da trigger timer) | Attivata, continua, pianificata |
| Eventi di attivazione | [Timer](functions-bindings-timer.md), [Azure DocumentDB](functions-bindings-documentdb.md), [Hub eventi di Azure](functions-bindings-event-hubs), [HTTP/webhook (GitHub, Slack)](functions-bindings-http-webhook.md), [App per dispositivi mobili del servizio app di Azure](functions-bindings-mobile-apps.md), [Hub di notifica di Azure](functions-bindings-notification-hubs.md), [Bus di servizio di Azure](functions-bindings-service-bus.md), [Archiviazione di Azure](articles/functions-bindings-storage.md) | [Archiviazione di Azure](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md), [Bus di servizio di Azure](websites-dotnet-webjobs-sdk-service-bus.md) |
| Sviluppo nel browser | x | |
| Windows Scripting | sperimentale | x |
| PowerShell | sperimentale | x |
| C# | x | x |
| F# | x | |
| Bash | sperimentale | x |
| PHP | sperimentale | x |
| Python | sperimentale | x |
| JavaScript | x | x |
| Java | sperimentale | x |

L'uso di Funzioni o di Processi Web dipende in definitiva dalle operazioni che già si svolgono con il servizio app. Se è disponibile un'app del servizio app per cui si vogliono eseguire frammenti di codice e avere la possibilità di gestirli insieme nello stesso ambiente DevOps, è consigliabile usare Processi Web. Se si vogliono eseguire frammenti di codice per altri servizi di Azure o anche per app di terze parti oppure si vogliono gestire i frammenti di codice di integrazione separatamente dalle app del servizio app o ancora si vogliono chiamare i frammenti di codice da un'app per la logica, è consigliabile sfruttare tutti i miglioramenti disponibili in Funzioni.

<a name="together"></a>
## Flow, App per la logica e Funzioni insieme

Come accennato in precedenza, il servizio ottimale dipende dalla situazione specifica dell'utente.

- Per la semplice ottimizzazione aziendale, usare Flow.
- Se lo scenario di integrazione è troppo avanzato per Flow o sono necessarie le funzionalità di DevOps e la conformità ai requisiti di sicurezza, usare App per la logica.
- Se un passaggio nello scenario di integrazione richiede codice specializzato o di trasformazione altamente personalizzato, scrivere un'app per le funzioni e quindi attivare una funzione come azione nell'app per la logica.

È possibile chiamare un'app per la logica in un flusso. Si può anche chiamare una funzione in un'app per la logica e un'app per la logica in una funzione. L'integrazione tra Flow, App per la logica e Funzioni continua a migliorare nel tempo. È possibile creare un elemento in un servizio e usarlo in altri servizi. Qualsiasi investimento effettuato in queste tre tecnologie risulterà comunque proficuo.

## Passaggi successivi

Per un'introduzione a ognuno di questi servizi, creare il primo flusso, la prima app per la logica, la prima app per le funzioni o il primo processo Web. Fare clic su uno di questi collegamenti:

- [Get started with Microsoft Flow](https://flow.microsoft.com/it-IT/documentation/getting-started/) (Introduzione a Microsoft Flow)
- [Creare un'app per la logica](../app-service-logic/app-service-logic-create-a-logic-app.md)
- [Creare la prima funzione di Azure](../azure-functions/functions-create-first-azure-function.md)
- [Distribuzione di processi Web usando Visual Studio](../app-service-web/websites-dotnet-deploy-webjobs.md)

In alternativa, è possibile ottenere altre informazioni su questi servizi di integrazione usando i collegamenti seguenti:

- [Leveraging Azure Functions & Azure App Service for integration scenarios](http://www.biztalk360.com/integrate-2016-resources/leveraging-azure-functions-azure-app-service-integration-scenarios/) (Sfruttare Funzioni di Azure e Servizio app di Azure per scenari di integrazione) di Christopher Anderson
- [Integrations Made Simple](http://www.biztalk360.com/integrate-2016-resources/integrations-made-simple/) (Integrazione semplificata) di Charles Lamanna
- [Logic Apps Live Webcast](http://aka.ms/logicappslive) (Webcast live sulle app per la logica)
- [Microsoft Flow Frequently asked question ](https://flow.microsoft.com/documentation/frequently-asked-questions/) (Domande frequenti su Microsoft Flow)
- [Risorse di documentazione di Processi Web di Azure](../app-service-web/websites-webjobs-resources.md)

<!---HONumber=AcomDC_0914_2016-->