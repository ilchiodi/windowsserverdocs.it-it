---
title: 'che Ksetup: ChangePassword'
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 283078e7-a88f-4875-90e6-f8605e6b7ea7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 68b14388ff3c33458873b494c8d5a770b44f7545
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841784"
---
# <a name="ksetupchangepassword"></a>che Ksetup: ChangePassword



Usa il valore della password Centro distribuzione chiavi (KDC) (kpasswd) per modificare la password dell'utente che ha eseguito l'accesso. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
ksetup /changepassword <OldPasswd> <NewPasswd>
```

#### <a name="parameters"></a>Parametri

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

## <a name="examples"></a><a name=BKMK_Examples></a>Esempi

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

## <a name="additional-references"></a>Altre informazioni di riferimento

-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)