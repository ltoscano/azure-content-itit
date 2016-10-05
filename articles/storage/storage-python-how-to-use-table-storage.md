<properties
	pageTitle="Come usare l'archiviazione tabelle da Python | Microsoft Azure"
	description="Archiviare dati non strutturati nel cloud con il servizio di archiviazione tabelle di Azure, ovvero un archivio dati NoSQL."
	services="storage"
	documentationCenter="python"
	authors="tamram"
	manager="carmonm"
	editor="tysonn"/> 

<tags
	ms.service="storage"
	ms.workload="storage"
	ms.tgt_pltfrm="na"
	ms.devlang="python"
	ms.topic="article"
	ms.date="09/20/2016"
	ms.author="gusapost;tamram"/> 


# Come usare l'archiviazione tabelle da Python

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)] <br/> [AZURE.INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## Panoramica

In questa guida viene illustrato come eseguire scenari comuni utilizzando il servizio di archiviazione tabelle di Azure. Gli esempi sono scritti in Python e usano [Microsoft Azure Storage SDK per Python]. Gli scenari presentati includono la creazione ed eliminazione di una tabella, oltre all’inserimento di entità ed esecuzione di query sulle entità in una tabella.

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## Creare una tabella

L'oggetto **TableService** consente di utilizzare i servizi tabelle. Il codice seguente consente di creare un oggetto **TableService**. Aggiungere il codice seguente vicino all'inizio del file Python da cui si desidera accedere all'archiviazione di Azure a livello di codice:

	from azure.storage.table import TableService, Entity

Il codice seguente crea un oggetto **TableService** usando il nome dell'account di archiviazione e la chiave dell'account. Sostituire 'myaccount' e 'mykey' con l'account e la chiave da usare.

	table_service = TableService(account_name='myaccount', account_key='mykey')

	table_service.create_table('tasktable')

## Aggiungere un'entità a una tabella

Per aggiungere un'entità, creare prima un dizionario o un'entità che definisca i nomi e i valori della proprietà dell'entità. Si noti che per ogni entità è necessario specificare un oggetto **PartitionKey** e un oggetto **RowKey**. Questi sono identificatori univoci delle entità. Con questi valori è possibile eseguire query molto più rapidamente rispetto a quanto sia possibile fare per altre proprietà. Il sistema utilizza **PartitionKey** per distribuire automaticamente le entità della tabella su molti nodi di archiviazione. Le entità con lo stesso oggetto **PartitionKey** vengono archiviate nello stesso nodo. **RowKey** è l'ID univoco dell'entità all'interno della partizione cui appartiene.

Per aggiungere un'entità alla tabella, passare un oggetto dizionario al metodo **insert\_entity**.

	task = {'PartitionKey': 'tasksSeattle', 'RowKey': '1', 'description' : 'Take out the trash', 'priority' : 200}
	table_service.insert_entity('tasktable', task)

È anche possibile passare un'istanza della classe **Entity** al metodo **insert\_entity**.

	task = Entity()
	task.PartitionKey = 'tasksSeattle'
	task.RowKey = '2'
	task.description = 'Wash the car'
	task.priority = 100
	table_service.insert_entity('tasktable', task)

## Aggiornare un'entità

Questo codice indica come sostituire la versione precedente di un'entità esistente con una versione aggiornata.

	task = {'PartitionKey': 'tasksSeattle', 'RowKey': '1', 'description' : 'Take out the garbage', 'priority' : 250}
	table_service.update_entity('tasktable', task)

Se l'entità da aggiornare non esiste, l'operazione di aggiornamento non riuscirà. Se si desidera archiviare un'entità indipendentemente dalla sua precedente esistenza, utilizzare **insert\_or\_replace\_entity**. Nell'esempio seguente, la prima chiamata sostituirà l'entità esistente. La seconda chiamata inserirà una nuova entità, poiché nella tabella non esiste alcuna entità con oggetto **PartitionKey** ed **RowKey**.

	task = {'PartitionKey': 'tasksSeattle', 'RowKey': '1', 'description' : 'Take out the garbage again', 'priority' : 250}
	table_service.insert_or_replace_entity('tasktable', task)

	task = {'PartitionKey': 'tasksSeattle', 'RowKey': '3', 'description' : 'Buy detergent', 'priority' : 300}
	table_service.insert_or_replace_entity('tasktable', task)

## Modificare un gruppo di entità

È talvolta consigliabile inviare più operazioni in un batch per garantire l'elaborazione atomica da parte del server. A tale scopo, usare la classe **TableBatch**. Quando si desidera inviare il batch, si chiamerà **commit\_batch**. Si noti che, affinché sia possibile modificarle come batch, tutte le entità devono trovarsi nella stessa partizione. Nell'esempio seguente vengono aggiunte due entità assieme in un batch.

	from azure.storage.table import TableBatch
	batch = TableBatch()
	task10 = {'PartitionKey': 'tasksSeattle', 'RowKey': '10', 'description' : 'Go grocery shopping', 'priority' : 400}
	task11 = {'PartitionKey': 'tasksSeattle', 'RowKey': '11', 'description' : 'Clean the bathroom', 'priority' : 100}
	batch.insert_entity(task10)
	batch.insert_entity(task11)
	table_service.commit_batch('tasktable', batch)

I batch possono essere usati anche con la sintassi di gestione contesto:

	task12 = {'PartitionKey': 'tasksSeattle', 'RowKey': '12', 'description' : 'Go grocery shopping', 'priority' : 400}
	task13 = {'PartitionKey': 'tasksSeattle', 'RowKey': '13', 'description' : 'Clean the bathroom', 'priority' : 100}

	with table_service.batch('tasktable') as batch:
		batch.insert_entity(task12)
		batch.insert_entity(task13)


## Eseguire una query su un'entità

Per eseguire una query su un'entità in una tabella, utilizzare il metodo **get\_entity** passando i parametri **PartitionKey** e **RowKey**.

	task = table_service.get_entity('tasktable', 'tasksSeattle', '1')
	print(task.description)
	print(task.priority)

## Eseguire query su un set di entità

In questo esempio vengono individuate tutte le attività di Seattle basate su **PartitionKey**.

	tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'")
	for task in tasks:
		print(task.description)
		print(task.priority)

## Eseguire query su un subset di proprietà di entità

Mediante una query su una tabella è possibile recuperare solo alcune proprietà da un'entità. Questa tecnica, denominata *proiezione*, consente di ridurre la larghezza di banda e di migliorare le prestazioni della query, in particolare per entità di grandi dimensioni. Utilizzare il parametro **select** e passare i nomi delle proprietà da inoltrare al client.

La query nel codice seguente restituisce solo le descrizioni delle entità nella tabella.

[AZURE.NOTE] Il frammento seguente funziona solo su un servizio di archiviazione cloud. Non è supportato dall'emulatore di archiviazione.

	tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'", select='description')
	for task in tasks:
		print(task.description)

## Eliminare un'entità

È possibile eliminare un'entità usando le relative chiavi di partizione e di riga.

	table_service.delete_entity('tasktable', 'tasksSeattle', '1')

## Eliminare una tabella

Nell'esempio di codice seguente viene illustrato come eliminare una tabella da un account di archiviazione.

	table_service.delete_table('tasktable')

## Passaggi successivi

A questo punto, dopo avere appreso le nozioni di base dell'archiviazione tabelle, visitare i collegamenti seguenti per altre informazioni.

- [Centro per sviluppatori di Python](/develop/python/)
- [API REST dei servizi di archiviazione di Azure](http://msdn.microsoft.com/library/azure/dd179355)
- [Blog del team di Archiviazione di Azure]
- [Microsoft Azure Storage SDK per Python]

[Blog del team di Archiviazione di Azure]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure Storage SDK per Python]: https://github.com/Azure/azure-storage-python

<!---HONumber=AcomDC_0921_2016-->