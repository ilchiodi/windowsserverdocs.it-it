---
title: getsecurityflags Bitsadmin
description: 'Argomento i comandi di Windows per **bitsadmin getsecurityflags** : segnala i flag di sicurezza HTTP per il reindirizzamento degli URL e controlli eseguiti sul certificato del server durante il trasferimento.'
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c2e73519-34f4-487b-af11-97d5d08ef9bb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 97d889b54c4b0de0230c50e1d7c8d21617ea881a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835852"
---
#<a name="bitsadmin-getsecurityflags"></a>getsecurityflags Bitsadmin

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Flag di protezione report HTTP per il reindirizzamento degli URL e i controlli eseguiti sul certificato del server durante il trasferimento.

## <a name="syntax"></a>Sintassi

```
bitsadmin /GetSecurityFlags <Job> 
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------|--------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="BKMK_examples"></a>Esempi
Nell'esempio seguente recupera i flag securitly da un processo denominato *myJob*.

```
C:\>bitsadmin /GetSecurityFlags myJob 
```

## <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave sintassi della riga di comando](command-line-syntax-key.md)


