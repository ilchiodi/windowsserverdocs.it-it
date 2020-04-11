---
title: bitsadmin setnotifyflags
description: Windows Commands Topic for **BITSAdmin setnotifyflags**, che imposta i flag di notifica degli eventi per il processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d5763d95-94a6-45ca-9e03-891c20047e06
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 73c088ce2bae8d2ad99b313417c14449ddd822b5
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122796"
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
| lavoro | Nome visualizzato o GUID del processo. |
| notifyflags | Può includere uno o più dei flag di notifica seguenti, tra cui:<ul><li>**1.** genera un evento quando tutti i file del processo sono stati trasferiti.</li><li>**2.** genera un evento quando si verifica un errore.</li><li>**3.** genera un evento quando tutti i file hanno completato il trasferimento o quando si verifica un errore.</li><li>**4.** Disabilita le notifiche.</li></ul> |

## <a name="examples"></a>Esempi

Nell'esempio seguente vengono impostati i flag di notifica per generare un evento quando si verifica un errore, per un processo denominato *myDownloadJob*.

```
C:\>bitsadmin /setnotifyflags myDownloadJob 2
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)