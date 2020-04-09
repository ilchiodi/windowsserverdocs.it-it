---
title: Problema di utilizzo elevato della CPU nel server SMB
description: Viene illustrato come risolvere il problema di utilizzo elevato della CPU nel server SMB.
author: Deland-Han
manager: dcscontentpm
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: 9634ca5b0eb07beff8d8213f88e771d76734e6fc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80815435"
---
# <a name="high-cpu-usage-issue-on-the-smb-server"></a>Problema di utilizzo elevato della CPU nel server SMB

Questo articolo illustra come risolvere il problema di utilizzo elevato della CPU nel server SMB.

## <a name="high-cpu-usage-because-of-storage-performance-issues"></a>Utilizzo CPU elevato a causa di problemi di prestazioni di archiviazione

Problemi di prestazioni di archiviazione possono causare un utilizzo elevato della CPU nei server SMB. Prima di risolvere i problemi, verificare che nel server SMB sia installato l'aggiornamento cumulativo più recente per eliminare eventuali problemi noti in srv2. sys.

Nella maggior parte dei casi, si noterà il problema di utilizzo elevato della CPU nel processo di sistema. Prima di procedere, utilizzare Process Explorer per assicurarsi che srv2. sys o NTFS. sys utilizzi risorse della CPU eccessive.

### <a name="storage-area-network-san-scenario"></a>Scenario SAN (Storage Area Network)

A livello di aggregazione, le prestazioni globali della SAN possono sembrare ottimali. Tuttavia, quando si lavora con i problemi SMB, il tempo di risposta della richiesta individuale è quello più importante.

In genere, questo problema può essere causato da una forma di Accodamento dei comandi nella rete SAN. È possibile utilizzare **PerfMon** per acquisire una traccia **Microsoft-Windows-Storport** e analizzarla per determinare accuratamente la velocità di risposta dell'archiviazione.

### <a name="disk-io-latency"></a>Latenza IO disco

La latenza IO del disco è una misura del ritardo tra il momento in cui viene creata e completata una richiesta di i/o del disco.

La latenza di i/o misurata in PerfMon include tutto il tempo dedicato ai livelli hardware più il tempo trascorso nella coda dei driver della porta Microsoft (Storport. sys per SCSI). Se i processi in esecuzione generano una coda StorPort di grandi dimensioni, aumenta la latenza misurata. Ciò è dovuto al fatto che IO deve attendere prima che venga inviato ai livelli hardware.

In PerfMon i contatori seguenti mostrano la latenza del disco fisico:

- "Oggetto prestazioni disco fisico"-\> "Media letture disco/sec": Mostra la latenza di lettura media.

- "Oggetto prestazioni disco fisico"-\> "media scritture disco/sec": Mostra la latenza di scrittura media.

- "Oggetto prestazione disco fisico"-\> "media sec/trasferimento disco": Mostra le medie combinate sia per le letture che per le Scritture.

L'istanza "\_Total" è una media delle latenze per tutti i dischi fisici del computer. Ognuna delle altre istanze rappresenta un singolo disco fisico.

> [!NOTE]
> Non confondere questi contatori con media trasferimenti disco/sec. Si tratta di contatori completamente diversi.

### <a name="windows-storage-stack-follows"></a>Stack di archiviazione di Windows segue

In questa sezione viene fornita una breve spiegazione sullo stack di archiviazione di Windows.

Quando un'applicazione crea una richiesta di i/o, Invia la richiesta al sottosistema IO di Windows nella parte superiore dello stack. L'i/o scorre fino al sottosistema "disk" dell'hardware. Quindi, la risposta viene spostata in tutto il backup. Durante questo processo, ogni livello esegue la relativa funzione e quindi passa l'IO al livello successivo.

![Flusso dello stack](media/high-cpu-usage-issue-on-smb-server-1.png)

PerfMon non crea alcun dato sulle prestazioni al secondo. Utilizza invece i dati forniti da altri sottosistemi all'interno di Windows.

Per l'oggetto prestazioni disco fisico, i dati vengono acquisiti a livello di "Gestione partizioni" nello stack di archiviazione.

Quando si misurano i contatori indicati nella sezione precedente, viene misurato tutto il tempo dedicato dalla richiesta sotto il livello "Partition Manager". Quando la richiesta di i/o viene inviata da Gestione partizioni nello stack, viene ora timbrata. Quando viene restituito, il timestamp viene nuovamente timbrato e viene calcolato il tempo di differenza. La differenza di tempo è la latenza.

In questo caso, viene conteggiato il tempo dedicato ai componenti seguenti:

- Driver di classe: gestisce il tipo di dispositivo, ad esempio dischi, nastri e così via.

- Driver porta: gestisce il protocollo di trasporto, ad esempio SCSI, FC, SATA e così via.

- Driver miniport del dispositivo: driver di dispositivo per l'adattatore di archiviazione. Viene fornito dal produttore dei dispositivi, ad esempio controller RAID e HBA FC.

- Sottosistema disco: include tutto ciò che si trova sotto il driver miniport del dispositivo. Questa operazione può essere semplice quanto un cavo connesso a un singolo disco rigido fisico o complesso come rete di archiviazione. Se il problema è stato determinato a causa di questo componente, è possibile contattare il fornitore dell'hardware per ulteriori informazioni sulla risoluzione dei problemi.

### <a name="disk-queuing"></a>Accodamento del disco

È presente una quantità limitata di i/o che un sottosistema del disco può accettare in un determinato momento. L'i/o in eccesso viene accodato fino a quando il disco non può accettare di nuovo IO. Il tempo impiegato per le operazioni di i/o nelle code al di sotto del livello "Partition Manager" viene considerato nelle misure di latenza del disco fisico Perfmon. Quando le dimensioni delle code crescono e i/o devono attendere più a lungo, aumenta anche la latenza misurata.

Sono presenti più code al di sotto del livello "Partition Manager", come indicato di seguito:

- Coda driver porta Microsoft-coda SCSIport o Storport

- Coda driver di dispositivo fornita dal produttore-driver di dispositivo OEM

- Code hardware, ad esempio coda controller disco, coda commutatori SAN, coda controller array e coda disco rigido

Viene inoltre considerato il tempo impiegato dal disco rigido per la manutenzione attiva dell'i/o e il tempo di viaggio che la richiesta deve restituire al livello "Partition Manager" per essere contrassegnato come completato.

Infine, è necessario prestare particolare attenzione alla coda di driver della porta (per SCSI Storport. sys). Il driver della porta è l'ultimo componente Microsoft che consente di toccare un IO prima di passarlo al driver miniport del dispositivo fornito dal produttore.

Se il driver miniport del dispositivo non accetta più i/o perché la coda o le code hardware al di sotto di essa sono saturate, si inizierà ad accumulare i/o nella coda del driver della porta. Le dimensioni della coda di driver della porta Microsoft sono limitate solo dalla memoria di sistema disponibile (RAM) e possono aumentare notevolmente. Causando una latenza misurata elevata.

## <a name="high-cpu-caused-by-enumerating-folders"></a>CPU elevata causata dall'enumerazione delle cartelle 

Per risolvere questo problema, disabilitare la funzionalità di enumerazione basata sull'accesso (ABE).

Per determinare per quali condivisioni SMB è abilitato ABE, eseguire il comando PowerShell seguente:

```PowerShell
Get-SmbShare | Select Name, FolderEnumerationMode
```

Unrestricted = ABE disabilitato. <br />
AccessBase = ABE abilitato.


È possibile abilitare ABE in **Server Manager**. Navigare in **Servizi file e archiviazione** > **condivisioni**, fare clic con il pulsante destro del mouse sulla condivisione, scegliere **proprietà**, passare a **Impostazioni** e quindi selezionare **Abilita enumerazione basata sull'accesso**.

![Opzioni dell'interfaccia utente](media/high-cpu-usage-issue-on-smb-server-2.png)

Inoltre, è possibile ridurre **ABELevel** a un livello inferiore (1 o 2) per migliorare le prestazioni.

È possibile controllare le prestazioni del disco quando l'enumerazione è lenta aprendo la cartella localmente tramite una console o una sessione RDP.
