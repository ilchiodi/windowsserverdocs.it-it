---
title: bitsadmin addfileset
description: Argomento di riferimento per il comando Bitsadmin ADDFILESET, che aggiunge uno o più file al processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 75466994-262f-4724-b14d-f813c5397675
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d610c1330818cf820923b6d4f2e3555dc477444b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718472"
---
# <a name="bitsadmin-addfileset"></a>bitsadmin addfileset

Aggiunge uno o più file al processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /addfileset <job> <textfile>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| processo | Nome visualizzato o GUID del processo. |
| textfile | Un file di testo, ogni riga contenente un nome di file remoto e un nome di file locale. **Nota:** I nomi devono essere delimitati da spazi. Le righe che iniziano `#` con un carattere vengono considerate come un commento. |

## <a name="examples"></a>Esempi

```
bitsadmin /addfileset files.txt
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
