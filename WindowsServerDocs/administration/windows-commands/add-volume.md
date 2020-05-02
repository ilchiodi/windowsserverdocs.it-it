---
title: Aggiungi volume
description: Argomento di riferimento per il comando Add volume, che aggiunge volumi al set di copie shadow, ovvero il set di volumi da replicare.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b7d4d35d-8bda-46d2-8df5-eb598cecaaba
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a8cfd3d8f7d9f008e3136d8f694dc00370b8b0f2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719204"
---
# <a name="add-volume"></a>Aggiungi volume

Aggiunge volumi Shadow copia impostato, ovvero il set di volumi per la copia shadow. Quando viene creata una copia shadow, una variabile di ambiente collega l'alias per l'ID di ombreggiatura, l'alias può essere utilizzato quindi per la creazione di script.

I volumi vengono aggiunti uno alla volta. Ogni volta che viene aggiunto un volume, questo viene controllato per assicurarsi che VSS supporti la creazione di copie shadow per tale volume. Questo controllo può essere invalidato da un uso successivo del comando **Imposta contesto** .

Questo comando è necessario creare copie shadow. Se utilizzata senza parametri, **aggiungere volume** Visualizza la Guida al prompt dei comandi.

## <a name="syntax"></a>Sintassi

```
add volume <volume> [provider <providerid>]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<volume>` | Specifica un volume da aggiungere all'insieme di copia Shadow. Almeno un volume è necessario per la creazione di copie shadow. |
| `[provider \<providerid>]` | Specifica l'ID provider per un provider registrato da usare per creare la copia shadow. Se **provider** non è specificato, viene utilizzato il provider predefinito. |

## <a name="examples"></a>Esempi

Per visualizzare l'elenco corrente dei provider registrati, il `diskshadow>` digitare:

```
list providers
```

L'output seguente viene visualizzato un solo provider, che verrà utilizzato per impostazione predefinita:

```
* ProviderID: {b5946137-7b9f-4925-af80-51abd60b20d5}
        Type: [1] VSS_PROV_SYSTEM
        Name: Microsoft Software Shadow Copy provider 1.0
        Version: 1.0.0.7
        CLSID: {65ee1dba-8ff4-4a58-ac1c-3470ee2f376a}
1 provider registered.
```

Per aggiungere l'unità C: al set di copie shadow e assegnare un alias denominato *system1*, digitare:

```
add volume c: alias System1
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Imposta contesto (comando)](set-context.md)
