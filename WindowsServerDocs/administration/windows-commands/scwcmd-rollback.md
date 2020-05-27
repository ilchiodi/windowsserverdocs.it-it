---
title: Rollback scwcmd
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4fd9f89b-0420-420a-ad20-4a328636b1e7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 288c5bb14602e895648cfdc1535b734a823b7233
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820961"
---
# <a name="scwcmd-rollback"></a>Scwcmd: rollback

> Si applica a: Windows Server 2012 R2, Windows Server 2012

Applica i criteri di ripristino più recente disponibili e quindi Elimina il criterio di ripristino.

## <a name="syntax"></a>Sintassi

```
scwcmd rollback /m:<ComputerName> [/u:<UserName>] [/pw:<Password>]
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/m: \< nomecomputer>|Specifica il nome NetBIOS, un nome DNS o un indirizzo IP di un computer in cui deve essere eseguita l'operazione di rollback.|
|/u: \< nome utente>|Specifica un account utente alternativo da utilizzare quando si esegue un rollback remoto. Il valore predefinito è connesso per utente.|
|/PW: \< Password>|Specifica una credenziale utente alternativo da utilizzare quando si esegue un rollback remoto. Il valore predefinito è connesso per utente.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni

Scwcmd.exe è disponibile solo nei computer che eseguono Windows Server 2008 R2, Windows Server 2008 o Windows Server 2003.

## <a name="examples"></a>Esempi

Per ripristinare i criteri di sicurezza in un computer a indirizzo IP 172.16.0.0, digitare:
```
scwcmd rollback /m:172.16.0.0
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)