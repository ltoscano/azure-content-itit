##Soluzioni a disponibilità elevata con Gestione traffico di Azure

È necessario determinare se i requisiti di disponibilità elevata del carico di lavoro possono essere soddisfatti usando esclusivamente Gestione traffico di Azure oppure se è necessario combinare Gestione traffico con altre soluzioni o processi DNS. A seconda delle esigenze, è possibile usare:

- **Esclusivamente Gestione traffico **. Se per il carico di lavoro è sufficiente un tempo di attività del 99,99%, è possibile usare esclusivamente Gestione traffico. In caso di errore nel servizio di gestione traffico, gli utenti non saranno in grado di accedere al carico di lavoro fino a quando il servizio di gestione traffico non verrà ristabilito.

- **Usare un'altra soluzione di gestione traffico insieme a Gestione traffico di Azure**. In caso di errore nel servizio di gestione traffico, è possibile modificare il record CNAME in modo da puntare all'altro servizio di gestione traffico. L'accesso al carico di lavoro è ancora disponibile e distribuito in tutte le posizioni che ospitano il carico di lavoro. Questa è la soluzione più costosa, ma potrebbe essere necessaria per i carichi di lavoro che richiedono un contratto di servizio superiore.
