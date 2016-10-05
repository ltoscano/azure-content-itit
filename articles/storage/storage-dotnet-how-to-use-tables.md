<properties
	pageTitle="Introduzione all'archiviazione tabelle di Azure con .NET | Microsoft Azure"
	description="Archiviare dati non strutturati nel cloud con il servizio di archiviazione tabelle di Azure, ovvero un archivio dati NoSQL."
	services="storage"
	documentationCenter=".net"
	authors="tamram"
	manager="carmonm"
	editor="tysonn"/>

<tags
	ms.service="storage"
	ms.workload="storage"
	ms.tgt_pltfrm="na"
	ms.devlang="dotnet"
	ms.topic="hero-article"
	ms.date="09/20/2016"
	ms.author="gusapost;tamram"/>


# Introduzione all'archiviazione tabelle di Azure con .NET

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## Overview

Il servizio di archiviazione tabelle di Azure archivia dati NoSQL non strutturati nel cloud. Archiviazione tabelle è un archivio di chiavi/attributi con una struttura senza schema. Poiché l'archiviazione tabelle è senza schema, è facile adattare i dati con il variare delle esigenze dell'applicazione. L'accesso ai dati è rapido e conveniente per tutti i tipi di applicazione. L'archiviazione tabelle presenta in genere costi decisamente più bassi rispetto alle soluzioni SQL tradizionali per volumi simili di dati.

È possibile usare l'archiviazione tabelle per archiviare set di dati flessibili, ad esempio i dati utente per le applicazioni Web, le rubriche, le informazioni sui dispositivi e altri tipi di metadati richiesti dal servizio. In una tabella possono essere archiviate il numero desiderato di tabelle e un account di archiviazione può contenere un numero qualsiasi di tabelle, fino a che non viene raggiunto il limite di capacità dell'account di archiviazione.

### Informazioni sull'esercitazione

Questa esercitazione illustra come scrivere codice .NET per alcuni scenari comuni che usano l'archiviazione tabelle di Azure, incluse le operazioni di creazione ed eliminazione di una tabella, nonché di inserimento, aggiornamento, eliminazione dei dati della tabella e di esecuzione delle query.

**Tempo previsto per il completamento:** 45 minuti

**Prerequisiti:**

- [Microsoft Visual Studio](https://www.visualstudio.com/it-IT/visual-studio-homepage-vs.aspx)
- [Libreria client di archiviazione di Azure per .NET](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [Gestione configurazione di Azure per .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
- Un [account di archiviazione di Azure](storage-create-storage-account.md#create-a-storage-account)

[AZURE.INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

### Altri esempi

Per altri esempi di uso dell'archivio tabelle, vedere [Getting Started with Azure Table Storage in .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/) (Introduzione all'archivio tabelle di Azure in .NET). È possibile scaricare l'applicazione di esempio ed eseguirla oppure esaminare il codice in GitHub.


[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### Aggiungere le dichiarazioni dello spazio dei nomi

Aggiungere le istruzioni `using` seguenti all'inizio del file `program.cs`:

	using Microsoft.Azure; // Namespace for CloudConfigurationManager
	using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
    using Microsoft.WindowsAzure.Storage.Table; // Namespace for Table storage types

### Analizzare la stringa di connessione

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### Creare il client del servizio tabelle

La classe **CloudTableClient** consente di recuperare le tabelle e le entità archiviate nell'archivio tabelle. Ecco come creare il client del servizio:

	// Create the table client.
	CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

A questo punto si è pronti a scrivere codice che legge e scrive i dati nell'archivio tabelle.

## Creare una tabella

Questo esempio illustra come creare una tabella, se non esiste già:

	// Retrieve the storage account from the connection string.
	CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
	    CloudConfigurationManager.GetSetting("StorageConnectionString"));

	// Create the table client.
	CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

	// Retrieve a reference to the table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create the table if it doesn't exist.
    table.CreateIfNotExists();

## Aggiungere un'entità a una tabella

Per eseguire il mapping di entità a oggetti C#, viene utilizzata una classe personalizzata derivata da **TableEntity**. Per aggiungere un'entità a una classe, creare una classe che definisca le proprietà dell'entità. Il codice seguente consente di definire una classe di entità che utilizza il nome e il cognome del cliente rispettivamente come chiave di riga e chiave di partizione. La combinazione della chiave di riga e della chiave di partizione di un'entità consentono di identificare in modo univoco l'entità nella tabella. Le query su entità con la stessa chiave di partizione vengono eseguite più rapidamente di quelle con chiavi di partizione diverse, tuttavia l'utilizzo di chiavi di partizione diverse assicura una maggiore scalabilità delle operazioni parallele. Tutte le proprietà da archiviare nel servizio tabelle devono essere una proprietà pubblica di un tipo supportato che espone `get` e `set`. Il tipo di entità *deve* inoltre esporre un costruttore senza parametri.

    public class CustomerEntity : TableEntity
    {
        public CustomerEntity(string lastName, string firstName)
        {
            this.PartitionKey = lastName;
            this.RowKey = firstName;
        }

        public CustomerEntity() { }

        public string Email { get; set; }

        public string PhoneNumber { get; set; }
    }

Le operazioni su tabella che interessano le entità vengono eseguite mediante l'oggetto **CloudTable** creato precedentemente nella sezione "Creare una tabella". L'operazione da eseguire è rappresentata da un oggetto **TableOperation**. Nell'esempio di codice seguente viene illustrata la creazione dell'oggetto **CloudTable** e quindi di un oggetto **CustomerEntity**. Per preparare l'operazione, viene creato un oggetto **TableOperation** per inserire l'entità customer nella tabella. Infine, per eseguire l'operazione viene chiamato **CloudTable.Execute**.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

	// Create the CloudTable object that represents the "people" table.
	CloudTable table = tableClient.GetTableReference("people");

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create the TableOperation object that inserts the customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute the insert operation.
    table.Execute(insertOperation);

## Inserire un batch di entità

Per inserire un batch di entità in una tabella, è possibile usare un'unica operazione di scrittura. Di seguito sono riportate altre informazioni sulle operazioni batch:

-  È possibile utilizzare una singola operazione batch per eseguire operazioni di aggiornamento, eliminazione e inserimento.
-  Una singola operazione batch può includere fino a 100 entità.
-  A tutte le entità di una singola operazione batch deve essere associata la stessa chiave di partizione.
-  È possibile eseguire una query come operazione batch, ma deve essere l'unica operazione del batch.

<!-- -->
L'esempio di codice seguente consente di creare due oggetti entità e di aggiungere ciascuno a un oggetto **TableBatchOperation** utilizzando il metodo **Insert**. Per eseguire l'operazione, viene quindi chiamato **CloudTable.Execute**.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

	// Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create the batch operation.
    TableBatchOperation batchOperation = new TableBatchOperation();

    // Create a customer entity and add it to the table.
	CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
	customer1.Email = "Jeff@contoso.com";
	customer1.PhoneNumber = "425-555-0104";

	// Create another customer entity and add it to the table.
	CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
	customer2.Email = "Ben@contoso.com";
	customer2.PhoneNumber = "425-555-0102";

	// Add both customer entities to the batch insert operation.
	batchOperation.Insert(customer1);
	batchOperation.Insert(customer2);

	// Execute the batch operation.
	table.ExecuteBatch(batchOperation);

## Recuperare tutte le entità di una partizione

Per eseguire una query su una tabella e recuperare tutte le entità di una partizione, utilizzare un oggetto **TableQuery**. Nell'esempio di codice seguente viene specificato un filtro per le entità in cui la chiave di partizione è 'Smith'. Questo esempio consente di stampare sulla console i campi di ogni entità inclusa nei risultati della query.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

    // Print the fields for each customer.
    foreach (CustomerEntity entity in table.ExecuteQuery(query))
    {
        Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
            entity.Email, entity.PhoneNumber);
    }

## Recuperare un intervallo di entità in una partizione

Se non si desidera eseguire una query su tutte le entità di una partizione, è possibile specificare un intervallo combinando il filtro della chiave di partizione con quello della chiave di riga. Nell'esempio di codice seguente vengono usati due filtri per recuperare tutte le entità della partizione 'Smith' in cui la chiave di riga (nome) inizia con una lettera che precede la 'E' nell'alfabeto e quindi stampare i risultati della query.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

	// Create the table query.
    TableQuery<CustomerEntity> rangeQuery = new TableQuery<CustomerEntity>().Where(
        TableQuery.CombineFilters(
            TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"),
            TableOperators.And,
            TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.LessThan, "E")));

    // Loop through the results, displaying information about the entity.
    foreach (CustomerEntity entity in table.ExecuteQuery(rangeQuery))
    {
        Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
            entity.Email, entity.PhoneNumber);
    }

## Recuperare una singola entità

Per recuperare una singola entità specifica, è possibile scrivere una query. Nel codice seguente viene utilizzato un oggetto **TableOperation** per specificare il cliente 'Ben Smith'. Questo metodo restituisce una sola entità, anziché una raccolta, e il valore restituito in **TableResult.Result** è un oggetto **CustomerEntity**. La specifica delle chiavi di partizione e di riga in una query costituisce la soluzione più rapida per recuperare una singola entità dal servizio tabelle.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the retrieve operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Print the phone number of the result.
	if (retrievedResult.Result != null)
	   Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
	else
	   Console.WriteLine("The phone number could not be retrieved.");

## Sostituire un'entità

Per aggiornare un'entità, recuperarla dal servizio tabelle, modificare l'oggetto entità e quindi salvare le modifiche nel servizio tabelle. Il codice seguente consente di modificare il numero di telefono di un cliente esistente. Invece di una chiamata a **Insert**, nel codice viene utilizzato **Replace**. In questo modo l'entità viene completamente sostituita nel server, a meno che non sia stata modificata da quando è stata recuperata. In questo caso, infatti, l'operazione non viene eseguita per impedire all'applicazione di sovrascrivere inavvertitamente una modifica effettuata tra il recupero e l'aggiornamento da parte di un altro componente dell'applicazione. Per risolvere questo errore, recuperare di nuovo l'entità, apportare le modifiche, se ancora valide, quindi eseguire un'altra operazione **Replace**. La sezione successiva illustra come ovviare a questo comportamento.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity object.
    CustomerEntity updateEntity = (CustomerEntity)retrievedResult.Result;

    if (updateEntity != null)
	{
	   // Change the phone number.
	   updateEntity.PhoneNumber = "425-555-0105";

	   // Create the Replace TableOperation.
	   TableOperation updateOperation = TableOperation.Replace(updateEntity);

	   // Execute the operation.
	   table.Execute(updateOperation);

	   Console.WriteLine("Entity updated.");
	}

	else
	   Console.WriteLine("Entity could not be retrieved.");

## Inserire o sostituire un'entità

Le operazioni **Replace** non vengono eseguite se l'entità è stata modificata rispetto a quando è stata recuperata dal server. Per la corretta esecuzione dell’operazione **Replace** è inoltre necessario recuperare prima l'entità dal server. In alcuni casi, tuttavia, non è noto se l'entità è già esistente nel server e i valori in essa archiviati sono irrilevanti, pertanto devono essere sovrascritti completamente dall'aggiornamento. A tale scopo, è necessario utilizzare un'operazione **InsertOrReplace**. Questa operazione inserisce l'entità se non è già esistente oppure la sostituisce se esiste già, indipendentemente dalla data dell'ultimo aggiornamento. Nell'esempio di codice seguente l'entità customer per Ben Smith viene comunque recuperata, ma viene salvata di nuovo nel server utilizzando **InsertOrReplace**. Tutte le modifiche apportate all'entità tra le operazioni di recupero e aggiornamento verranno sovrascritte.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity object.
    CustomerEntity updateEntity = (CustomerEntity)retrievedResult.Result;

    if (updateEntity != null)
	{
	   // Change the phone number.
	   updateEntity.PhoneNumber = "425-555-1234";

	   // Create the InsertOrReplace TableOperation.
	   TableOperation insertOrReplaceOperation = TableOperation.InsertOrReplace(updateEntity);

	   // Execute the operation.
	   table.Execute(insertOrReplaceOperation);

	   Console.WriteLine("Entity was updated.");
	}

	else
	   Console.WriteLine("Entity could not be retrieved.");

## Eseguire query su un subset di proprietà di entità

Una query tabella consente di recuperare alcune proprietà da un'entità, ma non tutte. Questa tecnica, denominata proiezione, consente di ridurre la larghezza di banda e di migliorare le prestazioni della query, in particolare per entità di grandi dimensioni. La query nel codice seguente restituisce solo gli indirizzi di posta elettronica di entità nella tabella. A tale scopo viene utilizzata una query di **DynamicTableEntity** e anche **EntityResolver**. Per ulteriori informazioni sulla proiezione, vedere [il post di BLOG Introduzione di Upsert e proiezione di query][]. Si noti che la proiezione non è supportata nell'emulatore di archiviazione locale, pertanto questo codice viene eseguito solo se si usa un account sul servizio tabelle.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Define the query, and select only the Email property.
    TableQuery<DynamicTableEntity> projectionQuery = new TableQuery<DynamicTableEntity>().Select(new string[] { "Email" });

    // Define an entity resolver to work with the entity after retrieval.
    EntityResolver<string> resolver = (pk, rk, ts, props, etag) => props.ContainsKey("Email") ? props["Email"].StringValue : null;

    foreach (string projectedEmail in table.ExecuteQuery(projectionQuery, resolver, null, null))
    {
        Console.WriteLine(projectedEmail);
    }

## Eliminare un'entità

Per eliminare facilmente un'entità dopo averla recuperata, è possibile utilizzare lo stesso modello illustrato per aggiornare un'entità. Il codice seguente consente di recuperare ed eliminare un'entità customer.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that expects a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity.
    CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

    // Create the Delete TableOperation.
	if (deleteEntity != null)
	{
	   TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

	   // Execute the operation.
	   table.Execute(deleteOperation);

	   Console.WriteLine("Entity deleted.");
	}

	else
	   Console.WriteLine("Could not retrieve the entity.");

## Eliminare una tabella

L'esempio di codice seguente consente infine di eliminare una tabella dall'account di archiviazione. Una tabella eliminata non potrà essere creata nuovamente per un certo periodo di tempo.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Delete the table it if exists.
    table.DeleteIfExists();

## Recuperare entità nelle pagine in modo asincrono

Se si legge un numero elevato di entità e si desidera elaborare/visualizzare le entità man mano che vengono recuperate anziché attendere che vengano tutte restituite, è possibile recuperare le entità utilizzando una query segmentata. In questo esempio viene spiegato come restituire i risultati nelle pagine utilizzando il modello Async-Await, in modo che l'esecuzione non venga bloccata durante l'attesa della restituzione di un set di risultati di grandi dimensioni. Per ulteriori informazioni sull'utilizzo del modello Async-Await in .NET, vedere [Programmazione asincrona con Async e Await (C# e Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx).

    // Initialize a default TableQuery to retrieve all the entities in the table.
    TableQuery<CustomerEntity> tableQuery = new TableQuery<CustomerEntity>();

    // Initialize the continuation token to null to start from the beginning of the table.
    TableContinuationToken continuationToken = null;

    do
    {
        // Retrieve a segment (up to 1,000 entities).
        TableQuerySegment<CustomerEntity> tableQueryResult =
            await table.ExecuteQuerySegmentedAsync(tableQuery, continuationToken);

        // Assign the new continuation token to tell the service where to
        // continue on the next iteration (or null if it has reached the end).
        continuationToken = tableQueryResult.ContinuationToken;

        // Print the number of rows retrieved.
        Console.WriteLine("Rows retrieved {0}", tableQueryResult.Results.Count);

    // Loop until a null continuation token is received, indicating the end of the table.
    } while(continuationToken != null);

## Passaggi successivi

A questo punto, dopo aver appreso le nozioni di base dell'archiviazione tabelle, visitare i collegamenti seguenti per altre informazioni sulle attività di archiviazione più complesse:

- Per altri esempi di archivio tabelle, vedere [Getting Started with Azure Table Storage in .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/) (Introduzione all'archivio tabelle di Azure in .NET)
- Per informazioni dettagliate sulle API disponibili, vedere la documentazione di riferimento del servizio tabelle:
    - [Informazioni di riferimento sulla libreria client di archiviazione per .NET](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
    - [Informazioni di riferimento sulle API REST](http://msdn.microsoft.com/library/azure/dd179355)
- Per altre informazioni su come semplificare il codice scritto da usare con Archiviazione di Azure, vedere [Informazioni su Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-get-started.md).
- Per ulteriori opzioni di archiviazione dei dati in Azure, consultare altre guide alle funzionalità.
    - Per archiviare dati non strutturati, vedere [Introduzione all'archivio BLOB di Azure con .NET](storage-dotnet-how-to-use-blobs.md).
    - Per archiviare dati relazionali, vedere [Connettersi al database SQL tramite .NET (C#)](sql-database-dotnet-how-to-use.md).

  [Download and install the Azure SDK for .NET]: /develop/net/
  [Creating an Azure Project in Visual Studio]: http://msdn.microsoft.com/library/azure/ee405487.aspx

  [Blob5]: ./media/storage-dotnet-how-to-use-table-storage/blob5.png
  [Blob6]: ./media/storage-dotnet-how-to-use-table-storage/blob6.png
  [Blob7]: ./media/storage-dotnet-how-to-use-table-storage/blob7.png
  [Blob8]: ./media/storage-dotnet-how-to-use-table-storage/blob8.png
  [Blob9]: ./media/storage-dotnet-how-to-use-table-storage/blob9.png

  [il post di BLOG Introduzione di Upsert e proiezione di query]: http://blogs.msdn.com/b/windowsazurestorage/archive/2011/09/15/windows-azure-tables-introducing-upsert-and-query-projection.aspx
  [.NET Client Library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
  [Azure Storage Team blog]: http://blogs.msdn.com/b/windowsazurestorage/
  [Configure Azure Storage connection strings]: http://msdn.microsoft.com/library/azure/ee758697.aspx
  [OData]: http://nuget.org/packages/Microsoft.Data.OData/5.0.2
  [Edm]: http://nuget.org/packages/Microsoft.Data.Edm/5.0.2
  [Spatial]: http://nuget.org/packages/System.Spatial/5.0.2
  [How to: Programmatically access Table storage]: #tablestorage

<!---HONumber=AcomDC_0921_2016-->