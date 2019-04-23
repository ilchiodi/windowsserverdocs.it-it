---
title: tzutil
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bcf6e007-c9b6-4df5-83c5-ed7b4b1b5913
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 41a46ea7974b67cc557973484428480e7beb5484
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876802"
---
# <a name="tzutil"></a>tzutil

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente di visualizzare il Windows ora zona utilità. 
## <a name="syntax"></a>Sintassi
```
tzutil [/?] [/g] [/s <timeZoneID>[_dstoff]] [/l]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|/?|Visualizza la guida al prompt dei comandi.|
|/g|Visualizza l'ID fuso orario corrente.|
|/s \<timeZoneID>[_dstoff]|Imposta il fuso orario corrente usando l'ID fuso orario specificato. Il **_dstoff** suffisso disabilita le regolazioni dell'ora legale per il fuso orario (dove applicabile).|
|/l|gli elenchi di tutto il tempo valido ID della zona e i nomi visualizzati. L'output sarà:<br /><br />-   \<nome visualizzato ><br />-   \<ID fuso orario >|

## <a name="remarks"></a>Note
Un codice di uscita **0** indica il comando completato correttamente.

## <a name="BKMK_Examples"></a>Esempi
Per visualizzare l'ID fuso orario corrente, digitare:
```
tzutil /g
```
Per impostare il fuso orario ora solare Pacifico, digitare:
```
tzutil /s Pacific Standard time
```
Per impostare il fuso orario corrente per ora solare Pacifico e disabilita le regolazioni dell'ora legale, digitare:
```
tzutil /s Pacific Standard time_dstoff
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)

