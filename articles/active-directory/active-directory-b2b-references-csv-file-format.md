<properties
   pageTitle="Formato di file CSV per l'anteprima di Azure Active Directory B2B Collaboration | Microsoft Azure"
   description="Azure Active Directory B2B supporta le relazioni tra società abilitando i partner commerciali ad accedere in modo selettivo alle applicazioni aziendali."
   services="active-directory"
   documentationCenter=""
   authors="viv-liu"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# Anteprima di Collaborazione B2B di Azure AD: formato file CSV

La versione di anteprima di Azure AD B2B Collaboration richiede un file CSV che specifica le informazioni sugli utenti partner da caricare nel portale di Azure AD. Il file CSV dovrebbe contenere le etichette obbligatorie indicate di seguito e i campi facoltativi, se necessario. Modificare il file CSV di esempio (sotto) senza cambiare l'ortografia delle etichette della prima riga.

>[AZURE.NOTE] La prima riga di etichette, ad esempio Email, DisplayName e così via, è necessaria per analizzare correttamente il file CSV. L'ortografia deve corrispondere ai campi specificati sotto. Queste etichette identificano il contenuto corrispondente. Le etichette dei campi facoltativi non necessari possono essere rimosse dal file CSV. L'intera colonna può essere lasciata vuota.

## Campi obbligatori: <br/>
**Email:** indirizzo di posta elettronica dell'utente invitato. <br/> **DisplayName:** nome visualizzato dell'utente invitato, in genere nome e cognome.<br/>


## Campi facoltativi: <br/>

**InvitationText:** personalizzare il testo del messaggio di posta elettronica di invito dopo la personalizzazione dell'applicazione e prima del collegamento di riscatto.<br/> **InvitedToApplications:** AppID delle applicazioni aziendali per l'assegnazione degli utenti. Per recuperare gli AppID in PowerShell, chiamare `Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId`<br/> **InvitedToGroups:** ObjectID per i gruppi a cui aggiungere l'utente. Per recuperare gli ObjectID in PowerShell, chiamare `Get-MsolGroup | fl DisplayName, ObjectId`<br/> **InviteRedirectURL:** URL per indirizzare un utente invitato dopo l'accettazione dell'invito. Deve essere un URL specifico della società, ad esempio [*contoso.my.salesforce.com*](http://contoso.my.salesforce.com/). Se questo campo facoltativo non viene specificato, l'utente invitato viene indirizzato al pannello di accesso alle app da dove può accedere alle app aziendali scelte. L'URL del pannello di accesso all'app è nel formato `https://account.activedirectory.windowsazure.com/applications/default.aspx?tenantId=<TenantID>`.<br/> **CcEmailAddress**: indirizzo di posta elettronica per la copia dell'invito inviato tramite posta elettronica. Se viene usato il campo CcEmailAddress, non è possibile usare l'invito per la creazione di utenti verificati tramite posta elettronica o tenant.<br/> **Language:** lingua dell'invito inviato tramite posta elettronica e dell'esperienza di riscatto. Se non specificato, viene usato "en" (inglese) come valore predefinito. Gli altri 10 codici lingua supportati sono:<br/>
1. de: tedesco<br/>
2. es: spagnolo<br/>
3. fr: francese<br/>
4. it: italiano<br/>
5. ja: giapponese<br/>
6. ko: coreano<br/>
7. pt-BR: portoghese (Brasile)<br/>
8. ru: russo<br/>
9. zh-HANS: cinese semplificato<br/>
10. zh-HANT: cinese tradizionale<br/>

## File CSV di esempio
Qui è riportato un file CSV di esempio che è possibile modificare.

>[AZURE.NOTE] Copiarlo e incollarlo nel Blocco note e salvarlo con l'estensione di file ".csv", quindi modificarlo in Excel. Verrà strutturato in una tabella con le etichette nella prima riga.

> Aggiungere altri campi facoltativi in Excel specificando l'etichetta e popolando la rispettiva colonna.

```
Email,DisplayName,InvitationText,InviteRedirectUrl,InvitedToApplications,InvitedToGroups,CcEmailAddress,Language
wharp@contoso.com,Walter Harp,Hi Walter! I hope you’ve been doing well.,,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,,you@yourcompany.com,en
jsmith@contoso.com,Jeff Smith,Hi Jeff! I hope you’ve been doing well.,,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,,you@yourcompany.com,en
bsmith@contoso.com,Ben Smith,Hi Ben! I hope you’ve been doing well.,,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,,you@yourcompany.com,en

```

## Articoli correlati
Vedere gli altri articoli su Collaborazione B2B di Azure AD

- [Che cos'è Azure AD B2B Collaboration?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Funzionamento](active-directory-b2b-how-it-works.md)
- [Procedura dettagliata](active-directory-b2b-detailed-walkthrough.md)
- [Formato del token per l'utente esterno](active-directory-b2b-references-external-user-token-format.md)
- [Modifiche agli attributi dell'oggetto utente esterno](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Limitazioni correnti della versione di anteprima](active-directory-b2b-current-preview-limitations.md)
- [Indice di articoli per la gestione di applicazioni in Azure Active Directory](active-directory-apps-index.md)

<!---HONumber=AcomDC_0518_2016-->