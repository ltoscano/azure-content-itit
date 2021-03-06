<properties
   pageTitle="Problemi di riavvio o ridimensionamento della VM | Microsoft Azure"
   description="Risolvere i problemi della distribuzione Resource Manager con il riavvio e il ridimensionamento di una macchina virtuale Linux esistente in Azure"
   services="virtual-machines-linux, azure-resource-manager"
   documentationCenter=""
   authors="Deland-Han"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>

<tags
   ms.service="virtual-machines-linux"
   ms.topic="support-article"
   ms.tgt_pltfrm="vm-linux"
   ms.devlang="na"
   ms.workload="required"
   ms.date="09/09/2016"
   ms.author="delhan"/>

# Risolvere i problemi della distribuzione Resource Manager con il riavvio e il ridimensionamento di una macchina virtuale Linux esistente in Azure

Quando si prova ad avviare una macchina virtuale (VM) di Azure arrestata o se ne ridimensiona una esistente, l'errore comune che si verifica è un errore di allocazione. L'errore si verifica quando il cluster o l'area non ha risorse disponibili o non può supportare le dimensioni della VM richieste.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## Raccogliere log di controllo

Per avviare la risoluzione dei problemi, raccogliere i log di controllo per identificare l'errore associato al problema. I collegamenti seguenti contengono informazioni dettagliate sul processo:

[Risoluzione dei problemi relativi alle distribuzioni di gruppi di risorse con il portale di Azure](../resource-manager-troubleshoot-deployments-portal.md)

[Operazioni di controllo con Gestione risorse](../resource-group-audit.md)

## Problema: Errore durante l'avvio di una VM arrestata

Si prova ad avviare una VM arrestata ma viene visualizzato un errore di allocazione.

### Causa

La richiesta di avvio della VM arrestata deve essere eseguita nel cluster originale che ospita il servizio cloud. Tuttavia, il cluster non ha spazio disponibile per soddisfare la richiesta.

### Risoluzione

*	Arrestare tutte le VM nel set di disponibilità e quindi riavviare ogni VM.

  1. Fare clic su **Gruppi di risorse** > _gruppo di risorse personale_ > **Risorse** > _set di disponibilità personale_ > **Macchine virtuali** > _macchina virtuale personale_ > **Arresta**.

  2. Dopo l'arresto di tutte le VM, selezionare le VM arrestate e fare clic su Avvia.

*	Ripetere la richiesta di riavvio in un secondo momento.

## Problema: Errore durante il ridimensionamento di una VM esistente

Si prova a ridimensionare una VM esistente ma viene visualizzato un errore di allocazione.

### Causa

La richiesta di ridimensionamento della VM deve essere eseguita nel cluster originale che ospita il servizio cloud. Tuttavia, il cluster non supporta le dimensioni della VM richieste.

### Risoluzione

* Ripetere la richiesta usando una VM di dimensioni inferiori.

* Se le dimensioni della VM richieste non possono essere modificate:

  1. Arrestare tutte le VM nel set di disponibilità.

    * Fare clic su **Gruppi di risorse** > _gruppo di risorse personale_ > **Risorse** > _set di disponibilità personale_ > **Macchine virtuali** > _macchina virtuale personale_ > **Arresta**.

  2. Dopo l'arresto di tutte le VM, ridimensionare la VM desiderata impostando una dimensione maggiore.
  3. Selezionare la VM ridimensionata e fare clic su **Avvia**, quindi avviare ognuna delle VM arrestate.

## Passaggi successivi

Se si verificano problemi durante la creazione di una nuova VM Linux in Azure, vedere [Risolvere i problemi della distribuzione classica con la creazione di una nuova macchina virtuale Linux in Azure](../virtual-machines/virtual-machines-linux-troubleshoot-deployment-new-vm.md).

<!---HONumber=AcomDC_0914_2016-->