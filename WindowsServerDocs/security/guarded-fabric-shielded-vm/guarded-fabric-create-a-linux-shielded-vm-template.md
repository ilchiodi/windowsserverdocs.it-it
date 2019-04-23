---
title: Creare schermate di Linux un disco di modello di macchina virtuale
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: d0e1d4fb-97fc-4389-9421-c869ba532944
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: e791d18e027e5e3e1c5b9c52659641ff588a96a2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858672"
---
# <a name="create-a-linux-shielded-vm-template-disk"></a>Creare schermate di Linux un disco di modello di macchina virtuale

> Si applica a: Windows Server 2019, Windows Server (canale semestrale), 

Questo argomento illustra come preparare un disco modello per macchine virtuali che possono essere utilizzate per creare un'istanza di tenant di uno o più macchine virtuali schermate di Linux.

## <a name="prerequisites"></a>Prerequisiti

Per preparare e testare una schermata di Linux della macchina virtuale, sarà necessario le risorse seguenti:

- Un server con capababilities di virtualizzazione che esegue Windows Server, versione 1709 o successiva
- Un secondo computer (Windows 10 o Windows Server 2016) in grado di eseguire Hyper-V Manager per connettersi alla console della macchina virtuale in esecuzione
- Un'immagine ISO per uno di Linux supportate schermate sistemi operativi della macchina virtuale:
    - Ubuntu 16.04 LTS con kernel 4.4
    - Red Hat Enterprise Linux 7.3
    - SUSE Linux Enterprise Server 12 Service Pack 2
- Accesso a Internet per scaricare il pacchetto lsvmtools e aggiornamenti del sistema operativo

> [!IMPORTANT]
> Le versioni più recenti del precedente che OSes Linux può includere un bug noto di driver TPM che impedirà il correttamente il provisioning come VM schermate.
> Non è consigliabile di aggiornare i modelli o macchine virtuali schermate per una versione più recente fino a quando non è disponibile una correzione.
> L'elenco dei sistemi operativi supportati precedente verrà aggiornata quando gli aggiornamenti sono stati resi pubblici.

## <a name="prepare-a-linux-vm"></a>Preparare una macchina virtuale Linux

Le macchine virtuali schermate vengono create da dischi di modelli sicuri.
Dischi modello contengono il sistema operativo per la macchina virtuale e i metadati, inclusi una firma digitale delle partizioni /boot e /root, per assicurarsi che componenti del sistema operativo di base non vengono modificati prima della distribuzione.

Per creare un disco modello, è innanzitutto necessario creare una normale VM (eseguendolo) che si preparerà come immagine di base per le macchine virtuali schermate future.
Il software installato e le modifiche di configurazione apportate a questa macchina virtuale verranno applicate a tutte le macchine virtuali schermate create da questo disco modello.
Questa procedura illustrerà i requisiti minimi per preparare una VM Linux templatization.

> [!NOTE]
> Crittografia dei dischi Linux viene configurata quando il disco è partizionato.
> Ciò significa che è necessario creare una nuova macchina virtuale pre-crittografata usando dm-crypt per creare una Linux schermate disco di modello di macchina virtuale.


1.  Nel server di virtualizzazione, verificare che Hyper-V e le funzionalità di supporto Hyper-V per sorveglianza Host siano installate eseguendo i comandi seguenti in una console di PowerShell con privilegi elevata:

    ```powershell
    Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
    ```

2.  Scaricare l'immagine ISO da una fonte attendibile e archiviarli nel server virtualization, o in una condivisione file accessibile al server di virtualizzazione.

3.  Nel computer di gestione che esegue Windows Server versione 1709, installare la schermatura della macchina virtuale strumenti di amministrazione remota eseguendo il comando seguente:

    ```powershell
    Install-WindowsFeature RSAT-Shielded-VM-Tools
    ```

4.  Aprire **Hyper-V Manager** nel computer di gestione e connettersi al server di virtualizzazione.
    È possibile farlo facendo clic su "Connetti al Server..." nel riquadro azioni o pulsante destro del mouse facendo clic su gestione di Hyper-V e scegliere "Connetti al Server..." Specificare il nome DNS per il server Hyper-V e, se necessario, le credenziali necessarie per connettersi a esso.

5.  Uso di gestione di Hyper-V [configurare un commutatore esterno](https://docs.microsoft.com/windows-server/virtualization/hyper-v/get-started/create-a-virtual-switch-for-hyper-v-virtual-machines) sul server di virtualizzazione in modo che la VM Linux di accedere a Internet per ottenere gli aggiornamenti.

6.  Successivamente, creare una nuova macchina virtuale per installare il sistema operativo Linux in.
    Nel riquadro azioni, fare clic su **New** > **macchina virtuale** per visualizzare la procedura guidata.
    Specificare un nome descrittivo per la macchina virtuale, ad esempio "Pre-templatized Linux" e fare clic su **successivo**.

7.  Nella seconda pagina della procedura guidata, selezionare **Generation 2** per assicurarsi che viene eseguito il provisioning della macchina virtuale con un profilo del firmware basato su UEFI.

8.  Completare il resto della procedura guidata in base alle preferenze.
    Non usare un disco differenze per questa macchina virtuale; dischi di modelli di macchine Virtuali schermati non è possibile usare dischi differenze.
    Infine, collegare l'immagine ISO scaricato in precedenza per l'unità DVD virtuale per questa macchina virtuale in modo che è possibile installare il sistema operativo.

9.  Gestione di Hyper-V, selezionare la macchina virtuale appena creata e fare clic su **Connetti...**  nel riquadro azioni per collegare a una console virtuale della macchina virtuale.
    Nella finestra visualizzata, fare clic su **avviare** per accendere la macchina virtuale.

10. Procedere con il processo di installazione per la distribuzione di Linux selezionata.
    Mentre ogni distribuzione di Linux Usa una diversa procedura guidata, devono essere soddisfatti i requisiti seguenti per le macchine virtuali che diventeranno Linux schermate dischi modello di macchina virtuale:

    - Il disco deve essere partizionato utilizzando il layout di tabella Partitioning GUID (GPT)
    - La partizione radice deve essere crittografata con dm-crypt. La passphrase deve essere impostata su **passphrase** (tutte lettere minuscole). La passphrase sarà casuale e la partizione ricrittografato quando viene eseguito il provisioning di una macchina virtuale schermata.
    - La partizione di avvio è necessario usare il **ext2** file system

11. Dopo che il sistema operativo Linux sia avviata completamente e aver effettuato l'accesso, si consiglia di installare il kernel linux virtuale e associare i pacchetti integration services di Hyper-V.
    Inoltre, si dovrà installare un server SSH o un altro strumento di gestione remota per accedere alla macchina virtuale dopo che è schermata.

    In Ubuntu, eseguire il comando seguente per installare questi componenti:

    ```bash
    sudo apt-get install linux-virtual linux-tools-virtual linux-cloud-tools-virtual linux-image-extra-virtual openssh-server
    ```

    In RHEL, eseguire invece il comando seguente:

    ```bash
    sudo yum install hyperv-daemons openssh-server
    sudo service sshd start
    ```

    E in SLES, eseguire il comando seguente:

    ```bash
    sudo zypper install hyper-v
    sudo chkconfig hv_kvp_daemon on
    sudo systemctl enable sshd
    ```

12. Configurare il sistema operativo Linux in base alle esigenze.
    Qualsiasi software installare, account utente che si aggiungono, e si apportano modifiche alla configurazione a livello di sistema verranno applicate a tutte le macchine virtuali successive create da questo disco modello.
    È consigliabile evitare il salvataggio di segreti o pacchetti non necessari nel disco.

13. Se si prevede di usare System Center Virtual Machine Manager per distribuire le macchine virtuali, installare l'agente guest VMM per consentire a VMM specializzare il sistema operativo durante il provisioning della macchina virtuale.
    La specializzazione consente a ogni macchina virtuale per essere configurato in modo sicuro con diversi utenti e le chiavi SSH, le configurazioni di rete e i passaggi di installazione personalizzato.
    Informazioni su come [ottenere e installare l'agente guest VMM](https://docs.microsoft.com/system-center/vmm/vm-linux#install-the-vmm-guest-agent) nella documentazione di VMM.

14. Successivamente [aggiungere il Repository Software Linux di Microsoft per la gestione pacchetti](../../administration/linux-package-repository-for-microsoft-software.md).

15. Usa la gestione pacchetti, installare il pacchetto lsvmtools contenente Linux schermate shim del caricatore di avvio della macchina virtuale, il provisioning di componenti e strumento di preparazione del disco.

    ```bash
    # Ubuntu 16.04
    sudo apt-get install lsvmtools

    # SLES 12 SP2
    sudo zypper install lsvmtools

    # RHEL 7.3
    sudo yum install lsvmtools
    ```

14. Dopo aver completato la personalizzazione del sistema operativo Linux, individuare il programma di installazione lsvmprep nel sistema ed eseguirlo.
    
    ```bash
    # The path below may change based on the version of lsvmprep installed
    # Run "find /opt -name lsvmprep" to locate the lsvmprep executable
    sudo /opt/lsvmtools-1.0.0-x86-64/lsvmprep
    ```

15. Arrestare la macchina virtuale.

16. Se è stato creato alcun checkpoint della macchina virtuale (inclusi i checkpoint automatici creati da Hyper-V con il di Windows 10 Fall Creators Update), assicurarsi di eliminarli prima di continuare.
    I checkpoint creano i dischi differenze (avhdx) che non sono supportati dalla creazione guidata disco modello.
    
    Per eliminare i checkpoint, aprire **gestione di Hyper-V**, selezionare la macchina virtuale, fare clic con il pulsante destro del checkpoint più in alto nel riquadro dei checkpoint, quindi fare clic su **Elimina sottoalbero Checkpoint**.

    ![Eliminare tutti i checkpoint per il modello di macchina virtuale nella console di gestione Hyper-V](../media/Guarded-Fabric-Shielded-VM/delete-checkpoints-lsvm-template.png)

## <a name="protect-the-template-disk"></a>Proteggere il disco modello

La macchina virtuale è preparato nella sezione precedente è quasi pronta per essere usato come una schermata di Linux disco di modello di macchina virtuale.
L'ultimo passaggio è per l'esecuzione del disco tramite la creazione guidata disco modello, quale sarà l'hashing e firmare digitalmente lo stato corrente delle partizioni radice e avvio.
L'hash e firma digitale verificate quando viene eseguito il provisioning di una macchina virtuale schermata per garantire che le due partizioni tra le creazione di modelli e la distribuzione sono stati eseguiti senza modifiche non autorizzate.

### <a name="obtain-a-certificate-to-sign-the-disk"></a>Ottenere un certificato per firmare il disco

Per firmare digitalmente il misurazioni del disco, è necessario ottenere un certificato nel computer in cui si eseguirà la creazione guidata disco modello.
Il certificato deve soddisfare i requisiti seguenti:

Proprietà del certificato | Valore obbligatorio
---------------------|---------------
Algoritmo della chiave | RSA
Dimensione minima della chiave | 2048 bit
Algoritmo di firma | SHA256 (scelta consigliata)
Utilizzo chiavi | Firma digitale

Informazioni dettagliate su questo certificato verranno visualizzate ai tenant quando si creano i file di dati di schermatura e autorizza i dischi di che cui si fidano.
Pertanto, è importante ottenere questo certificato da un'autorità di certificazione attendibile reciprocamente dall'utente e i tenant.
In scenari aziendali in cui si è il tenant sia l'host, è possibile considerare il rilascio di questo certificato da autorità di certificazione aziendale.
Questo certificato viene protetto con attenzione, perché chiunque in possesso del certificato può creare nuovi dischi modello che sono attendibili lo stesso come il disco autentico.

In un ambiente di testing, è possibile creare un certificato autofirmato con il comando PowerShell seguente:

```powershell
New-SelfSignedCertificate -Subject "CN=Linux Shielded VM Template Disk Signing Certificate"
```

### <a name="process-the-disk-with-the-template-disk-wizard-cmdlet"></a>Elaborare il disco con il cmdlet di creazione guidata disco modello

Copiare il disco modello e un certificato in un computer che esegue Windows Server, versione 1709, quindi eseguire i comandi seguenti per avviare il processo di firma.
Il file VHDX è fornire la `-Path` parametro verrà sovrascritto con il disco modello aggiornato, pertanto assicurarsi di creare una copia prima di eseguire il comando.

> [!IMPORTANT]
> Gli strumenti di amministrazione Server remota disponibile in Windows Server 2016 o Windows 10 non può essere utilizzati per preparare un schermate di Linux disco di modello di macchina virtuale.
> Usare solo il [Proteggi TemplateDisk](https://docs.microsoft.com/powershell/module/shieldedvmtemplate/protect-templatedisk?view=win10-ps) cmdlet disponibili in Windows Server, versione 1709 o gli strumenti di amministrazione Server remota disponibile in Windows Server 2019 per preparare un Linux schermate disco di modello di macchina virtuale.

```powershell
# Replace "THUMBPRINT" with the thumbprint of your template disk signing certificate in the line below
$certificate = Get-Item Cert:\LocalMachine\My\THUMBPRINT

Protect-TemplateDisk -Path 'C:\temp\MyLinuxTemplate.vhdx' -TemplateName 'Ubuntu 16.04' -Version 1.0.0.0 -Certificate $certificate -ProtectedTemplateTargetDiskType PreprocessedLinux
```

Il disco modello è ora pronto per essere usato per eseguire il provisioning di macchine virtuali schermate di Linux.
Se si usa System Center Virtual Machine Manager per distribuire la VM, è ora possibile copiare il file VHDX per la libreria VMM.

È anche possibile estrarre il catalogo delle firme del volume da nel disco VHDX.
Questo file viene usato per fornire informazioni sul certificato di firma, il nome del disco e versione ai proprietari della macchina virtuale che desiderano utilizzare un modello.
È necessario importare questo file in guidata File dati di schermatura per concedere l'autorizzazione, all'autore del modello in possesso del certificato di firma, per creare questa e modello future dischi per loro.

Per estrarre il catalogo delle firme del volume, eseguire il comando seguente in PowerShell:

```powershell
Save-VolumeSignatureCatalog -TemplateDiskPath 'C:\temp\MyLinuxTemplate.vhdx' -VolumeSignatureCatalogPath 'C:\temp\MyLinuxTemplate.vsc'
```
