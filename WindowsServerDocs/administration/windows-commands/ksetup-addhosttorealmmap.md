---
title: ksetup:addhosttorealmmap
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 237742d5-fa68-466c-b97e-636f489248ea
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 25cf258309c94f0efde980018dd5dcf3c7df4d60
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837502"
---
# <a name="ksetupaddhosttorealmmap"></a>ksetup:addhosttorealmmap



Aggiunge un mapping del nome dell'entità (SPN) del servizio tra l'host dichiarato e l'area di autenticazione. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
ksetup /addhosttorealmmap <HostName> <RealmName>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<HostName>|Il nome host è il nome del computer e può essere dichiarata come nome di dominio completo del computer.|
|\<RealmName >|Il nome dell'area di autenticazione è specificato come un nome DNS lettere maiuscole, ad esempio CORP. CONTOSO.COM.|

## <a name="remarks"></a>Note

Questo comando consente di eseguire il mapping di un host o più host che condividono lo stesso suffisso DNS per l'area di autenticazione.

Il mapping viene registrato nel Registro di sistema **HKEY_LOCAL_MACHINE\SYSTEM\CurrentContolSet\Lsa\Kerberos\HostToRealm**.

## <a name="BKMK_Examples"></a>Esempi

Nell'ambito della configurazione dell'area di autenticazione CONTOSO, eseguire il mapping del computer host IPops897 all'area di autenticazione:
```
ksetup /addhosttorealmmap IPops897 CONTOSO
```
Verificare nel Registro di sistema che il mapping sia come previsto.

#### <a name="additional-references"></a>Altri riferimenti

-   [Ksetup:delhosttorealmmap](ksetup-delhosttorealmmap.md)
-   [Ksetup](ksetup.md)
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)