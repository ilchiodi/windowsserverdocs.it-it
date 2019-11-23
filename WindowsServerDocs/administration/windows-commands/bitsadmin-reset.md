---
title: bitsadmin reset
description: "Argomento dei comandi di Windows per la **reimpostazione di Bitsadmin** : Annulla tutti i processi nella coda di trasferimento di proprietà dell'utente corrente."
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: adc6b07a7b5d1414c733fe6a3ac05eba7cb3029e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380807"
---
# <a name="bitsadmin-reset"></a>bitsadmin reset

Annulla tutti i processi nella coda di trasferimento a cui appartiene l'utente corrente.

**BITSAdmin 1,5 e versioni precedenti**: se si dispone di privilegi di amministratore, **reimpostare** Annulla tutti i processi nella coda. L'opzione/AllUsers non è supportata.

## <a name="syntax"></a>Sintassi

```
bitsadmin /Reset [/AllUsers]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|AllUsers|Facoltativo: Annulla tutti i processi nella coda.|

## <a name="remarks"></a>Osservazioni

È necessario disporre dei privilegi di amministratore per utilizzare il **AllUsers** parametro.

> [!NOTE]
> Gli amministratori non possono reimpostare i processi creati dal sistema locale. Usare l'utilità di pianificazione per pianificare questo comando come attività usando le credenziali del sistema locale.

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente Annulla tutti i processi nella coda di trasferimento per l'utente corrente.
```
C:\>bitsadmin /Reset
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)