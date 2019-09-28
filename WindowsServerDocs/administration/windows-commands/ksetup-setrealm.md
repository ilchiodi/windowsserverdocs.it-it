---
title: 'che Ksetup: Unreal'
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ab268c40-276b-46ef-ab16-d5ce7667fbed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1bbe5c000b7e84066c19511639fe3d92d7e4b558
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374903"
---
# <a name="ksetupsetrealm"></a>che Ksetup: Unreal



Imposta il nome di un'area di autenticazione Kerberos. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
ksetup /setrealm <DNSDomainName>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<DNSDomainName >|Il nome di dominio DNS può essere nel formato di un nome di dominio completo o di un nome di dominio semplice.|

## <a name="remarks"></a>Note

Il parametro del nome di dominio DNS deve essere immesso in lettere maiuscole. In caso contrario, il comando **che Ksetup** richiederà la verifica per continuare.

L'impostazione dell'area di autenticazione Kerberos in un controller di dominio non è supportata. Se si tenta di eseguire questa operazione, verrà generato un avviso e un errore di comando.

## <a name="BKMK_Examples"></a>Esempi

Impostare l'area di autenticazione per questo computer su un nome di dominio specifico per limitare l'accesso da parte di un controller non di dominio solo all'area di autenticazione Kerberos di CONTOSO:
```
ksetup /setrealm CONTOSO
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [Che Ksetup](ksetup.md)
-   [Ksetup:removerealm](ksetup-removerealm.md)