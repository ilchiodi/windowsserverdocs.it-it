---
title: Panoramica di NTFS
description: Spiegazione delle novità di NTFS.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 06/17/2019
ms.localizationpriority: medium
ms.openlocfilehash: 2e45fd1eb13044fdf0ba0f66a6e909a3f2d39bc3
ms.sourcegitcommit: 6fec3ca19ddaecbc936320d98cca0736dd8505d1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/18/2019
ms.locfileid: "67196161"
---
# <a name="ntfs-overview"></a>Panoramica di NTFS

>Si applica a: Windows 10, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

NTFS, ovvero il file system primario delle versioni recenti di Windows e Windows Server, fornisce un set completo di funzionalità, tra cui i descrittori di protezione, crittografia, le quote disco e metadata complessi e può essere usato con volumi condivisi Cluster (CSV) per fornire in modo continuo volumi disponibili che possono accedere contemporaneamente da più nodi di un cluster di failover.

Per altre informazioni sulle funzionalità nuove e modificate in NTFS in Windows Server 2012 R2, vedere [What ' s New in NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11)). Per informazioni sulle funzionalità aggiuntive, vedere la [informazioni aggiuntive](#additional-information) sezione di questo argomento. Per altre informazioni sulle più recenti Resilient File System (ReFS), vedere [Panoramica di Resilient File System (ReFS)](../refs/refs-overview.md).

## <a name="practical-applications"></a>Applicazioni pratiche

### <a name="increased-reliability"></a>Una maggiore affidabilità

NTFS utilizza le informazioni di checkpoint e file di log per ripristinare la coerenza del file system quando il computer viene riavviato dopo un errore di sistema. Dopo un errore di settori danneggiati, NTFS in modo dinamico esegue un nuovo mapping del cluster che contiene il settore danneggiato, alloca un nuovo cluster per i dati, contrassegna il cluster originale non valido e non viene più utilizzato il vecchio cluster. Ad esempio, dopo un arresto anomalo del server, NTFS può ripristinare i dati riproducendo i file di log.

NTFS monitora continuamente e corregge i problemi di danneggiamento temporanei in background senza portare offline il volume (questa funzionalità è detta [correzione automatica di NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10)), introdotta in Windows Server 2008). Per problemi di danneggiamento dei più grandi, l'utilità Chkdsk in Windows Server 2012 e versioni successive, analizza e analizza le unità, mentre il volume è online, la limitazione di tempo offline per il tempo necessario per ripristinare la coerenza dei dati nel volume. Quando NTFS viene usato con volumi condivisi del Cluster, non è necessario alcun tempo di inattività. Per altre informazioni, vedere [Integrità NTFS e Chkdsk](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11)).

### <a name="increased-security"></a>Una maggiore sicurezza

- **In base al controllo elenco ACL di sicurezza per i file e cartelle**: NTFS è possibile impostare autorizzazioni su un file o cartella, specificare i gruppi e utenti per cui si desidera limitare o consentire l'accesso e selezionare il tipo di accesso.

- **Supporto per crittografia unità BitLocker**: crittografia unità BitLocker offre sicurezza aggiuntiva per le informazioni di sistema critici e altri dati archiviati in volumi NTFS. Partire da Windows Server 2012 R2 e Windows 8.1, BitLocker offre supporto per la crittografia del dispositivo nei computer x86 e basate su x64 con un Trusted Platform Module (TPM) che supporta lo standby connesso (in precedenza disponibili solo nei dispositivi Windows RT). Crittografia del dispositivo consente di proteggere i dati nei computer basati su Windows, e consente di impedire agli utenti malintenzionati di accedere ai file di sistema si basano su per individuare la password dell'utente, o accedere a un'unità rimuovendola dal PC fisicamente e installandolo in un uno diverso. Per altre informazioni, vedere [quali sono le novità di BitLocker](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn306081(v%3dws.11)).

- **Supporto per volumi di grandi dimensioni**: NTFS può supportare volumi di dimensioni pari a 256 TB. Volume con dimensioni sono interessate dalle dimensioni del cluster e il numero di cluster è supportata. Con (2<sup>32</sup> – 1) sono supportati i cluster (il numero massimo di cluster che supporta NTFS), il volume seguente e le dimensioni dei file.

  |Dimensione del cluster|Volume più grande|File di dimensioni maggiori|
  |---|---|---|
  |4 KB (dimensione predefinita)|16 TB|16 TB|
  |8 KB|32 TB|32 TB|
  |16 KB|64 TB|64 TB|
  |32 KB|128 TB|128 TB|
  |64 KB (dimensione massima)|256 TB|256 TB|

>[!IMPORTANT]
>Servizi e App imponga limiti aggiuntivi sulle dimensioni di file e il volume. Ad esempio, la dimensione massima volume è 64 TB se si usa la funzionalità versioni precedenti o un'app di backup che consente di usare gli snapshot servizio Copia Shadow del Volume (VSS) (e non si usa un'enclosure RAID o SAN). Tuttavia, si potrebbe essere necessario usare i volumi di dimensioni inferiori a seconda del carico di lavoro e le prestazioni delle risorse di archiviazione.

### <a name="formatting-requirements-for-large-files"></a>Requisiti di formattazione per file di grandi dimensioni

Per consentire l'estensione corretta dei file vhdx di grandi dimensioni, sono disponibili nuove raccomandazioni per la formattazione di volumi. Quando si formatta i volumi che verranno usati con la deduplicazione dei dati è o saranno ospitata file molto grandi, ad esempio file con estensione vhdx superiori a 1 TB, usare il **Format-Volume** cmdlet in Windows PowerShell con i parametri seguenti.

|Parametro|Descrizione|
|---|---|
|-AllocationUnitSize 64KB|Imposta una dimensione di unità di allocazione NTFS 64 KB.|
|-UseLargeFRS|Abilita il supporto per segmenti di record di grandi dimensioni file (FRS). È necessario aumentare il numero di extent consentito per ogni file nel volume. Per record del servizio Replica file di grandi dimensioni, il limite aumenta da extent circa 1,5 milioni per gli extent di circa 6 milioni.|

Ad esempio, i formati di cmdlet seguenti l'unità D come volume NTFS, con il servizio Replica file abilitata e una dimensione di unità di allocazione di 64 KB.

```PowerShell
Format-Volume -DriveLetter D -FileSystem NTFS -AllocationUnitSize 64KB -UseLargeFRS
```

È anche possibile usare la **formato** comando. Un prompt dei comandi di sistema, immettere il comando seguente, dove **/L** formatta un volume elevato di FRS e **/A:64 k** imposta una dimensione di unità di allocazione di 64 KB:

```PowerShell
format /L /A:64k
```

### <a name="maximum-file-name-and-path"></a>Percorso e nome file massima

NTFS supporta nomi di file lunghi e sui percorsi estesi a lunghezza, con i valori massimi seguenti:

- **Supporto per i nomi di file di lunga durata, con compatibilità con le versioni precedenti**: NTFS è nomi file lunghi, archiviare un 8.3 alias su disco (in formato Unicode) per garantire la compatibilità con sistemi di file che è previsto un limite 8.3 per i nomi di file ed estensioni. Se necessario (per motivi di prestazioni), è possibile disabilitare in modo selettivo gli alias 8.3 nei singoli volumi NTFS in Windows Server 2008 R2, Windows 8 e versioni più recenti del sistema operativo Windows.
  In Windows Server 2008 R2 e successive, i nomi brevi sono disabilitati per impostazione predefinita quando si formatta un volume con il sistema operativo. Compatibilità delle applicazioni, i nomi brevi ancora sono abilitati nel volume di sistema.

- **Il supporto dei percorsi di lunghezza esteso**: molte API di Windows funzioni hanno versioni di Unicode che consentono un percorso di lunghezza esteso di circa 32.767 caratteri, ovvero oltre il limite di 260 caratteri percorso definito per il numero massimo di\_impostazione del percorso. Nome del file dei dettagli e i requisiti di formato di percorso e indicazioni per l'implementazione di percorsi estesi a lunghezza, vedere [denominazione di file, percorsi e spazi dei nomi](https://msdn.microsoft.com/library/windows/desktop/aa365247).

- **Archiviazione in cluster**: quando usati nei cluster di failover, NTFS supporta volumi continuamente disponibili che sono accessibili da più nodi del cluster contemporaneamente quando usato in combinazione con il file system di volumi condivisi Cluster (CSV). Per altre informazioni, vedere [usare volumi condivisi Cluster in un Cluster di Failover](../../failover-clustering/failover-cluster-csvs.md).

### <a name="flexible-allocation-of-capacity"></a>Assegnazione flessibile della capacità

Se lo spazio su un volume è limitato, NTFS nei seguenti modi per lavorare con la capacità di archiviazione di un server:

- Usare le quote disco per tenere traccia e controllare l'utilizzo di spazio su disco nei volumi NTFS per i singoli utenti.
- Utilizzare la compressione del file per ottimizzare la quantità di dati che possono essere archiviati.
- Aumentare le dimensioni di un volume NTFS mediante l'aggiunta di spazio non allocato dallo stesso disco o da un altro disco.
- Montare un volume in una cartella vuota in un volume NTFS locale se si esaurisce le lettere di unità o è necessario creare spazio aggiuntivo che è accessibile da una cartella esistente.

## <a name="additional-information"></a>Altre informazioni

- [Consigli relativi alle dimensioni del cluster per NTFS e ReFS](https://techcommunity.microsoft.com/t5/Storage-at-Microsoft/Cluster-size-recommendations-for-ReFS-and-NTFS/ba-p/425960)
- [Panoramica di resilient File System (ReFS)](../refs/refs-overview.md)
- [Che cosa sono le novità di NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11)) (Windows Server 2012 R2)
- [Che cosa sono le novità di NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff383236(v=ws.10)) (Windows Server 2008 R2, Windows 7)
- [Integrità di NTFS e CHKDSK](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11))
- [Riparazione automatica di NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10)) (introdotta in Windows Server 2008)
- [Transactional NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc730726(v%3dws.10)) (introdotta in Windows Server 2008)
- [Archiviazione in Windows Server](../storage.md)