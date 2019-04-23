---
title: bitsadmin reset
description: Argomento i comandi di Windows per **reimpostazione bitsadmin** -Annulla tutti i processi nella coda di trasferimento a cui appartiene l'utente corrente.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0e4f9d1d-072c-493f-8d7a-f6d713c3ef29
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b7c29aac55393cd87145583814b3ffa8f0a2c3b3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874252"
---
# <a name="bitsadmin-reset"></a>bitsadmin reset

Annulla tutti i processi nella coda di trasferimento a cui appartiene l'utente corrente.

**BITSAdmin 1.5 e versioni precedenti**: Se si dispone di privilegi di amministratore **reimpostare** Annulla tutti i processi nella coda. L'opzione /AllUsers non è supportata.

## <a name="syntax"></a>Sintassi

```
bitsadmin /Reset [/AllUsers]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|AllUsers|Facoltativo: Annulla tutti i processi nella coda.|

## <a name="remarks"></a>Note

È necessario disporre dei privilegi di amministratore per utilizzare il **AllUsers** parametro.

> [!NOTE]
> Gli amministratori non possono reimpostare i processi creati dal sistema locale. Utilizzare l'utilità di pianificazione per pianificare questo comando come attività utilizzando le credenziali di sistema locale.

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente Annulla tutti i processi nella coda di trasferimento per l'utente corrente.
```
C:\>bitsadmin /Reset
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)