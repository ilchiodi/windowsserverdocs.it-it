---
ms.assetid: 21225c11-7c72-4ea2-96bd-e63d4beb3be5
title: Fsutil quota
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 5e1c6793ca866ecacd8b00aa7e01d632c2538405
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376839"
---
# <a name="fsutil-quota"></a>Fsutil quota
>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Gestisce le quote disco nei volumi NTFS per fornire un controllo più preciso dell'archiviazione basata sulla rete.

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
|    /disable    |                                                         Disabilita il rilevamento e l'imposizione delle quote nel volume specificato.                                                          |
|    applicare    |                                                                   Impone l'utilizzo della quota nel volume specificato.                                                                   |
|    modify     |                                                              Modifica una quota del disco esistente o crea una nuova quota.                                                              |
|     query     |                                                                            Elenca le quote disco esistenti.                                                                            |
|     tenere traccia     |                                                                    Tiene traccia dell'utilizzo del disco nel volume specificato.                                                                     |
|  Violazioni   | Esegue la ricerca nei registri di sistema e dell'applicazione e visualizza un messaggio per indicare che sono state rilevate violazioni di quota o che un utente ha raggiunto una soglia di quota o un limite di quota. |
| \<VolumePath > |                                  Obbligatorio. Specifica il nome dell'unità seguito da due punti o dal GUID nel formato **volume {** <em>GUID</em> **}** .                                  |
| \<Threshold >  |                            Imposta il limite (in byte) in cui vengono emessi gli avvisi. Questo parametro è obbligatorio per il comando **fsutil quota modify** .                            |
|   \<Limit >    |                                Imposta l'utilizzo massimo del disco consentito (in byte). Questo parametro è obbligatorio per il comando **fsutil quota modify** .                                |
|  \<UserName >  |                                      Specifica il nome del dominio o dell'utente. Questo parametro è obbligatorio per il comando **fsutil quota modify** .                                       |

## <a name="remarks"></a>Note

-   Le quote disco sono implementate in base ai singoli volumi e consentono di implementare sia limiti di archiviazione rigidi che complessivi per ogni singolo utente.

-   È possibile utilizzare script di scrittura che utilizzano la **quota fsutil** per impostare i limiti di quota ogni volta che si aggiunge un nuovo utente o per tenere traccia automaticamente dei limiti di quota, compilarli in un report e inviarli automaticamente all'amministratore di sistema tramite posta elettronica.

### <a name="BKMK_examples"></a>Esempi
Per elencare le quote disco esistenti per un volume del disco specificato con il GUID, {928842df-5A01-11de-a85c-806e6f6e6963}, digitare:

```
fsutil quota query Volume{928842df-5a01-11de-a85c-806e6f6e6963}
```

Per elencare le quote disco esistenti per un volume del disco specificato con la lettera di unità **C:** , digitare:

```
Fsutil quota query C:
```

#### <a name="additional-references"></a>Altri riferimenti
[Indicazioni generali sulla sintassi della riga di comando](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


