---
title: ChangePassword che Ksetup
description: Argomento di riferimento per il comando che Ksetup ChangePassword, che usa il valore della password Centro distribuzione chiavi (KDC) (kpasswd) per modificare la password dell'utente connesso.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 283078e7-a88f-4875-90e6-f8605e6b7ea7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5c1ed9d9b611a7911c4a22c7ca803b480f52f323
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83817981"
---
# <a name="ksetup-changepassword"></a>ChangePassword che Ksetup

Usa il valore della password Centro distribuzione chiavi (KDC) (kpasswd) per modificare la password dell'utente che ha eseguito l'accesso. L'output del comando segnala lo stato di esito positivo o negativo.

È possibile verificare se **kpasswd** è impostato, eseguendo il `ksetup /dumpstate` comando e visualizzando l'output.


## <a name="syntax"></a>Sintassi

```
ksetup /changepassword <oldpassword> <newpassword>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<oldpassword>` | Specifica la password esistente dell'utente connesso. |
| `<newpassword>` | Specifica la nuova password dell'utente connesso. Questa password deve soddisfare tutti i requisiti per le password impostati nel computer. |

#### <a name="remarks"></a>Osservazioni

- Se l'account utente non viene trovato nel dominio corrente, il sistema chiederà di specificare il nome di dominio in cui risiede l'account utente.

- Se si vuole forzare una modifica della password all'accesso successivo, questo comando consente di usare l'asterisco (*), in modo che all'utente venga richiesto di immettere una nuova password.

-

### <a name="examples"></a>Esempi

Per modificare la password di un utente attualmente connesso al computer in questo dominio, digitare:

```
ksetup /changepassword Pas$w0rd Pa$$w0rd
```

Per modificare la password di un utente attualmente connesso nel dominio contoso, digitare:

```
ksetup /domain CONTOSO /changepassword Pas$w0rd Pa$$w0rd
```

Per forzare l'utente attualmente connesso a modificare la password all'accesso successivo, digitare:

```
ksetup /changepassword Pas$w0rd *
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando che Ksetup](ksetup.md)

- [comando che Ksetup dumpstate](ksetup-dumpstate.md)

- [comando che Ksetup addkpasswd](ksetup-addkpasswd.md)

- [comando che Ksetup delkpasswd](ksetup-delkpasswd.md)

- [comando che Ksetup dumpstate](ksetup-dumpstate.md)
