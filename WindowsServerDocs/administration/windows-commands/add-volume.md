---
title: Aggiungi volume
description: Windows Commands argomento per **Add volume**, che aggiunge i volumi al set di copie shadow, ovvero il set di volumi da replicare.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b7d4d35d-8bda-46d2-8df5-eb598cecaaba
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 806ab273dbb63eb7341520f56a07691fe3fac214
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851354"
---
# <a name="add-volume"></a>Aggiungi volume

Aggiunge volumi Shadow copia impostato, ovvero il set di volumi per la copia shadow. Questo comando è necessario creare copie shadow. Se utilizzata senza parametri, **aggiungere volume** Visualizza la Guida al prompt dei comandi.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
add volume <Volume> [provider <ProviderID>]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
| `<Volume>` | Specifica un volume da aggiungere all'insieme di copia Shadow. Almeno un volume è necessario per la creazione di copie shadow.|
| `[provider \<ProviderID>]` | Specifica l'ID del Provider di un provider registrato da utilizzare per creare la copia shadow. Se **provider** non è specificato, viene utilizzato il provider predefinito.|

## <a name="remarks"></a>Note

-   I volumi vengono aggiunti uno alla volta.

-   Ogni volta che viene aggiunto un volume, viene controllato per verificare che Visual Sourcesafe supporta la creazione della copia shadow del volume. Questo controllo primario può essere invalidato, tuttavia, da un utilizzo successivo del **Imposta contesto** comando.

-   Quando viene creata una copia shadow, una variabile di ambiente collega l'alias per l'ID di ombreggiatura, l'alias può essere utilizzato quindi per la creazione di script.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per visualizzare l'elenco corrente dei provider registrati, il `DISKSHADOW>` digitare:

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

Per aggiungere l'unità C per l'insieme di copia Shadow e assegnare un alias denominato System1, digitare:

```
add volume c: alias System1
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)