---
title: Piano per la distribuzione di dispositivi usando discreti dispositivo assegnazione
description: Informazioni su come DDA funziona in Windows Server
ms.prod: windows-server-threshold
ms.service: na
ms.technology: hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: chrishuybregts
ms.author: chrihu
ms.date: 02/06/2018
ms.openlocfilehash: c64c2b75c00f97622278c3e590db46995e108218
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840202"
---
# <a name="plan-for-deploying-devices-using-discrete-device-assignment"></a>Pianificare la distribuzione di dispositivi utilizzando discreti dispositivo assegnazione
>Si applica a: Microsoft Hyper-V Server 2016, Windows Server 2016, Microsoft Hyper-V Server 2019, Windows Server 2019

Assegnazione dispositivo discreti consente hardware PCIe fisico deve essere accessibile direttamente da una macchina virtuale.  Questa guida illustra il tipo di dispositivi che possono usare discreti dispositivo assegnazione, requisiti di sistema host, le limitazioni imposte alle macchine virtuali, nonché le implicazioni di sicurezza di assegnazione dispositivo discreti.

Per la versione iniziale dell'assegnazione dispositivo discreti, siamo concentrati su due classi di dispositivi devono essere formalmente supportati da Microsoft: Schede video grafiche e archiviazione NVMe dispositivi.  Altri dispositivi sono probabile che funzionino e fornitori di hardware sono in grado di offrire le istruzioni di supporto per tali dispositivi.  Per questi dispositivi, contattare i fornitori di hardware per il supporto.

Se si è pronti per provare discreti dispositivo assegnazione, è possibile al discorso [distribuzione di grafica dispositivi usando discreti dispositivo assegnazione](../deploy/Deploying-graphics-devices-using-dda.md) oppure [distribuisce i dispositivi di archiviazione usando discreti dispositivo assegnazione](../deploy/Deploying-storage-devices-using-dda.md) Per iniziare!

## <a name="supported-virtual-machines-and-guest-operating-systems"></a>Le macchine virtuali supportate e i sistemi operativi Guest
Assegnazione dispositivo discreti è supportata per la generazione 1 o 2 macchine virtuali.  Inoltre, i guest supportati includono Windows 10, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 con [KB 3133690](https://support.microsoft.com/kb/3133690) applicato e diverse distribuzioni del [del sistema operativo Linux.](../supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows.md)

## <a name="system-requirements"></a>Requisiti di sistema
Oltre al [requisiti di sistema per Windows Server](../../../get-started/System-Requirements--and-Installation.md) e il [requisiti di sistema per Hyper-V](../System-requirements-for-Hyper-V-on-Windows.md), discreti dispositivo assegnazione richiede hardware di classe server che è in grado di concedere il controllo del sistema operativo sulla configurazione dell'infrastruttura PCIe (PCI Express controllo nativo). Inoltre, la complessa radice PCIe deve supportare "Access Control Services" (ACS), che consente a Hyper-V forzare tutto il traffico PCIe attraverso il MMU dei/o.

Queste funzionalità in genere non sono esposte direttamente nel BIOS del server e sono spesso nascosta dietro le altre impostazioni.  Ad esempio, le stesse funzionalità sono necessari per il supporto di SR-IOV e nel BIOS potrebbe essere necessario impostare "Abilitare SR-IOV."  Contattare il fornitore del sistema se non si riesce a identificare la corretta impostazione nel BIOS.

Per garantire l'hardware di hardware è in grado di assegnazione dispositivo discreti, i tecnici hanno mettere insieme una [Script del profilo computer](#machine-profile-script) che è possibile eseguire in un host Hyper-V abilitato per verificare se il server sia correttamente il programma di installazione e cosa i dispositivi sono in grado di assegnazione dispositivo discreti.

## <a name="device-requirements"></a>Requisiti del dispositivo
Non tutti i dispositivi PCIe sono utilizzabile con assegnazione dispositivo discreti.  Ad esempio, dispositivi meno recenti che consentono di sfruttare gli interrupt legacy PCI (INTx) non sono supportati. Di Jake Oshin [post di blog](https://blogs.technet.microsoft.com/virtualization/2015/11/20/discrete-device-assignment-machines-and-devices/) passare in modo più dettagliato, tuttavia, per il consumer, in esecuzione il [Script del profilo computer](#machine-profile-script) verranno visualizzati i dispositivi che sono in grado di utilizzato per l'assegnazione dispositivo discreti.

Produttori del dispositivo impiegano possono rivolgersi al proprio rappresentante Microsoft per altri dettagli.

## <a name="device-driver"></a>Driver di dispositivo
Come discreti dispositivo assegnazione passa l'intero dispositivo PCIe alla macchina virtuale Guest, un driver di host non è necessario installare prima il dispositivo in cui vengono montato nella macchina virtuale.  L'unico requisito dell'host è che il dispositivo [PCIe Location Path](#pcie-location-path) può essere determinato.  Driver del dispositivo può essere installata facoltativamente se questo consente di identificare il dispositivo.  Ad esempio, una GPU senza il relativo driver di dispositivo installato nell'host può essere un dispositivo di eseguire il rendering di base Microsoft.  Se è installato il driver di dispositivo, il produttore e modello verrà probabilmente visualizzati.

Una volta il dispositivo è montato nella macchina Guest, il driver del produttore dispositivo ora può essere installato come normale all'interno della macchina virtuale guest.  

## <a name="virtual-machine-limitations"></a>Limitazioni di macchina virtuale
A causa della natura della modalità di implementazione discreti dispositivo assegnazione, alcune funzionalità di una macchina virtuale sono limitate, mentre un dispositivo è collegato.  Le funzionalità seguenti non sono disponibili:
- Salvare/ripristinare la macchina virtuale
- Migrazione in tempo reale di una macchina virtuale
- L'utilizzo della memoria dinamica
- Aggiunta della macchina virtuale a un cluster a disponibilità elevata (HA)

## <a name="security"></a>Sicurezza
Assegnazione dispositivo discreti passa l'intero dispositivo alla macchina virtuale.  Ciò significa che tutte le funzionalità del dispositivo sono accessibili dal sistema operativo guest. Alcune funzionalità, ad esempio l'aggiornamento del firmware, potrebbero compromettere la stabilità del sistema. Di conseguenza, numerosi avvisi vengono presentati all'amministratore quando la si smonta il dispositivo dall'host. È consigliabile che discreti dispositivo assegnazione viene usato solo in cui i tenant delle macchine virtuali sono attendibili.  

Se l'amministratore desidera usare un dispositivo con un tenant non attendibile, Microsoft ha fornito i produttori di dispositivi con la possibilità di creare un driver di dispositivo mitigazione dei rischi che può essere installato nell'host.  Per informazioni dettagliate su che forniscono un Driver di mitigazione dei rischi del dispositivo, contattare il produttore del dispositivo.

Se si desidera ignorare i controlli di sicurezza per un dispositivo che non ha un Driver di mitigazione dei rischi del dispositivo, è necessario passare il `-Force` parametro per il `Dismount-VMHostAssignableDevice` cmdlet.  Tenere presente che in questo modo, è stato modificato il profilo di sicurezza del sistema e questo è solo consigliato durante la creazione di prototipi o attendibili gli ambienti.

## <a name="pcie-location-path"></a>Percorso PCIe
Il percorso del PCIe è necessario smontare e montare il dispositivo dall'Host.  Un esempio di percorso di percorso simile al seguente: `"PCIROOT(20)#PCI(0300)#PCI(0000)#PCI(0800)#PCI(0000)"`.   Il [Script del profilo computer](#machine-profile-script) restituirà anche il percorso del dispositivo PCIe.

### <a name="getting-the-location-path-by-using-device-manager"></a>Ottenere il percorso utilizzando Gestione dispositivi
![Gestione dispositivi](../deploy/media/dda-devicemanager.png)
- Aprire Gestione dispositivi e individuare il dispositivo.  
- Fare clic con il pulsante destro del dispositivo e selezionare "Proprietà".
- Passare alla scheda dettagli e selezionare "Percorsi" nell'elenco a discesa proprietà.  
- Pulsante destro del mouse, fare clic sulla voce che inizia con "PCIROOT" e selezionare "Copia".  È ora disponibile il percorso per il dispositivo.

## <a name="mmio-space"></a>MMIO Space
Alcuni dispositivi, in particolare le GPU, richiedono ulteriore spazio MMIO da allocare alla macchina virtuale per la memoria del dispositivo sia accessibile. Per impostazione predefinita, ogni macchina virtuale di avvio inizia con 128MB di spazio insufficiente MMIO e 512MB di spazio MMIO elevato allocata. Tuttavia, un dispositivo potrebbe richiedere più spazio MMIO o più dispositivi possono essere passati tramite tale che i requisiti combinati superano questi valori.  Modifica spaziatura MMIO è estremamente semplice e può essere eseguita in PowerShell usando i comandi seguenti:

```
Set-VM -LowMemoryMappedIoSpace 3Gb -VMName $vm
Set-VM -HighMemoryMappedIoSpace 33280Mb -VMName $vm
```
Il modo più semplice per determinare la quantità di spazio da allocare MMIO consiste nell'usare la [Script del profilo computer](#machine-profile-script).  In alternativa, è possibile calcolarla mediante la gestione di dispositivi. Vedere il blog TechNet post [discreti dispositivo assegnazione - GPU](https://blogs.technet.microsoft.com/virtualization/2015/11/23/discrete-device-assignment-gpus/) per altri dettagli.

## <a name="machine-profile-script"></a>Script del profilo computer
Per semplificare l'identificazione se il server sia configurato correttamente e quali dispositivi sono disponibili per essere passato tramite l'utilizzo dell'assegnazione dispositivo discreti, uno dei tecnici Microsoft raccolto lo script di PowerShell seguente: [SurveyDDA.ps1.](https://github.com/Microsoft/Virtualization-Documentation/blob/live/hyperv-tools/DiscreteDeviceAssignment/SurveyDDA.ps1)

Prima di usare lo script, assicurarsi si è installato il ruolo Hyper-V e si esegue lo script da una finestra di comando di PowerShell con privilegi di amministratore.

Se il sistema è configurato correttamente per supportare l'assegnazione dispositivo discreti, lo strumento visualizzerà un messaggio di errore su ciò che è errato. Se lo strumento rileva che il sistema configurato correttamente, permette di enumerare tutti i dispositivi che sono disponibili nel PCIe Bus.

Per ogni dispositivo che viene trovata, verrà visualizzato se è in grado di essere utilizzato con assegnazione dispositivo discreti. Se un dispositivo viene identificato come compatibile con l'assegnazione dispositivo discreti, lo script verrà fornito un motivo.  Quando un dispositivo viene identificato correttamente come compatibile, verrà visualizzato il percorso di posizione del dispositivo.  Inoltre, se è necessario che il dispositivo [spazio MMIO](#mmio-space), verrà visualizzato anche.

![SurveyDDA.ps1](./images/hyper-v-surveydda-ps1.png)

## <a name="frequently-asked-questions"></a>Domande frequenti

### <a name="how-does-remote-desktops-remotefx-vgpu-technology-relate-to-discrete-device-assignment"></a>Modo tecnologia GPU virtualizzata RemoteFX del Desktop remoto è correlato a discreti dispositivo assegnazione?
Sono completamente separate le tecnologie. GPU virtualizzata RemoteFX non dovrà essere installato per l'assegnazione dispositivo discreti lavorare. Inoltre, ruoli aggiuntivi non sono richiesti da installare. GPU virtualizzata RemoteFX richiede il ruolo RDVH venga installato in ordine per il driver di GPU virtualizzata RemoteFX deve essere presente nella macchina virtuale. Per l'assegnazione dispositivo discreti, poiché verrà installato il driver del fornitore dell'Hardware alla macchina virtuale, ruoli aggiuntivi non devono essere presenti.  

### <a name="ive-passed-a-gpu-into-a-vm-but-remote-desktop-or-an-application-isnt-recognizing-the-gpu"></a>Ho passato una GPU in una macchina virtuale ma Desktop remoto o un'applicazione non è il riconoscimento della GPU
Esistono diversi motivi, che questo problema può verificarsi, ma alcuni problemi comuni sono elencati di seguito.
- Verificare che il driver del fornitore GPU più recenti sia installato e non segnala un errore controllando lo stato del dispositivo nella gestione dispositivi.
- Assicurarsi che il dispositivo abbia sufficiente [spazio MMIO](#mmio-space) ad essa allocato nella macchina virtuale.
- Verificare che si sta usando una GPU che supporta il fornitore utilizzato in questa configurazione. Ad esempio, alcuni fornitori impediscono le relative schede consumer il funzionamento quando si è passati a una macchina virtuale.
- Verificare che l'applicazione in esecuzione supporta l'esecuzione all'interno di una macchina virtuale e che sia GPU e i driver associati sono supportati dall'applicazione. Alcune applicazioni presentano degli elenchi elementi consentiti di GPU e ambienti.
- Se si usa il ruolo Host sessione Desktop remoto o Windows Multipoint Services nel guest, è necessario assicurarsi che una voce specifica di criteri di gruppo è impostata per consentire l'utilizzo della GPU predefinita. Usando un oggetto Criteri di gruppo applicati a guest (o l'Editor criteri di gruppo locali nel guest), passare all'elemento dei criteri di gruppo seguente:
   - Configurazione computer
   - Modelli di amministrazione
   - Componenti di Windows
   - Servizi Desktop remoto
   - Host sessione Desktop remoto
   - Ambiente di sessione remota
   - Usare l'adattatore di grafica hardware predefinito per tutte le sessioni di Servizi Desktop remoto

    Impostare questo valore su abilitato, quindi riavviare la macchina virtuale una volta che viene applicato il criterio.

### <a name="can-discrete-device-assignment-take-advantage-of-remote-desktops-avc444-codec"></a>Assegnazione dispositivo discreti trarre vantaggio dalle codec AVC444 del Desktop remoto?
Sì, vedere questo post di blog per altre informazioni: [Remote Desktop Protocol (RDP) 10 AVC/H.264 miglioramenti Windows 10 e Windows Server 2016 Technical Preview.](https://blogs.technet.microsoft.com/enterprisemobility/2016/01/11/remote-desktop-protocol-rdp-10-avch-264-improvements-in-windows-10-and-windows-server-2016-technical-preview/)

### <a name="can-i-use-powershell-to-get-the-location-path"></a>È possibile usare PowerShell per ottenere il percorso della posizione?
Sì, esistono diversi modi per eseguire questa operazione. Ecco un esempio:
```
#Enumerate all PNP Devices on the system
$pnpdevs = Get-PnpDevice -presentOnly
#Select only those devices that are Display devices manufactured by NVIDIA
$gpudevs = $pnpdevs |where-object {$_.Class -like "Display" -and $_.Manufacturer -like "NVIDIA"}
#Select the location path of the first device that's available to be dismounted by the host.
$locationPath = ($gpudevs | Get-PnpDeviceProperty DEVPKEY_Device_LocationPaths).data[0]
```

### <a name="can-discrete-device-assignment-be-used-to-pass-a-usb-device-into-a-vm"></a>Discreti dispositivo assegnazione può essere usato per passare un dispositivo USB in una macchina virtuale?
Anche se non è ufficialmente supportati, i clienti hanno utilizzato discreti dispositivo assegnazione per eseguire questa operazione passando l'intero controller USB3 in una macchina virtuale.  Come viene passato nell'intero controller, ogni dispositivo USB collegato in tale controller anche saranno accessibile nella macchina virtuale.  Si noti che solo alcuni controller USB3 potrebbero funzionare, USB2 controller non è possibile usare con l'assegnazione dispositivo discreti.
