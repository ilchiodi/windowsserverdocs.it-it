---
title: bitsadmin getproxybypasslist
description: Windows Commands Topic for **BITSAdmin getproxybypasslist**, che consente di recuperare l'elenco proxy bypass per il processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 50959be3-7014-4bc9-9a7b-68f1ff94a94a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9cd81aaef22c4173f198b765246b78b3d3bae136
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850534"
---
# <a name="bitsadmin-getproxybypasslist"></a>bitsadmin getproxybypasslist

Recupera l'elenco di bypass del proxy per il processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /getproxybypasslist <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| lavoro | Nome visualizzato o GUID del processo. |

## <a name="remarks"></a>Note

L'elenco di bypass contiene i nomi host o gli indirizzi IP, o entrambi, che non devono essere instradati tramite un proxy. L'elenco può contenere `<local>` per fare riferimento a tutti i server nella stessa LAN. L'elenco può essere punto e virgola (;) o delimitato da spazi.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente viene recuperato l'elenco di bypass del proxy per il processo denominato *myDownloadJob*.

```
C:\>bitsadmin /getproxybypasslist myDownloadJob
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)