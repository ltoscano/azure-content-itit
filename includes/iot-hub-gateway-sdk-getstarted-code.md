## Output tipico

Di seguito è riportato un esempio dell'output scritto nel file di log per l'esempio Hello World. Sono stati aggiunti caratteri di nuova riga e di tabulazione per migliorare la leggibilità:

```
[{
	"time": "Mon Apr 11 13:48:07 2016",
	"content": "Log started"
}, {
	"time": "Mon Apr 11 13:48:48 2016",
	"properties": {
		"helloWorld": "from Azure IoT Gateway SDK simple sample!"
	},
	"content": "aGVsbG8gd29ybGQ="
}, {
	"time": "Mon Apr 11 13:48:55 2016",
	"properties": {
		"helloWorld": "from Azure IoT Gateway SDK simple sample!"
	},
	"content": "aGVsbG8gd29ybGQ="
}, {
	"time": "Mon Apr 11 13:49:01 2016",
	"properties": {
		"helloWorld": "from Azure IoT Gateway SDK simple sample!"
	},
	"content": "aGVsbG8gd29ybGQ="
}, {
	"time": "Mon Apr 11 13:49:04 2016",
	"content": "Log stopped"
}]
```

## Frammenti di codice

In questa sezione vengono descritti alcuni elementi chiave del codice nell'esempio di Hello World.

### Creazione di gateway

Lo sviluppatore deve scrivere il *processo del gateway*. Questo programma crea l'infrastruttura interna, ovvero il broker, carica i moduli e configura tutto in modo che funzioni correttamente. L'SDK specifica la funzione **Gateway\_Create\_From\_JSON** per consentire di avviare un gateway da un file JSON. Per usare la funzione **Gateway\_Create\_From\_JSON** è necessario chiamarla dal percorso di un file JSON che specifica i moduli da caricare.

È possibile trovare il codice per il processo del gateway per l'esempio Hello World nel file [main. c][lnk-main-c]. Per migliorare la leggibilità, il frammento di codice riportato di seguito illustra una versione abbreviata del codice del processo del gateway. Questo programma crea un gateway e quindi attende che l'utente prema **INVIO** prima di rimuove il gateway.

```
int main(int argc, char** argv)
{
    GATEWAY_HANDLE gateway;
    if ((gateway = Gateway_Create_From_JSON(argv[1])) == NULL)
    {
        printf("failed to create the gateway from JSON\n");
    }
    else
    {
        printf("gateway successfully created from JSON\n");
        printf("gateway shall run until ENTER is pressed\n");
        (void)getchar();
        Gateway_LL_Destroy(gateway);
    }
	return 0;
}
```

Il file di impostazioni JSON contiene un elenco di moduli da caricare. Ogni modulo deve specificare:

- **module\_name**: un nome univoco per il modulo.
- **module\_path**: il percorso della libreria che contiene il modulo. Per Linux si tratta di un file con estensione so, per Windows si tratta di un file con estensione dll.
- **args**: le informazioni di configurazione necessarie per il modulo.

Il file JSON contiene anche i collegamenti tra i moduli che verranno passati al broker. Un collegamento ha due proprietà:
- **source**: il nome di un modulo dalla sezione `modules` oppure "*".
- **sink**: il nome di un modulo dalla sezione `modules`.

Ogni collegamento definisce una route messaggi e una direzione. I messaggi dal modulo `source` devono essere recapitati al modulo `sink`. Il modulo `source` può essere impostato su "*", a indicare che i messaggi da qualsiasi modulo verranno ricevuti dal `sink`.

Nell'esempio seguente viene illustrato il file di impostazioni JSON usato per configurare l'esempio Hello World in Linux. Tutti i messaggi generati dal modulo `hello_world` verranno utilizzati dal modulo `logger`. La necessità che un modulo specifichi un argomento dipende dalla progettazione del modulo stesso. In questo esempio, il modulo di logger usa un argomento corrispondente al percorso del file di output e il modulo di Hello World non usa alcun argomento:

```
{
    "modules" :
    [ 
        {
            "module name" : "logger",
            "module path" : "./modules/logger/liblogger_hl.so",
            "args" : {"filename":"log.txt"}
        },
        {
            "module name" : "hello_world",
            "module path" : "./modules/hello_world/libhello_world_hl.so",
			"args" : null
        }
    ],
    "links" :
    [
        {
            "source" : "hello_world",
            "sink" : "logger"
        }
    ]
}
```

### Pubblicazione dei messaggi del modulo di Hello World

Il codice usato dal modulo "hello world" per pubblicare i messaggi è disponibile nel file ['hello\_world.c'][lnk-helloworld-c]. Il frammento di codice riportato di seguito riporta una versione modificata, ai fini di una maggior leggibilità, in cui sono stati aggiunti commenti ed è stato rimosso del codice per la gestione degli errori:

```
int helloWorldThread(void *param)
{
    // create data structures used in function.
    HELLOWORLD_HANDLE_DATA* handleData = param;
    MESSAGE_CONFIG msgConfig;
    MAP_HANDLE propertiesMap = Map_Create(NULL);
    
    // add a property named "helloWorld" with a value of "from Azure IoT
    // Gateway SDK simple sample!" to a set of message properties that
    // will be appended to the message before publishing it. 
    Map_AddOrUpdate(propertiesMap, "helloWorld", "from Azure IoT Gateway SDK simple sample!")

    // set the content for the message
    msgConfig.size = strlen(HELLOWORLD_MESSAGE);
    msgConfig.source = HELLOWORLD_MESSAGE;

    // set the properties for the message
    msgConfig.sourceProperties = propertiesMap;
    
    // create a message based on the msgConfig structure
    MESSAGE_HANDLE helloWorldMessage = Message_Create(&msgConfig);

    while (1)
    {
        if (handleData->stopThread)
        {
            (void)Unlock(handleData->lockHandle);
            break; /*gets out of the thread*/
        }
        else
        {
            // publish the message to the broker
            (void)Broker_Publish(handleData->brokerHandle, helloWorldMessage);
            (void)Unlock(handleData->lockHandle);
        }

        (void)ThreadAPI_Sleep(5000); /*every 5 seconds*/
    }

    Message_Destroy(helloWorldMessage);

    return 0;
}
```

### Elaborazione dei messaggi del modulo di Hello World

Il modulo Hello World non deve mai elaborare messaggi pubblicati da altri moduli nel broker. Di conseguenza, l'implementazione del callback dei messaggi nel modulo Hello World è una funzione no-op.

```
static void HelloWorld_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{
    /* No action, HelloWorld is not interested in any messages. */
}
```

### Elaborazione e pubblicazione dei messaggi del modulo di logger

Il modulo di logger riceve messaggi dal broker e li scrive in un file, ma non pubblica mai messaggi. Il codice del modulo logger, quindi, non chiama mai la funzione **Broker\_Publish**.

La funzione **Logger\_Recieve** nel file [logger.c][lnk-logger-c] è il callback che viene richiamato dal broker per recapitare i messaggi al modulo logger. Il frammento di codice riportato di seguito riporta una versione modificata, ai fini di una maggior leggibilità, in cui sono stati aggiunti commenti ed è stato rimosso del codice per la gestione degli errori:

```
static void Logger_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{

    time_t temp = time(NULL);
    struct tm* t = localtime(&temp);
    char timetemp[80] = { 0 };

    // Get the message properties from the message
    CONSTMAP_HANDLE originalProperties = Message_GetProperties(messageHandle); 
    MAP_HANDLE propertiesAsMap = ConstMap_CloneWriteable(originalProperties);

    // Convert the collection of properties into a JSON string
    STRING_HANDLE jsonProperties = Map_ToJSON(propertiesAsMap);

    //  base64 encode the message content
    const CONSTBUFFER * content = Message_GetContent(messageHandle);
    STRING_HANDLE contentAsJSON = Base64_Encode_Bytes(content->buffer, content->size);

    // Start the construction of the final string to be logged by adding
    // the timestamp
    STRING_HANDLE jsonToBeAppended = STRING_construct(",{"time":"");
    STRING_concat(jsonToBeAppended, timetemp);

    // Add the message properties
    STRING_concat(jsonToBeAppended, "","properties":"); 
    STRING_concat_with_STRING(jsonToBeAppended, jsonProperties);

    // Add the content
    STRING_concat(jsonToBeAppended, ","content":"");
    STRING_concat_with_STRING(jsonToBeAppended, contentAsJSON);
    STRING_concat(jsonToBeAppended, ""}]");

    // Write the formatted string
    LOGGER_HANDLE_DATA *handleData = (LOGGER_HANDLE_DATA *)moduleHandle;
    addJSONString(handleData->fout, STRING_c_str(jsonToBeAppended);
}
```

## Passaggi successivi

Per informazioni su come usare l'SDK per gateway, vedere quanto segue:

- [IoT Gateway SDK: inviare messaggi da dispositivo a cloud con un dispositivo simulato usando Linux][lnk-gateway-simulated].
- [Azure IoT Gateway SDK][lnk-gateway-sdk] in GitHub.

<!-- Links -->
[lnk-main-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/samples/hello_world/src/main.c
[lnk-helloworld-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/modules/hello_world/src/hello_world.c
[lnk-logger-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/modules/logger/src/logger.c
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/
[lnk-gateway-simulated]: ../articles/iot-hub/iot-hub-linux-gateway-sdk-simulated-device.md

<!---HONumber=AcomDC_0928_2016-->