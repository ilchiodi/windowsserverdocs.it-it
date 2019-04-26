---
title: ksetup:removerealm
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 39f0c6f0-4c50-4781-941e-0893495405e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3f62208d6576890529be80b1c6cb3cc073a2b4e6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853362"
---
# <a name="ksetupremoverealm"></a>ksetup:removerealm



Elimina tutte le informazioni per l'area di autenticazione specificato dal Registro di sistema. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
ksetup /removerealm <RealmName>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<RealmName >|Il nome dell'area di autenticazione è specificato come un nome DNS lettere maiuscole, ad esempio CORP. CONTOSO.COM e è elencato come area di autenticazione predefinito quando **che ksetup** viene eseguito.|

## <a name="remarks"></a>Note

Il nome dell'area di autenticazione viene archiviato in due posizioni nel Registro di sistema: **HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001** e **\CurrentControlSet\Control\Lsa\Kerberos**.

Poiché questa operazione ripristina le informazioni DNS e rimuoverlo potrebbe rendere inutilizzabile il controller di dominio, è possibile rimuovere il nome dell'area di autenticazione predefinito dal controller di dominio.

## <a name="BKMK_Examples"></a>Esempi

Erroneamente impostato il nome dell'area di autenticazione dall'errore di ortografia ". COM? nel computer locale per CORP. CONTOSO. CON
```
ksetup /setrealm CORP.CONTOSO.CON
```
Rimuovere tale nome errato dell'area di autenticazione dal computer locale:
```
ksetup /removerealm CORP.CONTOSO.CON
```
Verificare la rimozione eseguendo **che ksetup** ed esaminare l'output.

#### <a name="additional-references"></a>Altri riferimenti

-   [Ksetup](ksetup.md)
-   [Ksetup:setrealm](ksetup-setrealm.md)
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)