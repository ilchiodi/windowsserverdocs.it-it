---
title: Miglioramenti di gestione della memoria e della cache
description: Cache e i miglioramenti di gestione della memoria in Windows Server 2016
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Pavel; ATales
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: bd15cfc0714d1992e6b15d716b6b96ce259786da
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868582"
---
# <a name="cache-and-memory-manager-improvements"></a>Miglioramenti di gestione della memoria e della cache

Questo argomento descrive i miglioramenti di gestione della Cache e gestione della memoria in Windows Server 2012 e 2016.

## <a name="cache-manager-improvements-in-windows-server-2016"></a>Miglioramenti di gestione della cache in Windows Server 2016
Gestione della cache è inoltre aggiunto supporto per true asincrona memorizzata nella cache legge.
Questo potenzialmente può migliorare le prestazioni di un'applicazione se lo si basa principalmente sulle operazioni di lettura asincrone memorizzata nella cache.  Mentre la maggior parte dei file System in arrivo hanno supportato letture asincrone in cache per un periodo di tempo, si sono verificati spesso limitazioni delle prestazioni a causa di varie scelte di progettazione relative alla gestione di pool di thread e le code di lavoro interno dei file System.  Grazie al supporto di kernel corretto, gestione della Cache adesso nasconde tutti i pool di thread e complessità di gestione di coda di lavoro dal file System, rendendo più efficiente in gestione asincrona memorizzata nella cache le letture. Gestione della cache con un set di controllo datastructures per ognuno dei (valore massimo del sistema supportato) i livelli di annidamento del disco rigido virtuale per ottimizzare il parallelismo.


## <a name="cache-manager-improvements-in-windows-server-2012"></a>Miglioramenti di gestione della cache in Windows Server 2012
Oltre a miglioramenti della gestione della Cache per la lettura logica per carichi di lavoro sequenziali, una nuova API [CcSetReadAheadGranularityEx](https://msdn.microsoft.com/library/windows/hardware/hh406341.aspx) è stato aggiunto per consentire a file di driver di sistema, ad esempio SMB, modificare i relativi parametri read ahead. Consente una migliore velocità effettiva per gli scenari di file remoto mediante l'invio di che più piccole dimensioni con richieste di lettura anziché inviare che un unico grande richiesta ahead di lettura. Solo i componenti kernel, ad esempio driver del file system, possono configurare a livello di programmazione questi valori in base al file.

## <a name="memory-manager-improvements-in-windows-server-2012"></a>Miglioramenti di gestione della memoria in Windows Server 2012
Abilitazione della combinazione di pagina può ridurre l'utilizzo della memoria nei server che dispone di numerose pagine private, paginabile con contenuto identico. Ad esempio, i server che eseguono più istanze della stessa app a elevato utilizzo di memoria o una singola app che funziona con i dati molto ripetitivi, potrebbero essere buoni candidati per provare la combinazione di pagina. Lo svantaggio dell'abilitazione di combinazione di pagina è un aumento dell'utilizzo della CPU.

Di seguito sono riportati alcuni esempi di ruoli del server in cui pagina combinazione è improbabile che offrono molti vantaggi:

-   File server (la maggior parte della memoria vengono utilizzati dalle pagine di file che non sono privati e pertanto non combinable)

-   Microsoft SQL Server che sono configurati per utilizzare AWE o pagine di grandi dimensioni (la maggior parte della memoria è privato, ma non divisibile in pagine)

La combinazione di pagina è disabilitata per impostazione predefinita, ma può essere abilitato usando il [Enable-MMAgent](https://technet.microsoft.com/library/jj658954.aspx) cmdlet di Windows PowerShell. La combinazione di pagina è stata aggiunta in Windows Server 2012.
