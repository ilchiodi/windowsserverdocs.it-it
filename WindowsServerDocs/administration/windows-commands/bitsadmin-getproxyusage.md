---
title: bitsadmin getproxyusage
description: Windows Commands Topic for **BITSAdmin getproxyusage**, che recupera l'impostazione di utilizzo del proxy per il processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f940a70e-3b02-497e-a47f-b37b905c299e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 01c9bb9a1d413fa847482f652e18eed30ad76109
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850514"
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
| lavoro | Nome visualizzato o GUID del processo. |

## <a name="remarks"></a>Note

I valori di utilizzo del proxy includono:

- **Preconfig** : usare le impostazioni predefinite di Internet Explorer del proprietario.

- **No_Proxy** : non usare un server proxy.

- **Override** : usare un elenco di proxy esplicito.

- **Rilevamento** automatico: rileva automaticamente le impostazioni del proxy.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente viene recuperato l'utilizzo di proxy per il processo denominato *myDownloadJob*.

```
C:\>bitsadmin /getproxyusage myDownloadJob
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)