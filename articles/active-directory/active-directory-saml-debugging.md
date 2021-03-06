<properties 
    pageTitle="Come eseguire il debug di single sign-on basato su SAML per applicazioni in Azure Active Directory | Microsoft Azure" 
    description="Informazioni su come eseguire il debug di single sign-on basato su SAML per applicazioni in Azure Active Directory" 
    services="active-directory" 
    authors="asmalser-msft"  
    documentationCenter="na" manager="stevenpo"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="02/09/2016" 
    ms.author="asmalser" />

#Come eseguire il debug di single sign-on basato su SAML per applicazioni in Azure Active Directory

Durante il debug di un'integrazione di applicazioni basate su SAML, è spesso utile utilizzare uno strumento come [Fiddler](http://www.telerik.com/fiddler) per visualizzare la richiesta SAML, la risposta SAML e il token SAML reale emesso per l'applicazione. Esaminando il token SAML, è possibile garantire che tutti gli attributi richiesti, il nome utente nell’oggetto SAML e l'URI dell'autorità emittente vengano ricevuti come previsto.

![][1]

La risposta da Azure AD che contiene il token SAML è in genere quella che si verifica dopo un reindirizzamento HTTP 302 da https://login.windows.net e viene inviata all’**URL di risposta** configurato per l'applicazione.
 
È possibile visualizzare il token SAML selezionando la riga e quindi la scheda **Ispettori > WebForms** nel riquadro di destra. A questo punto, fare clic con il pulsante destro sul valore **SAMLResponse** e selezionare **Invia a TextWizard**. Quindi selezionare **Da Base64** nel menu **Trasforma** per decodificare il token e visualizzarne il contenuto.
 
**Nota**: per visualizzare il contenuto di questa richiesta HTTP, Fiddler potrebbe richiedere di configurare la decrittografia del traffico HTTPS; è necessario eseguire questa operazione.

## Articoli correlati

- [Indice di articoli per la gestione di applicazioni in Azure Active Directory](active-directory-apps-index.md)
- [Configurazione del servizio Single Sign-On in applicazioni non presenti nella raccolta di applicazioni di Azure Active Directory](active-directory-saas-custom-apps.md)
- [Come personalizzare lle attestazioni rilasciate nel token SAML per le app preintegrate](active-directory-saml-claims-customization.md)

<!--Image references-->
[1]: ./media/active-directory-saml-debugging/fiddler.png

<!---HONumber=AcomDC_0211_2016-->