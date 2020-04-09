---
title: Flussi di integrità ReFS
author: gawatu
ms.author: jgerend
manager: dmoss
ms.date: 10/16/2018
ms.topic: article
ms.prod: windows-server
ms.technology: storage
ms.assetid: 1f1215cd-404f-42f2-b55f-3888294d8a1f
ms.openlocfilehash: 5e4ce1870d8aea01de0ab621d7efe197026643db
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861334"
---
# <a name="refs-integrity-streams"></a>Flussi di integrità ReFS
>Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server (canale semestrale), Windows 10

I flussi di integrità sono una funzionalità facoltativa di ReFS che convalida e mantiene l'integrità dei dati mediante l'uso di checksum. Anche se usa sempre checksum per i metadati, per impostazione predefinita ReFS non genera o convalida checksum per i dati di file. I flussi di integrità rappresentano una funzionalità opzionale che consente agli utenti di utilizzare i checksum per i dati dei file. Quando i flussi di integrità sono abilitati, ReFS può determinare chiaramente se i dati sono validi o danneggiati. Inoltre, ReFS e Spazi di archiviazione possono correggere congiuntamente i dati e i metadati danneggiati in modo automatico.

## <a name="how-it-works"></a>Come funziona 

I flussi di integrità possono essere abilitati per file e directory individuali o per l'intero volume e le impostazioni di tali flussi possono essere attivate e disattivate in qualsiasi momento. Inoltre, le impostazioni dei flussi di integrità di file e directory vengono ereditate dalle directory padre. 

Una volta abilitati i flussi di integrità, ReFS crea e gestisce un checksum per i file specificati nei metadati dei file stessi. Questo checksum permette a ReFS di convalidare l'integrità dei dati prima di accedervi. Prima di restituire dati per cui sono abilitati i flussi di integrità, ReFS ne calcola prima di tutto il checksum:

![Calcolo del checksum per i dati dei file](media/compute-checksum.gif)

Questo checksum viene quindi confrontato con il checksum contenuto nei metadati dei file. Se i checksum corrispondono, i dati vengono contrassegnati come validi e restituiti all'utente. Se i checksum non corrispondono, i dati sono danneggiati. La resilienza del volume determina il modo in cui ReFS risponde alla situazione di danno.

- Se ReFS è montato su uno spazio semplice non resiliente o su una semplice unità, mostra un errore all'utente senza restituire i dati danneggiati. 
- Se ReFS è montato su uno spazio mirror o di parità resiliente, prova a correggere il problema. 
    - Se il tentativo riesce, ReFS applica una scrittura correttiva per ripristinare l'integrità dei dati e restituisce i dati validi all'applicazione. All'applicazione non arriverà alcuna traccia o indicazione di dati danneggiati.
    - Se il tentativo non riesce, ReFS restituisce un errore. 

ReFS registra tutti i danneggiamenti nel registro eventi di sistema, in cui viene indicato anche se i problemi sono stati corretti. 

![L'integrità dei dati per il ripristino della scrittura correttiva](media/corrective-write.gif)

## <a name="performance"></a>Prestazioni 

Benché i flussi di integrità assicurino una maggiore integrità dei dati per il sistema, questo avviene ai danni delle prestazioni. I motivi sono due:
- Se i flussi di integrità sono abilitati, tutte le operazioni di scrittura diventano operazioni di allocazione in fase di scrittura. Anche se in questo modo si evitano colli di bottiglia read-modify-write poiché ReFS non necessita di leggere o modificare i dati esistenti, i dati dei file diventano spesso frammentati, ritardando di conseguenza la lettura. 
- A seconda del carico di lavoro e delle risorse di archiviazione sottostanti del sistema, il costo del calcolo e della convalida del checksum può causare un aumento della latenza di I/O. 

Poiché i flussi di integrità implicano un costo in termini di prestazioni, è consigliabile lasciarli disabilitati in sistemi sensibili alle prestazioni. 

## <a name="integrity-scrubber"></a>Strumento di pulitura dell'integrità

Come descritto in precedenza, ReFS convaliderà automaticamente l'integrità dei dati prima di accedervi. ReFS usa anche uno strumento di pulitura in background, che gli permette di convalidare i dati a cui si accede raramente. Questo strumento di pulitura analizza periodicamente il volume, identifica i danneggiamenti latenti e attiva in modo proattivo il ripristino di eventuali dati danneggiati.

  >[!NOTE]
  >Lo strumento di pulitura dell'integrità dei dati può convalidare i dati solo nel caso di file in cui sono abilitati i flussi di integrità.

Per impostazione predefinita, lo strumento di pulitura viene eseguito ogni quattro settimane, anche se questo intervallo può essere configurato in Utilità di pianificazione in Microsoft\Windows\Data Integrity Scan. 

## <a name="examples"></a>Esempi
Per monitorare e modificare le impostazioni di integrità dei dati dei file, ReFS usa i cmdlet **Get-FileIntegrity** e **Set-FileIntegrity**.

### <a name="get-fileintegrity"></a>Get-FileIntegrity
Per determinare se i flussi di integrità sono abilitati per i dati dei file, usa il cmdlet **Get-FileIntegrity**. 

```PowerShell
PS C:\> Get-FileIntegrity -FileName 'C:\Docs\TextDocument.txt'
```

Puoi anche usare il cmdlet **Get-Item** per ottenere le impostazioni dei flussi di integrità per tutti i file in una directory specificata. 

```PowerShell
PS C:\> Get-Item -Path 'C:\Docs\*' | Get-FileIntegrity
```

### <a name="set-fileintegrity"></a>Set-FileIntegrity
Per abilitare/disabilitare i flussi di integrità per i dati dei file, usa il cmdlet **Set-FileIntegrity**. 

```PowerShell
PS C:\> Set-FileIntegrity -FileName 'H:\Docs\TextDocument.txt' -Enable $True
```

Puoi anche usare il cmdlet **Set-Item** per configurare le impostazioni dei flussi di integrità per tutti i file in una directory specificata. 

```PowerShell
PS C:\> Get-Item -Path 'H\Docs\*' | Set-FileIntegrity -Enable $True 
```

Il cmdlet **Set-FileIntegrity** può essere usato anche direttamente in volumi e directory. 

```PowerShell
PS C:\> Set-FileIntegrity H:\ -Enable $True
PS C:\> Set-FileIntegrity H:\Docs -Enable $True
```

## <a name="see-also"></a>Vedere anche

-   [Panoramica di ReFS](refs-overview.md)
-   [Clonazione blocco ReFS](block-cloning.md)
-   [Panoramica di Spazi di archiviazione diretta](../storage-spaces/storage-spaces-direct-overview.md)
