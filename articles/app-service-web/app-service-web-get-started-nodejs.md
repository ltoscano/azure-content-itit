<properties
	pageTitle="Distribuire un'app Web Node.js in Azure in cinque minuti | Microsoft Azure"
	description="Informazioni su come eseguire facilmente app Web nel servizio app mediante la distribuzione di un'app di esempio. È possibile eseguire rapidamente vere e proprie attività di sviluppo con risultati immediati."
	services="app-service\web"
	documentationCenter=""
	authors="cephalin"
	manager="wpickett"
	editor=""
/>

<tags
	ms.service="app-service-web"
	ms.workload="web"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="hero-article"
	ms.date="09/16/2016"
	ms.author="cephalin"
/>

# Distribuire la prima app Web Node.js in Azure in cinque minuti

Questa esercitazione illustra come distribuire la prima app Web Node.js nel [servizio app di Azure](../app-service/app-service-value-prop-what-is.md). Il servizio app consente di creare app Web, [back-end di app per dispositivi mobili](/documentation/learning-paths/appservice-mobileapps/) e [app per le API](../app-service-api/app-service-api-apps-why-best-platform.md).

Si apprenderà come:

- Creare un'app Web nel servizio app di Azure.
- Distribuire codice Node.js di esempio.
- Osservare il codice in esecuzione nell'ambiente di produzione.
- Aggiornare l'app Web nello stesso modo in cui si effettua il [push dei commit Git](https://git-scm.com/docs/git-push).

## Prerequisiti

- [Installare Git](http://www.git-scm.com/downloads). Verificare l'esito positivo dell'installazione eseguendo `git --version` da un nuovo prompt dei comandi di Windows, una finestra di PowerShell, una shell di Linux o un terminale OS X.
- Ottenere un account Microsoft Azure. Se non si ha un account, è possibile [iscriversi per ottenere una versione di valutazione gratuita](/pricing/free-trial/?WT.mc_id=A261C142F) oppure [attivare i vantaggi della sottoscrizione di Visual Studio](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

>[AZURE.NOTE] È possibile [provare il servizio app](http://go.microsoft.com/fwlink/?LinkId=523751) senza avere un account Azure. Creare un'app iniziale e provarla per un'ora, senza impegno e senza dover usare la carta di credito.

<a name="create"></a>
## Creare un'app Web

1. Accedere al [portale di Azure](https://portal.azure.com) con il proprio account Azure.

2. Nel menu a sinistra fare clic su **Nuovo** > **Web e dispositivi mobili** > **App Web**.

    ![](./media/app-service-web-get-started-languages/create-web-app-portal.png)

3. Nel pannello di creazione dell'app usare le impostazioni seguenti per la nuova app:

    - **Nome dell'app**: digitare un nome univoco.
    - **Gruppo di risorse**: selezionare **Crea nuovo** e assegnare un nome al gruppo di risorse.
    - **Piano di servizio app/Località**: fare clic per configurare e quindi scegliere **Crea nuovo** per impostare il nome, la località e il piano tariffario del piano di servizio app. È possibile usare il piano tariffario **Gratuito**.

    Al termine, il pannello di creazione dell'app dovrebbe avere un aspetto simile al seguente:

    ![](./media/app-service-web-get-started-languages/create-web-app-settings.png)

3. Fare clic su **Crea** nella parte inferiore della schermata. È possibile fare clic sull'icona delle **notifiche** in alto per visualizzare lo stato di avanzamento.

    ![](./media/app-service-web-get-started-languages/create-web-app-started.png)

4. Al termine della distribuzione dovrebbe essere visualizzato il messaggio di notifica seguente. Fare clic sul messaggio per aprire il pannello della distribuzione.

    ![](./media/app-service-web-get-started-languages/create-web-app-finished.png)

5. Nel pannello **La distribuzione è riuscita** fare clic sul collegamento **Risorsa** per aprire il pannello della nuova app Web.

    ![](./media/app-service-web-get-started-languages/create-web-app-resource.png)

## Distribuire codice nell'app Web

Verrà ora illustrato come distribuire codice in Azure con Git.

5. Nel pannello dell'app Web scorrere verso il basso fino alla voce **Opzioni di distribuzione** oppure cercarla e quindi fare clic su di essa.

    ![](./media/app-service-web-get-started-languages/deploy-web-app-deployment-options.png)

6. Fare clic su **Scegliere l'origine** > **Repository Git locale** > **OK**.

7. Tornare al pannello dell'app Web e fare clic su **Credenziali per la distribuzione**.

8. Impostare le credenziali per la distribuzione e fare clic su **Salva**.

7. Tornare al pannello dell'app Web e scorrere verso il basso fino alla voce **Proprietà** oppure cercarla e quindi fare clic su di essa. Fare clic sul pulsante **Copia** accanto a **URL GIT**.

    ![](./media/app-service-web-get-started-languages/deploy-web-app-properties.png)

    A questo punto è possibile distribuire il codice con Git.

1. Nel terminale della riga di comando passare a una directory di lavoro (`CD`) e clonare l'app di esempio come segue:

        git clone https://github.com/Azure-Samples/app-service-web-nodejs-get-started.git

    ![Clonare il codice dell'app di esempio per la prima app Web in Azure](./media/app-service-web-get-started-languages/node-git-clone.png)

    Per *&lt;github\_sample\_url>* usare uno degli URL seguenti, a seconda del framework preferito.

2. Passare al repository dell'app di esempio. Ad esempio:

        cd app-service-web-nodejs-get-started

3. Configurare l'URL GIT copiato dal portale in uno dei passaggi precedenti come Git remoto per l'app Azure.

        git remote add azure <giturlfromportal>

4. Distribuire il codice di esempio nell'app Azure nello stesso modo in cui si effettua il push di qualsiasi codice con Git:

        git push azure master

    ![Effettuare il push di codice nella prima app Web in Azure](./media/app-service-web-get-started-languages/node-git-push.png)

    Se è stato usato uno dei framework di linguaggio, verrà visualizzato un output diverso. Ciò dipende dal fatto che `git push` non solo inserisce il codice in Azure, ma attiva anche le attività di distribuzione nel motore di distribuzione. Se nella radice del progetto (repository) sono presenti file package.json, lo script di distribuzione ripristina automaticamente i pacchetti necessari.

La procedura è terminata. A questo punto il codice è in esecuzione in Azure. Nel browser passare a http://*&lt;appname>*.azurewebsites.net per verificarne il funzionamento.

## Eseguire aggiornamenti dell'app

Ora è possibile usare Git per effettuare in qualsiasi momento il push dalla radice del progetto (repository) per eseguire un aggiornamento del sito live. La procedura è simile a quella usata per la prima distribuzione del codice. Quando si vuole effettuare il push di una nuova modifica testata in locale, ad esempio, è sufficiente eseguire i comandi seguenti dalla radice del progetto (repository):

    git add .
    git commit -m "<your_message>"
    git push azure master

## Passaggi successivi

[Creare, configurare e distribuire un'app Web Node.js Express in Azure](app-service-web-nodejs-get-started.md). Seguendo questa esercitazione è possibile acquisire le competenze di base necessarie per eseguire qualsiasi app Web Node.js in Azure, ad esempio:

- Creare e configurare app in Azure da PowerShell/Bash.
- Impostare la versione di Node.js.
- Usare un file di avvio che non si trova nella directory dell'applicazione radice.
- Automatizzare con NPM.
- Ottenere log di output e degli errori.

In alternativa, è possibile fare altre prove con la prima app Web, ad esempio:

- Provare [altri modi per distribuire il codice in Azure](../app-service-web/web-sites-deploy.md). Per eseguire la distribuzione da un repository GitHub, ad esempio, è sufficiente selezionare **GitHub** anziché **Repository Git locale** in **Opzioni di distribuzione**.
- Ottimizzare l'app Azure: autenticare gli utenti, ridimensionare l'app in base alla richiesta e configurare alcuni avvisi sulle prestazioni, tutto con pochi clic. Vedere [Aggiungere funzionalità alla prima app Web](app-service-web-get-started-2.md).

<!----HONumber=AcomDC_0920_2016-->
