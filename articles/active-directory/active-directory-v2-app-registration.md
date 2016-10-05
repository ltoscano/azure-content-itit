<properties
	pageTitle="Registrazione dell'app v 2.0 | Microsoft Azure"
	description="Come registrare un'app in Microsoft per abilitare l'accesso ai servizi Microsoft tramite l'endpoint v2.0"
	services="active-directory"
	documentationCenter=""
	authors="dstrockis"
	manager="mbaldwin"
	editor=""/> 

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/16/2016"
	ms.author="dastrock"/> 

# Come registrare un'app con l'endpoint v2.0

Per creare un'app che consente di accedere tramite account Microsoft e Azure AD, è innanzitutto necessario registrare un'app con Microsoft. Con gli account Azure AD o Microsoft non sarà possibile usare un'app esistente, ma è necessario crearne una nuova.

> [AZURE.NOTE]
	Non tutti gli scenari e le funzionalità di Azure Active Directory sono supportati dall'endpoint 2.0. Per determinare se è necessario usare l'endpoint v2.0, leggere le informazioni sulle [limitazioni v2.0](active-directory-v2-limitations.md).

## Visitare il portale di registrazione delle app Microsoft
Accedere innanzitutto al sito Web all'indirizzo [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com). Il nuovo portale di registrazione delle app consente di gestire le app Microsoft.

Accedere con un account Microsoft personale, aziendale o dell'istituto di istruzione. Se non si dispone di alcun account, iscriversi per creare un nuovo account personale. La procedura richiederà pochi minuti.

Dopo aver completato l'operazione, è possibile esaminare l'elenco delle app Microsoft, che probabilmente sarà vuoto. A tale scopo, seguire questa procedura.

Fare clic sul pulsante per **aggiungere un'app** e attribuirle un nome. Il portale assegnerà all'app un ID applicazione univoco globale che verrà usato successivamente nel codice. Se l’app include un componente sul lato server che necessità di token di accesso per chiamare le API (ad esempio, Office, Azure o un’API Web), è opportuno creare anche un **segreto dell’applicazione**.
<!-- TODO: Link for app secrets -->

Successivamente, aggiungere le piattaforme usate dall'app.

- Per le applicazioni basate sul Web, specificare un **URI di reindirizzamento** dove possono essere inviati messaggi di accesso.
- Per le app per dispositivi mobili, copiare l'URI di reindirizzamento predefinito creato automaticamente.

Facoltativamente, è possibile personalizzare l'aspetto della pagina di accesso nella sezione del profilo. Assicurarsi di **salvare** prima di procedere.

> [AZURE.NOTE] Quando si crea un'applicazione usando [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com), l'applicazione verrà registrata nel tenant dell'account usato per accedere al portale. Ciò significa che non è possibile registrare un'applicazione nel tenant di Azure AD usando un account Microsoft personale. Se si desidera registrare in modo esplicito un'applicazione in un particolare tenant, accedere con un account creato nel tenant.

## Creare un'app di avvio rapido
Dopo aver creato un'app Microsoft, è possibile completare una delle esercitazioni rapide v2.0. Di seguito sono elencati alcuni suggerimenti:

[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-v2-quickstart-table.md)]

<!---HONumber=AcomDC_0921_2016-->