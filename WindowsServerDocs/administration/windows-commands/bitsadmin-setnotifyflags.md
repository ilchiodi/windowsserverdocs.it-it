---
title: bitsadmin setnotifyflags
description: Argomento di riferimento per il comando Bitsadmin setnotifyflags, che imposta i flag di notifica degli eventi per il processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d5763d95-94a6-45ca-9e03-891c20047e06
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 00b704bf0943790ef01bbfbdbcbbde4dfd1845c6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717274"
---
# <a name="bitsadmin-setnotifyflags"></a>bitsadmin setnotifyflags

Imposta l'evento di flag di notifica per il processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /setnotifyflags <job> <notifyflags>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| processo | Nome visualizzato o GUID del processo. |
| notifyflags | Può includere uno o più dei flag di notifica seguenti, tra cui:<ul><li>**1.** genera un evento quando tutti i file del processo sono stati trasferiti.</li><li>**2.** genera un evento quando si verifica un errore.</li><li>**3.** genera un evento quando tutti i file hanno completato il trasferimento o quando si verifica un errore.</li><li>**4.** Disabilita le notifiche.</li></ul> |

## <a name="examples"></a>Esempi

Per impostare i flag di notifica per generare un evento quando si verifica un errore, per un processo denominato *myDownloadJob*:

```
bitsadmin /setnotifyflags myDownloadJob 2
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
