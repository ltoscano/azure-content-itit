<properties
	pageTitle="Gestire immagini personalizzate di Azure DevTest Labs per creare macchine virtuali | Microsoft Azure"
	description="Informazioni su come creare un'immagine personalizzata da un file VHD o da una macchina virtuale esistente in Azure DevTest Labs"
	services="devtest-lab,virtual-machines"
	documentationCenter="na"
	authors="tomarcher"
	manager="douge"
	editor=""/>

<tags
	ms.service="devtest-lab"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/07/2016"
	ms.author="tarcher"/>

# Gestire immagini personalizzate di Azure DevTest Labs per creare macchine virtuali

In Azure DevTest Labs, le immagini personalizzate consentono di creare rapidamente VM, senza dover attendere l'installazione di tutto il software necessario nel computer di destinazione. Le immagini personalizzate consentono di preinstallare tutto il software necessario in un file VHD da utilizzare per creare una VM. Poiché il software è già installato, creare la VM richiede molto meno tempo. Inoltre, con le immagini personalizzate è possibile clonare le VM, creando l'immagine personalizzata da una VM e usandola quindi per creare le VM.

In questo articolo viene spiegato come:

- [Creare un'immagine personalizzata da un file VHD](#create-a-custom-image-from-a-vhd-file) e usarla per creare una VM.
- [Creare un'immagine personalizzata da una VM](#create-a-custom-image-from-a-vm) per clonare velocemente una VM.

## Creare un'immagine personalizzata da un file VHD

Questa sezione descrive come creare un'immagine personalizzata da un file VHD. Per eseguire tutti i passaggi di questa sezione, è necessario accedere a un file VHD valido.


1. Accedere al [portale di Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Selezionare **Altri servizi** e quindi **DevTest Labs** dall'elenco.

1. Nell'elenco dei lab selezionare il lab desiderato.

1. Nel pannello del lab selezionare **Configurazione**.

1. Nel pannello **Configurazione** del lab selezionare **Immagini personalizzate**.

1. Nel pannello **Immagini personalizzate** selezionare **+ Immagine personalizzata**.

    ![Aggiungere un'immagine personalizzata](./media/devtest-lab-create-template/add-custom-image.png)

1. Immettere il nome dell'immagine personalizzata. Il nome viene visualizzato nell'elenco delle immagini di base durante la creazione di una VM.

1. Immettere la descrizione dell'immagine personalizzata. La descrizione viene visualizzata nell'elenco delle immagini di base durante la creazione di una VM.

1. Selezionare **File VHD**.

1. Se si ha accesso a un file VHD che non è elencato, aggiungerlo seguendo le istruzioni della sezione [Caricare un file VHD](#upload-a-vhd-file) e tornare qui al termine.

1. Selezionare il file VHD desiderato.

1. Selezionare **OK** per chiudere il pannello **File VHD**.

1. Selezionare **Configurazione sistema operativo**.

1. Nella scheda **Configurazione sistema operativo** selezionare **Windows** o **Linux**.

1. Se si seleziona **Windows**, usare la casella di controllo per specificare se *Sysprep* è stato eseguito nel computer.

1. Selezionare **OK** per chiudere il pannello **Configurazione sistema operativo**.

1. Selezionare **OK** per creare l'immagine personalizzata.

1. Passare alla sezione [Passaggi successivi](#next-steps).

###Caricare un file VHD

Per aggiungere un'immagine personalizzata, è necessario accedere a un file VHD.

1. Nel pannello **File VHD** selezionare l'opzione **Carica un disco rigido virtuale con PowerShell**.

    ![Caricare un'immagine](./media/devtest-lab-create-template/upload-image-using-psh.png)

1. Il pannello successivo visualizzerà le istruzioni per la modifica e l'esecuzione di uno script di PowerShell che carica un file VHD nella sottoscrizione di Azure. **Nota:** questo processo può richiedere molto tempo a seconda delle dimensioni del file VHD e della velocità della connessione.

## Creare un'immagine personalizzata da una macchina virtuale
Se è già stata configurata una VM, è possibile creare un'immagine personalizzata da tale VM e quindi usare l'immagine personalizzata per creare altre VM identiche. I passaggi seguenti illustrano come creare un'immagine personalizzata da una macchina virtuale:

1. Accedere al [portale di Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Selezionare **Altri servizi** e quindi **DevTest Labs** dall'elenco.

1. Nell'elenco dei lab selezionare il lab desiderato.

1. Nel pannello del lab selezionare **My virtual machines** (Macchine virtuali personali).
 
1. Nel pannello **My virtual machines** (Macchine virtuali personali) del lab selezionare la VM da cui creare l'immagine personalizzata.

1. Nel pannello della macchina virtuale selezionare **Crea immagine personalizzata (disco rigido virtuale)**.

	![Voce di menu Crea immagine personalizzata](./media/devtest-lab-create-template/create-custom-image.png)

1. Nel pannello **Immagine personalizzata** immettere un nome e una descrizione per l'immagine personalizzata. Queste informazioni vengono visualizzate nell'elenco delle immagini di base quando si crea una VM.

	![Pannello Crea immagine personalizzata](./media/devtest-lab-create-template/create-custom-image-blade.png)

1. Indicare se è stato eseguito sysprep nella macchina virtuale. Se non è stato eseguito sysprep nella VM, specificare se si desidera eseguirlo quando si crea una VM da questa immagine personalizzata.

1. Selezionare **OK** al termine per creare l'immagine personalizzata.

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## Post di blog correlati

- [Custom images or formulas? (Immagini personalizzate o formule?)](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Copying Custom Images between Azure DevTest Labs (Copia di immagini personalizzate tra istanze di Azure DevTest Labs)](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

##Passaggi successivi

Dopo aver aggiunto un'immagine personalizzata da usare durante la creazione di una macchina virtuale, il passaggio successivo consiste nell'[aggiungere una macchina virtuale al lab](./devtest-lab-add-vm-with-artifacts.md).

<!---HONumber=AcomDC_0907_2016-->