---
title: bitsadmin monitor
description: Argomento di riferimento per il comando Bitsadmin monitor, che consente di monitorare i processi nella coda di trasferimento di proprietà dell'utente corrente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2c424d27-e011-49c2-b579-a2c235467c39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4c8fa52f9fcf30a66b41c9cdbf7b7e1fab69f06e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717381"
---
# <a name="bitsadmin-monitor"></a>bitsadmin monitor

Monitora i processi nella coda di trasferimento di proprietà dell'utente corrente.

## <a name="syntax"></a>Sintassi

```
bitsadmin /monitor [/allusers] [/refresh <seconds>]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| /allusers | Facoltativa. Monitora i processi per tutti gli utenti. Per utilizzare questo parametro, è necessario disporre dei privilegi di amministratore. |
| /Refresh | Facoltativa. Aggiorna i dati in corrispondenza di un intervallo specificato da `<seconds>`. L'intervallo di aggiornamento predefinito è cinque secondi. Per arrestare l'aggiornamento, premere CTRL + C. |

## <a name="examples"></a>Esempi

Per monitorare la coda di trasferimento per i processi di proprietà dell'utente corrente e aggiornare le informazioni ogni 60 secondi.

```
bitsadmin /monitor /refresh 60
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
