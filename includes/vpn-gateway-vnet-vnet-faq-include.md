- Le reti virtuali possono essere nelle sottoscrizioni uguale o diverse.

- Un servizio cloud o un endpoint di bilanciamento del carico non può estendersi tra reti virtuali, anche se sono connesse tra loro.

- Il collegamento di più reti virtuali di Azure non richiede alcun gateway VPN locale, a meno che la connettività cross-premise non sia obbligatoria.

- La connettività da VNet a VNet supporta la connessione di reti virtuali. Non supporta le macchine virtuali o servizi non in una rete virtuale cloud.

- La connettività tra reti virtuali richiede la presenza di gateway VPN con tipi VPN RouteBased (in precedenza denominato routing dinamico).

- La connettività di rete virtuale può essere usata contemporaneamente alle VPN multisito, con un massimo di 10 (gateway predefiniti/standard) o 30 (gateway ad alte prestazioni) tunnel VPN per un gateway VPN di rete virtuale che si connette ad altre reti virtuali o a siti locali.

- Gli spazi degli indirizzi delle reti virtuali e dei siti di rete locali non devono sovrapporsi. La sovrapposizione degli spazi di indirizzi causerà errori nella creazione di connessioni tra reti virtuali.

- Non sono supportati tunnel ridondanti tra una coppia di reti virtuali.

- Tutti i tunnel VPN della rete virtuale condividono la larghezza di banda disponibile sul gateway VPN di Azure e i tempi di servizio del gateway VPN dello stesso contratto di servizio in Azure.

- Il traffico tra reti virtuali passa per la rete Microsoft, non Internet.

- Il traffico tra reti virtuali all'interno della stessa area è gratuito in entrambe le direzioni, mentre per il traffico tra reti virtuali in aree diverse vengono applicate le tariffe di trasferimento dati in uscita tra reti virtuali in base alle aree di origine. Per altri dettagli, vedere [Prezzi di Gateway VPN](https://azure.microsoft.com/pricing/details/vpn-gateway/).

<!---HONumber=AcomDC_0720_2016-->