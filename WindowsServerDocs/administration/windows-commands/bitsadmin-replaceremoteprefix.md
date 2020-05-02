---
title: bitsadmin replaceremoteprefix
description: Argomento di riferimento per il comando Bitsadmin REPLACEREMOTEPREFIX, che consente di modificare l'URL remoto per tutti i file del processo da *oldprefix* a *newprefix*, se necessario.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d0e0abb1-bdb4-4c74-abbc-16c809f5fd81
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 745d026513413db799e86df3422d5ee19c89274f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717033"
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
| processo | Nome visualizzato o GUID del processo. |
| oldprefix | Prefisso URL esistente. |
| newprefix | Nuovo prefisso URL. |

## <a name="examples"></a>Esempi

Per modificare l'URL remoto per tutti i file nel processo denominato *myDownloadJob*, *http://stageserver* da *http://prodserver*a.

```
bitsadmin /replaceremoteprefix myDownloadJob http://stageserver http://prodserver
```

## <a name="additional-information"></a>Informazioni aggiuntive

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
