<properties
	pageTitle="Cos'è l'insieme di credenziali chiave di Azure? | Microsoft Azure"
	description="L'insieme di credenziali chiave di Azure consente di proteggere le chiavi e i segreti di crittografia usati da servizi e applicazioni cloud. Con l'insieme di credenziali chiave di Azure i clienti possono crittografare chiavi e segreti (ad esempio, chiavi di autenticazione, chiavi dell'account di archiviazione, chiavi di crittografia dati, file PFX e password) usando chiavi protette da moduli di protezione hardware (HSM)."
	services="key-vault"
	documentationCenter=""
	authors="cabailey"
	manager="mbaldwin"
	tags="azure-resource-manager"/>

<tags
	ms.service="key-vault"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="09/27/2016"
	ms.author="cabailey"/>



# Cos'è l'insieme di credenziali chiave di Azure?

L'insieme di credenziali delle chiavi di Azure è disponibile nella maggior parte delle aree. Per altre informazioni, vedere la [pagina Insieme di credenziali delle chiavi - Prezzi](https://azure.microsoft.com/pricing/details/key-vault/).

## Introduzione

L'insieme di credenziali chiave di Azure consente di proteggere le chiavi e i segreti di crittografia usati da servizi e applicazioni cloud. Con l'insieme di credenziali chiave è possibile crittografare chiavi e segreti (ad esempio, chiavi di autenticazione, chiavi dell'account di archiviazione, chiavi di crittografia dati, file PFX e password) usando chiavi protette da moduli di protezione hardware (HSM). Per una maggiore sicurezza, è possibile importare o generare le chiavi in moduli di protezione hardware. Se si sceglie di eseguire questa operazione, Microsoft elabora le chiavi in moduli di protezione hardware FIPS 140-2 livello 2 convalidati (hardware e firmware).

L'insieme di credenziali chiave semplifica il processo di gestione delle chiavi e consente di mantenere il controllo delle chiavi che accedono ai dati e li crittografano. Gli sviluppatori possono creare chiavi per lo sviluppo e il test in pochi minuti e quindi eseguirne facilmente la migrazione alle chiavi di produzione. Gli amministratori della sicurezza possono concedere (e revocare) le autorizzazioni per chiavi, in base alle esigenze.

La seguente tabella permette di capire meglio come l'insieme di credenziali chiave aiuti a soddisfare le esigenze degli sviluppatori e degli amministratori della sicurezza.





| Ruolo | Presentazione del problema | Soluzione offerta dall'insieme di credenziali chiave di Azure |
| ------------- |-------------|-----|
| Sviluppatore di un'applicazione Azure | "Voglio scrivere un'applicazione per Azure che usi chiavi per la firma e la crittografia, che però dovranno essere esterne all'applicazione, in modo che la soluzione sia adatta a un'applicazione geograficamente distribuita. <br/><br/>Voglio anche che queste chiavi e questi segreti siano protetti, senza dover scrivere manualmente il codice, e voglio poterli usare facilmente dall'applicazione ottenendo prestazioni ottimali." | √ Le chiavi vengono archiviate in un insieme di credenziali e richiamate dall'URI quando è necessario.<br/><br/> √ Le chiavi sono protette da Azure con algoritmi, lunghezze di chiave e moduli di protezione hardware standard del settore.<br/><br/> √ Le chiavi vengono elaborate in moduli di protezione hardware che risiedono negli stessi data center di Azure in cui si trovano le applicazioni. In questo modo si ottiene una migliore affidabilità e una latenza ridotta rispetto a quando le chiavi si trovano in una posizione diversa, ad esempio in locale.|
| Sviluppatore di software come un servizio (SaaS) |"Non voglio essere in alcun modo responsabile delle chiavi e dei segreti dei tenant dei miei clienti. <br/><br/>Voglio che siano i clienti a possedere e gestire le chiavi per potermi concentrare su ciò che so fare meglio, ovvero fornire le principali funzionalità del software." | √ I clienti possono importare le loro chiavi in Azure e gestirle. Quando un'applicazione SaaS deve eseguire operazioni di crittografia usando le chiavi dei clienti, è l'insieme di credenziali delle chiavi a eseguirle per conto dell'applicazione. L'applicazione non vede le chiavi dei clienti.|
| Responsabile della sicurezza | "Voglio la certezza che le applicazioni siano conformi ai moduli di protezione hardware FIPS 140-2 livello 2 per la gestione delle chiavi protetta. <br/><br/>Voglio assicurarmi che la mia organizzazione abbia il controllo del ciclo di vita delle chiavi e possa monitorare l'utilizzo delle chiavi. <br/><br/>Anche se usiamo più servizi e risorse di Azure, voglio gestire le chiavi da una sola posizione in Azure. |√ I moduli di protezione hardware hanno la certificazione FIPS 140-2 livello 2.<br/><br/>√ L'insieme di credenziali delle chiavi è progettato in modo che Microsoft non possa vedere né estrarre le chiavi.<br/><br/>√ Registrazione dell'utilizzo delle chiavi in tempo quasi reale <br/><br/>√ L'insieme di credenziali offre un'unica interfaccia, indipendentemente dal numero di insiemi di credenziali disponibili in Azure, dalle aree supportate e dalle applicazioni che li usano. |


Chiunque abbia una sottoscrizione di Azure può creare e usare insiemi di credenziali delle chiavi. Anche se l'insieme di credenziali chiave è un vantaggio per sviluppatori e amministratori della sicurezza, può essere implementato e gestito dall'amministratore di un'organizzazione che gestisce altri servizi di Azure per un'organizzazione. Ad esempio, questo amministratore potrebbe accedere con una sottoscrizione di Azure, creare un insieme di credenziali per l'organizzazione in cui archiviare le chiavi e quindi essere responsabile di attività operative, ad esempio:

+ Creare o importare una chiave o un segreto
+ Revocare o eliminare una chiave o un segreto
+ Autorizzare gli utenti o le applicazioni per l'accesso all'insieme di credenziali delle chiavi, per consentire la gestione o l'uso delle chiavi e dei segreti corrispondenti
+ Configurare l'utilizzo delle chiavi (ad esempio, la firma o la crittografia)
+ Monitorare l'utilizzo delle chiavi

L'amministratore quindi fornirebbe agli sviluppatori gli URI da chiamare dalle applicazioni e fornirebbe all'amministrazione della sicurezza le informazioni sulla registrazione dell'utilizzo delle chiavi.

   ![Panoramica dell'insieme di credenziali chiave di Azure][1]

Gli sviluppatori possono gestire le chiavi anche direttamente, usando le API. Per altre informazioni, vedere la [Guida per gli sviluppatori dell'insieme di credenziali delle chiavi di Azure](key-vault-developers-guide.md).

## Passaggi successivi

Per un'esercitazione introduttiva per gli amministratori, vedere [Introduzione all'insieme di credenziali chiave di Azure](key-vault-get-started.md).

Per altre informazioni sulla registrazione dell'utilizzo per l'insieme di credenziali delle chiavi, vedere [Registrazione dell'insieme di credenziali delle chiavi di Azure](key-vault-logging.md).

Per altre informazioni sull'uso di chiavi e segreti con l'insieme di credenziali delle chiavi di Azure, vedere [About Keys, Secrets, and Certificates](https://msdn.microsoft.com/library/azure/dn903623(v=azure.1).aspx) (Informazioni su chiavi, segreti e certificati).


<!--Image references-->
[1]: ./media/key-vault-whatis/AzureKeyVault_overview.png

<!---HONumber=AcomDC_0928_2016-->