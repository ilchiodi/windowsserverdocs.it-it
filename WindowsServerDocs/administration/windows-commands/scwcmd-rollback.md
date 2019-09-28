---
title: Rollback scwcmd
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4fd9f89b-0420-420a-ad20-4a328636b1e7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3f089ea3e6e5d5b95080356dd239272b95a76b37
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371208"
---
# <a name="scwcmd-rollback"></a>Scwcmd: rollback

> Si applica a: Windows Server 2012 R2, Windows Server 2012

Applica i criteri di ripristino più recente disponibili e quindi Elimina il criterio di ripristino.

## <a name="syntax"></a>Sintassi

```
scwcmd rollback /m:<ComputerName> [/u:<UserName>] [/pw:<Password>]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/m: \<ComputerName >|Specifica il nome NetBIOS, un nome DNS o un indirizzo IP di un computer in cui deve essere eseguita l'operazione di rollback.|
|/u: \<UserName >|Specifica un account utente alternativo da utilizzare quando si esegue un rollback remoto. Il valore predefinito è connesso per utente.|
|/PW: > \<Password|Specifica una credenziale utente alternativo da utilizzare quando si esegue un rollback remoto. Il valore predefinito è connesso per utente.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

Scwcmd.exe è disponibile solo nei computer che eseguono Windows Server 2008 R2, Windows Server 2008 o Windows Server 2003.

## <a name="BKMK_Examples"></a>Esempi

Per ripristinare i criteri di sicurezza in un computer a indirizzo IP 172.16.0.0, digitare:
```
scwcmd rollback /m:172.16.0.0
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)