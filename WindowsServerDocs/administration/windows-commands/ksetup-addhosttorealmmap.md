---
title: 'che Ksetup: addhosttorealmmap'
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: e6a28c6001707fac245de7136b5fb5bd38495027
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375654"
---
# <a name="ksetupaddhosttorealmmap"></a>che Ksetup: addhosttorealmmap



Aggiunge un mapping del nome dell'entità servizio (SPN) tra l'host dichiarato e l'area di autenticazione. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
ksetup /addhosttorealmmap <HostName> <RealmName>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<HostName >|Il nome host è il nome del computer e può essere dichiarato come nome di dominio completo del computer.|
|\<RealmName >|Il nome dell'area di autenticazione è specificato come un nome DNS lettere maiuscole, ad esempio CORP. CONTOSO.COM.|

## <a name="remarks"></a>Note

Questo comando consente di eseguire il mapping di un host o di più host che condividono lo stesso suffisso DNS per l'area di autenticazione.

Il mapping viene registrato nel registro di sistema in **HKEY_LOCAL_MACHINE\SYSTEM\CurrentContolSet\Lsa\Kerberos\HostToRealm**.

## <a name="BKMK_Examples"></a>Esempi

Come parte della configurazione dell'area di autenticazione CONTOSO, mappare il computer host IPops897 all'area di autenticazione:
```
ksetup /addhosttorealmmap IPops897 CONTOSO
```
Verificare nel registro di sistema che il mapping sia quello desiderato.

#### <a name="additional-references"></a>Altri riferimenti

-   [Ksetup:delhosttorealmmap](ksetup-delhosttorealmmap.md)
-   [Che Ksetup](ksetup.md)
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)