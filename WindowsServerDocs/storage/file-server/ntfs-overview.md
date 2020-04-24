---
title: Panoramica di NTFS
description: Spiegazione della funzionalità NTFS.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 06/17/2019
ms.localizationpriority: medium
ms.openlocfilehash: b98877d0a94ff8033b65bf74d0118e2a5f1ea092
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "71402081"
---
# <a name="ntfs-overview"></a>Panoramica di NTFS

>Si applica a: Windows 10, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

NTFS, il file system primario delle versioni recenti di Windows e Windows Server, offre un set completo di funzionalità, tra cui descrittori di sicurezza, crittografia, quote disco e metadati complessi. Può essere usato con volumi condivisi cluster per fornire volumi disponibili in modo continuo a cui è possibile accedere contemporaneamente da più nodi di un cluster di failover.

Per altre informazioni sulle funzionalità nuove e modificate di NTFS in Windows Server 2012 R2, vedi [Novità di NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11)). Per altre informazioni sulle funzionalità, vedi la sezione [informazioni aggiuntive](#additional-information) in questo argomento. Per altre informazioni sulla funzionalità ReFS (Resilient File System) più recente, vedi [Panoramica di Resilient File System (ReFS)](../refs/refs-overview.md).

## <a name="practical-applications"></a>Applicazioni pratiche

### <a name="increased-reliability"></a>Una maggiore affidabilità

NTFS usa le informazioni dei file di log e dei checkpoint per ripristinare la coerenza del file system quando il computer viene riavviato dopo un errore di sistema. Dopo un errore di settore non valido, NTFS esegue di nuovo in modo dinamico il mapping del cluster che contiene il settore danneggiato, alloca un nuovo cluster per i dati, contrassegna il cluster originale come non valido e non lo usa più. Ad esempio, dopo un arresto anomalo del server, NTFS è in grado di ripristinare i dati riproducendo i file di log.

NTFS monitora e corregge continuamente i problemi di danneggiamento temporaneo in background senza portare offline il volume. Questa funzionalità è nota come [NTFS con riparazione automatica](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10)) ed è stata introdotta in Windows Server 2008. Per problemi di danneggiamento più estesi, l'utilità Chkdsk, in Windows Server 2012 e versioni successive, analizza l'unità mentre il volume è online, limitando l'uso offline al tempo necessario per ripristinare la coerenza dei dati nel volume. Quando NTFS viene usato con volumi condivisi cluster, non è necessario alcun tempo di inattività. Per altre informazioni, vedere [Integrità NTFS e Chkdsk](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11)).

### <a name="increased-security"></a>Maggiore sicurezza

- **Sicurezza basata sull'elenco di controllo di accesso (ACL) per file e cartelle**: NTFS consente di impostare le autorizzazioni per un file o una cartella, di specificare i gruppi e gli utenti per i quali vuoi limitare o consentire l'accesso e di selezionare il tipo di accesso.

- **Supporto di Crittografia unità BitLocker**: Crittografia unità BitLocker fornisce sicurezza aggiuntiva per informazioni critiche del sistema e altri dati archiviati su volumi NTFS. A partire da Windows Server 2012 R2 e Windows 8.1, BitLocker supporta la crittografia dei dispositivi nei computer x86 e x64 con un modulo TPM (Trusted Platform Module) che supporta lo standby connesso (precedentemente disponibile solo su dispositivi Windows RT). La crittografia dei dispositivi consente di proteggere i dati nei computer Windows e contribuisce a impedire a utenti malintenzionati di accedere ai file di sistema su cui si basano per individuare la password dell'utente oppure di accedere a un'unità rimuovendola fisicamente dal PC e installandola in un computer diverso. Per altre informazioni, vedi [Novità di BitLocker](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn306081(v%3dws.11)).

- **Supporto di volumi di grandi dimensioni**: NTFS può supportare volumi di dimensioni fino a 256 terabyte. Le dimensioni supportate per i volumi dipendono dalle dimensioni e dal numero di cluster. Con (2<sup></sup> 32-1) cluster (il numero massimo di cluster supportati da NTFS), sono supportate le dimensioni di volumi e file seguenti.

  |Dimensioni del cluster|Volume massimo|Dimensioni massime dei file|
  |---|---|---|
  |4 KB (dimensione predefinita)|16 TB|16 TB|
  |8 KB|32 TB|32 TB|
  |16 KB|64 TB|64 TB|
  |32 KB|128 TB|128 TB|
  |64 KB (dimensione massima)|256 TB|256 TB|

>[!IMPORTANT]
>I servizi e le app potrebbero imporre limiti aggiuntivi sulle dimensioni di file e volumi. Il limite di dimensione dei volumi, ad esempio, è di 64 TB se usi la funzionalità Versioni precedenti o un'app di backup che usa gli snapshot del servizio Copia shadow del volume e se non usi un alloggiamento SAN o RAID. Tuttavia, potrebbe essere necessario usare dimensioni di volumi inferiori a seconda del carico di lavoro e delle prestazioni della memoria.

### <a name="formatting-requirements-for-large-files"></a>Requisiti di formattazione per file di grandi dimensioni

Per consentire l'estensione corretta di file vhdx di grandi dimensioni, sono disponibili nuove indicazioni per la formattazione dei volumi. Quando formatti volumi che verranno usati con Deduplicazione dati o che ospiteranno file di grandi dimensioni, ad esempio file vhdx di dimensioni superiori a 1 TB, usa il cmdlet **Format-Volume** in Windows PowerShell con i parametri seguenti.

|Parametro|Descrizione|
|---|---|
|-AllocationUnitSize 64KB|Imposta una dimensione delle unità di allocazione NTFS pari a 64 KB.|
|-UseLargeFRS|Abilita il supporto di segmenti di record di file di grandi dimensioni, necessario per aumentare il numero di extent possibili per ogni file nel volume. Per segmenti di record di file di grandi dimensioni, il limite aumenta da circa 1,5 milioni di extent a circa 6 milioni di extent.|

Il cmdlet seguente, ad esempio, formatta l'unità D come volume NTFS, con l'abilitazione di segmenti di record di file e una dimensione di unità di allocazione di 64 KB.

```PowerShell
Format-Volume -DriveLetter D -FileSystem NTFS -AllocationUnitSize 64KB -UseLargeFRS
```

Puoi anche usare il comando **format**. Al prompt dei comandi di sistema, immetti il comando seguente, dove **/L** formatta un volume con segmenti di record di file di grandi dimensioni e **/A: 64k** imposta una dimensione di unità di allocazione di 64 KB:

```PowerShell
format /L /A:64k
```

### <a name="maximum-file-name-and-path"></a>Lunghezza massima del nome e del percorso dei file

NTFS supporta nomi e percorsi di file lunghi con i valori massimi seguenti:

- **Supporto di nomi di file lunghi, con compatibilità con le versioni precedenti**: NTFS supporta nomi di file lunghi, archiviando un alias 8.3 su disco (in Unicode) per garantire la compatibilità con i file system che impongono un limite di 8.3 per i nomi e le estensioni dei file. Se necessario, per motivi di prestazioni, puoi disabilitare in modo selettivo l'aliasing 8.3 nei singoli volumi NTFS in Windows Server 2008 R2, Windows 8 e nelle versioni più recenti del sistema operativo Windows.
  Nei sistemi Windows Server 2008 R2 e versioni successive i nomi brevi sono disabilitati per impostazione predefinita quando un volume viene formattato con il sistema operativo. Per la compatibilità delle applicazioni, i nomi brevi sono comunque abilitati nel volume di sistema.

- **Supporto di percorsi lunghi**: molte funzioni API Windows dispongono di versioni Unicode che consentono un percorso lungo di circa 32.767 caratteri, superiore al limite di percorso di 260 caratteri definito dall'impostazione MAX\_PATH. Per informazioni dettagliate sui requisiti per il formato del nome e del percorso dei file e per l'implementazione di percorsi lunghi, vedi [Denominazione di file, percorsi e spazi dei nomi](https://msdn.microsoft.com/library/windows/desktop/aa365247).

- **Archiviazione in cluster**: se usato in cluster di failover, NTFS supporta volumi disponibili in modo continuo a cui è possibile accedere contemporaneamente da più nodi del cluster se usati insieme al file system Volume condiviso cluster. Per altre informazioni, vedi [Usare i volumi condivisi del cluster in un cluster di failover](../../failover-clustering/failover-cluster-csvs.md).

### <a name="flexible-allocation-of-capacity"></a>Allocazione flessibile della capacità

Se lo spazio su un volume è limitato, NTFS consente di procedere nei modi seguenti per usare la capacità di archiviazione di un server:

- Usa le quote disco per tenere traccia e controllare l'utilizzo dello spazio su disco nei volumi NTFS per i singoli utenti.
- Usa la compressione del file system per ottimizzare la quantità di dati che è possibile archiviare.
- Aumenta le dimensioni di un volume NTFS aggiungendo spazio non allocato dallo stesso disco o da un disco diverso.
- Monta un volume in qualsiasi cartella vuota di un volume NTFS locale se esaurisci le lettere di unità o devi creare altro spazio accessibile da una cartella esistente.

## <a name="additional-information"></a>Altre informazioni

- [Consigli sulle dimensioni del cluster per ReFS e NTFS](https://techcommunity.microsoft.com/t5/Storage-at-Microsoft/Cluster-size-recommendations-for-ReFS-and-NTFS/ba-p/425960)
- [Panoramica di Resilient File System (ReFS)](../refs/refs-overview.md)
- [Novità di NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11)) (Windows Server 2012 R2)
- [Novità di NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff383236(v=ws.10)) (Windows Server 2008 R2, Windows 7)
- [Integrità di NTFS e Chkdsk](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11))
- [NTFS con riparazione automatica](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10)) (funzionalità introdotta in Windows Server 2008)
- [NTFS transazionale](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc730726(v%3dws.10)) (funzionalità introdotta in Windows Server 2008)
- [Archiviazione in Windows Server](../storage.md)