<properties
    pageTitle="Reimpostare l'accesso a VM Linux di Azure tramite l'estensione VMAccess | Microsoft Azure"
    description="Reimpostare l'accesso a VM Linux di Azure tramite l'estensione VMAccess"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"
/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/30/2016"
    ms.author="v-livech"
/>

# Gestire utenti, SSH e dischi di controllo o di ripristino in VM Linux di Azure tramite l'estensione VMAccess

Questo articolo illustra come usare l'estensione VMAccess di Azure per controllare o ripristinare un disco, reimpostare l'accesso utente, gestire gli account utente o reimpostare la configurazione dei dischi SSHD in Linux.

Prerequisiti: [un account Azure](https://azure.microsoft.com/pricing/free-trial/), [chiavi SSH pubbliche e private](virtual-machines-linux-mac-create-ssh-keys.md) e l'interfaccia della riga di comando di Azure installata e con la modalità Azure Resource Manager attivata tramite `azure config mode arm`.

## Comandi rapidi

Esistono due modi per usare VMAccess nelle VM Linux:

- Usare l'interfaccia della riga di comando di Azure e i parametri richiesti.
- Usare file JSON non elaborati che VMAccess elabora e su cui basa le sue operazioni.

Per la sezione dei comandi rapidi si userà il metodo `azure vm reset-access` dell'interfaccia della riga di comando di Azure. Negli esempi di comandi seguenti sostituire i valori contenenti "example" con i valori dell'ambiente locale.

## Creare un gruppo di risorse e una VM Linux

```bash
azure group create resourcegroupexample westus
```

```bash
azure vm quick-create \
-M ~/.ssh/id_rsa.pub \
-u userexample \
-g resourcegroupexample \
-l westus \
-y Linux \
-n debianexamplevm \
-Q Debian
```

## Reimpostare la password radice

Per reimpostare la password radice:

```bash
azure vm reset-access -g exampleResourceGroup -n exampleVMName -u root -p examplenewPassword
```

## Reimpostazione della chiave SSH

Per reimpostare la chiave SSH di un utente non ROOT:

```bash
azure vm reset-access -g exampleResourceGroup -n exampleVMName -u userexample -M ~/.ssh/id_rsa.pub
```

## Creare un utente

Per creare un utente:

```bash
azure vm reset-access -g exampleResourceGroup -n exampleVMName -u userexample -p examplePassword
```

## Rimuovere un utente

```bash
azure vm reset-access -g exampleResourceGroup -n exampleVMName -R userexample
```

## Reimpostare un disco SSHD

Per reimpostare la configurazione di un disco SSHD:

```bash
azure vm reset-access -g exampleResourceGroup -n exampleVMName -r
```


## Procedura dettagliata

### Estensione VMAccess definita:

Il disco della VM Linux genera errori. In qualche modo la password radice della VM Linux è stata reimpostata o la chiave privata SSH è stata eliminata accidentalmente. In passato, quando nel data center si verificava questa situazione, era necessario accedere all'unità e quindi aprire il KVM per raggiungere la console del server. L'estensione VMAccess di Azure può essere concepita come il commutatore tastiera, video e mouse che consente di accedere alla console per reimpostare l'accesso a Linux o eseguire la manutenzione a livello di disco.

Per la procedura dettagliata si userà la forma estesa di VMAccess, che usa file JSON non elaborati. Questi file JSON di VMAccess possono essere chiamati anche dai modelli di Azure.

### Uso di VMAccess per controllare o riparare il disco di una VM Linux

Con VMAccess è possibile eseguire il comando fsck sul disco della VM Linux. È inoltre possibile eseguire una verifica del disco e ripristinarlo utilizzando un VMAccess.

Per controllare e quindi ripristinare il disco, usare questo script VMAccess:

`disk_check_repair.json`

```json
{
  "check_disk": "true",
  "repair_disk": "true, user-disk-name"
}
```

Eseguire lo script VMAccess con:

```bash
azure vm extension set exampleResourceGroup exampleVM \
VMAccessForLinux Microsoft.OSTCExtensions * \
--private-config-path disk_check_repair.json
```

### Uso di VMAccess per reimpostare l'accesso utente a Linux

Se si è perso l'accesso alla radice della VM Linux è possibile avviare uno script VMAccess per reimpostare la password radice.

Per reimpostare la password radice, usare questo script VMAccess:

`reset_root_password.json`

```json
{
  "username":"root",
  "password":"exampleNewPassword",   
}
```

Eseguire lo script VMAccess con:

```bash
azure vm extension set exampleResourceGroup exampleVM \
VMAccessForLinux Microsoft.OSTCExtensions * \
--private-config-path reset_root_password.json
```

Per reimpostare la chiave SSH di un utente non ROOT usare questo script VMAccess:

`reset_ssh_key.json`

```json
{
  "username":"exampleUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== exampleUser@exampleServer",   
}
```

Eseguire lo script VMAccess con:

```bash
azure vm extension set exampleResourceGroup exampleVM \
VMAccessForLinux Microsoft.OSTCExtensions * \
--private-config-path reset_ssh_key.json
```

### Uso di VMAccess per gestire gli account utente in Linux

VMAccess è uno script Python che può essere usato per gestire gli utenti nella VM Linux senza accedere e senza usare sudo o l'account radice.

Per creare un utente, usare questo script VMAccess:

`create_new_user.json`

```json
{
"username":"exampleNewUserName",
"ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== exampleUser@exampleServer",
"password":"examplePassword",
}
```

Eseguire lo script VMAccess con:

```bash
azure vm extension set exampleResourceGroup exampleVM \
VMAccessForLinux Microsoft.OSTCExtensions * \
--private-config-path create_new_user.json
```

Per creare un utente:

`remove_user.json`

```json
{
"remove_user":"exampleUser",
}
```

Eseguire lo script VMAccess con:

```bash
azure vm extension set exampleResourceGroup exampleVM \
VMAccessForLinux Microsoft.OSTCExtensions * \
--private-config-path remove_user.json
```

### Uso di VMAccess per reimpostare la configurazione SSHD

Se si apportano modifiche alla configurazione del disco SSHD della VM Linux e si chiude la connessione SSH prima di verificare le modifiche, potrebbe essere impedita una nuova connessione SSH. È possibile usare VMAccess per ripristinare una configurazione SSHD valida nota senza aver effettuato l'accesso tramite SSH.

Per reimpostare la configurazione SSHD usare questo script VMAccess:

`reset_sshd.json`

```json
{
  "reset_ssh": true
}
```

Eseguire lo script VMAccess con:

```bash
azure vm extension set exampleResourceGroup exampleVM \
VMAccessForLinux Microsoft.OSTCExtensions * \
--private-config-path reset_sshd.json
```

## Passaggi successivi

L'aggiornamento di Linux mediante le estensioni VMAccess di Azure è un metodo per apportare modifiche a una VM Linux in esecuzione. È inoltre possibile usare strumenti come cloud-init e modelli di Azure per modificare la VM Linux in fase di avvio.

[Informazioni sulle estensioni e sulle funzionalità delle macchine virtuali](virtual-machines-linux-extensions-features.md)

[Creazione di modelli di Azure Resource Manager con le estensioni di VM Linux](virtual-machines-linux-extensions-authoring-templates.md)

[Uso di cloud-init per personalizzare una VM Linux durante la creazione](virtual-machines-linux-using-cloud-init.md)

<!---HONumber=AcomDC_0831_2016-->