---
title: Aggiornare la versione della macchina virtuale in Hyper-V in Windows 10 o Windows Server
description: Vengono fornite istruzioni e considerazioni per l'aggiornamento della versione di una macchina virtuale
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.topic: article
ms.assetid: 897f2454-5aee-445c-a63e-f386f514a0f6
author: jasongerend
ms.author: jgerend
ms.date: 05/22/2019
ms.openlocfilehash: e08d13e4d9b493b80cad59561c8088c7d3a12b57
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860874"
---
# <a name="upgrade-virtual-machine-version-in-hyper-v-on-windows-10-or-windows-server"></a>Aggiornare la versione della macchina virtuale in Hyper-V in Windows 10 o Windows Server

>Si applica a: Windows 10, Windows Server 2019, Windows Server 2016, Windows Server (canale semestrale)

Rendere disponibili le funzionalità di Hyper-V più recenti nelle macchine virtuali aggiornando la versione di configurazione. Non eseguire questa operazione fino a quando:

- Aggiornare gli host Hyper-V per la versione più recente di Windows o Windows Server.
- L'aggiornamento a livello funzionale del cluster.
- Si è certi che non sarà necessario spostare nuovamente la macchina virtuale in un host Hyper-V che esegue una versione precedente di Windows o Windows Server.

Per ulteriori informazioni, vedere [aggiornamento in sequenza del sistema operativo del cluster](../../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md) ed [eseguire un aggiornamento in sequenza di un cluster host Hyper-V in VMM](https://docs.microsoft.com/system-center/vmm/hyper-v-rolling-upgrade).

## <a name="step-1-check-the-virtual-machine-configuration-versions"></a>Passaggio 1: Verificare le versioni di configurazione macchina virtuale

1. Sul desktop di Windows fare clic sul pulsante Start e digitare qualsiasi parte del nome **Windows PowerShell**.
2. Fare doppio clic su Windows PowerShell e selezionare **Esegui come amministratore**.
3. Usare il cmdlet [Get-VM](https://docs.microsoft.com/powershell/module/hyper-v/get-vm). Eseguire il comando seguente per ottenere le versioni delle macchine virtuali.

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
>È possibile importare le macchine virtuali create per un host Hyper-V che esegue una versione precedente di Windows o ripristinarle da un backup. Se la versione di configurazione della macchina virtuale non è elencata come supportata per il sistema operativo host Hyper-V nella tabella seguente, è necessario aggiornare la versione di configurazione della macchina virtuale prima di poter avviare la macchina virtuale.

### <a name="supported-vm-configuration-versions-for-long-term-servicing-hosts"></a>Versioni di configurazione della macchina virtuale supportate per gli host di manutenzione a lungo termine

La tabella seguente elenca le versioni di configurazione della macchina virtuale supportate negli host che eseguono una versione di manutenzione a lungo termine di Windows.

| Versione di Windows di host Hyper-V | 9,1 | 9,0 | 8.3 | 8.2 | 8.1 | 8.0 | 7.1 | 7.0 | 6.2 | 5.0 |
| --- |---|---|---|---|---|---|---|---|---|---|
|Windows Server 2019|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 Enterprise LTSC 2019|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server 2016|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 Enterprise 2016 LTSB|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 Enterprise 2015 LTSB|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|
|Windows Server 2012 R2|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|
|Windows 8.1|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|

### <a name="supported-vm-configuration-versions-for-semi-annual-channel-hosts"></a>Versioni di configurazione della macchina virtuale supportate per gli host del canale semestrale

La tabella seguente elenca le versioni di configurazione delle macchine virtuali per gli host che eseguono una versione del canale semestrale attualmente supportata di Windows. Per ottenere altre informazioni sulle versioni di canale semestrale di Windows, visitare le pagine seguenti per [Windows Server](../../../get-started-19/servicing-channels-19.md) e [Windows 10](https://docs.microsoft.com/windows/deployment/update/waas-overview#servicing-channels)

| Versione di Windows di host Hyper-V | 9,1 | 9,0 | 8.3 | 8.2 | 8.1 | 8.0 | 7.1 | 7.0 | 6.2 | 5.0 |
| --- |---|---|---|---|---|---|---|---|---|---|
| Windows 10 maggio 2019 aggiornamento (versione 1903) |&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;| &#10004;|
| Windows Server, versione 1903 |&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;| &#10004;|
|Windows Server, versione 1809|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Aggiornamento di Windows 10 ottobre 2018 (versione 1809)|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server versione 1803|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Aggiornamento di Windows 10 aprile 2018 (versione 1803)|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 Fall Creators Update (versione 1709)|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 Creators Update (versione 1703)|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Aggiornamento dell'anniversario di Windows 10 (versione 1607)|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|

## <a name="why-should-i-upgrade-the-virtual-machine-configuration-version"></a>Perché è necessario aggiornare la versione di configurazione macchina virtuale?

Quando si sposta o si importa una macchina virtuale in un computer che esegue Hyper-V in Windows Server 2019, Windows Server 2016 o Windows 10, la configurazione della macchina virtuale non viene aggiornata automaticamente. Ciò significa che è possibile riportare la macchina virtuale in un host Hyper-V che esegue una versione precedente di Windows o Windows Server. Tuttavia, ciò significa anche che non è possibile utilizzare alcune delle nuove funzionalità di macchina virtuale finché non si aggiorna manualmente la versione di configurazione. È possibile effettuare il downgrade la versione di configurazione macchina virtuale dopo averla sostituita.

La versione di configurazione macchina virtuale rappresenta la compatibilità di configurazione della macchina virtuale, salvata lo stato e i file di snapshot con la versione di Hyper-V. Quando si aggiorna la versione di configurazione, si modifica la struttura dei file che viene utilizzata per archiviare la configurazione di macchine virtuali e i file del checkpoint. È inoltre possibile aggiornare la versione di configurazione per la versione più recente supportata dall'host Hyper-V. Le macchine virtuali aggiornate usano un nuovo formato di file di configurazione progettato per aumentare l'efficienza di lettura e scrittura dei dati di configurazione della macchina virtuale. L'aggiornamento riduce inoltre il rischio di danneggiamento dei dati in caso di errore di memoria.

Nella tabella seguente elenca le descrizioni, estensioni di file e percorsi predefiniti per ogni tipo di file che viene utilizzato per le macchine virtuali nuove o aggiornate.

 |Tipi di file di macchina virtuale | Descrizione|
 |---|---|
|Configurazione |Informazioni sulla configurazione di macchina virtuale archiviata in formato binario. <br /> Estensione del nome file: .vmcx <br /> Percorso predefinito: C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual macchine|
 |Stato di runtime|Informazioni sullo stato runtime macchina virtuale archiviata in formato binario. <br />Estensione del nome file:. vmrs e. VMgs <br />Percorso predefinito: C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual macchine|
|Disco rigido virtuale|Archivia i dischi rigidi virtuali per la macchina virtuale. <br /> Estensione del nome file: file con estensione vhd o vhdx. <br />Percorso predefinito: dischi rigidi C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual|
 |Automatica del disco rigido virtuale |Differenze file su disco utilizzati per i checkpoint della macchina virtuale. <br /> Estensione del nome file: avhdx <br /> Percorso predefinito: dischi rigidi C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual|
 |Punto di controllo|I checkpoint sono archiviati in più file di checkpoint. Ogni checkpoint crea un file di configurazione e file di stato di runtime. <br /> Estensioni di file: .vmrs e .vmcx <br />Percorso predefinito: C:\ProgramData\Microsoft\Windows\Snapshots|

## <a name="what-happens-if-i-dont-upgrade-the-virtual-machine-configuration-version"></a>Cosa accade se non aggiornare la versione di configurazione macchina virtuale?

Se si dispone di macchine virtuali create con una versione precedente di Hyper-V, alcune funzionalità disponibili nel sistema operativo host più recente potrebbero non funzionare con tali macchine virtuali finché non si aggiorna la versione di configurazione.

Come indicazione generale, è consigliabile aggiornare la versione di configurazione dopo aver aggiornato correttamente gli host di virtualizzazione a una versione più recente di Windows e avere la certezza di non dover eseguire il rollback. Quando si usa la funzionalità di [aggiornamento in sequenza del sistema operativo del cluster](https://docs.microsoft.com/windows-server/failover-clustering/Cluster-Operating-System-Rolling-Upgrade) , questa situazione si verifica in genere dopo l'aggiornamento del livello di funzionalità del cluster. In questo modo, si trarranno vantaggio anche dalle nuove funzionalità e dalle ottimizzazioni interne.

>[!NOTE]
>Una volta aggiornata la versione di configurazione della macchina virtuale, la macchina virtuale non potrà essere avviata in host che non supportano la versione di configurazione aggiornata.

La tabella seguente illustra la versione minima di configurazione della macchina virtuale necessaria per usare alcune funzionalità di Hyper-V.

|Caratteristica|Versione minima di configurazione macchina Virtuale|
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
|Supporto per la sicurezza basata sulla virtualizzazione Guest (VBS)|8.0|
|Virtualizzazione annidata|8.0|
|Numero di processori virtuali|8.0|
|Macchine virtuali di grandi quantità di memoria|8.0|
|Aumentare il numero massimo predefinito per i dispositivi virtuali a 64 per dispositivo (ad esempio, rete e dispositivi assegnati)|8.3|
|Consenti funzionalità aggiuntive del processore per PerfMon|9,0|
|Esporre automaticamente la configurazione [multithreading simultanea](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types#background) per le macchine virtuali in esecuzione negli host che usano l' [utilità di pianificazione principale](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types#windows-server-2019-hyper-v-defaults-to-using-the-core-scheduler)|9,0|
|Supporto per l'ibernazione|9,0|

Per ulteriori informazioni su queste funzionalità, vedere la pagina relativa alle [novità di Hyper-V in Windows Server](../What-s-new-in-Hyper-V-on-Windows.md).

