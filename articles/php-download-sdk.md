<properties
	pageTitle="Scaricare Azure SDK per PHP"
	description="Informazioni su come scaricare e installare Azure SDK per PHP."
	documentationCenter="php"
	services="app-service\web"
	authors="allclark"
	manager="douge"
	editor=""/>

<tags
	ms.service="app-service-web"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="PHP"
	ms.topic="article"
	ms.date="06/01/2016"
	ms.author="allclark;yaqiyang"/>

#Download di Azure SDK per PHP

## Panoramica

Azure SDK per PHP include componenti che consentono di sviluppare, distribuire e gestire applicazioni PHP per Azure. In particolare, Azure SDK per PHP include i seguenti componenti:

* **Librerie client PHP per Azure**. Queste librerie di classi offrono un'interfaccia per accedere alle funzionalità di Azure, ad esempio i servizi di gestione dati e i servizi cloud.  
* **Interfaccia della riga di comando di Azure per Mac, Linux e Windows** Un insieme di comandi che permette la distribuzione e la gestione di servizi di Azure, come Siti Web e Macchine virtuali di Azure. L’interfaccia della riga di comando di Azure funziona su qualsiasi piattaforma, incluse le piattaforme Mac, Linux e Windows.
* **Azure PowerShell (solo Windows)**. Questo insieme di cmdlet di PowerShell consente la distribuzione e la gestione di servizi di Azure, come servizi cloud e macchine virtuali.
* **Emulatori di Azure (solo Windows)**. Gli emulatori di calcolo e archiviazione sono emulatori locali di servizi cloud e di gestione dati che consentono di testare un'applicazione in locale. Gli emulatori di Azure funzionano solo su Windows.

Le sezioni seguenti illustrano come eseguire il download e l'installazione dei componenti sopra descritti.

Le istruzioni fornite in questo argomento presuppongono che [PHP][install-php] sia installato nel computer in uso.

> [AZURE.NOTE] È necessario avere PHP 5.5 o versione successiva per usare le librerie client PHP per Azure.

##Librerie client PHP per Azure

Le librerie client PHP per Azure offrono un'interfaccia per accedere alle funzionalità di Azure, ad esempio i servizi di gestione dati e i servizi cloud, da qualsiasi sistema operativo. Queste librerie possono essere installate tramite il programma di creazione.

Per informazioni su come usare le librerie client PHP per Azure, vedere [Come usare il servizio BLOB][blob-service], [Come usare il servizio tabelle][table-service] e [Come usare il servizio di accodamento][queue-service].

###Installazione tramite Composer

1. [Installare Git][install-git].


	> [AZURE.NOTE] In Windows sarà inoltre necessario aggiungere l'eseguibile Git alla variabile di ambiente PATH.

2. Creare un file denominato **composer.json** nella radice del progetto, quindi aggiungere nel file il codice seguente:

        {
			"require": {
				"microsoft/windowsazure": "^0.4"
			}
        }

3. Scaricare **[composer.phar][composer-phar]** nella radice del progetto.

4. Aprire un prompt dei comandi ed eseguire quanto segue nella radice del progetto

		php composer.phar install

##Azure PowerShell ed emulatori di Azure

Azure PowerShell è un insieme di cmdlet di PowerShell per la distribuzione e la gestione di servizi di Azure, come servizi cloud e macchine virtuali. Gli emulatori di Azure sono emulatori di servizi cloud e di gestione dati che consentono di verificare un'applicazione in locale. Questi componenti sono supportati solo in Windows.

Il metodo consigliato per installare Azure PowerShell e gli emulatori di Azure è tramite l'[Installazione guidata piattaforma Web Microsoft][download-wpi]. Si noti che è inoltre possibile scegliere di installare altri componenti di sviluppo, come PHP, SQL Server, i driver Microsoft per SQL Server per PHP e WebMatrix.

Per informazioni sull'uso di Azure PowerShell, vedere [Come usare Azure PowerShell][powershell-tools].

##Interfaccia della riga di comando di Azure

Un’interfaccia della riga di comando di Azure è un insieme di comandi per la distribuzione e la gestione di servizi di Azure, come Siti Web e Macchine virtuali. Per informazioni sull'installazione di Azure CLI, vedere [Installare Azure CLI](xplat-cli-install.md).

## Passaggi successivi

Per ulteriori informazioni, vedere il [Centro per sviluppatori di PHP](/develop/php/).


[install-php]: http://www.php.net/manual/en/install.php
[composer-github]: https://github.com/composer/composer
[composer-phar]: http://getcomposer.org/composer.phar
[nodejs-org]: http://nodejs.org/
[install-node-linux]: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager
[download-wpi]: http://go.microsoft.com/fwlink/?LinkId=253447
[mac-installer]: http://go.microsoft.com/fwlink/?LinkId=252249
[blob-service]: http://go.microsoft.com/fwlink/?LinkId=252714
[table-service]: http://go.microsoft.com/fwlink/?LinkId=252715
[queue-service]: http://go.microsoft.com/fwlink/?LinkId=252716
[azure cli]: http://go.microsoft.com/fwlink/?LinkId=252717
[powershell-tools]: http://go.microsoft.com/fwlink/?LinkId=252718
[php-sdk-github]: http://go.microsoft.com/fwlink/?LinkId=252719
[install-git]: http://git-scm.com/book/en/Getting-Started-Installing-Git

<!---HONumber=AcomDC_0601_2016-->