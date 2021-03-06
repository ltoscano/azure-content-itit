<properties
	pageTitle="Non è possibile accedere per gestire la sottoscrizione di Azure | Microsoft Azure"
	description="Descrive le informazioni di risoluzione dei problemi per alcuni dei problemi di accesso della sottoscrizione di Azure"
	services=""
	documentationCenter=""
	authors="genlin"
	manager="msmbaldwin"
	editor=""
	tags="billing"
	/>

<tags
	ms.service="billing"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/09/2016"
	ms.author="genli"/>

# Non è possibile accedere per gestire la sottoscrizione di Azure

Questo articolo illustra alcuni dei metodi più comuni per risolvere i problemi di accesso.

> [AZURE.NOTE] Se in qualsiasi punto dell'articolo sono necessarie altre informazioni, è possibile [contattare il supporto tecnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) per ottenere una rapida risoluzione del problema.

## Portali di Azure

| Nome | Descrizione | URL |
|---|---|---|
| Portale di Azure |Una posizione centrale in cui è possibile effettuare il provisioning delle risorse di Azure e gestirle | [https://portal.azure.com](https://portal.azure.com) |
| Portale di Azure classico | Il portale di Azure precedente |[https://manage.windowsazure.com](https://manage.windowsazure.com) |
| Centro account di Azure | Una posizione centrale in cui è possibile tenere traccia dell'utilizzo di Azure e gestire la sottoscrizione |[https://account.windowsazure.com/Subscriptions](https://account.windowsazure.com/Subscriptions) ||

## Sintomo: la pagina resta in stato di caricamento

Questo può essere causato da un problema del browser Internet.

Per risolvere il problema, provare i metodi seguenti, nell'ordine indicato. Al termine di ciascun metodo, provare a riconnettersi alla pagina di accesso nel portale.

-	Aggiornare la pagina.
-	Utilizzare un altro browser Internet.
-	Se si utilizza Microsoft Internet Explorer, accedere al portale di Azure con la modalità InPrivate Browsing. A questo scopo, seguire questa procedura:

	A. Fare clic su **Strumenti** ![pulsante strumenti](./media/billing-cannot-login-subscription/Toolsbutton.png) > **Sicurezza** > **InPrivate Browsing**.

	B. Andare al [portale di Azure](https://portal.azure.com) o al [portale di Azure classico](https://manage.windowsazure.com), quindi effettuare l'accesso al portale.

## Sintomo: messaggio di errore "Nessuna sottoscrizione trovata"

Questo problema può verificarsi se l'account non ha diritti utente sufficienti. Un amministratore account può accedere al [Centro Account](https://account.windowsazure.com/) solo se gli amministratori del servizio (SA) e i co-amministratori (CA) hanno accesso solo al [portale di Azure](https://portal.azure.com) o al [portale di Azure classico](https://manage.windowsazure.com/).

**Scenario 1: il messaggio di errore appare nel portale di Azure o nel portale di Azure classico**

Per risolvere il problema, [aggiungere il ruolo di co-amministratore o proprietario](billing-add-change-azure-subscription-administrator.md) dell'account.

**Scenario 2: il messaggio di errore appare nel centro Account di Azure**

Controllare se l'account utilizzato è l'amministratore account. Per verificare chi è l'amministratore dell'account, attenersi alla procedura seguente:

1.	Accedere al [portale di Azure](https://portal.azure.com).
2.	Nel menu Hub, selezionare **Sottoscrizione**.
3.	Scegliere la sottoscrizione da controllare, quindi selezionare **Impostazioni**.
4.	Selezionare **Proprietà**. L'amministratore account della sottoscrizione viene visualizzato nella casella **Amministratore account**.

## Sintomo: è stato eseguito l'accesso automatico come un altro utente

Questo problema può verificarsi se si utilizza più di un account utente in un browser Internet.

Per risolvere il problema, utilizzare uno dei seguenti metodi alternativi:

- Disconnettersi dal portale e accedere nuovamente con l'account che si desidera utilizzare.
-	Svuotare la cache ed eliminare i cookie di Internet. Per farlo in Internet Explorer, fare clic su **Strumenti** ![pulsante strumenti](./media/billing-cannot-login-subscription/Toolsbutton.png) > **Opzioni Internet** > **Elimina**, verificare di aver selezionato siano le caselle di controllo per file temporanei, cookie, password e cronologia esplorazioni, quindi fare clic su Elimina.

-	Riconfigurare le impostazioni di Internet Explorer per ripristinare le impostazioni personali definite. A tale scopo, fare clic su **Strumenti** ![pulsante strumenti](./media/billing-cannot-login-subscription/Toolsbutton.png)> **Opzioni Internet** > **Avanzate** > selezionare la casella di controllo **Elimina impostazioni personali** > **Ripristina**.

-	Accedere al portale di Azure in modalità InPrivate Browsing. Per farlo, fare clic su **Strumenti** ![pulsante strumenti](./media/billing-cannot-login-subscription/Toolsbutton.png) > **Sicurezza** > **InPrivate Browsing**.

## Account Microsoft vs account dell'organizzazione

L’account Microsoft è l'indirizzo di posta elettronica che si utilizza, insieme alla password, per accedere a qualsiasi programma o servizio Windows Live, ad esempio Outlook, Hotmail, MSN o OneDrive. È possibile impostare un account Microsoft usando qualsiasi indirizzo di posta elettronica appartenente all’utente, compresa la posta elettronica aziendale. Vedere [www.microsoft.com/account](http://www.microsoft.com/account) per ulteriori dettagli.

Se l'account è associato a un account aziendale, selezionare l'opzione di accesso corretta come illustrato di seguito. Per altre informazioni sull'uso dell’account aziendale, vedere [Iscrizione ad Azure come organizzazione](./active-directory/sign-up-organization.md):

![pagina di accesso](./media/billing-cannot-login-subscription/signin.png)

> [AZURE.NOTE] Per altre domande, è possibile [contattare il supporto tecnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) per ottenere una rapida risoluzione del problema.

<!---HONumber=AcomDC_0914_2016-->