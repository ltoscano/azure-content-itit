I dati dell'account di archiviazione di Microsoft Azure vengono sempre replicati per assicurarne durabilità e disponibilità elevata, rispettando il [contratto di servizio di Archiviazione](https://azure.microsoft.com/support/legal/sla/storage/) anche in caso di errori hardware temporanei. Quando si crea un account di archiviazione, è necessario selezionare una delle opzioni di replica seguenti:

- **Archiviazione con ridondanza locale (LRS).** Con l'archiviazione con ridondanza locale vengono conservate tre copie dei dati. Tale tipo di archiviazione viene replicato per tre volte all'interno di una singola struttura di una singola area. Questa opzione di replica protegge i dati dai normali errori hardware ma non dagli errori di una singola struttura.  
  
	L'archiviazione con ridondanza locale viene offerta a tariffe scontate. Per la massima durabilità, è consigliabile usare l'archiviazione con ridondanza geografica descritta di seguito.


- **Archiviazione con ridondanza della zona (ZRS).** Con l'archiviazione con ridondanza della zona vengono conservate tre copie dei dati. Tale tipo di archiviazione viene replicato per tre volte in due o tre strutture all'interno di una singola area o in due aree, fornendo una durabilità maggiore rispetto all'archiviazione con ridondanza locale. Questa opzione di replica assicura la durabilità dei dati all'interno di una singola area.

	L'archiviazione con ridondanza della zona fornisce un livello di durabilità maggiore rispetto all'archiviazione con ridondanza locale. Tuttavia, per la massima durabilità, è consigliabile usare l'archiviazione con ridondanza geografica descritta di seguito.

	> [AZURE.NOTE] ZRS è attualmente disponibile solo per i BLOB in blocchi ed è supportata solo nelle versioni 2014-02-14 e versioni successive.
	> 
	> Dopo aver creato l'account di archiviazione e selezionato l'archiviazione con ridondanza della zona, non è possibile convertirlo per usarlo con un altro tipo di replica o viceversa.

- **Archiviazione con ridondanza geografica (GRS)**. L'archiviazione con ridondanza geografica è abilitata per impostazione predefinita quando si crea l'account di archiviazione. Con tale tipo di archiviazione vengono conservate sei copie dei dati. Con questa opzione di replica, i dati vengono replicati per tre volte all'interno dell'area primaria e per tre volte in un'area secondaria situata a centinaia di chilometri di distanza dall'area primaria, fornendo il massimo livello di durabilità. In caso di errore nell'area primaria, il servizio di archiviazione di Azure eseguirà il failover all'area secondaria. L'archiviazione con ridondanza geografica assicura la durabilità dei dati in due aree distinte.


- **Archiviazione con ridondanza geografica e accesso in lettura (RA-GRS)**. L'archiviazione con ridondanza geografica e accesso in lettura replica i dati in una posizione geografica secondaria e fornisce inoltre l'accesso in lettura ai dati nella posizione secondaria. È possibile accedere ai dati dalla posizione primaria o secondaria, qualora una di queste diventi non disponibile.

	> [AZURE.IMPORTANT] È possibile cambiare la modalità di replica dei dati dopo la creazione dell'account di archiviazione, a meno che non sia stata specificata l'archiviazione con ridondanza della zona al momento della creazione dell'account. Si noti tuttavia che se si passa dall'archiviazione con ridondanza locale all'archiviazione con ridondanza geografica o all'archiviazione con ridondanza geografica e accesso in lettura, potrebbe essere addebitato un costo una tantum per il trasferimento dei dati.
 
Per altri dettagli sulle opzioni di replica di archiviazione, vedere [Replica di Archiviazione di Azure](../articles/storage/storage-redundancy.md).

Per informazioni sui prezzi per la replica dell'account di archiviazione, vedere [Prezzi di Archiviazione di Azure](https://azure.microsoft.com/pricing/details/storage/).

Per i dettagli architetturali sulla durabilità con Archiviazione di Azure, vedere [Paper SOSP - Archiviazione di Microsoft Azure: Un servizio di archiviazione cloud a elevata disponibilità con coerenza assoluta](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).

<!---HONumber=AcomDC_0309_2016-->