---
title: 'che Ksetup: ChangePassword'
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 283078e7-a88f-4875-90e6-f8605e6b7ea7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 51be9e71c2b290e6346d23144543e0eec29f9d07
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375183"
---
# <a name="ksetupchangepassword"></a>che Ksetup: ChangePassword



Usa il valore della password Centro distribuzione chiavi (KDC) (kpasswd) per modificare la password dell'utente che ha eseguito l'accesso. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
ksetup /changepassword <OldPasswd> <NewPasswd>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<OldPasswd >|Indica la password esistente dell'utente connesso.|
|\<NewPasswd >|Indica la nuova password dell'utente connesso.|

## <a name="remarks"></a>Note

Questo comando usa il valore della password KDC (kpasswd) per modificare la password dell'utente che ha eseguito l'accesso. Il kpasswd, se impostato, viene visualizzato nell'output eseguendo il comando **che Ksetup/dumpstate** .

La nuova password dell'utente deve soddisfare tutti i requisiti di password impostati nel computer.

Se l'account utente non viene trovato nel dominio corrente, il sistema chiederà di specificare il nome di dominio in cui risiede l'account utente.

Se si vuole forzare una modifica della password all'accesso successivo, questo comando consente di usare l'asterisco (*), in modo che all'utente venga richiesto di immettere una nuova password.

L'output del comando segnala lo stato di esito positivo o negativo.

## <a name="BKMK_Examples"></a>Esempi

Modificare la password di un utente attualmente connesso al computer in questo dominio:
```
ksetup /changepassword Pas$w0rd Pa$$w0rd
```
Modificare la password di un utente attualmente connesso nel dominio contoso:
```
ksetup /domain CONTOSO /changepassword Pas$w0rd Pa$$w0rd
```
Forza l'utente attualmente connesso a modificare la password all'accesso successivo:
```
ksetup /changepassword Pas$w0rd *
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)