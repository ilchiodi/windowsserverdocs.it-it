---
title: nslookup root
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9c29edc3-ec49-43f2-bc49-86bf0612d816
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 47a26be99a5eee510970d3eee6b486331a98b159
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436903"
---
# <a name="nslookup-root"></a>nslookup root

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cambia il server predefinito per il server per la radice dello spazio dei nomi di dominio sistema DNS (Domain Name).
## <a name="syntax"></a>Sintassi
```
root 
```
## <a name="parameters"></a>Parametri

|    Parametro    |                      Descrizione                      |
|-----------------|-------------------------------------------------------|
| {help &#124; ?} | Viene visualizzato un breve riepilogo di **nslookup** sottocomandi. |

## <a name="remarks"></a>Note
- Attualmente, viene usato il server dei nomi DDN.mil. Questo comando è un sinonimo di lserver DDN.mil. È possibile modificare il nome del server principale con il **radice set** comando.
  ## <a name="additional-references"></a>Riferimenti aggiuntivi
  [Chiave sintassi della riga di comando](command-line-syntax-key.md)
  [nslookup Imposta radice](nslookup-set-root.md)
