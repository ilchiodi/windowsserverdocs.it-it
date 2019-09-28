---
title: Panoramica di NTFS
description: Una spiegazione della funzionalità NTFS.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 06/17/2019
ms.localizationpriority: medium
ms.openlocfilehash: b98877d0a94ff8033b65bf74d0118e2a5f1ea092
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402081"
---
# <a name="ntfs-overview"></a>Panoramica di NTFS

>Si applica a: Windows 10, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

NTFS, il file system principale per le versioni recenti di Windows e Windows Server, offre un set completo di funzionalità, tra cui descrittori di sicurezza, crittografia, quote disco e metadati avanzati, e può essere usato con volumi condivisi cluster (CSV) per fornire continuamente volumi disponibili a cui è possibile accedere simultaneamente da più nodi di un cluster di failover.

Per ulteriori informazioni sulle funzionalità nuove e modificate in NTFS in Windows Server 2012 R2, vedere Novità [di NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11)). Per ulteriori informazioni sulle funzionalità, vedere la sezione [informazioni aggiuntive](#additional-information) in questo argomento. Per ulteriori informazioni su ReFS (Resilient file System), vedere Panoramica di [Resilient file System (Refs)](../refs/refs-overview.md).

## <a name="practical-applications"></a>Applicazioni pratiche

### <a name="increased-reliability"></a>Una maggiore affidabilità

NTFS utilizza le informazioni relative al file di log e al checkpoint per ripristinare la coerenza del file system quando il computer viene riavviato dopo un errore di sistema. Dopo un errore di settore non valido, NTFS esegue automaticamente il mapping del cluster che contiene il settore danneggiato, alloca un nuovo cluster per i dati, contrassegna il cluster originale come non valido e non usa più il vecchio cluster. Ad esempio, dopo un arresto anomalo del server, NTFS è in grado di ripristinare i dati riproducendo i file di log.

NTFS monitora e corregge continuamente i problemi di danneggiamento temporaneo in background senza portare il volume offline. questa funzionalità è nota come NTFS con correzione [automatica](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10)), introdotta in Windows Server 2008. Per i problemi di danneggiamento più grandi, l'utilità Chkdsk, in Windows Server 2012 e versioni successive, analizza e analizza l'unità mentre il volume è online, limitando il tempo offline al tempo necessario per ripristinare la coerenza dei dati nel volume. Quando NTFS viene usato con volumi condivisi del cluster, non è necessario alcun tempo di inattività. Per altre informazioni, vedere [Integrità NTFS e Chkdsk](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11)).

### <a name="increased-security"></a>Maggiore sicurezza

- **Sicurezza basata sull'elenco di controllo di accesso (ACL) per file e cartelle**: NTFS consente di impostare le autorizzazioni per un file o una cartella, specificare i gruppi e gli utenti di cui si vuole limitare o consentire l'accesso e selezionare tipo di accesso.

- **Supporto per crittografia unità BitLocker**: crittografia unità BitLocker fornisce sicurezza aggiuntiva per informazioni di sistema critiche e altri dati archiviati in volumi NTFS. A partire da Windows Server 2012 R2 e Windows 8.1, BitLocker fornisce supporto per la crittografia dei dispositivi nei computer x86 e x64 con una Trusted Platform Module (TPM) che supporta la modalità standby connessa (disponibile in precedenza solo nei dispositivi Windows RT). La crittografia del dispositivo consente di proteggere i dati nei computer basati su Windows e contribuisce a impedire agli utenti malintenzionati di accedere ai file di sistema su cui si basano per individuare la password dell'utente o di accedere a un'unità rimuovendo fisicamente dal PC e installando il file in un un altro. Per ulteriori informazioni, vedere Novità [di BitLocker](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn306081(v%3dws.11)).

- **Supporto per volumi di grandi dimensioni**: NTFS può supportare volumi di dimensioni pari a 256 terabyte. Le dimensioni del volume supportate sono influenzate dalle dimensioni del cluster e dal numero di cluster. Con (2<sup>32</sup> -1) cluster (il numero massimo di cluster supportati da NTFS), sono supportate le dimensioni del volume e dei file seguenti.

  |Dimensioni del cluster|Volume più grande|File più grande|
  |---|---|---|
  |4 KB (dimensione predefinita)|16 TB|16 TB|
  |8 KB|32 TB|32 TB|
  |16 KB|64 TB|64 TB|
  |32 KB|128 TB|128 TB|
  |64 KB (dimensioni massime)|256 TB|256 TB|

>[!IMPORTANT]
>I servizi e le app potrebbero imporre limiti aggiuntivi sulle dimensioni dei file e dei volumi. Il limite delle dimensioni del volume, ad esempio, è 64 TB se si usa la funzionalità versioni precedenti o un'app di backup che usa gli snapshot Servizio Copia Shadow del volume (VSS) e non si usa un'enclosure SAN o RAID. Tuttavia, potrebbe essere necessario usare dimensioni del volume inferiori a seconda del carico di lavoro e delle prestazioni dell'archiviazione.

### <a name="formatting-requirements-for-large-files"></a>Requisiti di formattazione per file di grandi dimensioni

Per consentire l'estensione corretta di file VHDX di grandi dimensioni, sono disponibili nuove raccomandazioni per la formattazione dei volumi. Quando si formattano i volumi che verranno usati con la deduplicazione dati o si ospitano file di grandi dimensioni, ad esempio file con estensione VHDX di dimensioni superiori a 1 TB, usare il cmdlet **Format-volume** in Windows PowerShell con i parametri seguenti.

|Parametro|Descrizione|
|---|---|
|-AllocationUnitSize 64KB|Imposta una dimensione dell'unità di allocazione NTFS di 64 KB.|
|-UseLargeFRS|Abilita il supporto per i segmenti di record di file di grandi dimensioni (FRS). Questa operazione è necessaria per aumentare il numero di extent consentiti per ogni file nel volume. Per i record FRS di grandi dimensioni, il limite aumenta da circa 1,5 milioni extent a circa 6 milioni extent.|

Il cmdlet seguente, ad esempio, formatta l'unità D come volume NTFS, con FRS abilitato e una dimensione dell'unità di allocazione di 64 KB.

```PowerShell
Format-Volume -DriveLetter D -FileSystem NTFS -AllocationUnitSize 64KB -UseLargeFRS
```

È anche possibile usare il **formato** comando. Al prompt dei comandi di sistema, immettere il comando seguente, dove **/l** formatta un volume FRS di grandi dimensioni e **/a: 64K** imposta una dimensione dell'unità di allocazione di 64 KB:

```PowerShell
format /L /A:64k
```

### <a name="maximum-file-name-and-path"></a>Nome file e percorso massimi

NTFS supporta nomi di file lunghi e percorsi a lunghezza estesa con i valori massimi seguenti:

- **Supporto per nomi di file lunghi, con compatibilità con le versioni precedenti**: NTFS consente nomi di file lunghi, archiviando un alias 8,3 su disco (in Unicode) per garantire la compatibilità con i file System che impongono un limite di 8,3 per i nomi di file e le estensioni. Se necessario, per motivi di prestazioni, è possibile disabilitare selettivamente gli alias 8,3 nei singoli volumi NTFS in Windows Server 2008 R2, Windows 8 e versioni più recenti del sistema operativo Windows.
  Nei sistemi Windows Server 2008 R2 e versioni successive, i nomi brevi sono disabilitati per impostazione predefinita quando un volume viene formattato con il sistema operativo. Per la compatibilità delle applicazioni, i nomi brevi sono ancora abilitati nel volume di sistema.

- **Supporto per percorsi a lunghezza estesa**: molte funzioni API Windows hanno versioni Unicode che consentono un percorso di lunghezza estesa di circa 32.767 caratteri, oltre il limite del percorso di 260 caratteri definito dall'impostazione\_del percorso massimo. Per i requisiti dettagliati per il nome e il formato del percorso e per l'implementazione di percorsi a lunghezza estesa, vedere [denominazione di file, percorsi e spazi dei nomi](https://msdn.microsoft.com/library/windows/desktop/aa365247).

- **Archiviazione in cluster**: se usato nei cluster di failover, NTFS supporta i volumi continuamente disponibili a cui è possibile accedere contemporaneamente da più nodi del cluster se usati insieme ai volumi condivisi del cluster (CSV) file System. Per ulteriori informazioni, vedere [utilizzare volumi condivisi cluster in un cluster di failover](../../failover-clustering/failover-cluster-csvs.md).

### <a name="flexible-allocation-of-capacity"></a>Allocazione flessibile della capacità

Se lo spazio su un volume è limitato, NTFS fornisce i modi seguenti per lavorare con la capacità di archiviazione di un server:

- Usare le quote disco per tenere traccia e controllare l'utilizzo dello spazio su disco nei volumi NTFS per i singoli utenti.
- Utilizzare la compressione file system per ottimizzare la quantità di dati che è possibile archiviare.
- Aumentare le dimensioni di un volume NTFS aggiungendo spazio non allocato dallo stesso disco o da un disco diverso.
- Montare un volume in qualsiasi cartella vuota in un volume NTFS locale se si esauriscono le lettere di unità oppure è necessario creare ulteriore spazio accessibile da una cartella esistente.

## <a name="additional-information"></a>Altre informazioni

- [Consigli sulle dimensioni del cluster per ReFS e NTFS](https://techcommunity.microsoft.com/t5/Storage-at-Microsoft/Cluster-size-recommendations-for-ReFS-and-NTFS/ba-p/425960)
- [Panoramica di Resilient file System (ReFS)](../refs/refs-overview.md)
- [Novità di NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11)) (Windows Server 2012 R2)
- [Novità di NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff383236(v=ws.10)) (Windows Server 2008 R2, Windows 7)
- [Integrità NTFS e CHKDSK](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11))
- [Correzione automatica di NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10)) (introdotta in Windows Server 2008)
- [NTFS transazionale](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc730726(v%3dws.10)) (introdotta in Windows Server 2008)
- [Archiviazione in Windows Server](../storage.md)