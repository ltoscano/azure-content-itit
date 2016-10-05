<properties
	pageTitle="Esercitazione: Integrazione di Azure Active Directory con Allocadia | Microsoft Azure"
	description="Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory e Allocadia."
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


# Esercitazione: Integrazione di Azure Active Directory con Allocadia

Questa esercitazione descrive come integrare Allocadia con Azure Active Directory (Azure AD).

L'integrazione di Allocadia con Azure AD offre i vantaggi seguenti:

- È possibile controllare in Azure AD chi può accedere ad Allocadia
- È possibile abilitare gli utenti per l'accesso automatico ad Allocadia (Single Sign-On) con i propri account Azure AD
- È possibile gestire gli account da una posizione centrale: il portale di Azure classico

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](active-directory-appssoaccess-whatis.md).

## Prerequisiti

Per configurare l'integrazione di Azure AD con Allocadia, sono necessari gli elementi seguenti:

- Sottoscrizione di Azure AD.
- Sottoscrizione di Allocadia abilitata per l'accesso Single Sign-On.


> [AZURE.NOTE] Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione.


A questo scopo, è consigliabile seguire le indicazioni seguenti:

- Non usare l'ambiente di produzione, a meno che non sia necessario.
- Se non è disponibile un ambiente di prova di Azure AD, è possibile ottenere una versione di prova di un mese [qui](https://azure.microsoft.com/pricing/free-trial/).


## Descrizione dello scenario
In questa esercitazione viene eseguito il test dell'accesso Single Sign-On di Azure AD in un ambiente di test. Lo scenario descritto in questa esercitazione prevede i due blocchi predefiniti seguenti:

1. Aggiunta di Allocadia dalla raccolta
2. Configurazione e test dell'accesso Single Sign-On di Azure AD


## Aggiunta di Allocadia dalla raccolta
Per configurare l'integrazione di Allocadia in Azure AD, è necessario aggiungere Allocadia dalla raccolta al proprio elenco di app SaaS gestite.

**Per aggiungere Allocadia dalla raccolta, seguire questa procedura:**

1. Nel **portale di Azure classico** fare clic su **Active Directory** nel riquadro di spostamento sinistro.

	![Active Directory][1] 

2. Nell'elenco **Directory** selezionare la directory per la quale si desidera abilitare l'integrazione delle directory.

3. Per aprire la visualizzazione applicazioni, nella visualizzazione directory fare clic su **Applications** nel menu superiore.

	![Applicazioni][2] 

4. Fare clic su **Add** nella parte inferiore della pagina.

	![Applicazioni][3] 

5. Nella finestra di dialogo **Come procedere** fare clic su **Aggiungere un'applicazione dalla raccolta**.

	![Applicazioni][4]

6. Nella casella di ricerca digitare** Allocadia**.

	![Creazione di un utente test di Azure AD](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_01.png)

7. Nel riquadro dei risultati selezionare **Allocadia** e quindi fare clic su **Completa** per aggiungere l'applicazione.

	![Creazione di un utente test di Azure AD](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_06.png)

##  Configurazione e test dell'accesso Single Sign-On di Azure AD
In questa sezione viene configurato e testato l'accesso Single Sign-On di Azure AD con Allocadia usando un utente test di nome "Britta Simon".

Per il funzionamento dell'accesso Single Sign-On, Azure AD deve conoscere l'utente controparte di Allocadia che corrisponde a un utente di Azure AD. In altre parole deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in Allocadia. La relazione di collegamento viene stabilita assegnando lo stesso valore al **nome utente** in Azure AD e a **Username** in Allocadia.

Per configurare e testare l'accesso Single Sign-On di Azure AD con Allocadia, è necessario completare i blocchi predefiniti seguenti:

1. **[Configurazione dell'accesso Single Sign-On di Azure AD](#configuring-azure-ad-single-single-sign-on)**: per abilitare gli utenti all'uso di questa funzionalità.
2. **[Creazione di un utente test di Azure AD](#creating-an-azure-ad-test-user)**: per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
4. **[Creazione di un utente di test di Allocadia](#creating-an-allocadia-test-user)**: per avere una controparte di Britta Simon in Allocadia collegata alla relativa rappresentazione in Azure AD.
5. **[Assegnazione dell'utente test di Azure AD](#assigning-the-azure-ad-test-user)**: per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
5. **[Test dell'accesso Single Sign-On](#testing-single-sign-on)**: per verificare se la configurazione funziona.

### Configurazione dell'accesso Single Sign-On di Azure AD

In questa sezione viene abilitato l'accesso Single Sign-On di Azure AD nel portale classico e viene configurato l'accesso Single Sign-On nell'applicazione Allocadia.


L'applicazione Allocadia si aspetta che le asserzioni SAML abbiano un formato specifico. Configurare le attestazioni seguenti per questa applicazione. È possibile gestire i valori di questi attributi dalla scheda **Attribute (Attributo)** dell'applicazione. La schermata seguente illustra un esempio relativo a questa operazione.

![Configura accesso Single Sign-On](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_07.png)

**Per configurare l'accesso Single Sign-On di Azure AD con Hightail, seguire questa procedura:**


1. Nel menu visualizzato nella parte superiore della pagina di integrazione dell'applicazione **Allocadia** del portale di Azure classico fare clic su **Attributi**.

	![Configura accesso Single Sign-On](./media/active-directory-saas-allocadia-tutorial/tutorial_general_80.png)


2. Nella finestra di dialogo **Attributi token SAML** seguire questa procedura per ciascuna riga della tabella:

	| Nome attributo | Valore attributo |
	| --- | --- |    
	| firstname | user.givenname |
    | lastname | user.surname |
	| email | user.mail |
	

	a. Fare clic su **aggiungi attributo utente** per aprire la finestra di dialogo **Aggiungi attributo a utente**.

	![Configura accesso Single Sign-On](./media/active-directory-saas-allocadia-tutorial/tutorial_general_81.png)


	b. Nella casella di testo **Nome attributo** digitare il nome dell'attributo indicato per quella riga.

    c. Nell'elenco **Valore attributo** selezionare il valore dell'attributo indicato per quella riga.

    d. Fare clic su **Complete**.
	

3. Nel menu in alto fare clic su **Avvio rapido**.

	![Configura accesso Single Sign-On](./media/active-directory-saas-allocadia-tutorial/tutorial_general_83.png)


4. Nella pagina **Stabilire come si desidera che gli utenti accedano ad Allocadia** selezionare **Single Sign-On di Azure AD** e quindi fare clic su **Avanti**.
 	
	![Configura accesso Single Sign-On](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_03.png)

5. Nella pagina **Configurare le impostazioni dell'app** seguire questa procedura:

	![Configura accesso Single Sign-On](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_04.png) 

	a. Nella casella IDENTIFICATORE digitare l'URL nel formato seguente: per l'ambiente di test usare un URL come **"https://na2standby.allocadia.com"** e per l'ambiente di produzione usare **"https://na2.allocadia.com"**

	b. Nella casella URL di risposta digitare l'URL nel formato seguente: per l'ambiente di test usare un modello di URL come **"https://na2standby.allocadia.com/allocadia/saml/SSO"** e per l'ambiente di produzione usare **"https://na2.allocadia.com/allocadia/saml/SSO"**


6. Nella pagina **Configura accesso Single Sign-On in Allocadia** seguire questa procedura:

	![Configura accesso Single Sign-On](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_05.png)

    a. Fare clic su **Scarica metadati** e quindi salvare il file nel computer.

    b. Fare clic su **Next**.


7.  Per la procedura di configurazione dell'accesso SSO per l'applicazione, contattare il team di [supporto di Allocadia](mailTo:support@allocadia.com) che fornirà l'aiuto richiesto. Si noti che è necessario inviare un messaggio di posta elettronica e allegare il file dei metadati scaricato per configurare l'accesso SSO sul lato Allocadia.
 
	> [AZURE.NOTE] Accertarsi che il team di Allocadia imposti il valore Identificatore nell'ambiente di test come **"https://na2standby.allocadia.com"** mentre per l'ambiente di produzione deve essere **"https://na2.allocadia.com"**


8. Nel portale classico selezionare la conferma della configurazione dell'accesso Single Sign-On e quindi fare clic su **Avanti**.
	
	![Accesso Single Sign-On di Azure AD][10] 

9. Nella pagina **Conferma Single Sign-on** fare clic su **Completa**.
  	
	![Accesso Single Sign-On di Azure AD][11] 



### Creazione di un utente test di Azure AD

In questa sezione viene creato un utente test chiamato Britta Simon nel portale classico. Nell'elenco di utenti selezionare **Britta Simon**.

![Creare un utente di Azure AD][20]

**Per creare un utente test in Azure AD, eseguire la procedura seguente:**

1. Nel **portale di Azure classico** fare clic su **Active Directory** nel riquadro di spostamento sinistro.
	
	![Creazione di un utente test di Azure AD](./media/active-directory-saas-allocadia-tutorial/create_aaduser_09.png) 

2. Nell'elenco **Directory** selezionare la directory per la quale si desidera abilitare l'integrazione delle directory.

3. Per visualizzare l'elenco di utenti, fare clic su **Utenti** nel menu in alto.
	
	![Creazione di un utente test di Azure AD](./media/active-directory-saas-allocadia-tutorial/create_aaduser_03.png) 

4. Per aprire la finestra di dialogo **Aggiungi utente**, fare clic su **Aggiungi utente** nella barra degli strumenti in basso.

	![Creazione di un utente test di Azure AD](./media/active-directory-saas-allocadia-tutorial/create_aaduser_04.png) 

5. Nella pagina della finestra di dialogo **Informazioni sull'utente** seguire questa procedura:
 
	![Creazione di un utente test di Azure AD](./media/active-directory-saas-allocadia-tutorial/create_aaduser_05.png)

    a. In Tipo di utente selezionare Nuovo utente nell'organizzazione.

    b. Nella casella di testo **Nome utente** digitare **BrittaSimon**.

    c. Fare clic su **Next**.

6.  Nella pagina della finestra di dialogo **Profilo utente** seguire questa procedura:

	![Creazione di un utente test di Azure AD](./media/active-directory-saas-allocadia-tutorial/create_aaduser_06.png) 

    a. Nella casella di testo **Nome** digitare **Britta**.

    b. Nella casella di testo **Cognome** digitare **Simon**.

    c. Nella casella di testo **Nome visualizzato** digitare **Britta Simon**.

    d. Nell'elenco **Ruolo**, selezionare **Utente**.

    e. Fare clic su **Avanti**.

7. Nella pagina **Ottieni password temporanea** fare clic su **crea**.

	![Creazione di un utente test di Azure AD](./media/active-directory-saas-allocadia-tutorial/create_aaduser_07.png) 

8. Nella pagina **Ottieni password temporanea** seguire questa procedura:

	![Creazione di un utente test di Azure AD](./media/active-directory-saas-allocadia-tutorial/create_aaduser_08.png) 

    a. Prendere nota del valore visualizzato in **Nuova password**.

    b. Fare clic su **Complete**.



### Creazione di un utente di test di Allocadia

In questa sezione viene creato un utente chiamato Britta Simon in Allocadia. L'applicazione Allocadia supporta il provisioning degli utenti just-in-time. Se le attestazioni sono state configurate come indicato nella precedente sezione **[Configurazione dell'accesso Single Sign-On di Azure AD](#configuring-azure-ad-single-single-sign-on)**, si otterrà il provisioning degli utenti nell'applicazione.


> [AZURE.NOTE] Nel caso si debba creare un utente manualmente o creare un batch di utenti, è necessario contattare il team di supporto di Allocadia.


### Assegnazione dell'utente test di Azure AD

In questa sezione Britta Simon viene abilitata per l'uso dell'accesso Single Sign-On di Azure concedendole l'accesso ad Allocadia.

![Assegna utente][200]

**Per assegnare Britta Simon ad Allocadia, seguire questa procedura:**

1. Per aprire la visualizzazione delle applicazioni nel portale classico, nella visualizzazione Directory fare clic su **Applicazioni** nel menu in alto.

	![Assegna utente][201]

2. Nell'elenco delle applicazioni selezionare **Allocadia**.

	![Configura accesso Single Sign-On](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_50.png)

1. Scegliere **Utenti** dal menu in alto.

	![Assegna utente][203] 

1. Nell'elenco di utenti selezionare **Britta Simon**.

2. Fare clic su **Assegna** sulla barra degli strumenti in basso.

	![Assegna utente][205] 


### Test dell'accesso Single Sign-On

In questa sezione viene testata la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso. Quando si fa clic sul riquadro Allocadia nel pannello di accesso, si dovrebbe accedere automaticamente all'applicazione Allocadia.


## Risorse aggiuntive

* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](active-directory-appssoaccess-whatis.md)



<!--Image references--> 

[1]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_205.png

<!---HONumber=AcomDC_0921_2016-->