<properties 
	pageTitle="Domande frequenti | Microsoft Azure" 
	description="Domande frequenti (FAQ)" 
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


#Domande frequenti

##Domande frequenti generali su AMS

D: Come scalare l'indicizzazione?

R: Le unità riservate sono le stesse per le attività di codifica e indicizzazione. Seguire le istruzioni in [Come scalare le unità riservate di codifica](media-services-scale-media-processing-overview.md). **Tenere presente** che le prestazioni dell'indicizzatore non vengono influenzate dal tipo di unità riservata.

D: Ho caricato, codificato e pubblicato un video. Quale può essere il motivo per cui il video non viene riprodotto quando provo a trasmetterlo in streaming?

R: Uno dei motivi più comuni per questo tipo di problema è che sull'endpoint di streaming da cui si tenta di riprodurre il video non è allocata alcuna unità riservata di streaming. Seguire le istruzioni riportate in [Come scalare unità riservate di streaming](media-services-portal-scale-streaming-endpoints.md).

D: È possibile eseguire la composizione in un flusso live?

A: La composizione in flussi live non è attualmente disponibile in Servizi multimediali di Azure Media ed è quindi necessario eseguire una composizione preliminare sul proprio computer.

D: È possibile usare la rete CDN di Azure con Live Streaming?

R: Servizi multimediali supporta l'integrazione con la rete CDN di Azure (per altre informazioni, vedere [Come gestire gli endpoint di streaming in un account di Servizi multimediali](media-services-portal-manage-streaming-endpoints.md)). È quindi possibile usare Live streaming con la rete CDN. Servizi multimediali di Azure fornisce output in formato Smooth Streaming, HLS e MPEG-DASH. Tutti questi formati usano il protocollo HTTP per trasferire dati e ottenere i vantaggi derivanti dalla cache HTTP. In Live Streaming i dati audio/video effettivi vengono divisi in frammenti, ciascuno dei quali viene memorizzato nella rete CDN. L'aggiornamento è necessario solo per i dati manifesto e viene effettuato periodicamente dalla rete CDN.

D: Servizi multimediali di Azure supporta anche l'archiviazione di immagini?

R: Se si desidera archiviare immagini JPEG o PNG, è consigliabile memorizzarle nell'Archiviazione BLOB di Azure. Aggiungerle all'account di Servizi multimediali non comporta infatti alcun vantaggio, a meno che non si desideri tenerle associate ai propri asset audio o video o sia necessario usarle come sovrimpressioni nel codificatore video. Media Encoder Standard supporta infatti la sovrimpressione di immagini nella parte superiore dei video ed è per questo che i formati JPEG e PNG sono elencati tra i formati di input supportati. Per altre informazioni, vedere [Creazione di sovrimpressioni](media-services-custom-mes-presets-with-dotnet.md#overlay).

D: Come è possibile copiare gli asset da un account di Servizi multimediali a un altro?

R: Per copiare gli asset da un account di Servizi multimediali a un altro con .NET, usare il metodo di estensione [IAsset.Copy](https://github.com/Azure/azure-sdk-for-media-services-extensions/blob/dev/MediaServices.Client.Extensions/IAssetExtensions.cs#L354) disponibile nel repository [Azure Media Services .NET SDK Extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions/). Per altre informazioni, vedere [questo](https://social.msdn.microsoft.com/Forums/azure/28912d5d-6733-41c1-b27d-5d5dff2695ca/migrate-media-services-across-subscription?forum=MediaServices) thread del forum.

D: Quali sono i caratteri supportati per la denominazione dei file quando si usa AMS?

R: Servizi multimediali usa il valore della proprietà IAssetFile.Name durante la creazione degli URL per i contenuti in streaming, ad esempio http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.. Per questo motivo, la codifica percentuale non è consentita. Il valore della proprietà **Name** non può contenere i [caratteri riservati per la codifica percentuale](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters) seguenti: !*'();:@&=+$,/?%#". L'estensione del nome di file, inoltre, può essere preceduta da un solo punto (.).


D: Come connettersi usando REST?

R: Dopo avere stabilito la connessione a https://media.windows.net, si riceverà un reindirizzamento 301 che indica un altro URI di Servizi multimediali. Le chiamate successive dovranno essere eseguite al nuovo URI, come descritto in [Connessione a un account di Servizi multimediali mediante l'API REST](media-services-rest-connect-programmatically.md).


D: Come è possibile ruotare un video durante il processo di codifica.

R: Il [codificatore multimediale standard](media-services-dotnet-encode-with-media-encoder-standard.md) supporta la rotazione in base ad angoli di 90/180/270. Il comportamento predefinito è "Auto", che tenta di rilevare i metadati di rotazione nel file MP4/MOV in arrivo per la compensazione. Includere l'elemento **Sources** seguente in uno dei set di impostazioni JSON definiti [qui](http://msdn.microsoft.com/library/azure/mt269960.aspx):
	
	"Version": 1.0,
	"Sources": [
	{
	  "Streams": [],
	  "Filters": {
	    "Rotation": "90"
	  }
	}
	],
	"Codecs": [
	
	...




##Percorsi di apprendimento di Servizi multimediali

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##Fornire commenti e suggerimenti

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

<!---HONumber=AcomDC_0921_2016-->