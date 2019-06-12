---
title: È necessario creare una macchina virtuale di generazione 1 o 2 in Hyper-V?
description: Fornisce metodi di avvio di considerazioni, ad esempio supportati e altre differenze di funzionalità che consentono di scegliere quale generazione soddisfa le proprie esigenze.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 02e31413-6140-4723-a8d6-46c7f667792d
author: KBDAzure
ms.author: kathydav
ms.date: 12/05/2016
ms.openlocfilehash: 95ececde8a1b8c591ea2baf367a93f63ee55a6e3
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811992"
---
# <a name="should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v"></a>È necessario creare una macchina virtuale di generazione 1 o 2 in Hyper-V?

>Si applica a: Windows 10, Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

> [!NOTE]
> Se si prevede di mai caricare una macchina virtuale di Windows (VM) da un'istanza locale di Microsoft Azure, la generazione 1 e generazione 2 macchine virtuali nel formato di file VHD e avere un disco a dimensione fissa sono supportati. Visualizzare [macchine virtuali di seconda generazione 2 in Azure](https://docs.microsoft.com/azure/virtual-machines/windows/generation-2) per altre informazioni sulle funzionalità di generazione 2 è supportata in Azure. Per altre informazioni sul caricamento di un disco rigido virtuale Windows o VHDX, vedere [preparare un disco rigido virtuale Windows o vhdx prima del caricamento in Azure](https://docs.microsoft.com/azure/virtual-machines/windows/prepare-for-upload-vhd-image).

La scelta per creare una generazione 1 o una macchina virtuale di generazione 2 dipende dal sistema operativo guest si desidera installare e il metodo di avvio da usare per distribuire la macchina virtuale. È consigliabile creare una macchina virtuale di generazione 2 per sfruttare i vantaggi delle funzionalità come avvio protetto, a meno che non viene soddisfatta una delle istruzioni seguenti:  

- Non è il disco rigido virtuale che si desidera eseguire l'avvio dal [compatibile con UEFI](https://technet.microsoft.com/library/hh824898.aspx).  
- Generazione 2 non supporta il sistema operativo da eseguire nella macchina virtuale.  
- Generazione 2 non supporta il metodo di avvio da usare.  

Per altre informazioni sulle funzionalità disponibili con le macchine virtuali di generazione 2, vedere [compatibilità con la funzionalità Hyper-V per la generazione e guest](../Hyper-V-feature-compatibility-by-generation-and-guest.md).

È possibile modificare la generazione di una macchina virtuale dopo averla creata. Pertanto, è consigliabile che tenere conto delle considerazioni, nonché scegliere il sistema operativo, il metodo di avvio e funzionalità che si desidera usare prima di scegliere una generazione.  

## <a name="which-guest-operating-systems-are-supported"></a>Quali sistemi operativi guest sono supportati?

Macchine virtuali di generazione 1 supporta la maggior parte dei sistemi operativi di guest. Macchine virtuali di generazione 2 supporta più versioni a 64 bit di Windows e nelle versioni più recenti dei sistemi operativi Linux e FreeBSD. Usare le sezioni seguenti per verificare quali la generazione della macchina virtuale supporta il sistema operativo guest da installare.  

- [Supporto del sistema operativo guest Windows](#windows-guest-operating-system-support)  

- [CentOS e Red Hat Enterprise Linux supporto del sistema operativo guest](#centos-and-red-hat-enterprise-linux-guest-operating-system-support)  

- [Supporto del sistema operativo guest Debian](#debian-guest-operating-system-support)  

- [Supporto sistema operativo guest di FreeBSD](#freebsd-guest-operating-system-support)  

- [Supporto sistema operativo guest di Oracle Linux](#oracle-linux-guest-operating-system-support)  

- [Supporto del sistema operativo guest SUSE](#suse-guest-operating-system-support)  

- [Supporto del sistema operativo guest Ubuntu](#ubuntu-guest-operating-system-support)  

### <a name="windows-guest-operating-system-support"></a>Supporto del sistema operativo guest Windows

La tabella seguente illustra le versioni a 64 bit di Windows è possibile usare come un sistema operativo guest per la generazione 1 e macchine virtuali di generazione 2.  

|versioni a 64 bit di Windows|Prima generazione|Seconda generazione|  
|-------------------------------|----------------|----------------|  
| Windows Server 2019 |&#10004;|&#10004;|  
| Windows Server 2016 |&#10004;|&#10004;|  
| Windows Server 2012 R2 |&#10004;|&#10004;|  
| Windows Server 2012 |&#10004;|&#10004;|  
|Windows Server 2008 R2|&#10004;| &#10006;|  
|Windows Server 2008|&#10004;| &#10006;|  
|Windows 10|&#10004;|&#10004;|  
|Windows 8.1|&#10004;|&#10004;|  
|Windows 8|&#10004;|&#10004;|  
|Windows 7|&#10004;| &#10006;|

La tabella seguente illustra le versioni a 32 bit di Windows è possibile usare come un sistema operativo guest per la generazione 1 e macchine virtuali di generazione 2.

|versioni a 32 bit di Windows|Prima generazione|Seconda generazione|  
|-------------------------------|----------------|----------------|  
|Windows 10|&#10004;| &#10006;|  
|Windows 8.1|&#10004;| &#10006;|  
|Windows 8|&#10004;| &#10006;|  
|Windows 7|&#10004;| &#10006;|  

### <a name="centos-and-red-hat-enterprise-linux-guest-operating-system-support"></a>CentOS e Red Hat Enterprise Linux supporto del sistema operativo guest

Nella tabella seguente sono indicate le versioni di Red Hat Enterprise Linux \(RHEL\) e CentOS è possibile usare come un sistema operativo guest per la generazione 1 e macchine virtuali di generazione 2.

|Versioni del sistema operativo|Prima generazione|Seconda generazione|  
|-----------------------------|----------------|----------------|  
|Serie RHEL/CentOS 7.x|&#10004;|&#10004;|  
|Serie RHEL/CentOS 6.x|&#10004;|&#10004;<br />**Nota:** Supportato solo in Windows Server 2016 e versioni successive.|  
|Serie RHEL/CentOS 5. x|&#10004;| &#10006;|  

Per altre informazioni, vedere [CentOS e Red Hat Enterprise Linux macchine virtuali in Hyper-V](../Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md).  

### <a name="debian-guest-operating-system-support"></a>Supporto del sistema operativo guest Debian  

La tabella seguente illustra le versioni di Debian è possibile usare come un sistema operativo guest per la generazione 1 e macchine virtuali di generazione 2.

|Versioni del sistema operativo|Prima generazione|Seconda generazione|  
|-----------------------------|----------------|----------------|  
|Debian 7 serie|&#10004;| &#10006;|  
|Serie di Debian 8. x|&#10004;|&#10004;|  

Per altre informazioni, vedere [macchine virtuali Debian in Hyper-V](../Supported-Debian-virtual-machines-on-Hyper-V.md).  

### <a name="freebsd-guest-operating-system-support"></a>Supporto sistema operativo guest di FreeBSD

La tabella seguente illustra le versioni di FreeBSD che è possibile usare come un sistema operativo guest per la generazione 1 e macchine virtuali di generazione 2.  

|Versioni del sistema operativo|Prima generazione|Seconda generazione|  
|-----------------------------|----------------|----------------|  
|FreeBSD 10 e 10.1|&#10004;| &#10006;|  
|9.1 FreeBSD e 9.3|&#10004;| &#10006;|  
|8.4 di FreeBSD|&#10004;| &#10006;|  

Per altre informazioni, vedere [macchine virtuali FreeBSD in Hyper-V](../Supported-FreeBSD-virtual-machines-on-Hyper-V.md).  

### <a name="oracle-linux-guest-operating-system-support"></a>Supporto sistema operativo guest di Oracle Linux  

La tabella seguente illustra le versioni di serie di Kernel compatibile Red Hat è possibile usare come un sistema operativo guest per la generazione 1 e macchine virtuali di generazione 2.  

|Versioni di serie di Kernel compatibile Red Hat|Prima generazione|Seconda generazione|  
|---------------------------------------------|----------------|----------------|  
|Serie di Oracle Linux 7.x|&#10004;|&#10004;|
|Serie di Oracle Linux 6.x|&#10004;| &#10006;|  

La tabella seguente illustra le versioni di Unbreakable Enterprise Kernel che è possibile usare come un sistema operativo guest per la generazione 1 e macchine virtuali di generazione 2.

|Versioni di unbreakable Enterprise Kernel (UEK)|Prima generazione|Seconda generazione|  
|--------------------------------------------------|----------------|----------------|  
|Oracle Linux UEK R3 QU3|&#10004;| &#10006;|  
|Oracle Linux UEK R3 QU2|&#10004;| &#10006;|  
|Oracle Linux UEK R3 QU1|&#10004;| &#10006;|  

Per altre informazioni, vedere [macchine virtuali Oracle Linux in Hyper-V](../Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md).  

### <a name="suse-guest-operating-system-support"></a>Supporto del sistema operativo guest SUSE

La tabella seguente illustra le versioni di SUSE che è possibile usare come un sistema operativo guest per la generazione 1 e macchine virtuali di generazione 2.

|Versioni del sistema operativo|Prima generazione|Seconda generazione|  
|-----------------------------|----------------|----------------|  
|Serie di SUSE Linux Enterprise Server 12|&#10004;|&#10004;|  
|Serie di SUSE Linux Enterprise Server 11|&#10004;| &#10006;|  
|Aprire SUSE 12.3 +|&#10004;| &#10006;|  

Per altre informazioni, vedere [macchine virtuali SUSE su Hyper-V](../Supported-SUSE-virtual-machines-on-Hyper-V.md).  

### <a name="ubuntu-guest-operating-system-support"></a>Supporto del sistema operativo guest Ubuntu

La tabella seguente illustra le versioni di Ubuntu è possibile usare come un sistema operativo guest per la generazione 1 e macchine virtuali di generazione 2.

|Versioni del sistema operativo|Prima generazione|Seconda generazione|  
|-----------------------------|----------------|----------------|  
|Ubuntu 14.04 e versioni successive|&#10004;|&#10004;|  
|Ubuntu 12.04|&#10004;| &#10006;|  

Per altre informazioni, vedere [macchine virtuali Ubuntu in Hyper-V](../Supported-Ubuntu-virtual-machines-on-Hyper-V.md).  

## <a name="how-can-i-boot-the-virtual-machine"></a>Come è possibile avviare la macchina virtuale?

La tabella seguente mostra quali avvio metodi sono supportati per la generazione 1 e macchine virtuali di generazione 2.  

|Metodo di avvio|Prima generazione|Seconda generazione|  
|---------------|----------------|----------------|  
|Avvio PXE tramite una scheda di rete standard| &#10006;|&#10004;|  
|Avvio PXE utilizzando una scheda di rete legacy|&#10004;| &#10006;|  
|Avvio da un disco rigido virtuale SCSI (. VHDX) o un DVD virtuale (. ISO)| &#10006;|&#10004;|  
|Avvio dal Controller IDE disco rigido virtuale (. Disco rigido virtuale) o un DVD virtuale (. ISO)|&#10004;| &#10006;|  
|Avvio da disco floppy (. VFD)|&#10004;| &#10006;|  

## <a name="what-are-the-advantages-of-using-generation-2-virtual-machines"></a>Quali sono i vantaggi dell'uso di macchine virtuali di generazione 2?

Ecco alcuni dei vantaggi che si ottengono quando si usa una macchina virtuale di generazione 2:  
- **Avvio protetto** questa è una funzionalità che verifica il caricatore di avvio sia firmato da un'autorità attendibile nel database UEFI per evitare che utenti non autorizzato firmware, sistemi operativi o driver UEFI in esecuzione in fase di avvio. L'avvio protetto è abilitato per impostazione predefinita per le macchine virtuali di seconda generazione. Se è necessario eseguire un sistema operativo guest che non è supportato da funzionalità avvio protetto, è possibile disabilitarlo dopo la creazione della macchina virtuale.  Per ulteriori informazioni, vedere [avvio protetto](https://technet.microsoft.com/library/dn486875.aspx).  

    Alle macchine virtuali di Linux generazione 2 di avvio protetto, è necessario scegliere il modello di avvio protetto di UEFI CA quando si crea la macchina virtuale.  

- **Più grande volume di avvio** il volume di avvio massimo per le macchine virtuali di generazione 2 è di 64 TB. Questa è la dimensione massima del disco supportata da una. VHDX. Per le macchine virtuali di generazione 1, il volume di avvio massima pari a 2TB per un. VHDX e 2040GB per una. DISCO RIGIDO VIRTUALE. Per altre informazioni, vedere [Hyper-V Virtual Hard Disk Format Overview](https://technet.microsoft.com/library/hh831446.aspx).  

  È anche possibile vedere un leggero miglioramento nei tempi di avvio e installazione di macchina virtuale con macchine virtuali di generazione 2.

## <a name="whats-the-difference-in-device-support"></a>Che cos'è la differenza in supporto dei dispositivi?

La tabella seguente confronta i dispositivi disponibili tra la generazione 1 e macchine virtuali di generazione 2.  

|Dispositivo di prima generazione|Sostituzione con la seconda generazione|Miglioramenti della seconda generazione|  
|-----------------------|----------------------------|-----------------------------|  
|Controller IDE|Controller SCSI virtuale|Avvio da vhdx (dimensione massima 64 TB e capacità di ridimensionamento online)|  
|CD-ROM IDE|CD-ROM SCSI virtuale|Supporto di 64 dispositivi DVD SCSI per ogni controller SCSI.|  
|BIOS legacy|Firmware UEFI|Avvio protetto|  
|Scheda di rete legacy|Scheda di rete sintetica|Avvio di rete con IPv4 e IPv6|  
|Controller floppy e controller DMA|Nessun supporto per il controller floppy|N/D|  
|Universal Asynchronous Receiver/Transmitter (UART) per porte COM|UART facoltativo per il debug|Più veloce e affidabile|  
|Controller tastiera i8042|Input basato sul software|Usa meno risorse perché non sono previste emulazioni. Inoltre riduce la superficie di attacco dal sistema operativo guest.|  
|Tastiera PS/2|Tastiera basata sul software|Usa meno risorse perché non sono previste emulazioni. Inoltre riduce la superficie di attacco dal sistema operativo guest.|  
|Mouse PS/2|Mouse basato sul software|Usa meno risorse perché non sono previste emulazioni. Inoltre riduce la superficie di attacco dal sistema operativo guest.|  
|Video S3|Video basato sul software|Usa meno risorse perché non sono previste emulazioni. Inoltre riduce la superficie di attacco dal sistema operativo guest.|  
|Bus PCI|Non più necessario|N/D|  
|Programmable Interrupt Controller (PIC)|Non più necessario|N/D|  
|Programmable Interval Timer (PIT)|Non più necessario|N/D|  
|Dispositivo Super I/O|Non più necessario|N/D|  

## <a name="more-about-generation-2-virtual-machines"></a>Informazioni sulle macchine virtuali di generazione 2

Di seguito sono riportati alcuni suggerimenti aggiuntivi sull'uso di macchine virtuali di generazione 2.

### <a name="attach-or-add-a-dvd-drive"></a>Connettere o aggiungere un'unità DVD

- È possibile collegare un'unità CD o DVD fisica a una macchina virtuale di generazione 2. L'unità DVD virtuale nelle macchine virtuali di seconda generazione supporta solo file di immagine ISO. Per creare un file immagine ISO di un ambiente Windows, è possibile usare lo strumento da riga di comando Oscdimg. Per altre informazioni, vedere [Opzioni della riga di comando di Oscdimg](https://msdn.microsoft.com/library/hh824847.aspx).
- Quando si crea una nuova macchina virtuale con il cmdlet New-VM Windows PowerShell, la macchina virtuale di generazione 2 non ha un'unità DVD. È possibile aggiungere un'unità DVD mentre la macchina virtuale è in esecuzione.

### <a name="use-uefi-firmware"></a>Usano un firmware UEFI

- Avvio protetto o firmware UEFI non è necessario sull'host Hyper-V fisico. Hyper-V offre firmware virtuale alle macchine virtuali che è indipendente di ciò che l'host Hyper-V.
- Firmware UEFI in una macchina virtuale di generazione 2 non supporta la modalità di installazione per avvio protetto.
- Non è supportata l'esecuzione di una shell UEFI o altre applicazioni UEFI in una macchina virtuale di generazione 2. L'uso di una shell UEFI o di applicazioni UEFI non Microsoft è tecnicamente possibile se vengono compilate direttamente dalle origini. Se queste applicazioni non in modo appropriato firmate digitalmente, è necessario disabilitare avvio protetto per la macchina virtuale.

### <a name="work-with-vhdx-files"></a>Lavorare con i file VHDX

- È possibile ridimensionare un file VHDX che contiene il volume di avvio per una macchina virtuale di generazione 2, mentre la macchina virtuale è in esecuzione.
- Microsoft non supporta o consigliabile creare un file VHDX di avvio per la generazione 1 e macchine virtuali di generazione 2.  
- La generazione è una proprietà della macchina virtuale e non del disco rigido virtuale. Pertanto, è possibile stabilire se un file VHDX è stato creato da una generazione 1 o una macchina virtuale di generazione 2.  
- Un file VHDX creato con una generazione 2 macchina virtuale può essere collegato al controller IDE o al controller SCSI di una macchina virtuale di generazione 1. Tuttavia, se si tratta di un file VHDX di avvio, non si avvia la macchina virtuale di generazione 1.

### <a name="use-ipv6-instead-of-ipv4"></a>Utilizzo di IPv6 anziché IPv4

Per impostazione predefinita, le macchine virtuali di seconda generazione usano IPv4. Per usare invece IPv6, eseguire la [Set-VMFirmware](https://technet.microsoft.com/library/dn464287.aspx) cmdlet di Windows PowerShell. Ad esempio, il comando seguente imposta il protocollo preferito su IPv6 per una macchina virtuale denominata TestVM:  

```powershell
Set-VMFirmware -VMName TestVM -IPProtocolPreference IPv6  
```  

## <a name="add-a-com-port-for-kernel-debugging"></a>Aggiungere una porta COM per il debug del kernel

Le porte COM non sono disponibili nelle macchine virtuali di generazione 2 fino a quando non vengono aggiunti. È possibile eseguire questa operazione con Windows PowerShell o Strumentazione gestione Windows (WMI). Questi passaggi illustrano come eseguire questa operazione con Windows PowerShell.

Per aggiungere una porta COM:  

1. Disabilitare l'avvio protetto. Debug del kernel non è compatibile con avvio protetto. Verificare che la macchina virtuale è disattivata, quindi usare il [Set-VMFirmware](https://technet.microsoft.com/library/dn464287.aspx) cmdlet. Ad esempio, questo comando disabilita la funzionalità avvio protetto nella macchina virtuale TestVM:  

    ```powershell  
    Set-VMFirmware -Vmname TestVM -EnableSecureBoot Off  
    ```  

2. Aggiungere una porta COM. Usare la [Set-VMComPort](https://technet.microsoft.com/library/hh848616.aspx) cmdlet per eseguire questa operazione. Ad esempio, il comando seguente consente di configurare la prima porta COM nella macchina virtuale, TestVM, per la connessione alla named pipe, TestPipe, sul computer locale:  

    ```powershell
    Set-VMComPort -VMName TestVM 1 \\.\pipe\TestPipe  
    ```  

> [!NOTE]  
> Le porte COM configurate non sono presenti nelle impostazioni di una macchina virtuale in Hyper-V Manager.

## <a name="see-also"></a>Vedere anche  

- [Linux e FreeBSD le macchine virtuali in Hyper-V](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)
- [Usare le risorse locali nella macchina virtuale Hyper-V con VMConnect](../learn-more/Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md)
- [Pianificare la scalabilità di Hyper-V in Windows Server 2016](Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md)
