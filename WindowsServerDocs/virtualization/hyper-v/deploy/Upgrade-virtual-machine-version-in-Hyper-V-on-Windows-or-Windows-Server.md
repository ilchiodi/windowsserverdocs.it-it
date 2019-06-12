---
title: Versione aggiornamento macchina virtuale in Hyper-V in Windows 10 o Windows Server
description: Fornisce le istruzioni e le considerazioni per l'aggiornamento della versione di una macchina virtuale
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 897f2454-5aee-445c-a63e-f386f514a0f6
author: jasongerend
ms.author: jgerend
ms.date: 05/22/2019
ms.openlocfilehash: 160adc0e838cb732ba792cbdd7fd9fa200c68794
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66810505"
---
# <a name="upgrade-virtual-machine-version-in-hyper-v-on-windows-10-or-windows-server"></a>Versione aggiornamento macchina virtuale in Hyper-V in Windows 10 o Windows Server

>Si applica a: Windows 10, Windows Server 2019, Windows Server 2016, Windows Server (canale semestrale)

Rendere disponibili le funzionalità di Hyper-V più recenti nelle macchine virtuali aggiornando la versione di configurazione. Non eseguire questa operazione fino a quando:

- Aggiornare gli host Hyper-V per la versione più recente di Windows o Windows Server.
- L'aggiornamento a livello funzionale del cluster.
- Si è certi che non sarà necessario spostare nuovamente la macchina virtuale in un host Hyper-V che esegue una versione precedente di Windows o Windows Server.

Per altre informazioni, vedere [aggiornamento in sequenza del Cluster del sistema operativo](../../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md) e [eseguire un aggiornamento in sequenza di un cluster host Hyper-V in VMM](https://docs.microsoft.com/system-center/vmm/hyper-v-rolling-upgrade).

## <a name="step-1-check-the-virtual-machine-configuration-versions"></a>Passaggio 1: Controllare le versioni di configurazione macchina virtuale

1. Sul desktop di Windows fare clic sul pulsante Start e digitare qualsiasi parte del nome **Windows PowerShell**.
2. Fare doppio clic su Windows PowerShell e selezionare **Esegui come amministratore**.
3. Usare la [Get-VM](https://docs.microsoft.com/powershell/module/hyper-v/get-vm)cmdlet. Eseguire il comando seguente per ottenere le versioni delle macchine virtuali.

```PowerShell
Get-VM * | Format-Table Name, Version
```

È inoltre possibile visualizzare la versione di configurazione Gestione di Hyper-V selezionando la macchina virtuale ed esaminando il **riepilogo** scheda.

## <a name="step-2-upgrade-the-virtual-machine-configuration-version"></a>Passaggio 2: Aggiornare la versione di configurazione macchina virtuale

1. Arrestare la macchina virtuale in Hyper-V Manager.
2. Selezionare l'azione > aggiornare la versione di configurazione. Se questa opzione non è disponibile per la macchina virtuale, quindi è già la versione di configurazione più elevato supportato dall'host Hyper-V.

Per aggiornare la versione di configurazione macchina virtuale con Windows PowerShell, utilizzare il [VMVersion aggiornamento](https://docs.microsoft.com/powershell/module/hyper-v/update-vmversion) cmdlet. Eseguire il comando seguente dove vmname è il nome della macchina virtuale.

```PowerShell
Update-VMVersion <vmname>
```

## <a name="supported-virtual-machine-configuration-versions"></a>Versioni di configurazione supportate della macchina virtuale

Eseguire il cmdlet PowerShell [Get-VMHostSupportedVersion](https://docs.microsoft.com/powershell/module/hyper-v/get-vmhostsupportedversion) per vedere quali versioni di configurazione macchina virtuale nell'Host Hyper-V supporta. Quando si crea una macchina virtuale, viene creato con la versione di configurazione predefinito. Per visualizzare il valore predefinito è, eseguire il comando seguente.

```PowerShell
Get-VMHostSupportedVersion -Default
```

Se è necessario creare una macchina virtuale che è possibile spostare in un Host Hyper-V in esecuzione una versione precedente di Windows, utilizzare il [New-VM](https://docs.microsoft.com/powershell/module/hyper-v/new-vm) cmdlet con il parametro - version. Ad esempio, per creare una macchina virtuale che è possibile spostare in un host Hyper-V che esegue Windows Server 2012 R2, eseguire il comando seguente. Questo comando crea una macchina virtuale denominata "WindowsCV5" con una versione 5.0 di configurazione.

```PowerShell
New-VM -Name "WindowsCV5" -Version 5.0
```

>[!NOTE]
>È possibile importare le macchine virtuali che sono state create per un host Hyper-V che esegue una versione precedente di Windows o ripristinarli dal backup. Se la versione di configurazione della macchina virtuale non è elencata come supportato per il sistema operativo host Hyper-V nella tabella seguente, è necessario aggiornare la versione di configurazione della macchina virtuale prima di poter avviare la macchina virtuale.

### <a name="supported-vm-configuration-versions-for-long-term-servicing-hosts"></a>Versioni di configurazione della macchina virtuale supportate per gli host di manutenzione a lungo termine

Nella tabella seguente elenca le versioni di configurazione della macchina virtuale sono supportate negli host che eseguono una versione di manutenzione a lungo termine di Windows.

| Versione di Windows di host Hyper-V | 9.1 | 9.0 | 8.3 | 8.2 | 8.1 | 8.0 | 7.1 | 7.0 | 6.2 | 5.0 |
| --- |---|---|---|---|---|---|---|---|---|---|
|Windows Server 2019|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 Enterprise LTSC 2019|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server 2016|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 Enterprise 2016 LTSB|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 Enterprise 2015 LTSB|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|
|Windows Server 2012 R2|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|
|Windows 8.1|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|

### <a name="supported-vm-configuration-versions-for-semi-annual-channel-hosts"></a>Versioni di configurazione della macchina virtuale supportate per gli host di canale semestrale

Nella tabella seguente elenca le versioni di configurazione della macchina virtuale per gli host che eseguono una versione di canale semestrale attualmente supportata di Windows. Per ottenere altre informazioni sulle versioni canale semestrale di Windows, visitare le pagine seguenti per [Windows Server](../../../get-started-19/servicing-channels-19.md) e [Windows 10](https://docs.microsoft.com/windows/deployment/update/waas-overview#servicing-channels)

| Versione di Windows di host Hyper-V | 9.1 | 9.0 | 8.3 | 8.2 | 8.1 | 8.0 | 7.1 | 7.0 | 6.2 | 5.0 |
| --- |---|---|---|---|---|---|---|---|---|---|
| Aggiornamento di Windows 10 potrebbero 2019 (versione 1903) |&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;| &#10004;|
| Windows Server, versione 1903 |&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;| &#10004;|
|Windows Server, versione 1809|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 ottobre 2018 Update (versione 1809)|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server, versione 1803|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 April 2018 Update (versione 1803)|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 Fall Creators Update (versione 1709)|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 Creators Update (version 1703)|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 Anniversary Update (versione 1607)|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|

## <a name="why-should-i-upgrade-the-virtual-machine-configuration-version"></a>Perché è necessario aggiornare la versione di configurazione macchina virtuale?

Quando si sposta o si importa una macchina virtuale in un computer che esegue Hyper-V in Windows Server 2019, Windows Server 2016 o Windows 10, la macchina virtuale "configurazione non viene aggiornato automaticamente. Ciò significa che è possibile riportare la macchina virtuale in un host Hyper-V che esegue una versione precedente di Windows o Windows Server. Tuttavia, ciò significa anche che non è possibile utilizzare alcune delle nuove funzionalità di macchina virtuale finché non si aggiorna manualmente la versione di configurazione. È possibile effettuare il downgrade la versione di configurazione macchina virtuale dopo averla sostituita.

La versione di configurazione macchina virtuale rappresenta la compatibilità di configurazione della macchina virtuale, salvata lo stato e i file di snapshot con la versione di Hyper-V. Quando si aggiorna la versione di configurazione, si modifica la struttura dei file che viene utilizzata per archiviare la configurazione di macchine virtuali e i file del checkpoint. È inoltre possibile aggiornare la versione di configurazione per la versione più recente supportata dall'host Hyper-V. Le macchine virtuali aggiornate usano un nuovo formato di file di configurazione progettato per aumentare l'efficienza di lettura e scrittura dei dati di configurazione della macchina virtuale. L'aggiornamento riduce inoltre il rischio di danneggiamento dei dati in caso di errore di memoria.

Nella tabella seguente elenca le descrizioni, estensioni di file e percorsi predefiniti per ogni tipo di file che viene utilizzato per le macchine virtuali nuove o aggiornate.

 |Tipi di file di macchina virtuale | Descrizione|
 |---|---|
|Configurazione |Informazioni sulla configurazione di macchina virtuale archiviata in formato binario. <br /> Estensione del nome file: .vmcx <br /> Percorso predefinito: C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual Machines|
 |Stato di runtime|Informazioni sullo stato runtime macchina virtuale archiviata in formato binario. <br />Estensione del nome file:. vmrs e .vmgs <br />Percorso predefinito: C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual Machines|
|Disco rigido virtuale|Archivia i dischi rigidi virtuali per la macchina virtuale. <br /> Estensione del nome file: file con estensione vhd o vhdx. <br />Percorso predefinito: C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual Hard Disks|
 |Automatica del disco rigido virtuale |Differenze file su disco utilizzati per i checkpoint della macchina virtuale. <br /> Estensione del nome file: avhdx <br /> Percorso predefinito: C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual Hard Disks|
 |Checkpoint|I checkpoint sono archiviati in più file di checkpoint. Ogni checkpoint crea un file di configurazione e file di stato di runtime. <br /> Estensioni di file: .vmrs e .vmcx <br />Percorso predefinito: C:\ProgramData\Microsoft\Windows\Snapshots|

## <a name="what-happens-if-i-dont-upgrade-the-virtual-machine-configuration-version"></a>Cosa accade se non aggiornare la versione di configurazione macchina virtuale?

Se si hanno macchine virtuali che è stato creato con una versione precedente di Hyper-V, alcune funzionalità che sono disponibili nell'host più recenti che sistemi operativi potrebbero non funzionare con le macchine virtuali finché non si aggiorna la versione di configurazione.

Come un'indicazione generale, è consigliabile aggiornare la versione di configurazione dopo aver aggiornato correttamente gli host di virtualizzazione a una versione più recente di Windows e avere la ragionevole certezza che non è necessario eseguire il rollback. Quando si usa la [l'aggiornamento in sequenza del sistema operativo del cluster](https://docs.microsoft.com/windows-server/failover-clustering/Cluster-Operating-System-Rolling-Upgrade) funzionalità, in genere sarebbe dopo aver aggiornato il livello funzionale del cluster. In questo modo, trarranno vantaggio da nuove funzionalità e modifiche interne e anche le ottimizzazioni.

>[!NOTE]
>Dopo aver aggiornata la versione di configurazione della macchina virtuale, la macchina virtuale non è possibile avviare negli host che non supportano la versione di aggiornamento della configurazione.

La tabella seguente illustra la versione di configurazione minima della macchina virtuale deve usare alcune funzionalità di Hyper-V.

|Funzionalità|Versione minima di configurazione macchina Virtuale|
|---|---|
|Aggiunta o rimozione di memoria a caldo|6.2|
|Avvio protetto per le macchine virtuali Linux|6.2|
|Checkpoint di produzione|6.2|
|PowerShell Direct |6.2|
|Raggruppamento di macchine virtuali|6.2|
|Virtual Trusted Platform Module (vTPM)|7.0|
|Macchina virtuale più code (VMMQ)|7.1|
|Supporto per XSAVE|8.0|
|Unità di archiviazione delle chiavi|8.0|
|Supporto della sicurezza basata su virtualizzazione guest (VBS)|8.0|
|Virtualizzazione annidata|8.0|
|Numero di processori virtuali|8.0|
|Macchine virtuali di grandi quantità di memoria|8.0|
|Aumentare il numero massimo predefinito per i dispositivi virtuali a 64 per ogni dispositivo (ad esempio, rete e assegnati i dispositivi)|8.3|
|Consenti le funzionalità aggiuntive del processore per Perfmon|9.0|
|Esporre automaticamente [simultanee multithreading](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types#background) configurazione per le macchine virtuali in esecuzione in host tramite la [Core dell'utilità di pianificazione](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types#windows-server-2019-hyper-v-defaults-to-using-the-core-scheduler)|9.0|
|Supporto di modalità di ibernazione|9.0|

Per altre informazioni su queste funzionalità, vedere [quali sono le novità in Hyper-V in Windows Server](../What-s-new-in-Hyper-V-on-Windows.md).

