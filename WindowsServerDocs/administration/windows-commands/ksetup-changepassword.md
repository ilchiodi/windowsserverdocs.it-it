---
title: ksetup:changepassword
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: f629c6c7930777583df38f5af900ed380ec60f9c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878532"
---
# <a name="ksetupchangepassword"></a>ksetup:changepassword



Usa il valore della password (kpasswd) Centro distribuzione chiavi (KDC) per modificare la password dell'utente connesso. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
ksetup /changepassword <OldPasswd> <NewPasswd>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<OldPasswd>|Indica la password esistente dell'utente connesso.|
|\<NewPasswd >|Gli stati usato per l'accesso nella nuova password dell'utente.|

## <a name="remarks"></a>Note

Questo comando Usa il valore della password (kpasswd) KDC per modificare la password dell'utente connesso. Kpasswd, se impostato, verrà visualizzato nell'output eseguendo le **che ksetup /dumpstate** comando.

La nuova password deve soddisfare tutti i requisiti di password che sono impostati su questo computer.

Se l'account utente non viene trovato nel dominio corrente, il sistema chiederà di specificare il nome di dominio in cui si trova l'account utente.

Se si vuole forzare una modifica della password all'accesso successivo, questo comando consente di usare l'asterisco (*) in modo che l'utente verrà richiesto per una nuova password.

L'output del comando segnala lo stato di esito positivo o negativo.

## <a name="BKMK_Examples"></a>Esempi

Modificare la password dell'utente attualmente connesso al computer in questo dominio:
```
ksetup /changepassword Pas$w0rd Pa$$w0rd
```
Modificare la password dell'utente attualmente connesso nel dominio Contoso:
```
ksetup /domain CONTOSO /changepassword Pas$w0rd Pa$$w0rd
```
Forza l'utente attualmente connesso per modificare la password al successivo accesso:
```
ksetup /changepassword Pas$w0rd *
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)