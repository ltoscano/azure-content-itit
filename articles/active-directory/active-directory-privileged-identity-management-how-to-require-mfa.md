<properties
   pageTitle="Come richiedere l'autenticazione a più fattori | Microsoft Azure"
   description="Informazioni su come richiedere l'autenticazione a più fattori (MFA, Multi-Factor Authentication) per identità con privilegi con l'estensione Azure Active Directory Privileged Identity Management."
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="07/01/2016"
   ms.author="kgremban"/>

# Come richiedere l'autenticazione MFA in Azure AD Privileged Identity Management

È consigliabile richiedere l'autenticazione Multi-Factor Authentication (MFA) per tutti gli amministratori. Ciò consente di ridurre il rischio di attacchi causato da una password compromessa.

È possibile richiedere agli utenti di compilare una richiesta per l'autenticazione MFA durante l'accesso. Il post di blog relativo a [MFA per Office 365 e MFA per Azure](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/) confronta le opzioni delle sottoscrizioni di Azure e Office con le funzionalità contenute nell'offerta di Microsoft Azure Multi-Factor Authentication.

È anche possibile richiedere agli utenti di compilare una richiesta per l'autenticazione MFA durante l'attivazione di un ruolo in Azure AD PIM. In questo modo, se l'utente non ha compilato una richiesta per l'autenticazione MFA durante l'accesso, PIM richiederà all'utente di eseguire tale operazione.

## Richiedere l'MFA in Azure AD Privileged Identity Management

Quando si gestiscono le identità in PIM come amministratore dei ruoli con privilegi, è possibile che vengano visualizzati avvisi che consigliano l'autenticazione MFA per gli account con privilegi. Fare clic sull'avviso di sicurezza nel dashboard di PIM. Verrà visualizzato un nuovo pannello con un elenco degli account amministratore che devono richiedere l'autenticazione MFA. È possibile richiedere l'autenticazione MFA selezionando più ruoli e quindi facendo clic sul pulsante **Correggi** oppure è possibile fare clic sui puntini di sospensione accanto ai singoli ruoli e quindi fare clic sul pulsante **Correggi**.

> [AZURE.IMPORTANT] Azure Multi-Factor Authentication (MFA) funziona attualmente solo con account aziendali o dell'istituto di istruzione, non con account Microsoft (account personale usato in genere per l'accesso a servizi Microsoft, come Skype, Xbox e Outlook.com). Gli utenti che usano un account Microsoft non possono essere amministratori idonei poiché non possono usare l'autenticazione MFA per attivare i propri ruoli. Se devono continuare a gestire i carichi di lavoro con un account Microsoft, elevare questi utenti a livello di amministratori permanenti per ora.

È anche possibile modificare il requisito di autenticazione MFA per un ruolo specifico facendo clic sul ruolo nella sezione Ruoli del dashboard di PIM. Fare clic su **Impostazioni** nel pannello del ruolo e selezionare **Abilita** sotto l'autenticazione a più fattori.

## Modalità di convalida della MFA da parte di Azure AD PIM

Sono disponibili due opzioni per la convalida di MFA quando un utente attiva un ruolo.

L'opzione più semplice consiste nell'usare Azure MFA per gli utenti che attivano un ruolo con privilegi. A tale scopo, verificare prima di tutto che gli utenti abbiano una licenza, se necessario, e che abbiano effettuato la registrazione per Azure MFA. Per altre informazioni su questa procedura, vedere [Introduzione ad Azure Multi-Factor Authentication nel cloud](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md). È consigliabile, ma non obbligatorio, configurare Azure AD in modo da rendere obbligatoria l'autenticazione MFA per questi utenti durante l'accesso. Le verifiche di MFA verranno infatti effettuate direttamente da Azure AD PIM.

In alternativa, se gli utenti eseguono l'autenticazione in locale, è possibile assegnare al provider di identità la responsabilità dell'autenticazione MFA. Ad esempio, se AD Federation Services è stato configurato per richiedere l'autenticazione basata su smart card prima dell'accesso ad Azure AD, vedere le istruzioni per la configurazione di AD FS per l'invio di attestazioni ad Azure AD nell'articolo [Protezione delle risorse cloud con Azure Multi-Factor Authentication e AD FS](../multi-factor-authentication/multi-factor-authentication-get-started-adfs-cloud.md). Quando un utente prova ad attivare un ruolo, Azure AD PIM accetta il fatto che l'autenticazione MFA sia già stata convalidata per l'utente alla ricezione delle attestazioni appropriate.


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## Passaggi successivi
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!---HONumber=AcomDC_0706_2016-->