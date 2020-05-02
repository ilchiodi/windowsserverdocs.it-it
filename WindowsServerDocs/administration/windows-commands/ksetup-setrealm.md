---
title: 'che Ksetup: Unreal'
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ab268c40-276b-46ef-ab16-d5ce7667fbed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 453977ac39dd3a52b4f5a3104995f944e4a48392
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724551"
---
# <a name="ksetupsetrealm"></a>che Ksetup: Unreal



Imposta il nome di un'area di autenticazione Kerberos.

## <a name="syntax"></a>Sintassi

```
ksetup /setrealm <DNSDomainName>
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<> nomedominiodns|Il nome di dominio DNS può essere nel formato di un nome di dominio completo o di un nome di dominio semplice.|

## <a name="remarks"></a>Osservazioni

Il parametro del nome di dominio DNS deve essere immesso in lettere maiuscole. In caso contrario, il comando **che Ksetup** richiederà la verifica per continuare.

L'impostazione dell'area di autenticazione Kerberos in un controller di dominio non è supportata. Se si tenta di eseguire questa operazione, verrà generato un avviso e un errore di comando.

## <a name="examples"></a>Esempi

Impostare l'area di autenticazione per questo computer su un nome di dominio specifico per limitare l'accesso da parte di un controller non di dominio solo all'area di autenticazione Kerberos di CONTOSO:
```
ksetup /setrealm CONTOSO
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [Che Ksetup](ksetup.md)
-   [Ksetup:removerealm](ksetup-removerealm.md)