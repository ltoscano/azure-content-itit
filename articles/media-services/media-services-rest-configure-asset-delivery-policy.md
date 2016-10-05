<properties 
	pageTitle="Configurazione dei criteri di distribuzione degli asset utilizzando l'API REST di Servizi multimediali | Microsoft Azure" 
	description="Questo argomento illustra come configurare criteri di distribuzione degli asset differenti utilizzando l'API REST di Servizi multimediali." 
	services="media-services" 
	documentationCenter="" 
	authors="Juliako" 
	manager="dwrede" 
	editor=""/>

<tags 
	ms.service="media-services" 
	ms.workload="media" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="09/19/2016"  
	ms.author="juliako"/>

#Configurazione dei criteri di distribuzione degli asset

[AZURE.INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

Se si prevede di distribuire asset crittografati in modo dinamico, uno dei passaggi del flusso di lavoro di distribuzione dei contenuti in Servizi multimediali consiste nella configurazione dei criteri di distribuzione degli asset. Questi criteri indicano a Servizi multimediali la modalità di distribuzione di un asset, ovvero il protocollo di streaming da usare per la creazione dinamica dei pacchetti (ad esempio, MPEG DASH, HLS, Smooth Streaming o tutti), se l'asset deve essere crittografato dinamicamente e l'eventuale modalità di crittografia (envelope o common).

Questo argomento illustra perché e come creare i criteri di distribuzione degli asset.

>[AZURE.NOTE]Per usare la funzionalità di creazione dinamica dei pacchetti e la crittografia dinamica, è necessario assicurarsi di avere almeno un'unità di scala, nota anche come unità di streaming. Per altre informazioni, vedere la sezione relativa al [ridimensionamento di un servizio multimediale](media-services-portal-manage-streaming-endpoints.md).
>
>Inoltre, l'asset deve contenere un set di file MP4 o Smooth Streaming a velocità in bit adattiva.

È possibile applicare criteri differenti allo stesso asset. È ad esempio possibile applicare la crittografia PlayReady a Smooth Streaming e la crittografia envelope AES (Advanced Encryption Standard) a MPEG DASH e HLS. Gli eventuali protocolli non definiti nei criteri di distribuzione (ad esempio quando si aggiunge un singolo criterio che specifica soltanto HLS come protocollo) verranno esclusi dallo streaming. Questo comportamento non si verifica quando non è presente alcun criterio di distribuzione degli asset. In tal caso, sono consentiti tutti i protocolli in chiaro.

Se si desidera distribuire un asset con memoria crittografata, è necessario configurare i criteri di distribuzione appropriati. Prima di trasmettere in streaming l'asset in base ai criteri specificati, il server rimuove la crittografia di archiviazione. Ad esempio, per distribuire l'asset crittografato con una chiave di crittografia envelope AES (Advanced Encryption Standard), impostare il tipo di criteri su **DynamicEnvelopeEncryption**. Per rimuovere la crittografia di archiviazione e trasmettere l'asset in chiaro, impostare il tipo di criteri su **NoDynamicEncryption**. I seguenti esempi mostrano come configurare questi tipi di criteri.

A seconda della modalità di configurazione dei criteri di distribuzione degli asset, sarà possibile creare dinamicamente i pacchetti, applicare la crittografia in modalità dinamica e trasmettere i protocolli di streaming seguenti: Smooth Streaming, HLS, MPEG DASH e HDS.

Di seguito sono illustrati i formati da usare per i flussi Smooth Streaming, HLS, DASH e HDS.

Smooth Streaming:

{nome endpoint di streaming-nome account servizi multimediali}.streaming.mediaservices.windows.net/{ID localizzatore}/{nome file}.ism/Manifest

HLS:

{nome endpoint di streaming-nome account servizi multimediali}.streaming.mediaservices.windows.net/{ID localizzatore}/{nome file}.ism/Manifest(format=m3u8-aapl)

MPEG DASH

{nome endpoint di streaming-nome account servizi multimediali}.streaming.mediaservices.windows.net/{ID localizzatore}/{nome file}.ism/Manifest(format=mpd-time-csf)

HDS:

{nome endpoint di streaming-nome account servizi multimediali}.streaming.mediaservices.windows.net/{ID localizzatore}/{nome file}.ism/Manifest(format=f4m-f4f)

Per istruzioni su come pubblicare un asset e creare un URL di streaming, vedere la sezione [Creare URL di streaming](media-services-deliver-streaming-content.md).


##Considerazioni

- Non è possbile eliminare un AssetDeliveryPolicy con un asset se esiste un localizzatore OnDemand (streaming) per quell’asset. Si suggerisce di rimuovere il criterio dall'asset prima di eliminare il criterio.
- Non è possibile creare un localizzatore di streaming in un asset crittografato per l’archiviazione se non è impostato alcun criterio di distribuzione degli asset. Se l'asset non è crittografato per l'archiviazione, il sistema consentirà di creare un localizzatore ed eseguire in streaming l'asset in chiaro senza un criterio di distribuzione degli asset.
- È possibile avere più criteri di distribuzione degli asset associati a un singolo asset, ma è possibile specificare solo un modo per gestire un determinato AssetDeliveryProtocol. Ciò significa che se si tenta di collegare due criteri di distribuzione che specificano il protocollo AssetDeliveryProtocol.SmoothStreaming, verrà generato un errore perché il sistema non sa quale applicare quando un client effettua una richiesta di Smooth Streaming.
- Se si dispone di un asset con un localizzatore di streaming esistente, non è possibile collegare un nuovo criterio all'asset, scollegare un criterio esistente dall'asset, o aggiornare un criterio di distribuzione associato all'asset. È innanzitutto necessario rimuovere il localizzatore di streaming, modificare i criteri e quindi creare nuovamente il localizzatore di streaming. È possibile utilizzare lo stesso ID quando si ricrea il localizzatore di streaming, ma è necessario assicurarsi che questo non causi problemi per i client poiché il contenuto può essere memorizzato nella cache per l'origine o una rete CDN a valle.

>[AZURE.NOTE] Quando si usa l'API REST di Servizi multimediali, tenere presenti le seguenti considerazioni:
>
>Quando si accede alle entità in Servizi multimediali, è necessario impostare valori e campi di intestazione specifici nelle richieste HTTP. Per altre informazioni, vedere [Panoramica dell'API REST di Servizi multimediali](media-services-rest-how-to-use.md).

>Dopo aver stabilito la connessione a https://media.windows.net, si riceverà un reindirizzamento 301 che indica un altro URI di Servizi multimediali. Le chiamate successive dovranno essere eseguite al nuovo URI, come descritto in [Connessione a un account di Servizi multimediali mediante l'API REST](media-services-rest-connect-programmatically.md).


##Criteri di distribuzione degli asset Clear

###<a id="create_asset_delivery_policy"></a>Creare criteri di distribuzione degli asset
La seguente richiesta HTTP crea criteri di distribuzione degli asset che indicano di non applicare la crittografia dinamica e di distribuire il flusso con uno dei seguenti protocolli: MPEG DASH, HLS e Smooth Streaming.

Per informazioni sui valori che è possibile specificare quando si crea un oggetto AssetDeliveryPolicy, vedere la sezione [Tipi usati durante la definizione di AssetDeliveryPolicy](#types).


Richiesta:
	  
	POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
	Content-Type: application/json
	DataServiceVersion: 1.0;NetFx
	MaxDataServiceVersion: 3.0;NetFx
	Accept: application/json
	Accept-Charset: UTF-8
	Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423397827&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=Szo6lbJAvL3dyecAeVmyAnzv3mGzfUNClR5shk9Ivbk%3d
	x-ms-version: 2.11
	x-ms-client-request-id: 4651882c-d7ad-4d5e-86ab-f07f47dcb41e
	Host: media.windows.net
	
	{"Name":"Clear Policy",
	"AssetDeliveryProtocol":7,
	"AssetDeliveryPolicyType":2,
	"AssetDeliveryConfiguration":null}

Risposta:
	
	HTTP/1.1 201 Created
	Cache-Control: no-cache
	Content-Length: 363
	Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
	Location: https://media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3A92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd')
	Server: Microsoft-IIS/8.5
	x-ms-client-request-id: 4651882c-d7ad-4d5e-86ab-f07f47dcb41e
	request-id: 6aedbf93-4bc2-4586-8845-fd45590136af
	x-ms-request-id: 6aedbf93-4bc2-4586-8845-fd45590136af
	X-Content-Type-Options: nosniff
	DataServiceVersion: 3.0;
	X-Powered-By: ASP.NET
	Strict-Transport-Security: max-age=31536000; includeSubDomains
	Date: Sun, 08 Feb 2015 06:21:27 GMT
	
	{"odata.metadata":"https://media.windows.net/api/$metadata#AssetDeliveryPolicies/@Element",
	"Id":"nb:adpid:UUID:92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd",
	"Name":"Clear Policy",
	"AssetDeliveryProtocol":7,
	"AssetDeliveryPolicyType":2,
	"AssetDeliveryConfiguration":null,
	"Created":"2015-02-08T06:21:27.6908329Z",
	"LastModified":"2015-02-08T06:21:27.6908329Z"}
	
###<a id="link_asset_with_asset_delivery_policy"></a>Collegare un asset ai criteri di distribuzione

La seguente richiesta HTTP collega l'asset specificato ai relativi criteri di distribuzione.

Richiesta:

	POST https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A86933344-9539-4d0c-be7d-f842458693e0')/$links/DeliveryPolicies HTTP/1.1
	DataServiceVersion: 1.0;NetFx
	MaxDataServiceVersion: 3.0;NetFx
	Accept: application/json
	Accept-Charset: UTF-8
	Content-Type: application/json
	Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-e769-3344-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423397827&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=Szo6lbJAvL3dyecAeVmyAnzv3mGzfUNClR5shk9Ivbk%3d
	x-ms-version: 2.11
	x-ms-client-request-id: 56d2763f-6e72-419d-ba3c-685f6db97e81
	Host: media.windows.net
	
	{"uri":"https://media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3A92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd')"}

Risposta:

	HTTP/1.1 204 No Content


##Criteri di distribuzione degli asset DynamicEnvelopeEncryption 

###Creare una chiave simmetrica di tipo EnvelopeEncryption e collegarla all'asset

Quando si specificano criteri di distribuzione DynamicEnvelopeEncryption, è necessario assicurarsi di collegare l'asset a una chiave simmetrica di tipo EnvelopeEncryption. Per altre informazioni, vedere [Creazione di una chiave simmetrica](media-services-rest-create-contentkey.md)).


###<a id="get_delivery_url"></a>Ottenere l'URL di distribuzione

Ottenere l'URL relativo al metodo di distribuzione specificato per la chiave simmetrica creata nel passaggio precedente. Un client usa l'URL restituito per richiedere una chiave AES oppure una licenza PlayReady allo scopo di riprodurre contenuto protetto.

Specificare il tipo di URL da ottenere nel corpo della richiesta HTTP. Se si desidera proteggere i contenuti con PlayReady, richiedere un URL di acquisizione licenza di PlayReady per Servizi multimediali, usando 1 per keyDeliveryType: {"keyDeliveryType":1}. Se si desidera proteggere i contenuti con la crittografia envelope, richiedere un URL di acquisizione chiave specificando 2 per keyDeliveryType: {"keyDeliveryType":2}.

Richiesta:
	
	POST https://media.windows.net/api/ContentKeys('nb:kid:UUID:dc88f996-2859-4cf7-a279-c52a9d6b2f04')/GetKeyDeliveryUrl HTTP/1.1
	Content-Type: application/json
	MaxDataServiceVersion: 3.0;NetFx
	Accept: application/json
	Accept-Charset: UTF-8
	Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423452029&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=IEXV06e3drSIN5naFRBdhJZCbfEqQbFZsGSIGmawhEo%3d
	x-ms-version: 2.11
	x-ms-client-request-id: 569d4b7c-a446-4edc-b77c-9fb686083dd8
	Host: media.windows.net
	Content-Length: 21
	
	{"keyDeliveryType":2}

Risposta:
	
	HTTP/1.1 200 OK
	Cache-Control: no-cache
	Content-Length: 198
	Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
	Server: Microsoft-IIS/8.5
	x-ms-client-request-id: 569d4b7c-a446-4edc-b77c-9fb686083dd8
	request-id: d26f65d2-fe65-4136-8fcf-31545be68377
	x-ms-request-id: d26f65d2-fe65-4136-8fcf-31545be68377
	X-Content-Type-Options: nosniff
	DataServiceVersion: 3.0;
	Strict-Transport-Security: max-age=31536000; includeSubDomains
	Date: Sun, 08 Feb 2015 21:42:30 GMT
	
	{"odata.metadata":"media.windows.net/api/$metadata#Edm.String","value":"https://amsaccount1.keydelivery.mediaservices.windows.net/?KID=dc88f996-2859-4cf7-a279-c52a9d6b2f04"}


###Creare criteri di distribuzione degli asset

La seguente richiesta HTTP crea l'oggetto **AssetDeliveryPolicy** configurato in modo da applicare la crittografia envelope dinamica (**DynamicEnvelopeEncryption**) al protocollo **HLS** (in questo esempio, gli altri protocolli vengono esclusi dallo streaming).


Per informazioni sui valori che è possibile specificare quando si crea un oggetto AssetDeliveryPolicy, vedere la sezione [Tipi usati durante la definizione di AssetDeliveryPolicy](#types).

Richiesta:

	POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
	Content-Type: application/json
	DataServiceVersion: 1.0;NetFx
	MaxDataServiceVersion: 3.0;NetFx
	Accept: application/json
	Accept-Charset: UTF-8
	User-Agent: Microsoft ADO.NET Data Services
	Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423480651&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=T2FG3tIV0e2ETzxQ6RDWxWAsAzuy3ez2ruXPhrBe62Y%3d
	x-ms-version: 2.11
	x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
	Host: media.windows.net
	
	{"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":4,"AssetDeliveryPolicyType":3,"AssetDeliveryConfiguration":"[{"Key":2,"Value":"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\\/"}]"}

	
Risposta:
	
	HTTP/1.1 201 Created
	Cache-Control: no-cache
	Content-Length: 460
	Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
	Location: media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3Aec9b994e-672c-4a5b-8490-a464eeb7964b')
	Server: Microsoft-IIS/8.5
	x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
	request-id: c2a1ac0e-9644-474f-b38f-b9541c3a7c5f
	x-ms-request-id: c2a1ac0e-9644-474f-b38f-b9541c3a7c5f
	X-Content-Type-Options: nosniff
	DataServiceVersion: 3.0;
	Strict-Transport-Security: max-age=31536000; includeSubDomains
	Date: Mon, 09 Feb 2015 05:24:38 GMT
	
	{"odata.metadata":"media.windows.net/api/$metadata#AssetDeliveryPolicies/@Element","Id":"nb:adpid:UUID:ec9b994e-672c-4a5b-8490-a464eeb7964b","Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":4,"AssetDeliveryPolicyType":3,"AssetDeliveryConfiguration":"[{"Key":2,"Value":"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\\/"}]","Created":"2015-02-09T05:24:38.9167436Z","LastModified":"2015-02-09T05:24:38.9167436Z"}


###Collegare un asset ai criteri di distribuzione

Vedere [Collegare un asset ai criteri di distribuzione](#link_asset_with_asset_delivery_policy)

##Criteri di distribuzione degli asset DynamicCommonEncryption 

###Creare una chiave simmetrica di tipo CommonEncryption e collegarla all'asset

Quando si specificano criteri di distribuzione DynamicEnvelopeEncryption, è necessario assicurarsi di collegare l'asset a una chiave simmetrica di tipo CommonEncryption. Per altre informazioni, vedere [Creazione di una chiave simmetrica](media-services-rest-create-contentkey.md)).


###Ottenere l'URL di distribuzione

Ottenere l'URL relativo al metodo di distribuzione PlayReady per la chiave simmetrica creata nel passaggio precedente. Un client usa l'URL restituito per richiedere una licenza PlayReady allo scopo di riprodurre contenuto protetto. Per altre informazioni, vedere [Ottenere l'URL di distribuzione](#get_delivery_url).

###Creare criteri di distribuzione degli asset

La seguente richiesta HTTP crea l'oggetto **AssetDeliveryPolicy** configurato in modo da applicare la crittografia common dinamica (**DynamicCommonEncryption**) al protocollo **Smooth Streaming** (in questo esempio, gli altri protocolli vengono esclusi dallo streaming).

Per informazioni sui valori che è possibile specificare quando si crea un oggetto AssetDeliveryPolicy, vedere la sezione [Tipi usati durante la definizione di AssetDeliveryPolicy](#types).


Richiesta:

	POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
	Content-Type: application/json
	DataServiceVersion: 1.0;NetFx
	MaxDataServiceVersion: 3.0;NetFx
	Accept: application/json
	Accept-Charset: UTF-8
	User-Agent: Microsoft ADO.NET Data Services
	Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423480651&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=T2FG3tIV0e2ETzxQ6RDWxWAsAzuy3ez2ruXPhrBe62Y%3d
	x-ms-version: 2.11
	x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
	Host: media.windows.net
	
	{"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":1,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{"Key":2,"Value":"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\/PlayReady\/"}]"}


Se si desidera proteggere il contenuto utilizzando DRM Widevine, aggiornare i valori AssetDeliveryConfiguration per utilizzare WidevineLicenseAcquisitionUrl (che ha il valore 7) e specificare l'URL di un servizio di recapito di licenza. È possibile utilizzare i seguenti partner AMS per consentire la distribuzione di licenze Widevine: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).

Ad esempio:
 
	
	{"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":2,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{"Key":7,"Value":"https:\\/\\/example.net\/WidevineLicenseAcquisition\/"}]"}

>[AZURE.NOTE]Durante la crittografia con Widevine, si sarebbe in grado di recapitare utilizzando DASH. Assicurarsi di specificare il protocollo di recapito asset DASH (2).
  
###Collegare un asset ai criteri di distribuzione

Vedere [Collegare un asset ai criteri di distribuzione](#link_asset_with_asset_delivery_policy)


##<a id="types"></a>Tipi usati durante la definizione di AssetDeliveryPolicy

###AssetDeliveryProtocol 

    /// <summary>
    /// Delivery protocol for an asset delivery policy.
    /// </summary>
    [Flags]
    public enum AssetDeliveryProtocol
    {
        /// <summary>
        /// No protocols.
        /// </summary>
        None = 0x0,

        /// <summary>
        /// Smooth streaming protocol.
        /// </summary>
        SmoothStreaming = 0x1,

        /// <summary>
        /// MPEG Dynamic Adaptive Streaming over HTTP (DASH)
        /// </summary>
        Dash = 0x2,

        /// <summary>
        /// Apple HTTP Live Streaming protocol.
        /// </summary>
        HLS = 0x4,

        /// <summary>
        /// Adobe HTTP Dynamic Streaming (HDS)
        /// </summary>
        Hds = 0x8,

        /// <summary>
        /// Include all protocols.
        /// </summary>
        All = 0xFFFF
    }

###AssetDeliveryPolicyType

    /// <summary>
    /// Policy type for dynamic encryption of assets.
    /// </summary>
    public enum AssetDeliveryPolicyType
    {
        /// <summary>
        /// Delivery Policy Type not set.  An invalid value.
        /// </summary>
        None,

        /// <summary>
        /// The Asset should not be delivered via this AssetDeliveryProtocol. 
        /// </summary>
        Blocked, 

        /// <summary>
        /// Do not apply dynamic encryption to the asset.
        /// </summary>
        /// 
        NoDynamicEncryption,  

        /// <summary>
        /// Apply Dynamic Envelope encryption.
        /// </summary>
        DynamicEnvelopeEncryption,

        /// <summary>
        /// Apply Dynamic Common encryption.
        /// </summary>
        DynamicCommonEncryption
        }

###ContentKeyDeliveryType


    /// <summary>
    /// Delivery method of the content key to the client.
    ///
    </summary>
    public enum ContentKeyDeliveryType
    {
        /// <summary>
        /// None.
        ///
        </summary>
        None = 0,

        /// <summary>
        /// Use PlayReady License acquistion protocol
        ///
        </summary>
        PlayReadyLicense = 1,

        /// <summary>
        /// Use MPEG Baseline HTTP key protocol.
        ///
        </summary>
        BaselineHttp = 2,

        /// <summary>
        /// Use Widevine License acquistion protocol
        ///
        </summary>
        Widevine = 3

    }


###AssetDeliveryPolicyConfigurationKey

    /// <summary>
    /// Keys used to get specific configuration for an asset delivery policy.
    /// </summary>

    public enum AssetDeliveryPolicyConfigurationKey
    {
        /// <summary>
        /// No policies.
        /// </summary>
        None,

        /// <summary>
        /// Exact Envelope key URL.
        /// </summary>
        EnvelopeKeyAcquisitionUrl,

        /// <summary>
        /// Base key url that will have KID=<Guid> appended for Envelope.
        /// </summary>
        EnvelopeBaseKeyAcquisitionUrl,
        
        /// <summary>
        /// The initialization vector to use for envelope encryption in Base64 format.
        /// </summary>
        EnvelopeEncryptionIVAsBase64,

        /// <summary>
        /// The PlayReady License Acquisition Url to use for common encryption.
        /// </summary>
        PlayReadyLicenseAcquisitionUrl,

        /// <summary>
        /// The PlayReady Custom Attributes to add to the PlayReady Content Header
        /// </summary>
        PlayReadyCustomAttributes,

        /// <summary>
        /// The initialization vector to use for envelope encryption.
        /// </summary>
        EnvelopeEncryptionIV,

        /// <summary>
        /// Widevine DRM acquisition url
        /// </summary>
        WidevineLicenseAcquisitionUrl
    }


##Percorsi di apprendimento di Servizi multimediali

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##Fornire commenti e suggerimenti

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

<!---HONumber=AcomDC_0921_2016-->