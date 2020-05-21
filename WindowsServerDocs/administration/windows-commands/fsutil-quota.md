---
title: fsutil quota
description: Argomento di riferimento per il comando fsutil quota, che consente di gestire le quote disco nei volumi NTFS per fornire un controllo più preciso dell'archiviazione basata sulla rete.
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
ms.assetid: 21225c11-7c72-4ea2-96bd-e63d4beb3be5
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 54c4f6fe5fd5ae7a43d5057cd5837374f1b94ecd
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2020
ms.locfileid: "83435817"
---
# <a name="fsutil-quota"></a>fsutil quota

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8

Gestisce le quote disco nei volumi NTFS per fornire un controllo più preciso dell'archiviazione basata sulla rete.

## <a name="syntax"></a>Sintassi

```
fsutil quota [disable] <volumepath>
fsutil quota [enforce] <volumepath>
fsutil quota [modify] <volumepath> <threshold> <limit> <username>
fsutil quota [query] <volumepath>
fsutil quota [track] <volumepath>
fsutil quota [violations]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| disabilitare | Disabilita il rilevamento e l'imposizione delle quote nel volume specificato. |
| applicare | Impone l'utilizzo della quota nel volume specificato. |
| modify | Modifica una quota del disco esistente o crea una nuova quota. |
| query | Elenca le quote disco esistenti. |
| track | Tiene traccia dell'utilizzo del disco nel volume specificato. |
| violazioni | Esegue la ricerca nei registri di sistema e dell'applicazione e visualizza un messaggio per indicare che sono state rilevate violazioni di quota o che un utente ha raggiunto una soglia di quota o un limite di quota. |
| `<volumepath>` | Obbligatorio. Specifica il nome dell'unità seguito da due punti o dal GUID nel formato `volume{GUID}` . |
| `<threshold>`  | Imposta il limite (in byte) in cui vengono emessi gli avvisi. Questo parametro è obbligatorio per il `fsutil quota modify` comando. |
| `<limit>` | Imposta l'utilizzo massimo del disco consentito (in byte). Questo parametro è obbligatorio per il `fsutil quota modify` comando. |
| `<username>` | Specifica il nome del dominio o dell'utente. Questo parametro è obbligatorio per il `fsutil quota modify` comando. |

#### <a name="remarks"></a>Osservazioni

- Le quote disco sono implementate in base ai singoli volumi e consentono di implementare sia limiti di archiviazione rigidi che complessivi per ogni singolo utente.

- È possibile utilizzare script di scrittura che utilizzano la **quota fsutil** per impostare i limiti di quota ogni volta che si aggiunge un nuovo utente o per tenere traccia automaticamente dei limiti di quota, compilarli in un report e inviarli automaticamente all'amministratore di sistema tramite posta elettronica.

### <a name="examples"></a>Esempi

Per elencare le quote disco esistenti per un volume del disco specificato con il GUID, {928842df-5A01-11de-a85c-806e6f6e6963}, digitare:

```
fsutil quota query volume{928842df-5a01-11de-a85c-806e6f6e6963}
```

Per elencare le quote disco esistenti per un volume del disco specificato con la lettera di unità **C:**, digitare:

```
fsutil quota query C:
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [fsutil](fsutil.md)
