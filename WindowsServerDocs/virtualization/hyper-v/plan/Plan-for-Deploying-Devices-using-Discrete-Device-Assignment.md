---
title: Pianificare la distribuzione di dispositivi con l'assegnazione di dispositivi discreti
description: Informazioni sul funzionamento di DDA in Windows Server
ms.prod: windows-server
ms.service: na
ms.technology: hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: chrishuybregts
ms.author: chrihu
ms.date: 02/06/2018
ms.openlocfilehash: 7084f4951ebe1d1203f4c9e45bc5f73cc6487a84
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364189"
---
# <a name="plan-for-deploying-devices-using-discrete-device-assignment"></a>Pianificare la distribuzione di dispositivi con l'assegnazione di dispositivi discreti
>Si applica a: Microsoft Hyper-V Server 2016, Windows Server 2016, Microsoft Hyper-V Server 2019, Windows Server 2019

L'assegnazione di un dispositivo discreto consente di accedere direttamente all'hardware PCIe fisico da una macchina virtuale.  Questa guida illustra il tipo di dispositivi che possono usare l'assegnazione di dispositivi discreti, i requisiti di sistema host, le limitazioni imposte alle macchine virtuali e le implicazioni di sicurezza dell'assegnazione di dispositivi discreti.

Per la versione iniziale dell'assegnazione di dispositivi discreti, Microsoft si è concentrata su due classi di dispositivi che sono formalmente supportate da Microsoft: Adattatori grafici e dispositivi di archiviazione NVMe.  È probabile che altri dispositivi lavorino e che i fornitori di hardware possano offrire i rapporti di supporto per tali dispositivi.  Per questi altri dispositivi, rivolgersi a tali fornitori di hardware per il supporto.

Se si è pronti per provare l'assegnazione di dispositivi discreti, è possibile passare alla [distribuzione di dispositivi grafici usando l'assegnazione di dispositivi discreti](../deploy/Deploying-graphics-devices-using-dda.md) o la distribuzione di dispositivi di archiviazione usando l'assegnazione di dispositivi [discreti](../deploy/Deploying-storage-devices-using-dda.md) per iniziare.

## <a name="supported-virtual-machines-and-guest-operating-systems"></a>Macchine virtuali e sistemi operativi guest supportati
L'assegnazione di dispositivi discreti è supportata per le macchine virtuali di generazione 1 o 2.  Inoltre, i guest supportati includono Windows 10, Windows Server 2019, Windows Server 2016, Windows Server 2012r2 con [KB 3133690](https://support.microsoft.com/kb/3133690) applicati e varie distribuzioni del [sistema operativo Linux.](../supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows.md)

## <a name="system-requirements"></a>Requisiti di sistema
Oltre ai requisiti di [sistema per Windows Server](../../../get-started/System-Requirements--and-Installation.md) e ai [requisiti di sistema per Hyper-V](../System-requirements-for-Hyper-V-on-Windows.md), l'assegnazione di dispositivi discreti richiede hardware della classe server in grado di concedere al controllo del sistema operativo la configurazione di PCIe. Fabric (controllo nativo PCI Express). Inoltre, il complesso della radice PCIe deve supportare il servizio di controllo di accesso (ACS), che consente a Hyper-V di forzare tutto il traffico PCIe attraverso la MMU di I/O.

Queste funzionalità in genere non sono esposte direttamente nel BIOS del server e spesso sono nascoste dietro altre impostazioni.  Ad esempio, le stesse funzionalità sono necessarie per il supporto di SR-IOV e nel BIOS potrebbe essere necessario impostare "Abilita SR-IOV".  Rivolgersi al fornitore del sistema se non si è in grado di identificare l'impostazione corretta nel BIOS.

Per garantire che l'hardware sia idoneo per l'assegnazione di dispositivi discreti, i tecnici hanno creato uno [script del profilo del computer](#machine-profile-script) che è possibile eseguire in un host Hyper-V abilitato per verificare se il server è configurato correttamente e quali dispositivi sono in grado di Assegnazione di dispositivi discreti.

## <a name="device-requirements"></a>Requisiti del dispositivo
Non ogni dispositivo PCIe può essere usato con l'assegnazione di dispositivi discreti.  Ad esempio, i dispositivi meno recenti che sfruttano gli interrupt PCI legacy (INTx) non sono supportati. I post di [Blog](https://blogs.technet.microsoft.com/virtualization/2015/11/20/discrete-device-assignment-machines-and-devices/) di Jake Oscin sono più dettagliati. Tuttavia, per l'utente, l'esecuzione dello [script del profilo del computer](#machine-profile-script) visualizzerà i dispositivi che possono essere usati per l'assegnazione di dispositivi discreti.

I produttori di dispositivi possono rivolgersi al proprio rappresentante Microsoft per altre informazioni.

## <a name="device-driver"></a>Driver di dispositivo
Quando l'assegnazione di un dispositivo discreto passa l'intero dispositivo PCIe alla VM guest, non è necessario installare un driver host prima del dispositivo montato all'interno della macchina virtuale.  L'unico requisito per l'host è che è possibile determinare il [percorso PCIe](#pcie-location-path) del dispositivo.  Se questo consente di identificare il dispositivo, è possibile installare il driver del dispositivo.  Ad esempio, una GPU senza il driver di dispositivo installato nell'host potrebbe essere visualizzata come un dispositivo di rendering di base di Microsoft.  Se il driver di dispositivo è installato, verrà probabilmente visualizzato il produttore e il modello.

Quando il dispositivo viene montato all'interno del Guest, il driver di dispositivo del produttore può ora essere installato come di consueto all'interno della macchina virtuale guest.  

## <a name="virtual-machine-limitations"></a>Limitazioni delle macchine virtuali
A causa della natura del modo in cui viene implementata l'assegnazione di dispositivi discreti, alcune funzionalità di una macchina virtuale vengono limitate mentre un dispositivo viene collegato.  Le funzionalità seguenti non sono disponibili:
- Salvataggio/ripristino della macchina virtuale
- Migrazione in tempo reale di una macchina virtuale
- Uso della memoria dinamica
- Aggiunta della macchina virtuale a un cluster a disponibilità elevata

## <a name="security"></a>Security
L'assegnazione di un dispositivo discreto passa l'intero dispositivo alla macchina virtuale.  Ciò significa che tutte le funzionalità del dispositivo sono accessibili dal sistema operativo guest. Alcune funzionalità, ad esempio l'aggiornamento del firmware, possono avere un impatto negativo sulla stabilità del sistema. Di conseguenza, molti avvisi vengono presentati all'amministratore quando si smonta il dispositivo dall'host. Si consiglia vivamente di usare l'assegnazione di dispositivi discreti solo se i tenant delle macchine virtuali sono attendibili.  

Se l'amministratore desidera usare un dispositivo con un tenant non attendibile, i produttori di dispositivi hanno la possibilità di creare un driver di mitigazione del dispositivo che può essere installato nell'host.  Contattare il produttore del dispositivo per informazioni dettagliate sul fatto che forniscano un driver di mitigazione dei dispositivi.

Se si desidera ignorare i controlli di sicurezza per un dispositivo che non dispone di un driver di mitigazione dei dispositivi, sarà necessario passare il parametro `-Force` al cmdlet `Dismount-VMHostAssignableDevice`.  Si tenga presente che, in questo modo, il profilo di sicurezza del sistema è stato modificato ed è consigliato solo per la realizzazione di prototipi o ambienti attendibili.

## <a name="pcie-location-path"></a>Percorso del percorso PCIe
Il percorso del percorso PCIe è necessario per smontare e montare il dispositivo dall'host.  Un percorso di esempio è simile al seguente: `"PCIROOT(20)#PCI(0300)#PCI(0000)#PCI(0800)#PCI(0000)"`.   Lo [script del profilo del computer](#machine-profile-script) restituirà anche il percorso del dispositivo PCIe.

### <a name="getting-the-location-path-by-using-device-manager"></a>Recupero del percorso utilizzando Device Manager
![Gestione dispositivi](../deploy/media/dda-devicemanager.png)
- Aprire Device Manager e individuare il dispositivo.  
- Fare clic con il pulsante destro del mouse sul dispositivo e scegliere "proprietà".
- Passare alla scheda Dettagli e selezionare "percorsi percorso" nell'elenco a discesa Proprietà.  
- Fare clic con il pulsante destro del mouse sulla voce che inizia con "PCIROOT" e selezionare "copia".  A questo punto è disponibile il percorso del dispositivo.

## <a name="mmio-space"></a>Spazio MMIO
Per alcuni dispositivi, in particolare GPU, è necessario allocare ulteriore spazio MMIO alla macchina virtuale per poter accedere alla memoria del dispositivo. Per impostazione predefinita, ogni macchina virtuale inizia con 128 MB di spazio MMIO basso e 512 MB di spazio MMIO elevato allocato. Tuttavia, un dispositivo potrebbe richiedere più spazio MMIO oppure è possibile che vengano passati più dispositivi in modo che i requisiti combinati superino questi valori.  La modifica dello spazio MMIO è diretta e può essere eseguita in PowerShell usando i comandi seguenti:

```PowerShell
Set-VM -LowMemoryMappedIoSpace 3Gb -VMName $vm
Set-VM -HighMemoryMappedIoSpace 33280Mb -VMName $vm
```

Il modo più semplice per determinare la quantità di spazio MMIO da allocare consiste nell'usare lo [script del profilo del computer](#machine-profile-script). Per scaricare ed eseguire lo script del profilo del computer, eseguire i comandi seguenti in una console di PowerShell:

```PowerShell
curl -o SurveyDDA.ps1 https://raw.githubusercontent.com/MicrosoftDocs/Virtualization-Documentation/live/hyperv-tools/DiscreteDeviceAssignment/SurveyDDA.ps1
.\SurveyDDA.ps1
```

Per i dispositivi che possono essere assegnati, lo script visualizzerà i requisiti di MMIO di un determinato dispositivo come nell'esempio seguente:

```PowerShell
NVIDIA GRID K520
Express Endpoint -- more secure.
    ...
    And it requires at least: 176 MB of MMIO gap space
...
```

Lo spazio MMIO ridotto viene usato solo da dispositivi e sistemi operativi a 32 bit che usano indirizzi a 32 bit. Nella maggior parte dei casi, l'impostazione dello spazio MMIO elevato di una macchina virtuale sarà sufficiente perché le configurazioni a 32 bit non sono molto comuni.

> [!IMPORTANT]
> Quando si assegna lo spazio MMIO a una macchina virtuale, l'utente deve assicurarsi di impostare lo spazio MMIO sulla somma dello spazio MMIO richiesto per tutti i dispositivi assegnati desiderati, oltre a un buffer aggiuntivo se sono presenti altri dispositivi virtuali che richiedono alcuni MB di spazio MMIO. Usare i valori predefiniti di MMIO descritti in precedenza come buffer per MMIO Bassi e alti (rispettivamente 128 MB e 512 MB).

Se un utente dovesse assegnare una singola GPU K520 come nell'esempio precedente, è necessario impostare lo spazio MMIO della macchina virtuale sul valore restituito dallo script del profilo del computer più un buffer, ovvero 176 MB + 512 MB. Se un utente dovesse assegnare tre GPU K520, deve impostare lo spazio MMIO su tre volte 176 MB più un buffer oppure 528 MB + 512 MB.

Per un'analisi più approfondita dello spazio MMIO, vedere la pagina relativa all' [assegnazione di dispositivi discreti-GPU](https://techcommunity.microsoft.com/t5/Virtualization/Discrete-Device-Assignment-GPUs/ba-p/382266) nel Blog di TechCommunity.

## <a name="machine-profile-script"></a>Script del profilo del computer
Per semplificare l'identificazione se il server è configurato correttamente e quali dispositivi sono disponibili per essere passati tramite l'assegnazione di dispositivi discreti, uno dei nostri tecnici riunisce il seguente script di PowerShell: [SurveyDDA. ps1.](https://github.com/Microsoft/Virtualization-Documentation/blob/live/hyperv-tools/DiscreteDeviceAssignment/SurveyDDA.ps1)

Prima di usare lo script, assicurarsi di aver installato il ruolo Hyper-V e di eseguire lo script da una finestra di comando di PowerShell con privilegi di amministratore.

Se il sistema non è configurato correttamente per supportare l'assegnazione di dispositivi discreti, lo strumento visualizzerà un messaggio di errore che indica il problema. Se lo strumento rileva il sistema configurato correttamente, enumera tutti i dispositivi che è in grado di trovare sul bus PCIe.

Per ogni dispositivo individuato, lo strumento visualizzerà se può essere usato con l'assegnazione di dispositivi discreti. Se un dispositivo viene identificato come compatibile con l'assegnazione di un dispositivo discreto, lo script fornirà un motivo.  Quando un dispositivo viene identificato correttamente come compatibile, viene visualizzato il percorso del dispositivo.  Inoltre, se il dispositivo richiede [spazio MMIO](#mmio-space), verrà visualizzato anche.

![SurveyDDA. ps1](./images/hyper-v-surveydda-ps1.png)

## <a name="frequently-asked-questions"></a>Domande frequenti

### <a name="how-does-remote-desktops-remotefx-vgpu-technology-relate-to-discrete-device-assignment"></a>In che modo Desktop remoto la tecnologia vGPU di RemoteFX è correlata all'assegnazione di dispositivi discreti?
Sono tecnologie completamente separate. Non è necessario installare RemoteFX vGPU per il funzionamento dell'assegnazione di dispositivi discreti. Inoltre, non è necessario installare altri ruoli. RemoteFX vGPU richiede l'installazione del ruolo RDVH affinché il driver RemoteFX vGPU sia presente nella macchina virtuale. Per l'assegnazione di dispositivi discreti, poiché il driver del fornitore dell'hardware verrà installato nella macchina virtuale, non è necessario che siano presenti ruoli aggiuntivi.  

### <a name="ive-passed-a-gpu-into-a-vm-but-remote-desktop-or-an-application-isnt-recognizing-the-gpu"></a>È stata passata una GPU a una macchina virtuale, ma Desktop remoto o un'applicazione non riconosce la GPU
Questa situazione può verificarsi per diversi motivi, ma sono elencati di seguito alcuni problemi comuni.
- Verificare che sia installato il driver del fornitore della GPU più recente e che non venga segnalato un errore controllando lo stato del dispositivo nella Device Manager.
- Verificare che il dispositivo disponga di [spazio di MMIO](#mmio-space) sufficiente per l'allocazione nella macchina virtuale.
- Assicurarsi di usare una GPU supportata dal fornitore in questa configurazione. Alcuni fornitori, ad esempio, impediscono il funzionamento delle schede consumer quando vengono passate a una macchina virtuale.
- Verificare che l'applicazione in esecuzione supporti l'esecuzione all'interno di una macchina virtuale e che sia la GPU che i driver associati siano supportati dall'applicazione. Alcune applicazioni includono elenchi di elementi consentiti di GPU e ambienti.
- Se si usa il ruolo host sessione Desktop remoto o servizi MultiPoint di Windows sul Guest, sarà necessario assicurarsi che una voce di Criteri di gruppo specifica sia impostata per consentire l'uso della GPU predefinita. Utilizzando un Criteri di gruppo oggetto applicato al Guest (o al Editor Criteri di gruppo locali nel guest), passare al Criteri di gruppo elemento seguente:
   - Configurazione computer
   - Modelli di amministratore
   - Componenti di Windows
   - Servizi Desktop remoto
   - Host sessione Desktop remoto
   - Ambiente sessione remota
   - Usare la scheda grafica predefinita hardware per tutte le sessioni di Servizi Desktop remoto

    Impostare questo valore su abilitato, quindi riavviare la macchina virtuale dopo aver applicato i criteri.

### <a name="can-discrete-device-assignment-take-advantage-of-remote-desktops-avc444-codec"></a>L'assegnazione di dispositivi discreti può trarre vantaggio dal codec AVC444 di Desktop remoto?
Sì, visita questo post di Blog per altre informazioni: [Miglioramenti di Remote Desktop Protocol (RDP) 10 AVC/H. 264 in Windows 10 e Windows Server 2016 Technical Preview.](https://blogs.technet.microsoft.com/enterprisemobility/2016/01/11/remote-desktop-protocol-rdp-10-avch-264-improvements-in-windows-10-and-windows-server-2016-technical-preview/)

### <a name="can-i-use-powershell-to-get-the-location-path"></a>È possibile usare PowerShell per ottenere il percorso?
Sì, esistono diversi modi per eseguire questa operazione. Ecco un esempio:
```
#Enumerate all PNP Devices on the system
$pnpdevs = Get-PnpDevice -presentOnly
#Select only those devices that are Display devices manufactured by NVIDIA
$gpudevs = $pnpdevs |where-object {$_.Class -like "Display" -and $_.Manufacturer -like "NVIDIA"}
#Select the location path of the first device that's available to be dismounted by the host.
$locationPath = ($gpudevs | Get-PnpDeviceProperty DEVPKEY_Device_LocationPaths).data[0]
```

### <a name="can-discrete-device-assignment-be-used-to-pass-a-usb-device-into-a-vm"></a>È possibile usare l'assegnazione di dispositivi discreti per passare un dispositivo USB a una macchina virtuale?
Sebbene non siano ufficialmente supportati, i clienti hanno usato l'assegnazione di dispositivi discreti per eseguire questa operazione passando l'intero controller USB3 in una macchina virtuale.  Quando viene passato l'intero controller, anche ogni dispositivo USB collegato a tale controller sarà accessibile nella macchina virtuale.  Si noti che possono funzionare solo alcuni controller USB3 e che i controller USB2 non possono essere usati con l'assegnazione di dispositivi discreti.
