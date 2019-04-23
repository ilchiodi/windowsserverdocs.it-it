---
title: ksetup:delhosttorealmmap
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3faee482-a96c-4614-86fd-aaa446643ec4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cf01edc4932fd5ec1cf98043de04286b3a100a34
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882342"
---
# <a name="ksetupdelhosttorealmmap"></a>ksetup:delhosttorealmmap



Rimuove un mapping del nome dell'entità (SPN) del servizio tra l'host dichiarato e l'area di autenticazione. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
ksetup /delhosttorealmmap <HostName> <RealmName>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<HostName>|Il nome host è il nome del computer e può essere dichiarata come nome di dominio completo del computer.|
|\<RealmName >|Il nome dell'area di autenticazione è specificato come un nome DNS lettere maiuscole, ad esempio CORP. CONTOSO.COM.|

## <a name="remarks"></a>Note

In presenza di un host dell'area di autenticazione (o più host all'area di autenticazione) mapping, questo comando rimuove tale mapping.

Il mapping viene registrato nel Registro di sistema **HKEY_LOCAL_MACHINE\SYSTEM\CurrentContolSet\Lsa\Kerberos\HostToRealm**. È necessario verificare il mapping nel Registro di sistema dopo l'uso di questo comando.

## <a name="BKMK_Examples"></a>Esempi

Modifica della configurazione dell'area di autenticazione CONTOSO, eliminare il mapping del computer host IPops897 all'area di autenticazione:
```
ksetup /delhosttorealmmap IPops897 CONTOSO
```
Dopo aver eseguito questo comando, si possono verificare nel Registro di sistema che il mapping sia come previsto.

#### <a name="additional-references"></a>Altri riferimenti

-   [Ksetup:addhosttorealmmap](ksetup-addhosttorealmmap.md)
-   [Ksetup](ksetup.md)
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)