---
title: 'che Ksetup: Server'
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e3407111-ac92-457f-aa1f-a04fe9109d59
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dd05fd294640c63e633b7b866307197ae6770476
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374965"
---
# <a name="ksetupserver"></a>che Ksetup: Server



Consente di specificare un nome per un computer che esegue il sistema operativo Windows in modo che le modifiche apportate utilizzando **che ksetup** verrà aggiornato il computer di destinazione. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
ksetup /server <ServerName>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<ServerName >|Il nome completo del computer in cui la configurazione sarà efficace, ad esempio IPops897.corp.contoso.com.</br>Se un incompleti completo è specificato il nome di computer di dominio, il comando avrà esito negativo.|

## <a name="remarks"></a>Note

Non è possibile rimuovere il nome del server di destinazione; è possibile modificarlo solo al nome del computer locale, ovvero l'impostazione predefinita.

Il nome del server di destinazione è archiviato nel Registro di sistema **HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\LSA\Kerberos**. Non verrà segnalato tramite **che ksetup**.

## <a name="BKMK_Examples"></a>Esempi

Rendere il **che ksetup** configurazioni effettiva nei computer IPops897 collegato nel dominio Contoso:
```
ksetup /server IPops897.corp.contoso.com
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Che Ksetup](ksetup.md)
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)