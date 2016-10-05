<properties
	pageTitle="Esercitazione: Integrazione di Azure Active Directory con Beeline | Microsoft Azure"
	description="Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory e Beeline."
	services="active-directory"
	documentationCenter=""
	authors="jeevansd"
	manager="femila"
	editor=""/> 

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/19/2016"
	ms.author="jeedes"/> 


# Esercitazione: Integrazione di Azure Active Directory con Beeline

Questa esercitazione descrive come integrare Beeline con Azure Active Directory (Azure AD).

L'integrazione di Beeline con Azure AD offre i vantaggi seguenti:

- È possibile controllare in Azure AD chi può accedere a Beeline
- È possibile abilitare gli utenti per l'accesso automatico a Beeline (Single Sign-On) con i propri account Azure AD
- È possibile gestire gli account da una posizione centrale: il portale di Azure classico

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](active-directory-appssoaccess-whatis.md).

## Prerequisiti

Per configurare l'integrazione di Azure AD con Beeline, sono necessari gli elementi seguenti:

- Sottoscrizione di Azure AD.
- Sottoscrizione di Beeline abilitata per l'accesso Single Sign-On.


> [AZURE.NOTE] Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione.


A questo scopo, è consigliabile seguire le indicazioni seguenti:

- Non usare l'ambiente di produzione, a meno che non sia necessario.
- Se non si dispone di un ambiente di prova di Azure AD, è possibile ottenere una versione di valutazione di un mese [qui](https://azure.microsoft.com/pricing/free-trial/).


## Descrizione dello scenario
In questa esercitazione viene eseguito il test dell'accesso Single Sign-On di Azure AD in un ambiente di test. Lo scenario descritto in questa esercitazione prevede i due blocchi predefiniti seguenti:

1. Aggiunta di Beeline dalla raccolta
2. Configurazione e test dell'accesso Single Sign-On di Azure AD


## Aggiunta di Beeline dalla raccolta
Per configurare l'integrazione di Beeline in Azure AD, è necessario aggiungere Beeline dalla raccolta al proprio elenco di app SaaS gestite.

**Per aggiungere Beeline dalla raccolta, seguire questa procedura:**

1. Nel **portale di Azure classico** fare clic su **Active Directory** nel riquadro di spostamento sinistro.

	![Active Directory][1] 

2. Nell'elenco **Directory** selezionare la directory per la quale si desidera abilitare l'integrazione delle directory.

3. Per aprire la visualizzazione applicazioni, nella visualizzazione directory fare clic su **Applications** nel menu superiore.

	![Applicazioni][2] 

4. Fare clic su **Add** nella parte inferiore della pagina.

	![Applicazioni][3] 

5. Nella finestra di dialogo **Come procedere** fare clic su **Aggiungere un'applicazione dalla raccolta**.

	![Applicazioni][4]

6. Nella casella di ricerca digitare **Beeline**.

	![Creazione di un utente test di Azure AD](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_01.png)

7. Nel riquadro dei risultati selezionare **Beeline** e quindi fare clic su **Completa** per aggiungere l'applicazione.

	![Creazione di un utente test di Azure AD](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_06.png)

##  Configurazione e test dell'accesso Single Sign-On di Azure AD
In questa sezione viene configurato e testato l'accesso Single Sign-On di Azure AD con Beeline usando un utente di test di nome "Britta Simon".

Per il funzionamento dell'accesso Single Sign-On, Azure AD deve conoscere l'utente controparte di Beeline che corrisponde a un utente di Azure AD. In altre parole deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in Beeline. La relazione di collegamento viene stabilita assegnando lo stesso valore al **nome utente** in Azure AD e a **Username** in Beeline.

Per configurare e testare l'accesso Single Sign-On di Azure AD con Beeline, è necessario completare i blocchi predefiniti seguenti:

1. **[Configurazione dell'accesso Single Sign-On di Azure AD](#configuring-azure-ad-single-single-sign-on)**: per abilitare gli utenti all'uso di questa funzionalità.
2. **[Creazione di un utente test di Azure AD](#creating-an-azure-ad-test-user)**: per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
4. **[Creazione di un utente di test di Beeline](#creating-an-beeline-test-user)**: per avere una controparte di Britta Simon in Beeline collegata alla relativa rappresentazione in Azure AD.
5. **[Assegnazione dell'utente test di Azure AD](#assigning-the-azure-ad-test-user)**: per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
5. **[Test dell'accesso Single Sign-On](#testing-single-sign-on)**: per verificare se la configurazione funziona.

### Configurazione dell'accesso Single Sign-On di Azure AD

In questa sezione viene abilitato l'accesso Single Sign-On di Azure AD nel portale classico e viene configurato l'accesso Single Sign-On nell'applicazione Beeline.

L'applicazione Beeline si aspetta che le asserzioni SAML abbiano un formato specifico. Rivolgersi prima di tutto al team di Beeline per individuare l'identificatore utente corretto di cui verrà eseguito il mapping nell'applicazione. Seguire anche le indicazioni del team di Beeline relative all'attributo da usare per il mapping. Microsoft consiglia di usare l'attributo **"NameIdentifier"** come identificatore utente. È possibile gestire il valore di questo attributo dalla scheda degli **attributi** dell'applicazione. La schermata seguente illustra un esempio relativo a questa operazione. Qui abbiamo mappato l'attestazione nameidentifier con l'attributo **userprincipalname**, ottenendo un ID utente univoco che sarà inviato all'applicazione Beeline in ogni risposta SAML corretta.

![Configura accesso Single Sign-On](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_07.png)


**Per configurare l'accesso Single Sign-On di Azure AD con Beeline, seguire questa procedura:**

1. Nella pagina di integrazione dell'applicazione **Beeline** del portale classico fare clic su **Configura accesso Single Sign-On** per aprire la finestra di dialogo **Configura accesso Single Sign-On**.

	 ![Configura accesso Single Sign-On][6]

2. Nella pagina **Stabilire come si desidera che gli utenti accedano a Beeline** selezionare **Single Sign-On di Azure AD** e quindi fare clic su **Avanti**.
 	
	![Configura accesso Single Sign-On](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_03.png)

3. Nella pagina **Configurare le impostazioni dell'app** seguire questa procedura:

	![Configura accesso Single Sign-On](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_04.png) 


    a. Nella casella di testo **Identificatore** digitare l'URL usato dagli utenti per accedere all'applicazione Beeline usando il modello seguente: `https://projects.beeline.net/<instance name>`

	b. Nella casella URL di risposta digitare l'URL nel formato seguente: `https://projects.beeline.net/<instance name>/SSO_External.ashx` o `https://projects.beeline.net/<company name>/SSO_External.ashx`


4. Nella pagina **Configura accesso Single Sign-On in Beeline** seguire questa procedura:

	![Configura accesso Single Sign-On](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_05.png)

    a. Fare clic su **Scarica metadati** e quindi salvare il file nel computer.

    b. Fare clic su **Next**.


5.  Al fine di configurare l'accesso SSO per l'applicazione, contattare il team di supporto di Beeline che fornirà l'aiuto richiesto. Si noti che è necessario inviare un messaggio di posta elettronica e allegare il file dei metadati scaricato oltre a fornire l'ID entità e l'URL del servizio Single Sign-Out.
  
6. Nel portale classico selezionare la conferma della configurazione dell'accesso Single Sign-On e quindi fare clic su **Avanti**.
	
	![Accesso Single Sign-On di Azure AD][10] 

7. Nella pagina **Conferma Single Sign-on** fare clic su **Completa**.
  	
	![Accesso Single Sign-On di Azure AD][11] 



### Creazione di un utente test di Azure AD
In questa sezione viene creato un utente test chiamato Britta Simon nel portale classico.


![Creare un utente di Azure AD][20] 

**Per creare un utente test in Azure AD, eseguire la procedura seguente:**

1. Nel **portale di Azure classico** fare clic su **Active Directory** nel riquadro di spostamento sinistro.
	
	![Creazione di un utente test di Azure AD](./media/active-directory-saas-beeline-tutorial/create_aaduser_09.png) 

2. Nell'elenco **Directory** selezionare la directory per la quale si desidera abilitare l'integrazione delle directory.

3. Per visualizzare l'elenco di utenti, fare clic su **Utenti** nel menu in alto.
	
	![Creazione di un utente test di Azure AD](./media/active-directory-saas-beeline-tutorial/create_aaduser_03.png) 

4. Per aprire la finestra di dialogo **Aggiungi utente**, fare clic su **Aggiungi utente** nella barra degli strumenti in basso.

	![Creazione di un utente test di Azure AD](./media/active-directory-saas-beeline-tutorial/create_aaduser_04.png) 

5. Nella pagina della finestra di dialogo **Informazioni sull'utente** seguire questa procedura:
 
	![Creazione di un utente test di Azure AD](./media/active-directory-saas-beeline-tutorial/create_aaduser_05.png)

    a. In Tipo di utente selezionare Nuovo utente nell'organizzazione.

    b. Nella casella di testo **Nome utente** digitare **BrittaSimon**.

    c. Fare clic su **Next**.

6.  Nella pagina della finestra di dialogo **Profilo utente** seguire questa procedura:

	![Creazione di un utente test di Azure AD](./media/active-directory-saas-beeline-tutorial/create_aaduser_06.png) 

    a. Nella casella di testo **Nome** digitare **Britta**.

    b. Nella casella di testo **Cognome** digitare **Simon**.

    c. Nella casella di testo **Nome visualizzato** digitare **Britta Simon**.

    d. Nell'elenco **Ruolo**, selezionare **Utente**.

    e. Fare clic su **Avanti**.

7. Nella pagina **Ottieni password temporanea** fare clic su **crea**.

	![Creazione di un utente test di Azure AD](./media/active-directory-saas-beeline-tutorial/create_aaduser_07.png) 

8. Nella pagina **Ottieni password temporanea** seguire questa procedura:

	![Creazione di un utente test di Azure AD](./media/active-directory-saas-beeline-tutorial/create_aaduser_08.png) 

    a. Prendere nota del valore visualizzato in **Nuova password**.

    b. Fare clic su **Complete**.



### Creazione di un utente di test di Beeline

In questa sezione viene creato un utente di nome Britta Simon in Beeline. L'applicazione Beeline richiede che venga eseguito il provisioning di tutti gli utenti all'interno dell'applicazione prima di eseguire l'accesso Single Sign-On. Rivolgersi al supporto tecnico di Beeline per eseguire il provisioning di tutti gli utenti nell'applicazione.


> [AZURE.NOTE] Nel caso si debba creare un utente manualmente o creare un batch di utenti, è necessario contattare il team di supporto di Beeline.


### Assegnazione dell'utente test di Azure AD

In questa sezione Britta Simon viene abilitata per l'uso dell'accesso Single Sign-On di Azure concedendole l'accesso a Beeline.

![Assegna utente][200]

**Per assegnare Britta Simon a Beeline, seguire questa procedura:**

1. Per aprire la visualizzazione delle applicazioni nel portale classico, nella visualizzazione Directory fare clic su **Applicazioni** nel menu in alto.

	![Assegna utente][201]

2. Nell'elenco delle applicazioni selezionare **Beeline**.

	![Configura accesso Single Sign-On](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_50.png)

1. Scegliere **Utenti** dal menu in alto.

	![Assegna utente][203] 

1. Nell'elenco di utenti selezionare **Britta Simon**.

2. Fare clic su **Assegna** sulla barra degli strumenti in basso.

	![Assegna utente][205] 


### Test dell'accesso Single Sign-On

In questa sezione viene testata la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso. Quando si fa clic sul riquadro Beeline nel pannello di accesso, si dovrebbe accedere automaticamente all'applicazione Beeline.


## Risorse aggiuntive

* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](active-directory-appssoaccess-whatis.md)



<!--Image references--> 

[1]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_205.png

<!---HONumber=AcomDC_0921_2016-->