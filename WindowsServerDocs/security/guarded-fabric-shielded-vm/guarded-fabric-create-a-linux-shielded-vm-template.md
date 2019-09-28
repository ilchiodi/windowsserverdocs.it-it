---
title: Creare un disco modello di macchina virtuale schermato Linux
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: d0e1d4fb-97fc-4389-9421-c869ba532944
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 66d5f70f747a6209f2856afde58b6f486ea597f8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386710"
---
# <a name="create-a-linux-shielded-vm-template-disk"></a>Creare un disco modello di macchina virtuale schermato Linux

> Si applica a: Windows Server 2019, Windows Server (canale semestrale), 

Questo argomento illustra come preparare un disco modello per VM schermate Linux che possono essere usate per creare un'istanza di una o più macchine virtuali tenant.

## <a name="prerequisites"></a>Prerequisiti

Per preparare e testare una macchina virtuale schermata di Linux, è necessario disporre delle risorse seguenti:

- Un server con capababilities di virtualizzazione che esegue Windows Server, versione 1709 o successiva
- Un secondo computer (Windows 10 o Windows Server 2016) in grado di eseguire la console di gestione di Hyper-V per connettersi alla console della macchina virtuale in esecuzione
- Un'immagine ISO per uno dei sistemi operativi VM schermati Linux supportati:
    - Ubuntu 16,04 LTS con il kernel 4,4
    - Red Hat Enterprise Linux 7,3
    - SUSE Linux Enterprise Server 12 Service Pack 2
- Accesso a Internet per scaricare il pacchetto lsvmtools e gli aggiornamenti del sistema operativo

> [!IMPORTANT]
> Le versioni più recenti dei sistemi operativi Linux precedenti possono includere un bug noto del driver TPM che ne impedirà il provisioning come VM schermate.
> Non è consigliabile aggiornare i modelli o le macchine virtuali schermate a una versione più recente finché non sarà disponibile una correzione.
> L'elenco dei sistemi operativi supportati precedenti verrà aggiornato quando gli aggiornamenti saranno resi pubblici.

## <a name="prepare-a-linux-vm"></a>Preparare una VM Linux

Le macchine virtuali schermate vengono create da dischi modello protetti.
I dischi modello contengono il sistema operativo per la macchina virtuale e i metadati, inclusa una firma digitale delle partizioni/boot e/root, per garantire che i componenti del sistema operativo di base non vengano modificati prima della distribuzione.

Per creare un disco modello, è innanzitutto necessario creare una macchina virtuale (non schermata) normale da preparare come immagine di base per le macchine virtuali schermate future.
Il software installato e le modifiche apportate alla configurazione apportate a questa macchina virtuale verranno applicate a tutte le macchine virtuali schermate create da questo disco modello.
Questi passaggi illustrano i requisiti minimi per ottenere una VM Linux pronta per templatization.

> [!NOTE]
> La crittografia del disco Linux viene configurata quando il disco è partizionato.
> Ciò significa che è necessario creare una nuova macchina virtuale pre-crittografata con dm-crypt per creare un disco modello di macchina virtuale schermato Linux.


1.  Nel server di virtualizzazione, verificare che Hyper-V e le funzionalità di supporto di Hyper-V sorveglianza host siano installate eseguendo i comandi seguenti in una console di PowerShell con privilegi elevati:

    ```powershell
    Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
    ```

2.  Scaricare l'immagine ISO da un'origine attendibile e archiviarla nel server di virtualizzazione o in una condivisione file accessibile al server di virtualizzazione.

3.  Nel computer di gestione che esegue Windows Server versione 1709 installare la macchina virtuale schermata Strumenti di amministrazione remota del server eseguendo il comando seguente:

    ```powershell
    Install-WindowsFeature RSAT-Shielded-VM-Tools
    ```

4.  Aprire la console di gestione di **Hyper-V** nel computer di gestione e connettersi al server di virtualizzazione.
    A tale scopo, fare clic su "Connetti al server...". nel riquadro azioni o facendo clic con il pulsante destro del mouse sulla console di gestione di Hyper-V e scegliendo "Connetti al server". Fornire il nome DNS per il server Hyper-V e, se necessario, le credenziali necessarie per la connessione.

5.  Utilizzando la console di gestione di Hyper-V, [configurare un](https://docs.microsoft.com/windows-server/virtualization/hyper-v/get-started/create-a-virtual-switch-for-hyper-v-virtual-machines) comportatore esterno nel server di virtualizzazione in modo che la macchina virtuale Linux possa accedere a Internet per ottenere gli aggiornamenti.

6.  Successivamente, creare una nuova macchina virtuale su cui installare il sistema operativo Linux.
    Nel riquadro azioni fare clic su **nuova** **macchina virtuale**  >  per visualizzare la procedura guidata.
    Specificare un nome descrittivo per la macchina virtuale, ad esempio "creato un modello Linux" e fare clic su **Next (avanti**).

7.  Nella seconda pagina della procedura guidata selezionare **generazione 2** per assicurarsi che la macchina virtuale venga sottoposta a provisioning con un profilo del firmware basato su UEFI.

8.  Completare il resto della procedura guidata in base alle proprie preferenze.
    Non usare un disco differenze per questa macchina virtuale. i dischi modello di macchina virtuale schermati non possono usare dischi differenze.
    Infine, connettere l'immagine ISO scaricata in precedenza all'unità DVD virtuale per questa macchina virtuale in modo che sia possibile installare il sistema operativo.

9.  Nella console di gestione di Hyper-V selezionare la macchina virtuale appena creata e fare clic su **Connetti..** . nel riquadro azioni per connettersi a una console virtuale della macchina virtuale.
    Nella finestra che viene visualizzata fare clic su **Avvia** per accendere la macchina virtuale.

10. Procedere con il processo di installazione per la distribuzione di Linux selezionata.
    Sebbene ogni distribuzione Linux usi un'installazione guidata diversa, è necessario soddisfare i requisiti seguenti per le macchine virtuali che diventeranno dischi di modelli di VM schermate Linux:

    - Il disco deve essere partizionato usando il layout della tabella GUID Paritioning (GPT)
    - La partizione radice deve essere crittografata con dm-crypt. La passphrase deve essere impostata su **passphrase** (all minuscole). Questa passphrase viene casuale e la partizione viene nuovamente crittografata quando viene effettuato il provisioning di una macchina virtuale schermata.
    - La partizione di avvio deve usare la file system **ext2**

11. Una volta che il sistema operativo Linux è stato completamente avviato ed è stato effettuato l'accesso, è consigliabile installare il kernel Linux virtuale e i pacchetti dei servizi di integrazione Hyper-V associati.
    Inoltre, sarà necessario installare un server SSH o un altro strumento di gestione remota per accedere alla macchina virtuale dopo che è stata schermata.

    In Ubuntu eseguire questo comando per installare i componenti seguenti:

    ```bash
    sudo apt-get install linux-virtual linux-tools-virtual linux-cloud-tools-virtual linux-image-extra-virtual openssh-server
    ```

    In RHEL eseguire invece il comando seguente:

    ```bash
    sudo yum install hyperv-daemons openssh-server
    sudo service sshd start
    ```

    In SLES eseguire il comando seguente:

    ```bash
    sudo zypper install hyper-v
    sudo chkconfig hv_kvp_daemon on
    sudo systemctl enable sshd
    ```

12. Configurare il sistema operativo Linux nel modo desiderato.
    Tutti i software installati, gli account utente aggiunti e le modifiche apportate alla configurazione di a livello verranno applicati a tutte le macchine virtuali future create da questo disco modello.
    È consigliabile evitare di salvare nel disco eventuali segreti o pacchetti non necessari.

13. Se si prevede di usare System Center Virtual Machine Manager per distribuire le macchine virtuali, installare l'agente guest VMM per consentire a VMM di specializzare il sistema operativo durante il provisioning della macchina virtuale.
    La specializzazione consente di configurare in modo sicuro ogni macchina virtuale con diversi utenti e chiavi SSH, configurazioni di rete e procedure di configurazione personalizzate.
    Informazioni su come [ottenere e installare l'agente guest VMM](https://docs.microsoft.com/system-center/vmm/vm-linux#install-the-vmm-guest-agent) nella documentazione di VMM.

14. Successivamente, [aggiungere il repository software Microsoft Linux a gestione pacchetti](../../administration/linux-package-repository-for-microsoft-software.md).

15. Usando Gestione pacchetti, installare il pacchetto lsvmtools che contiene lo shim di bootloader della VM schermata Linux, i componenti di provisioning e lo strumento di preparazione dischi.

    ```bash
    # Ubuntu 16.04
    sudo apt-get install lsvmtools

    # SLES 12 SP2
    sudo zypper install lsvmtools

    # RHEL 7.3
    sudo yum install lsvmtools
    ```

14. Al termine della personalizzazione del sistema operativo Linux, individuare il programma di installazione di lsvmprep nel sistema ed eseguirlo.
    
    ```bash
    # The path below may change based on the version of lsvmprep installed
    # Run "find /opt -name lsvmprep" to locate the lsvmprep executable
    sudo /opt/lsvmtools-1.0.0-x86-64/lsvmprep
    ```

15. Arrestare la macchina virtuale.

16. Se sono stati rilevati Checkpoint della VM (inclusi i checkpoint automatici creati da Hyper-V con Windows 10 Fall Creators Update), assicurarsi di eliminarli prima di continuare.
    I checkpoint creano dischi differenze (con estensione avhdx) che non sono supportati dalla creazione guidata disco modello.
    
    Per eliminare i checkpoint, aprire la console di **gestione di Hyper-V**, selezionare la macchina virtuale, fare clic con il pulsante destro del mouse sul checkpoint in primo piano nel riquadro Checkpoint, quindi scegliere **Elimina sottoalbero Checkpoint**.

    ![Eliminare tutti i checkpoint per la macchina virtuale modello nella console di gestione di Hyper-V](../media/Guarded-Fabric-Shielded-VM/delete-checkpoints-lsvm-template.png)

## <a name="protect-the-template-disk"></a>Proteggere il disco modello

La macchina virtuale preparata nella sezione precedente è quasi pronta per essere usata come disco modello di macchina virtuale schermato di Linux.
L'ultimo passaggio consiste nell'eseguire il disco tramite la creazione guidata disco modello, che eseguirà l'hashing e firmerà digitalmente lo stato corrente delle partizioni radice e di avvio.
L'hash e la firma digitale vengono verificati quando viene effettuato il provisioning di una macchina virtuale schermata per assicurarsi che non siano state apportate modifiche non autorizzate alle due partizioni tra la creazione e la distribuzione dei modelli.

### <a name="obtain-a-certificate-to-sign-the-disk"></a>Ottenere un certificato per firmare il disco

Per firmare digitalmente le misurazioni del disco, è necessario ottenere un certificato nel computer in cui verrà eseguita la creazione guidata disco modello.
Il certificato deve soddisfare i requisiti seguenti:

Proprietà certificato | Valore obbligatorio
---------------------|---------------
Algoritmo chiave | RSA
Dimensioni minime chiave | 2048 bit
Algoritmo di firma | SHA256 (scelta consigliata)
Utilizzo chiavi | Firma digitale

I dettagli relativi a questo certificato verranno visualizzati ai tenant quando creeranno i file di dati di schermatura e autorizzano i dischi considerati attendibili.
È quindi importante ottenere questo certificato da un'autorità di certificazione reciprocamente attendibile da te e dai tuoi tenant.
Negli scenari aziendali in cui si è sia il provider di hosting che il tenant, è possibile prendere in considerazione l'emissione del certificato dall'autorità di certificazione dell'organizzazione.
Proteggere questo certificato con attenzione, perché chiunque possiede questo certificato può creare nuovi dischi modello considerati attendibili come il disco autentico.

In un ambiente lab di test è possibile creare un certificato autofirmato con il comando PowerShell seguente:

```powershell
New-SelfSignedCertificate -Subject "CN=Linux Shielded VM Template Disk Signing Certificate"
```

### <a name="process-the-disk-with-the-template-disk-wizard-cmdlet"></a>Elaborare il disco con il cmdlet della creazione guidata disco modello

Copiare il disco modello e il certificato in un computer che esegue Windows Server, versione 1709, quindi eseguire i comandi seguenti per avviare il processo di firma.
Il valore di VHDX fornito al parametro `-Path` verrà sovrascritto con il disco modello aggiornato, quindi assicurarsi di creare una copia prima di eseguire il comando.

> [!IMPORTANT]
> Le Strumenti di amministrazione remota del server disponibili in Windows Server 2016 o Windows 10 non possono essere usate per preparare un disco modello di macchina virtuale schermato di Linux.
> Usare solo il cmdlet [Protect-TemplateDisk](https://docs.microsoft.com/powershell/module/shieldedvmtemplate/protect-templatedisk?view=win10-ps) disponibile in Windows Server, versione 1709 o il strumenti di amministrazione remota del server disponibile in windows server 2019 per preparare un disco modello di macchina virtuale schermato di Linux.

```powershell
# Replace "THUMBPRINT" with the thumbprint of your template disk signing certificate in the line below
$certificate = Get-Item Cert:\LocalMachine\My\THUMBPRINT

Protect-TemplateDisk -Path 'C:\temp\MyLinuxTemplate.vhdx' -TemplateName 'Ubuntu 16.04' -Version 1.0.0.0 -Certificate $certificate -ProtectedTemplateTargetDiskType PreprocessedLinux
```

Il disco modello è ora pronto per essere usato per eseguire il provisioning di macchine virtuali schermate di Linux.
Se si usa System Center Virtual Machine Manager per distribuire la VM, è ora possibile copiare il VHDX nella libreria VMM.

È anche possibile estrarre il catalogo delle firme del volume dal VHDX.
Questo file viene usato per fornire informazioni sul certificato di firma, il nome del disco e la versione ai proprietari delle macchine virtuali che vogliono usare il modello.
È necessario importare questo file nella creazione guidata file di dati di schermatura per autorizzare l'utente, l'autore del modello che possiede il certificato di firma, a creare questo e i dischi modello futuri.

Per estrarre il catalogo delle firme del volume, eseguire il comando seguente in PowerShell:

```powershell
Save-VolumeSignatureCatalog -TemplateDiskPath 'C:\temp\MyLinuxTemplate.vhdx' -VolumeSignatureCatalogPath 'C:\temp\MyLinuxTemplate.vsc'
```
