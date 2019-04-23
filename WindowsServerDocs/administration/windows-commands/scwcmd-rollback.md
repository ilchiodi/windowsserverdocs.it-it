---
title: Eseguire il rollback scwcmd
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6d6cd79c7068d86915141a37b5a4510bddefc94c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852202"
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
|/m:\<ComputerName>|Specifica il nome NetBIOS, un nome DNS o un indirizzo IP di un computer in cui deve essere eseguita l'operazione di rollback.|
|/u:\<UserName >|Specifica un account utente alternativo da utilizzare quando si esegue un rollback remoto. Il valore predefinito è connesso per utente.|
|/pw:\<Password>|Specifica una credenziale utente alternativo da utilizzare quando si esegue un rollback remoto. Il valore predefinito è connesso per utente.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

Scwcmd.exe è disponibile solo nei computer che eseguono Windows Server 2008 R2, Windows Server 2008 o Windows Server 2003.

## <a name="BKMK_Examples"></a>Esempi

Per ripristinare i criteri di sicurezza in un computer a indirizzo IP 172.16.0.0, digitare:
```
scwcmd rollback /m:172.16.0.0
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)