---
title: bitsadmin addfileset
description: Windows Commands Topic for **BITSAdmin ADDFILESET**, che aggiunge uno o più file al processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 75466994-262f-4724-b14d-f813c5397675
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1c4cff7dc8439fe8e1c54d1f5d231d1b487dc70c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850974"
---
# <a name="bitsadmin-addfileset"></a>bitsadmin addfileset

Aggiunge uno o più file al processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /addfileset <Job> <TextFile>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| Job | Nome visualizzato o GUID del processo. |
| TextFile | Un file di testo, ogni riga contenente un nome di file remoto e un nome di file locale. **Nota:** I nomi sono delimitati da spazi. Le righe che iniziano con un carattere di `#` vengono considerate come commenti. |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

```
C:\>bitsadmin /addfileset files.txt
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)