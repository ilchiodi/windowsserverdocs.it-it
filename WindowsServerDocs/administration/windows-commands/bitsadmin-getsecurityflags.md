---
title: getsecurityflags Bitsadmin
description: "Argomento dei comandi di Windows per **BITSAdmin getsecurityflags** : segnala i flag di sicurezza http per il reindirizzamento dell'URL e i controlli eseguiti sul certificato del server durante il trasferimento."
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: fb53664a6366b411ae1eb9b0fe7c93392d60b542
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381462"
---
# <a name="bitsadmin-getsecurityflags"></a>getsecurityflags Bitsadmin

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
[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)


