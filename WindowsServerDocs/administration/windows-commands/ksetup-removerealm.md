---
title: 'che Ksetup: removerealm'
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 39f0c6f0-4c50-4781-941e-0893495405e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bb7bf4663594a6c164d6495a9ba4cd81942afb79
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724600"
---
# <a name="ksetupremoverealm"></a>che Ksetup: removerealm



Elimina tutte le informazioni per l'area di autenticazione specificato dal Registro di sistema.

## <a name="syntax"></a>Sintassi

```
ksetup /removerealm <RealmName>
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<RealmName>|Il nome dell'area di autenticazione è specificato come un nome DNS lettere maiuscole, ad esempio CORP. CONTOSO.COM e è elencato come area di autenticazione predefinito quando **che ksetup** viene eseguito.|

## <a name="remarks"></a>Osservazioni

Il nome dell'area di autenticazione viene archiviato in due posizioni nel Registro di sistema: **HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001** e **\CurrentControlSet\Control\Lsa\Kerberos**.

Poiché questa operazione ripristina le informazioni DNS e rimuoverlo potrebbe rendere inutilizzabile il controller di dominio, è possibile rimuovere il nome dell'area di autenticazione predefinito dal controller di dominio.

## <a name="examples"></a>Esempi

Impostare erroneamente il nome dell'area di autenticazione in base a un errore di ortografia. COM nel computer locale a CORP. CONTOSO. CON
```
ksetup /setrealm CORP.CONTOSO.CON
```
Rimuovere tale nome errato dell'area di autenticazione dal computer locale:
```
ksetup /removerealm CORP.CONTOSO.CON
```
Verificare la rimozione eseguendo **che ksetup** ed esaminare l'output.

## <a name="additional-references"></a>Riferimenti aggiuntivi

-   [Che Ksetup](ksetup.md)
-   [Ksetup:setrealm](ksetup-setrealm.md)
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)