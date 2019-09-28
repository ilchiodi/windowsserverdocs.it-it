---
title: Miglioramenti di gestione cache e memoria
description: Miglioramenti di cache e Memory Manager in Windows Server 2016
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Pavel; ATales
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 5f66f43a0d1b003ab833c91538594510b44027d6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384929"
---
# <a name="cache-and-memory-manager-improvements"></a>Miglioramenti di gestione cache e memoria

Questo argomento descrive i miglioramenti apportati a gestione cache e gestione memoria in Windows Server 2012 e 2016.

## <a name="cache-manager-improvements-in-windows-server-2016"></a>Miglioramenti di gestione cache in Windows Server 2016
In Gestione cache è stato inoltre aggiunto il supporto per le letture della cache asincrone true.
Questo potrebbe potenzialmente migliorare le prestazioni di un'applicazione se si basa principalmente sulle letture asincrone memorizzate nella cache.  Sebbene la maggior parte dei filesystem predefiniti abbia supportato le letture con cache asincrona per un certo periodo di tempo, spesso si sono verificate limitazioni a livello di prestazioni a causa delle varie scelte di progettazione relative alla gestione delle code di lavoro interne dei pool di thread e dei filesystem.  Con il supporto di kernel-proper, gestione cache nasconde ora tutte le complessità di gestione dei pool di thread e delle code di lavoro dei filesystem che rendono più efficiente la gestione delle letture asincrone nella cache. Gestione cache dispone di un set di strutture di dati di controllo per ognuno dei livelli di nidificazione del disco rigido virtuale (massimo supportato dal sistema) per ottimizzare il parallelismo.


## <a name="cache-manager-improvements-in-windows-server-2012"></a>Miglioramenti di gestione cache in Windows Server 2012
Oltre ai miglioramenti apportati a gestione cache per la logica read-ahead per i carichi di lavoro sequenziali, è stata aggiunta una nuova API [CcSetReadAheadGranularityEx](https://msdn.microsoft.com/library/windows/hardware/hh406341.aspx) per consentire ai file System Driver, ad esempio SMB, di modificare i parametri read-ahead. Consente una migliore velocità effettiva per gli scenari di file remoti inviando più richieste read-ahead di dimensioni ridotte anziché inviare una singola richiesta read-ahead di grandi dimensioni. Solo i componenti del kernel, ad esempio file system driver, possono configurare questi valori a livello di codice in base ai singoli file.

## <a name="memory-manager-improvements-in-windows-server-2012"></a>Miglioramenti di Memory Manager in Windows Server 2012
L'abilitazione della combinazione di pagine può ridurre l'utilizzo della memoria nei server che dispongono di una grande quantità di pagine private e paginabili con contenuto identico. Ad esempio, i server che eseguono più istanze della stessa app con utilizzo intensivo della memoria o una singola app che funziona con dati estremamente ripetitivi possono essere candidati ideali per provare a combinare le pagine. Lo svantaggio dell'abilitazione della combinazione di pagine è un maggiore utilizzo della CPU.

Di seguito sono riportati alcuni esempi di ruoli del server in cui è improbabile che la combinazione di pagine fornisca molti vantaggi:

-   File server (la maggior parte della memoria viene utilizzata da pagine di file che non sono private e pertanto non combinabili)

-   Microsoft SQL Server configurati per l'utilizzo di AWE o di pagine di grandi dimensioni (la maggior parte della memoria è privata ma non paginabile)

Per impostazione predefinita, la combinazione di pagine è disabilitata, ma può essere abilitata usando il cmdlet [Enable-mmagent](https://technet.microsoft.com/library/jj658954.aspx) di Windows PowerShell. La combinazione di pagine è stata aggiunta in Windows Server 2012.
