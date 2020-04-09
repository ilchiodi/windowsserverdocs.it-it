---
title: bitsadmin monitor
description: Windows Commands Topic for **BITSAdmin monitor**, che monitora i processi nella coda di trasferimento di proprietà dell'utente corrente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2c424d27-e011-49c2-b579-a2c235467c39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bda268afd5fda24bba2afb04b32bac9cda9a05bb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850214"
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

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente consente di monitorare la coda di trasferimento per i processi di proprietà dell'utente corrente e aggiorna le informazioni ogni 60 secondi.

```
C:\>bitsadmin /monitor /refresh 60
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)