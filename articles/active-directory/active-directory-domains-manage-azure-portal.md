<properties
	pageTitle="Gestione dei nomi di dominio personalizzati in anteprima di Azure Active Directory | Microsoft Azure"
	description="Concetti relativi alla gestione e procedure dettagliate per gestire un nome di dominio in Azure Active Directory"
	services="active-directory"
	documentationCenter=""
	authors="jeffsta"
	manager="femila"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/12/2016"
	ms.author="curtand;jeffsta"/>

# Gestione dei nomi di dominio personalizzati in anteprima di Azure Active Directory

Un nome di dominio è una parte importante dell'identificatore per numerose risorse directory: è parte di un nome utente o di un indirizzo di posta elettronica per un utente, parte dell'indirizzo per un gruppo e può essere parte dell'URI dell'ID app per un'applicazione. Una risorsa in anteprima di Azure Active Directory (Azure AD) può includere un nome di dominio già verificato come di proprietà della directory contenente il servizio. [Funzionalità disponibili nell'anteprima](active-directory-preview-explainer.md) Solo un amministratore globale può eseguire attività di gestione del dominio in Azure AD.

## Impostare il nome di dominio primario per la directory di Azure AD

Quando viene creata la directory, il nome di dominio iniziale, ad esempio "contoso.onmicrosoft.com", è anche il nome di dominio primario. Il dominio primario è il nome di dominio predefinito per un nuovo utente quando si crea un nuovo utente. Questo contribuisce a semplificare il processo di creazione dei nuovi utenti per gli amministratori. Per cambiare il nome di dominio primario:

1.  Accedere al [portale di Azure](https://portal.azure.com) con un account di amministratore globale per la directory.

2.  Selezionare **Altri servizi**, immettere **Azure Active Directory** nella casella di testo e quindi selezionare **Invio**.

    ![Apertura di Gestione utenti](./media/active-directory-domains-add-azure-portal/user-management.png)

3. Nel pannello ***nome directory*** selezionare **Nomi di dominio**.

4. Nel pannello ***Nome directory* - Nomi di dominio** selezionare il nome di dominio che si vuole rendere il nome primario.

5.  Nel pannello ***nomedominio***, vale a dire il pannello visualizzato con il nuovo nome di dominio nel titolo, selezionare **Make primary** (Rendi primario). Confermare la scelta quando viene richiesto.

    ![Rendere primario un nome di dominio](./media/active-directory-domains-manage-azure-portal/make-primary.png)

Per il nome di dominio primario per la directory è possibile impostare qualsiasi dominio personalizzato verificato che non sia federato. La modifica del dominio primario per la directory non comporta la modifica dei nomi utente per gli utenti esistenti.

## Aggiungere nomi di dominio personalizzati alla directory di Azure AD

È possibile aggiungere fino a 900 nomi di dominio personalizzati a ogni directory di Azure AD. Per [aggiungere altri nomi di dominio personalizzati](active-directory-domains-add-azure-portal.md) è sufficiente seguire la stessa procedura usata per aggiungere il primo nome di dominio personalizzato.

## Aggiungere sottodomini di un dominio personalizzato

Per aggiungere un nome di dominio di terzo livello, ad esempio 'europe.contoso.com' alla directory, è prima necessario aggiungere e verificare il dominio di secondo livello, ovvero contoso.com. Il sottodominio verrà automaticamente verificato da Azure AD. Per ottenere la conferma dell'avvenuta verifica del dominio appena aggiunto, aggiornare la pagina del browser in cui sono visualizzati i domini.

## Cosa fare se si modifica il registrar DNS per il nome di dominio personalizzato

Se si modifica il registrar DNS per il nome di dominio personalizzato, è possibile continuare a usare il nome di dominio personalizzato in Azure AD senza interruzioni e senza ulteriori attività di configurazione. Se si usa il nome di dominio personalizzato con Office 365, Intune o altri servizi che si basano sui nomi di dominio personalizzati in Azure AD, vedere la documentazione per tali servizi.

## Eliminare un nome di dominio personalizzato

È possibile eliminare da Azure AD i nomi di dominio personalizzati che l'organizzazione non usa più o che vuole usare in un'altra directory di Azure AD.

Per eliminare un nome di dominio personalizzato, è prima necessario assicurarsi che nella directory non siano presenti risorse che si basano sul nome di dominio. Non è possibile eliminare un nome di dominio dalla directory nei casi seguenti:

-   Sono presenti utenti con un nome utente, un indirizzo di posta elettronica o un indirizzo proxy che include il nome di dominio.

-   Sono presenti gruppi con un indirizzo di posta elettronica o un indirizzo proxy che include il nome di dominio.

-   In Azure AD sono presenti applicazioni con un URI di ID app che include il nome di dominio.

È necessario modificare o eliminare tali risorse dalla directory di Azure AD prima di poter eliminare il nome di dominio personalizzato.

## Usare API Graph o PowerShell per gestire i nomi di dominio

La maggior parte delle attività di gestione relative ai nomi di dominio in Azure Active Directory può anche essere eseguita usando Microsoft PowerShell oppure a livello di codice usando l'API Graph (versione di anteprima pubblica).

-   [Uso di PowerShell per gestire i nomi di dominio in Azure AD](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)

-   [Uso dell'API Graph per gestire i nomi di dominio in Azure AD](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

## Passaggi successivi

-   [Aggiungere nomi di dominio personalizzati](active-directory-domains-add-azure-portal.md)

<!---HONumber=AcomDC_0914_2016-->