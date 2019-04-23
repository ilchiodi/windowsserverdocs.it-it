---
title: ksetup:setrealm
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: aa6b2a21904ec4dae1e60def5bd36647291b1af6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877402"
---
# <a name="ksetupsetrealm"></a>ksetup:setrealm



Imposta il nome di un'area di autenticazione Kerberos. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
ksetup /setrealm <DNSDomainName>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<DNSDomainName>|Il nome di dominio DNS può essere sotto forma di un nome di dominio completo o un nome di dominio semplice.|

## <a name="remarks"></a>Note

Il parametro di nome di dominio DNS deve essere immesso in lettere maiuscole. In caso contrario, il **che ksetup** comando verrà richiesto per la verifica continuare.

Impostare l'area di autenticazione Kerberos in un controller di dominio non è supportato. Tentativo di eseguire questa operazione genererà un avviso e un errore del comando.

## <a name="BKMK_Examples"></a>Esempi

Imposta l'area di autenticazione del computer a un nome di dominio specifico per limitare l'accesso da un controller non di dominio per l'area di autenticazione Kerberos di CONTOSO:
```
ksetup /setrealm CONTOSO
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)
-   [Ksetup](ksetup.md)
-   [Ksetup:removerealm](ksetup-removerealm.md)