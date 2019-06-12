---
ms.assetid: 21225c11-7c72-4ea2-96bd-e63d4beb3be5
title: Fsutil quota
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 1e844d73348ee31f309f44895831ded9a2e6365c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439066"
---
# <a name="fsutil-quota"></a>Fsutil quota
>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Consente di gestire le quote disco nei volumi NTFS per fornire un controllo più preciso dell'archiviazione basata su rete.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
fsutil quota [disable] <VolumePath>
fsutil quota [enforce] <VolumePath>
fsutil quota [modify] <VolumePath> <Threshold> <Limit> <UserName>
fsutil quota [query] <VolumePath>
fsutil quota [track] <VolumePath>
fsutil quota [violations]
```

## <a name="parameters"></a>Parametri

|   Parametro   |                                                                                    Descrizione                                                                                    |
|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /disable    |                                                         Disabilita la gestione delle quote e l'applicazione nel volume specificato.                                                          |
|    rinforzare    |                                                                   Impone l'utilizzo della quota nel volume specificato.                                                                   |
|    modify     |                                                              Modifica di una quota disco esistente o crea una nuova quota.                                                              |
|     query     |                                                                            Elenca le quote disco esistenti.                                                                            |
|     Track     |                                                                    Tracce di utilizzo del disco nel volume specificato.                                                                     |
|  Violazioni   | Cerca i log di sistema e dell'applicazione e viene visualizzato un messaggio per indicare che sono state rilevate le violazioni della quota o che un utente ha raggiunto un limite di quota o soglia di quota. |
| \<VolumePath> |                                  Obbligatorio. Specifica il nome di unità seguito da una virgola o il GUID nel formato **Volume {** <em>GUID</em> **}** .                                  |
| \<Soglia >  |                            Imposta il limite (in byte) in cui vengono emessi avvisi. Questo parametro è obbligatorio per il **quota di fsutil modificare** comando.                            |
|   \<Limite >    |                                Imposta l'utilizzo disco massima consentita (in byte). Questo parametro è obbligatorio per il **quota di fsutil modificare** comando.                                |
|  \<Nome utente >  |                                      Specifica il nome di dominio o un utente. Questo parametro è obbligatorio per il **quota di fsutil modificare** comando.                                       |

## <a name="remarks"></a>Note

-   Le quote disco sono implementate in base al volume, e consentono entrambi i limiti di archiviazione di tipo soft e hard a essere implementato per ogni utente.

-   È possibile utilizzare script che utilizzano **quota di fsutil** per impostare i limiti di quota ogni volta che si aggiunge un nuovo utente o per rilevare automaticamente i limiti di quota, compilarle in un report e inviare automaticamente all'amministratore di sistema nel messaggio di posta elettronica.

### <a name="BKMK_examples"></a>Esempi
Per elencare le quote disco esistente per un volume del disco che viene specificato con il GUID, {928842df-5a01-11de-a85c-806e6f6e6963}, digitare:

```
fsutil quota query Volume{928842df-5a01-11de-a85c-806e6f6e6963}
```

Per elencare le quote disco esistente per un volume del disco che viene specificato con la lettera dell'unità **c:** , tipo:

```
Fsutil quota query C:
```

#### <a name="additional-references"></a>Altri riferimenti
[Indicazioni generali sulla sintassi della riga di comando](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


