---
title: È necessario creare una macchina virtuale di generazione 1 o 2 in Hyper-V?
description: Vengono fornite considerazioni quali i metodi di avvio supportati e altre differenze di funzionalità che consentono di scegliere la generazione che soddisfa le proprie esigenze.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.topic: article
ms.assetid: 02e31413-6140-4723-a8d6-46c7f667792d
author: kbdazure
ms.author: kathydav
ms.date: 12/05/2016
ms.openlocfilehash: 0e8b8dbaa937229b5a87560f3993bb07d7cd7ff8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860774"
---
# <a name="should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v"></a>È necessario creare una macchina virtuale di generazione 1 o 2 in Hyper-V?

>Si applica a: Windows 10, Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

> [!NOTE]
> Se si prevede di caricare le macchine virtuali (VM) Windows dalle VM locali a Microsoft Azure, le VM di prima e seconda generazione nel formato di file VHD e sono supportati un disco a dimensione fissa. Per altre informazioni sulle funzionalità di generazione 2 supportate in Azure, vedere [macchine virtuali di seconda generazione in Azure](https://docs.microsoft.com/azure/virtual-machines/windows/generation-2) . Per ulteriori informazioni sul caricamento di un disco rigido virtuale Windows o VHDX, vedere [preparare un disco rigido virtuale Windows o VHDX per il caricamento in Azure](https://docs.microsoft.com/azure/virtual-machines/windows/prepare-for-upload-vhd-image).

La scelta di creare una macchina virtuale di prima o di seconda generazione dipende dal sistema operativo guest che si desidera installare e dal metodo di avvio che si desidera utilizzare per distribuire la macchina virtuale. È consigliabile creare una macchina virtuale di seconda generazione per sfruttare le funzionalità come l'avvio protetto, a meno che non sia soddisfatta una delle seguenti istruzioni:  

- Il disco rigido virtuale da cui si vuole eseguire l'avvio non è [compatibile con UEFI](https://technet.microsoft.com/library/hh824898.aspx).  
- La seconda generazione non supporta il sistema operativo che si vuole eseguire nella macchina virtuale.  
- La seconda generazione non supporta il metodo di avvio che si vuole usare.  

Per ulteriori informazioni sulle funzionalità disponibili con le macchine virtuali di seconda generazione, vedere [compatibilità delle funzionalità Hyper-V per generazione e Guest](../Hyper-V-feature-compatibility-by-generation-and-guest.md).

Non è possibile modificare la generazione di una macchina virtuale dopo averla creata. È quindi consigliabile esaminare qui le considerazioni, nonché scegliere il sistema operativo, il metodo di avvio e le funzionalità che si desidera utilizzare prima di scegliere una generazione.  

## <a name="which-guest-operating-systems-are-supported"></a>Quali sistemi operativi guest sono supportati?

Le macchine virtuali di prima generazione supportano la maggior parte dei sistemi operativi guest. Le macchine virtuali di seconda generazione supportano la maggior parte delle versioni a 64 bit di Windows e versioni più recenti dei sistemi operativi Linux e FreeBSD. Usare le sezioni seguenti per verificare la generazione di macchine virtuali che supporta il sistema operativo guest che si vuole installare.  

- [Supporto del sistema operativo guest Windows](#windows-guest-operating-system-support)  

- [Supporto del sistema operativo guest CentOS e Red Hat Enterprise Linux](#centos-and-red-hat-enterprise-linux-guest-operating-system-support)  

- [Supporto del sistema operativo guest Debian](#debian-guest-operating-system-support)  

- [Supporto del sistema operativo guest FreeBSD](#freebsd-guest-operating-system-support)  

- [Supporto del sistema operativo guest Oracle Linux](#oracle-linux-guest-operating-system-support)  

- [Supporto del sistema operativo guest SUSE](#suse-guest-operating-system-support)  

- [Supporto del sistema operativo guest Ubuntu](#ubuntu-guest-operating-system-support)  

### <a name="windows-guest-operating-system-support"></a>Supporto del sistema operativo guest Windows

La tabella seguente illustra le versioni di Windows a 64 bit che è possibile usare come sistema operativo guest per le macchine virtuali di prima e seconda generazione.  

|versioni di Windows a 64 bit|Prima generazione|Seconda generazione|  
|-------------------------------|----------------|----------------|  
| Windows Server 2019 |&#10004;|&#10004;|  
| Windows Server 2016 |&#10004;|&#10004;|  
| Windows Server 2012 R2 |&#10004;|&#10004;|  
| Windows Server 2012 |&#10004;|&#10004;|  
|Windows Server 2008 R2|&#10004;| &#10006;|  
|Windows Server 2008|&#10004;| &#10006;|  
|Windows 10|&#10004;|&#10004;|  
|Windows 8.1|&#10004;|&#10004;|  
|Windows 8|&#10004;|&#10004;|  
|Windows 7|&#10004;| &#10006;|

La tabella seguente illustra le versioni di Windows a 32 bit che è possibile usare come sistema operativo guest per le macchine virtuali di prima e seconda generazione.

|versioni di Windows a 32 bit|Prima generazione|Seconda generazione|  
|-------------------------------|----------------|----------------|  
|Windows 10|&#10004;| &#10006;|  
|Windows 8.1|&#10004;| &#10006;|  
|Windows 8|&#10004;| &#10006;|  
|Windows 7|&#10004;| &#10006;|  

### <a name="centos-and-red-hat-enterprise-linux-guest-operating-system-support"></a>Supporto del sistema operativo guest CentOS e Red Hat Enterprise Linux

La tabella seguente illustra le versioni di Red Hat Enterprise Linux \(RHEL\) e CentOS che è possibile usare come sistema operativo guest per le macchine virtuali di prima e seconda generazione.

|Versioni del sistema operativo|Prima generazione|Seconda generazione|  
|-----------------------------|----------------|----------------|  
|Serie RHEL/CentOS 7. x|&#10004;|&#10004;|  
|Serie RHEL/CentOS 6. x|&#10004;|&#10004;<br />**Nota:** Supportato solo in Windows Server 2016 e versioni successive.|  
|Serie RHEL/CentOS 5. x|&#10004;| &#10006;|  

Per ulteriori informazioni, vedere [CentOS e Red Hat Enterprise Linux macchine virtuali in Hyper-V](../Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md).  

### <a name="debian-guest-operating-system-support"></a>Supporto del sistema operativo guest Debian  

La tabella seguente illustra le versioni di Debian che è possibile usare come sistema operativo guest per le macchine virtuali di prima e seconda generazione.

|Versioni del sistema operativo|Prima generazione|Seconda generazione|  
|-----------------------------|----------------|----------------|  
|Serie Debian 7. x|&#10004;| &#10006;|  
|Serie Debian 8. x|&#10004;|&#10004;|  

Per ulteriori informazioni, vedere la pagina [relativa alle macchine virtuali Debian in Hyper-V](../Supported-Debian-virtual-machines-on-Hyper-V.md).  

### <a name="freebsd-guest-operating-system-support"></a>Supporto del sistema operativo guest FreeBSD

La tabella seguente illustra le versioni di FreeBSD che è possibile usare come sistema operativo guest per le macchine virtuali di prima e seconda generazione.  

|Versioni del sistema operativo|Prima generazione|Seconda generazione|  
|-----------------------------|----------------|----------------|  
|FreeBSD 10 e 10,1|&#10004;| &#10006;|  
|FreeBSD 9,1 e 9,3|&#10004;| &#10006;|  
|FreeBSD 8,4|&#10004;| &#10006;|  

Per ulteriori informazioni, vedere [FreeBSD Virtual Machines in Hyper-V](../Supported-FreeBSD-virtual-machines-on-Hyper-V.md).  

### <a name="oracle-linux-guest-operating-system-support"></a>Supporto del sistema operativo guest Oracle Linux  

La tabella seguente illustra le versioni di serie di kernel compatibili con Red Hat che è possibile usare come sistema operativo guest per le macchine virtuali di prima e seconda generazione.  

|Versioni della serie kernel compatibili con Red Hat|Prima generazione|Seconda generazione|  
|---------------------------------------------|----------------|----------------|  
|Serie Oracle Linux 7. x|&#10004;|&#10004;|
|Oracle Linux serie 6. x|&#10004;| &#10006;|  

La tabella seguente illustra le versioni del kernel Enterprise Unbreakable che è possibile usare come sistema operativo guest per le macchine virtuali di prima e seconda generazione.

|Versioni UEK (Unbreakable Enterprise kernel)|Prima generazione|Seconda generazione|  
|--------------------------------------------------|----------------|----------------|  
|Oracle Linux UEK R3 QU3|&#10004;| &#10006;|  
|Oracle Linux UEK R3 QU2|&#10004;| &#10006;|  
|Oracle Linux UEK R3 QU1|&#10004;| &#10006;|  

Per ulteriori informazioni, vedere [Oracle Linux macchine virtuali in Hyper-V](../Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md).  

### <a name="suse-guest-operating-system-support"></a>Supporto del sistema operativo guest SUSE

La tabella seguente illustra le versioni di SUSE che è possibile usare come sistema operativo guest per le macchine virtuali di prima e seconda generazione.

|Versioni del sistema operativo|Prima generazione|Seconda generazione|  
|-----------------------------|----------------|----------------|  
|Serie SUSE Linux Enterprise Server 12|&#10004;|&#10004;|  
|Serie SUSE Linux Enterprise Server 11|&#10004;| &#10006;|  
|Aprire SUSE 12.3 +|&#10004;| &#10006;|  

Per ulteriori informazioni, vedere la pagina [relativa alle macchine virtuali SUSE in Hyper-V](../Supported-SUSE-virtual-machines-on-Hyper-V.md).  

### <a name="ubuntu-guest-operating-system-support"></a>Supporto del sistema operativo guest Ubuntu

La tabella seguente illustra le versioni di Ubuntu che è possibile usare come sistema operativo guest per le macchine virtuali di prima e seconda generazione.

|Versioni del sistema operativo|Prima generazione|Seconda generazione|  
|-----------------------------|----------------|----------------|  
|Ubuntu 14,04 e versioni successive|&#10004;|&#10004;|  
|Ubuntu 12.04|&#10004;| &#10006;|  

Per ulteriori informazioni, vedere la pagina [relativa alle macchine virtuali Ubuntu in Hyper-V](../Supported-Ubuntu-virtual-machines-on-Hyper-V.md).  

## <a name="how-can-i-boot-the-virtual-machine"></a>Come è possibile avviare la macchina virtuale?

La tabella seguente illustra i metodi di avvio supportati dalle macchine virtuali di prima e di seconda generazione.  

|Metodo di avvio|Prima generazione|Seconda generazione|  
|---------------|----------------|----------------|  
|Avvio PXE tramite una scheda di rete standard| &#10006;|&#10004;|  
|Avvio PXE tramite una scheda di rete legacy|&#10004;| &#10006;|  
|Eseguire l'avvio da un disco rigido virtuale SCSI (. VHDX) o DVD virtuale (. ISO| &#10006;|&#10004;|  
|Avvio dal disco rigido virtuale del controller IDE (. VHD) o DVD virtuale (. ISO|&#10004;| &#10006;|  
|Avvio da floppy (. VFD|&#10004;| &#10006;|  

## <a name="what-are-the-advantages-of-using-generation-2-virtual-machines"></a>Quali sono i vantaggi dell'uso di macchine virtuali di seconda generazione?

Ecco alcuni dei vantaggi che si ottengono quando si usa una macchina virtuale di seconda generazione:  
- **Avvio protetto** Si tratta di una funzionalità che verifica che il caricatore di avvio sia firmato da un'autorità attendibile nel database UEFI per impedire l'esecuzione di firmware, sistemi operativi o driver UEFI non autorizzati in fase di avvio. L'avvio protetto è abilitato per impostazione predefinita per le macchine virtuali di seconda generazione. Se è necessario eseguire un sistema operativo guest che non è supportato dall'avvio protetto, è possibile disabilitarlo dopo la creazione della macchina virtuale.  Per ulteriori informazioni, vedere [avvio protetto](https://technet.microsoft.com/library/dn486875.aspx).  

    Per proteggere le macchine virtuali Linux di seconda generazione, è necessario scegliere il modello di avvio protetto della CA UEFI quando si crea la macchina virtuale.  

- **Volume di avvio più grande** Il volume di avvio massimo per le macchine virtuali di seconda generazione è 64 TB. Dimensioni massime del disco supportate da. VHDX. Per le macchine virtuali di prima generazione, il volume di avvio massimo è 2 TB per un. VHDX e 2040GB per un oggetto. VHD. Per ulteriori informazioni, vedere [Panoramica del formato del disco rigido virtuale Hyper-V](https://technet.microsoft.com/library/hh831446.aspx).  

  È anche possibile che si verifichi un lieve miglioramento dei tempi di avvio e di installazione delle macchine virtuali con le macchine virtuali di seconda generazione.

## <a name="whats-the-difference-in-device-support"></a>Qual è la differenza nel supporto dei dispositivi?

Nella tabella seguente vengono confrontati i dispositivi disponibili tra le macchine virtuali di prima e seconda generazione.  

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

## <a name="more-about-generation-2-virtual-machines"></a>Altre informazioni sulle macchine virtuali di seconda generazione

Ecco alcuni suggerimenti aggiuntivi sull'uso delle macchine virtuali di seconda generazione.

### <a name="attach-or-add-a-dvd-drive"></a>Connetti o Aggiungi un'unità DVD

- Non è possibile aggiungere un'unità CD o DVD fisica a una macchina virtuale di seconda generazione. L'unità DVD virtuale nelle macchine virtuali di seconda generazione supporta solo file di immagine ISO. Per creare un file immagine ISO di un ambiente Windows, è possibile usare lo strumento da riga di comando Oscdimg. Per altre informazioni, vedere [Opzioni della riga di comando di Oscdimg](https://msdn.microsoft.com/library/hh824847.aspx).
- Quando si crea una nuova macchina virtuale con il cmdlet di Windows PowerShell New-VM, la macchina virtuale di seconda generazione non ha un'unità DVD. È possibile aggiungere un'unità DVD mentre la macchina virtuale è in esecuzione.

### <a name="use-uefi-firmware"></a>USA firmware UEFI

- L'avvio protetto o il firmware UEFI non è necessario nell'host Hyper-V fisico. Hyper-V fornisce il firmware virtuale alle macchine virtuali indipendenti dall'host Hyper-V.
- Il firmware UEFI in una macchina virtuale di seconda generazione non supporta la modalità di installazione per l'avvio protetto.
- Non è supportata l'esecuzione di una Shell UEFI o altre applicazioni UEFI in una macchina virtuale di seconda generazione. L'uso di una shell UEFI o di applicazioni UEFI non Microsoft è tecnicamente possibile se vengono compilate direttamente dalle origini. Se queste applicazioni non hanno una firma digitale appropriata, è necessario disabilitare l'avvio protetto per la macchina virtuale.

### <a name="work-with-vhdx-files"></a>Usare i file VHDX

- È possibile ridimensionare un file VHDX contenente il volume di avvio per una macchina virtuale di seconda generazione mentre la macchina virtuale è in esecuzione.
- Non è supportata né consigliata la creazione di un file VHDX di avvio per le macchine virtuali di prima e di seconda generazione.  
- La generazione è una proprietà della macchina virtuale e non del disco rigido virtuale. Quindi, non è possibile stabilire se un file VHDX è stato creato da una macchina virtuale di prima o di seconda generazione.  
- Un file VHDX creato con una macchina virtuale di seconda generazione può essere collegato al controller IDE o al controller SCSI di una macchina virtuale di prima generazione. Tuttavia, se si tratta di un file VHDX di avvio, la macchina virtuale di prima generazione non verrà avviata.

### <a name="use-ipv6-instead-of-ipv4"></a>Usare IPv6 invece di IPv4

Per impostazione predefinita, le macchine virtuali di seconda generazione usano IPv4. Per usare invece IPv6, eseguire il cmdlet [set-VMFirmware](https://technet.microsoft.com/library/dn464287.aspx) di Windows PowerShell. Ad esempio, il comando seguente imposta il protocollo preferito su IPv6 per una macchina virtuale denominata TestVM:  

```powershell
Set-VMFirmware -VMName TestVM -IPProtocolPreference IPv6  
```  

## <a name="add-a-com-port-for-kernel-debugging"></a>Aggiungere una porta COM per il debug del kernel

Le porte COM non sono disponibili nelle macchine virtuali di seconda generazione fino a quando non vengono aggiunte. Questa operazione può essere eseguita con Windows PowerShell o Strumentazione gestione Windows (WMI). Questi passaggi illustrano come eseguire questa operazione con Windows PowerShell.

Per aggiungere una porta COM:  

1. Disabilitare l'avvio protetto. Il debug del kernel non è compatibile con l'avvio protetto. Verificare che lo stato della macchina virtuale sia disattivato, quindi usare il cmdlet [set-VMFirmware](https://technet.microsoft.com/library/dn464287.aspx) . Ad esempio, il comando seguente disabilita l'avvio protetto nella macchina virtuale TestVM:  

    ```powershell  
    Set-VMFirmware -Vmname TestVM -EnableSecureBoot Off  
    ```  

2. Aggiungere una porta COM. Per eseguire questa operazione, usare il cmdlet [set-VMComPort](https://technet.microsoft.com/library/hh848616.aspx) . Ad esempio, il comando seguente configura la prima porta COM nella macchina virtuale, TestVM, per connettersi al named pipe, TestPipe, nel computer locale:  

    ```powershell
    Set-VMComPort -VMName TestVM 1 \\.\pipe\TestPipe  
    ```  

> [!NOTE]  
> Le porte COM configurate non sono elencate nelle impostazioni di una macchina virtuale nella console di gestione di Hyper-V.

## <a name="see-also"></a>Vedi anche  

- [Macchine virtuali Linux e FreeBSD in Hyper-V](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)
- [Usare le risorse locali nella macchina virtuale Hyper-V con VMConnect](../learn-more/Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md)
- [Pianificare la scalabilità di Hyper-V in Windows Server 2016](Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md)
