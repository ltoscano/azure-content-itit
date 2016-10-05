<properties 
	pageTitle="Riproduzione di contenuti | Microsoft Azure" 
	description="Questo argomento elenca i lettori esistenti che è possibile usare per la riproduzione dei propri contenuti." 
	services="media-services" 
	documentationCenter="" 
	authors="Juliako" 
	manager="erikre" 
	editor=""/> 

<tags 
	ms.service="media-services" 
	ms.workload="media" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="09/19/2016" 
	ms.author="juliako"/> 


#Riproduzione di contenuti con i lettori esistenti

Servizi multimediali di Azure supporta molti formati di streaming noti, ad esempio Smooth Streaming, HTTP Live Streaming (HLS) e MPEG-DASH. Questo argomento descrive i lettori esistenti che possono essere usati per testare i propri flussi.

>[AZURE.NOTE]Per riprodurre contenuti organizzati in pacchetti dinamici o con crittografia dinamica, assicurarsi di disporre di almeno un'unità di streaming per l'endpoint di streaming da cui si prevede di inviare il contenuto. Per informazioni sul ridimensionamento delle unità di streaming, vedere la sezione relativa al [ridimensionamento di unità di streaming](media-services-portal-manage-streaming-endpoints.md)

###Lettore di contenuti di Servizi multimediali del portale di Azure classico

Il portale di **Azure** fornisce un lettore di contenuti che può essere usato per testare il proprio video.

Fare clic sul video desiderato (assicurarsi che sia stato [pubblicato](media-services-portal-publish.md)) e fare clic sul pulsante **Play** nella parte inferiore del portale.

Considerazioni applicabili:

- Il **LETTORE DI CONTENUTI DI SERVIZI MULTIMEDIALI** esegue la riproduzione dall'endpoint di streaming predefinito. Se si vuole eseguire la riproduzione da un endpoint di streaming diverso, usare un altro lettore, ad esempio [Lettore multimediale di Azure](http://amsplayer.azurewebsites.net/azuremediaplayer.html).


![AMSPlayer][AMSPlayer]

###Azure Media Player

Usare il [lettore multimediale di Azure](http://amsplayer.azurewebsites.net/azuremediaplayer.html) per riprodurre i contenuti (in chiaro o protetti) in uno dei seguenti formati:

- Smooth Streaming
- MPEG DASH
- HLS
- MP4 progressivo


###Flash Player

####Con crittografia AES con token

[http://aestoken.azurewebsites.net](http://aestoken.azurewebsites.net)

###Lettori Silverlight

####Monitoraggio

[http://smf.cloudapp.net/healthmonitor](http://smf.cloudapp.net/healthmonitor)

####PlayReady con token

[http://sltoken.azurewebsites.net](http://sltoken.azurewebsites.net)

### Lettori DASH

[http://dashplayer.azurewebsites.net](http://dashplayer.azurewebsites.net)

[http://dashif.org](http://dashif.org)

###Altri

Per testare gli URL HLS è inoltre possibile usare:

- **Safari** in un dispositivo iOS o
- **Lettore HLS 3ivx** in un dispositivo Windows.

##Sviluppo di lettori video

Per informazioni su come sviluppare i propri lettori, vedere [Sviluppo di lettori di contenuti video](media-services-develop-video-players.md)




##Percorsi di apprendimento di Servizi multimediali

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##Fornire commenti e suggerimenti

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


[AMSPlayer]: ./media/media-services-playback-content-with-existing-players/media-services-portal-player.png

<!---HONumber=AcomDC_0921_2016-->