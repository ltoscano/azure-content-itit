<properties
	pageTitle="Usare Analisi Servizi multimediali di Azure per convertire il contenuto di testo dei file video in testo digitale | Microsoft Azure"
	description="Il riconoscimento ottico dei caratteri (OCR) di Analisi servizi multimediali di Azure consente di convertire il contenuto di testo dei file video in testo digitale modificabile e sui cui è possibile eseguire ricerche. Ciò consente di automatizzare l'estrazione di metadati importanti dal segnale video del contenuto multimediale."
	services="media-services"
	documentationCenter=""
	authors="juliako"
	manager="erikre"
	editor=""/>

<tags
	ms.service="media-services"
	ms.workload="media"
	ms.tgt_pltfrm="na"
	ms.devlang="dotnet"
	ms.topic="article"
	ms.date="06/27/2016"   
	ms.author="juliako"/>
 
#Usare Analisi Servizi multimediali di Azure per convertire il contenuto di testo dei file video in testo digitale 

##Panoramica

Se è necessario estrarre il contenuto di testo da file video e generare un testo digitale modificabile e in cui sia possibile eseguire ricerche, usare il riconoscimento ottico dei caratteri (OCR) di Analisi servizi multimediali di Azure. Questo processore di contenuti multimediali di Azure rileva il contenuto di testo nel file video e genera file di testo pronti per l'uso. OCR consente di automatizzare l'estrazione di metadati importanti dal segnale video del contenuto multimediale.

Se usato in combinazione con un motore di ricerca, è possibile indicizzare facilmente i file multimediali dal testo e migliorare la scoperta dei contenuti. Questa funzione risulta molto utile per i video che contengono un porzione importante di testo, ad esempio una registrazione video o l'acquisizione dello schermo di una presentazione. Il processore di contenuti multimediali Azure OCR è ottimizzato per il testo digitale.

Il processore di contenuti multimediali **Azure Media OCR** è attualmente disponibile in anteprima.

Questo argomento contiene informazioni dettagliate su **Azure Media OCR** e illustra come usare questa funzionalità con l'SDK di Servizi Multimediali per .NET. Per altre informazioni ed esempi, vedere [questo blog](https://azure.microsoft.com/blog/announcing-video-ocr-public-preview-new-config/).

##File di input OCR

File video. Attualmente sono supportati i formati seguenti: MP4, MOV e WMV.

##Configurazione delle attività 

Configurazione delle attività (set di impostazioni). Quando si crea un'attività con **Azure Media OCR**, è necessario specificare un set di impostazioni di configurazione tramite JSON o XML.

###Descrizioni degli attributi

Nome attributo|Descrizione
---|---
Lingua|(Facoltativo) Descrive la lingua del testo da cercare. Uno dei seguenti: AutoDetect (default), Arabic, ChineseSimplified, ChineseTraditional, Czech, Danish, Dutch, English, Finnish, French, German, Greek, Hungarian, Italian, Japanese, Korean, Norwegian, Polish, Portuguese, Romanian, Russian, SerbianCyrillic, SerbianLatin, Slovak, Spanish, Swedish, Turkish.
TextOrientation|(Facoltativo) Descrive l'orientamento del testo da cercare. "Left" significa che la parte superiore di tutte le lettere è rivolta verso sinistra. Il testo predefinito (simile a quello di un libro) può essere orientato come "Up". Uno dei seguenti: AutoDetect (default), Up, Right, Down, Left.
TimeInterval|(Facoltativo) Descrive la frequenza di campionamento. Il valore predefinito è ogni 1/2 secondo.<br/>Formato JSON – OO:mm:ss.SSS (predefinito 00:00:00.500)<br/>Formato XML – durata primitivi W3C XSD (predefinito PT0.5)
DetectRegions|(Facoltativo) Una matrice di oggetti DetectRegion che specifica le aree del fotogramma video in cui rilevare il testo.<br/>Un oggetto DetectRegion è composto dai quattro valori interi seguenti:<br/>Left: pixel dal margine sinistro<br/>Top: pixel dal margine superiore<br/>Width: larghezza dell'area in pixel<br/>Height: altezza dell'area in pixel

####Esempio di set di impostazioni JSON
	 	
	{
	    'Version':'1.0', 
	    'Options': 
	    {
	    'Language':'English', 
	        'TimeInterval':'00:00:01.5',
	        'DetectRegions': 
	         [
	            {'Left':'1','Top':'1','Width':'1','Height':'1'},
	            {'Left':'2','Top':'2','Width':'2','Height':'2'}
	         ],
	        'TextOrientation':'Up'
	    }
	}

####Esempio di set di impostazioni XML

	<?xml version=""1.0"" encoding=""utf-16""?>
	<VideoOcrPreset xmlns:xsi=""http://www.w3.org/2001/XMLSchema-instance"" xmlns:xsd=""http://www.w3.org/2001/XMLSchema"" Version=""1.0"" xmlns=""http://www.windowsazure.com/media/encoding/Preset/2014/03"">
	  <Options>
	     <Language>English</Language>
	     <TimeInterval>PT1.5S</TimeInterval>
	     <DetectRegions>
	         <DetectRegion>
	               <Left>1</Left>
	               <Top>1</Top>
	               <Width>1</Width>
	               <Height>1</Height>
	        </DetectRegion>
	   </DetectRegions>
	   <TextOrientation>Up</TextOrientation>
	  </Options>
	</VideoOcrPreset>

##File di output OCR

L'output del processore multimediale OCR è un file JSON.

###Elementi del file di output JSON

L'output OCR del video include dati con segmentazione temporale sui caratteri del video. È possibile usare gli attributi, la lingua o l'orientamento per concentrarsi esattamente sulle parole che si desidera analizzare.

L'output contiene gli attributi seguenti:

Elemento|Descrizione
---|---
Scala cronologica|"Scatti" al secondo del video
Offset|Differenza di orario dei timestamp Nella versione 1.0 delle API Video, questo valore è sempre 0.
Frequenza fotogrammi|Fotogrammi al secondo del video
width|Larghezza del video in pixel
height|Altezza del video in pixel
Frammenti|Matrice di porzioni temporali di video in cui i metadati sono suddivisi in blocchi
start|Ora di inizio di un frammento in "scatti"
duration|Lunghezza di un frammento in "scatti"
interval|Intervallo di ogni evento all'interno del frammento specificato
events|Matrice contenente le aree
region|Oggetto che rappresenta le parole o le frasi rilevate
Lingua|Lingua del testo rilevato all'interno di un'area
orientation|Orientamento del testo rilevato all'interno di un'area
lines|Matrice di righe del testo rilevato all'interno di un'area
text|Il testo effettivo

###Esempio di output JSON

Il seguente output contiene le informazioni generali del video e numerosi frammenti video. In ogni frammento video, l'output contiene ogni area rilevata dal processore di contenuti multimediali OCR con la lingua e il relativo orientamento del testo. L'area contiene anche ogni riga di parole in questa area con il testo della riga, la posizione della riga e tutte le informazioni sulle parole (il contenuto, la posizione e la sicurezza delle parole) della riga. Di seguito è riportato un esempio, contenente anche alcuni commenti sulla riga.

	{
		"version": 1, 
		"timescale": 90000, 
		"offset": 0, 
		"framerate": 30, 
		"width": 640, 
		"height": 480,  // general video information
		"fragments": [
		    {
		        "start": 0, 
		        "duration": 180000, 
		        "interval": 90000,  // the time information about this fragment
		        "events": [
		            [
		               { 
		                    "region": { // the detected region array in this fragment 
		                        "language": "English",  // region language
		                        "orientation": "Up",  // text orientation
		                        "lines": [  // line information array in this region, including the text and the position
		                            {
		                                "text": "One Two", 
		                                "left": 10, 
		                                "top": 10, 
		                                "right": 210, 
		                                "bottom": 110, 
		                                "word": [  // word information array in this line
		                                    {
		                                        "text": "One", 
		                                        "left": 10, 
		                                        "top": 10, 
		                                        "right": 110, 
		                                        "bottom": 110, 
		                                        "confidence": 900
		                                    }, 
		                                    {
		                                        "text": "Two", 
		                                        "left": 110, 
		                                        "top": 10, 
		                                        "right": 210, 
		                                        "bottom": 110, 
		                                        "confidence": 910
		                                    }
		                                ]
		                            }
		                        ]
		                    }
		                }
		            ]
		        ]
		    }
		]
	}

## Codice di esempio

Il programma seguente illustra come:

1. Creare un asset e caricare un file multimediale nell'asset.
1. Crea un processo con un file di configurazione/set di impostazioni OCR.
1. Scaricare i file di output JSON.
		 
        using System;
		using System.Configuration;
		using System.IO;
		using System.Linq;
		using Microsoft.WindowsAzure.MediaServices.Client;
		using System.Threading;
		using System.Threading.Tasks;
		
		namespace OCR
		{
		    class Program
		    {
		        // Read values from the App.config file.
		        private static readonly string _mediaServicesAccountName =
		            ConfigurationManager.AppSettings["MediaServicesAccountName"];
		        private static readonly string _mediaServicesAccountKey =
		            ConfigurationManager.AppSettings["MediaServicesAccountKey"];
		
		        // Field for service context.
		        private static CloudMediaContext _context = null;
		        private static MediaServicesCredentials _cachedCredentials = null;
		
		        static void Main(string[] args)
		        {
		
		            // Create and cache the Media Services credentials in a static class variable.
		            _cachedCredentials = new MediaServicesCredentials(
		                            _mediaServicesAccountName,
		                            _mediaServicesAccountKey);
		            // Used the cached credentials to create CloudMediaContext.
		            _context = new CloudMediaContext(_cachedCredentials);
		
		            // Run the OCR job.
		            var asset = RunOCRJob(@"C:\supportFiles\OCR\presentation.mp4",
		                                        @"C:\supportFiles\OCR\config.json");
		
		            // Download the job output asset.
		            DownloadAsset(asset, @"C:\supportFiles\OCR\Output");
		        }
		
		        static IAsset RunOCRJob(string inputMediaFilePath, string configurationFile)
		        {
		            // Create an asset and upload the input media file to storage.
		            IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
		                "My OCR Input Asset",
		                AssetCreationOptions.None);
		
		            // Declare a new job.
		            IJob job = _context.Jobs.Create("My OCR Job");
		
		            // Get a reference to Azure Media OCR.
		            string MediaProcessorName = "Azure Media OCR";
		
		            var processor = GetLatestMediaProcessorByName(MediaProcessorName);
		
		            // Read configuration from the specified file.
		            string configuration = File.ReadAllText(configurationFile);
		
		            // Create a task with the encoding details, using a string preset.
		            ITask task = job.Tasks.AddNew("My OCR Task",
		                processor,
		                configuration,
		                TaskOptions.None);
		
		            // Specify the input asset.
		            task.InputAssets.Add(asset);
		
		            // Add an output asset to contain the results of the job.
		            task.OutputAssets.AddNew("My OCR Output Asset", AssetCreationOptions.None);
		
		            // Use the following event handler to check job progress.  
		            job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);
		
		            // Launch the job.
		            job.Submit();
		
		            // Check job execution and wait for job to finish.
		            Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
		
		            progressJobTask.Wait();
		
		            // If job state is Error, the event handling
		            // method for job progress should log errors.  Here we check
		            // for error state and exit if needed.
		            if (job.State == JobState.Error)
		            {
		                ErrorDetail error = job.Tasks.First().ErrorDetails.First();
		                Console.WriteLine(string.Format("Error: {0}. {1}",
		                                                error.Code,
		                                                error.Message));
		                return null;
		            }
		
		            return job.OutputMediaAssets[0];
		        }
		
		        static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
		        {
		            IAsset asset = _context.Assets.Create(assetName, options);
		
		            var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
		            assetFile.Upload(filePath);
		
		            return asset;
		        }
		
		        static void DownloadAsset(IAsset asset, string outputDirectory)
		        {
		            foreach (IAssetFile file in asset.AssetFiles)
		            {
		                file.Download(Path.Combine(outputDirectory, file.Name));
		            }
		        }
		
		        static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
		        {
		            var processor = _context.MediaProcessors
		                .Where(p => p.Name == mediaProcessorName)
		                .ToList()
		                .OrderBy(p => new Version(p.Version))
		                .LastOrDefault();
		
		            if (processor == null)
		                throw new ArgumentException(string.Format("Unknown media processor",
		                                                           mediaProcessorName));
		
		            return processor;
		        }
		
		        static private void StateChanged(object sender, JobStateChangedEventArgs e)
		        {
		            Console.WriteLine("Job state changed event:");
		            Console.WriteLine("  Previous state: " + e.PreviousState);
		            Console.WriteLine("  Current state: " + e.CurrentState);
		
		            switch (e.CurrentState)
		            {
		                case JobState.Finished:
		                    Console.WriteLine();
		                    Console.WriteLine("Job is finished.");
		                    Console.WriteLine();
		                    break;
		                case JobState.Canceling:
		                case JobState.Queued:
		                case JobState.Scheduled:
		                case JobState.Processing:
		                    Console.WriteLine("Please wait...\n");
		                    break;
		                case JobState.Canceled:
		                case JobState.Error:
		                    // Cast sender as a job.
		                    IJob job = (IJob)sender;
		                    // Display or log error details as needed.
		                    // LogJobStop(job.Id);
		                    break;
		                default:
		                    break;
		            }
		        }
		
		    }
        }


##Percorsi di apprendimento di Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##Fornire commenti e suggerimenti

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##Collegamenti correlati

[Panoramica di Analisi servizi multimediali di Azure](media-services-analytics-overview.md)

<!---HONumber=AcomDC_0629_2016-->