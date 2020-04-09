---
title: bitsadmin replaceremoteprefix
description: Windows Commands Topic for **BITSAdmin REPLACEREMOTEPREFIX**, che modifica l'URL remoto per tutti i file nel processo da *oldprefix* a *newprefix*, in base alle esigenze.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d0e0abb1-bdb4-4c74-abbc-16c809f5fd81
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0cea0108a292815e31e893e91dc4079305c1da9a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849814"
---
# <a name="bitsadmin-replaceremoteprefix"></a>bitsadmin replaceremoteprefix

Modifica l'URL remoto per tutti i file del processo da *oldprefix* a *newprefix*, in base alle esigenze.

## <a name="syntax"></a>Sintassi

```
bitsadmin /replaceremoteprefix <job> <oldprefix> <newprefix>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| lavoro | Nome visualizzato o GUID del processo. |
| oldprefix | Prefisso URL esistente. |
| newprefix | Nuovo prefisso URL. |

## <a name="examples"></a>Esempi

Nell'esempio seguente viene modificato l'URL remoto per tutti i file nel processo denominato *myDownloadJob*, da *http://stageserver* a *http://prodserver* .

```
C:\>bitsadmin /replaceremoteprefix myDownloadJob http://stageserver http://prodserver
```

## <a name="additional-information"></a>Informazioni aggiuntive

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)