---
title: Panoramica di NTFS
description: Una spiegazione di che cosa è NTFS.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/06/2018
ms.localizationpriority: medium
ms.openlocfilehash: 556110fe7bed1aed002ef6d985324ff5171e770e
ms.sourcegitcommit: 5ed023a2ef3a9002daf41c7717feb1df186d2a14
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/02/2019
ms.locfileid: "9122104"
---
# Panoramica di NTFS

>Si applica a: Windows 10, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

NTFS, il file system primario delle versioni recenti di Windows e Windows Server, offre un set completo di funzionalità che includono descrittori di sicurezza, crittografia, quote disco e metadata complessi e può essere usato con volumi condivisi Cluster (CSV) per fornire continuamente volumi disponibili a cui è possibile accedere contemporaneamente da più nodi di un cluster di failover.

Per altre informazioni sulle funzionalità nuove e modificate in NTFS in Windows Server 2012 R2, vedi [What ' s New in NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11)). Ulteriori informazioni sulle funzionalità, vedi la sezione [informazioni aggiuntive](#additional-information) di questo argomento. Per ulteriori informazioni su più recenti Resilient File System (ReFS), vedi [la panoramica Resilient File System (ReFS)](../refs/refs-overview.md).

## Applicazioni pratiche

### Migliorare l'affidabilità

NTFS utilizza le informazioni di checkpoint e di file di log per ripristinare la coerenza del file system, quando il computer viene riavviato dopo un errore di sistema. Dopo un errore settori danneggiati, NTFS in modo dinamico esegue un nuovo mapping del cluster che contiene il settore danneggiato, alloca un nuovo cluster per i dati, contrassegna del cluster originale come danneggiati e non utilizza più cluster precedente. Ad esempio, dopo un arresto anomalo del server, NTFS possibile ripristinare i dati per la riproduzione dei suoi file di log.

NTFS continuamente esegue il monitoraggio e corregge i problemi di danneggiamento temporaneo in background senza portare offline il volume (questa funzionalità è noto come [la riparazione automatica NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10)), introdotto in Windows Server 2008). Per i problemi di danneggiamento più grande, l'utilità Chkdsk, in Windows Server 2012 e versioni successive, analizza e analizza l'unità, mentre il volume è in linea, limitando tempo offline per il tempo necessario per ripristinare la coerenza dei dati nel volume. Quando NTFS viene usato con volumi condivisi Cluster, non è necessario alcun tempo di inattività. Per altre informazioni, vedi [NTFS integrità e Chkdsk](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11)).

### Maggiore protezione

- **La sicurezza basata su elenco di controllo di accesso per i file e cartelle**, ovvero NTFS consente di impostare le autorizzazioni in un file o una cartella, specificare i gruppi e utenti per cui vuoi impedire o consentire l'accesso e seleziona il tipo di accesso.

- **Supporto per la crittografia unità BitLocker**, ovvero la crittografia unità BitLocker offre sicurezza aggiuntiva per informazioni di sistema critici e altri dati archiviati in volumi NTFS. A partire da Windows Server 2012 R2 e Windows 8.1, BitLocker offre supporto per la crittografia dispositivo su x86 e x64 basato su computer con un Trusted Platform Module (TPM) che supporta connesso standby (in precedenza disponibile solo nei dispositivi Windows RT). Crittografia dispositivo protegge i dati nei computer basati su Windows e consente di bloccare utenti malintenzionati di accedere ai file di sistema e si basano su per individuare la password dell'utente o di accedere a un'unità da fisicamente rimuoverla dal PC e installarla in un uno diverso. Per altre informazioni, vedi [What ' s new in BitLocker](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn306081(v%3dws.11)).

- **Supporto per volumi di grandi dimensioni**, ovvero NTFS può supportare più grande di 256 terabyte volumi. Supportato volume dimensioni sono influenzate dalla dimensione del cluster e il numero di cluster. Con (2<sup>32</sup> – 1) sono supportate i cluster (il numero massimo di cluster che supporta NTFS), il volume seguente e dimensioni di file.

  |Dimensione del cluster|Volume più grande|File più grande|
  |---|---|---|
  |4 KB (dimensione predefinita)|16 TB|16 TB|
  |8 KB|32 TB|32 TB|
  |16 KB|64 TB|64 TB|
  |32 KB|DA 128 TB|DA 128 TB|
  |64 KB (dimensioni massime)|256 TB|256 TB|

>[!IMPORTANT]
>Servizi e le app potrebbero imporre limiti aggiuntivi sulle dimensioni dei file e il volume. Ad esempio, il limite di dimensioni di volume è 64 TB se stai usando la funzionalità di versioni precedenti o un'app di backup che rende l'utilizzo degli snapshot di servizio Copia Shadow del Volume (VSS) (e non usi una enclosure SAN o RAID). Tuttavia, devi usare i volumi di dimensioni inferiori a seconda il carico di lavoro e le prestazioni dello spazio di archiviazione.

### Requisiti di formattazione per file di grandi dimensioni

Per consentire il corretto estensione dei file di grandi dimensioni. vhdx, ci sono nuovi consigli per la formattazione volumi. Formattazione volumi che verranno utilizzati con deduplicazione dati o ospiteranno file di grandi dimensioni, ad esempio i file. vhdx maggiori di 1 TB, usano il **Formato-Volume** cmdlet di Windows PowerShell con i parametri seguenti.

|Parametro|Descrizione|
|---|---|
|-AllocationUnitSize 64KB|Imposta una dimensione unità di allocazione di 64 KB NTFS.|
|-UseLargeFRS|Abilita il supporto per i segmenti di record di file di grandi dimensioni (FRS). Questa operazione è necessaria per aumentare il numero di extent consentito per ogni file nel volume. Per i record di FRS grandi dimensioni, il limite aumenta da 1,5 milioni extent a circa 6 milioni extent.|

Ad esempio, i formati seguenti cmdlet unità D come un volume NTFS, con il servizio Replica file abilitato e una dimensione unità di allocazione di 64 KB.

```PowerShell
Format-Volume -DriveLetter D -FileSystem NTFS -AllocationUnitSize 64KB -UseLargeFRS
```

È inoltre possibile utilizzare il comando di **formato** . In un prompt dei comandi di sistema, Immetti il comando seguente, dove **/L** formatta un elevato volume di FRS e **/A:64 k** imposta una dimensione unità di allocazione di 64 KB:

```PowerShell
format /L /A:64k
```

### Percorso e nome del file massima

NTFS supporta i nomi di file lunghi e i percorsi di lunghezza estese, con i valori massimi seguenti:

- **Supporto per i nomi di file lunghi, mantenendo la compatibilità**, ovvero NTFS consente a lungo i nomi di file e archiviare un 8.3 alias nel disco (in Unicode) per garantire la compatibilità con i sistemi di file che impone un limite 8.3 sui nomi di file e le estensioni. Se necessario (per motivi di prestazioni), è possibile disabilitare in modo selettivo 8.3 aliasing nei singoli volumi NTFS in Windows Server 2008 R2, Windows 8 e versioni più recenti del sistema operativo Windows.
  In Windows Server 2008 R2 e nei sistemi successivi, nomi brevi sono disattivati per impostazione predefinita quando un volume è formattato con il sistema operativo. Per la compatibilità delle applicazioni, i nomi brevi comunque sono abilitati nel volume di sistema.

- **Supporto per i percorsi di lunghezza estese**, ovvero le funzioni molte API di Windows sono versioni Unicode che consentono a un percorso di lunghezza esteso di circa 32767 caratteri, ovvero oltre il limite di percorso 260 caratteri definito dall'impostazione MAX\_PATH. Per nome del file dettagliati e requisiti per il formato percorso e linee guida per l'implementazione di percorsi di lunghezza estese, vedi [denominazione dei file, percorsi e spazi dei nomi](https://msdn.microsoft.com/library/windows/desktop/aa365247).

- **Archiviazione Clustered**, ovvero se usato in cluster di failover, NTFS supporta volumi continuamente disponibili a cui è possibile accedere da più nodi del cluster contemporaneamente quando utilizzato in combinazione con il file system volumi condivisi Cluster (CSV). Per altre informazioni, vedi [Uso volumi condivisi del Cluster in un Cluster di Failover](../../failover-clustering/failover-cluster-csvs.md).

### Allocazione flessibile di capacità

Se lo spazio in un volume è limitato, NTFS nei seguenti modi per lavorare con la capacità di archiviazione di un server:

- Utilizzare le quote disco per tenere traccia e controllare l'utilizzo di spazio su disco in volumi NTFS per i singoli utenti.
- Usare la compressione del file per ottimizzare la quantità di dati che possono essere archiviati.
- Aumentare le dimensioni di un volume NTFS aggiungendo spazio non allocato da stesso disco o da un disco diverso.
- Creare un volume in qualsiasi cartella vuota in un volume NTFS locale se perdono di lettere di unità o ti serve creare spazio aggiuntivo che sia accessibile da una cartella esistente.

## Altre informazioni

|Tipo di contenuto|Riferimenti|
|---|---|
|Valutazione|- [What ' s New in NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11)) (Windows Server 2012 R2)<br>- [What ' s New in NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff383236(v=ws.10)) (Windows Server 2008 R2, Windows 7)<br>- [CHKDSK e l'integrità NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11))<br>- [La riparazione automatica NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10)) (introdotto in Windows Server 2008)<br>- [NTFS transazionale](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc730726(v%3dws.10)) (introdotto in Windows Server 2008)|
|Risorse della community|- [Blog del Team di archiviazione di Windows](https://blogs.msdn.microsoft.com/san/)|
|Tecnologie correlate|- [Archiviazione in Windows Server](../storage.md)<br>- [Uso dei volumi condivisi in un Cluster di Failover](../../failover-clustering/failover-cluster-csvs.md)<br>-Le sezioni [Volumi condivisi Cluster](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831630(v%3dws.11)#cluster-shared-volumes>) e la [Progettazione di archiviazione](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831630(v%3dws.11)#storage-design>) di [Progettazione di un'infrastruttura Cloud](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831630(v%3dws.11)) <br>- [Spazi di archiviazione](../storage-spaces/overview.md)<br>- [Panoramica resilient File System (ReFS)](../refs/refs-overview.md)
