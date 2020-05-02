---
title: 'che Ksetup: Server'
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e3407111-ac92-457f-aa1f-a04fe9109d59
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 91549eb78f825264016ec0e03b7035f79132f260
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724597"
---
# <a name="ksetupserver"></a>che Ksetup: Server



Consente di specificare un nome per un computer che esegue il sistema operativo Windows in modo che le modifiche apportate utilizzando **che ksetup** verrà aggiornato il computer di destinazione.

## <a name="syntax"></a>Sintassi

```
ksetup /server <ServerName>
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<Nomeserver>|Il nome completo del computer in cui la configurazione sarà efficace, ad esempio IPops897.corp.contoso.com.</br>Se un incompleti completo è specificato il nome di computer di dominio, il comando avrà esito negativo.|

## <a name="remarks"></a>Osservazioni

Non è possibile rimuovere il nome del server di destinazione; è possibile modificarlo solo al nome del computer locale, ovvero l'impostazione predefinita.

Il nome del server di destinazione è archiviato nel Registro di sistema **HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\LSA\Kerberos**. Non verrà segnalato tramite **che ksetup**.

## <a name="examples"></a>Esempi

Rendere il **che ksetup** configurazioni effettiva nei computer IPops897 collegato nel dominio Contoso:
```
ksetup /server IPops897.corp.contoso.com
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

-   [Che Ksetup](ksetup.md)
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)