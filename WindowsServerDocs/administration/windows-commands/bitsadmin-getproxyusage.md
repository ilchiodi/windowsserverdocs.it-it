---
title: bitsadmin getproxyusage
description: Argomento di riferimento per il comando Bitsadmin getproxyusage, che consente di recuperare l'impostazione di utilizzo del proxy per il processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f940a70e-3b02-497e-a47f-b37b905c299e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 13a3f216b1ed3c77dbbefee37d73a657525daa36
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717644"
---
# <a name="bitsadmin-getproxyusage"></a>bitsadmin getproxyusage

Recupera l'impostazione di utilizzo di proxy per il processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /getproxyusage <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| processo | Nome visualizzato o GUID del processo. |

#### <a name="output"></a>Output

I valori di utilizzo del proxy restituiti possono essere:

- **Preconfig** : usare le impostazioni predefinite di Internet Explorer del proprietario.

- **No_Proxy** : non usare un server proxy.

- **Override** : usare un elenco di proxy esplicito.

- **Rilevamento** automatico: rileva automaticamente le impostazioni del proxy.

## <a name="examples"></a>Esempi

Per recuperare l'utilizzo del proxy per il processo denominato *myDownloadJob*:

```
bitsadmin /getproxyusage myDownloadJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
