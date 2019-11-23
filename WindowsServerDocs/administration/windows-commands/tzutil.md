---
title: tzutil
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 347254bd5a00a8bfb4a80f20d518f1e0e8b593bf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392310"
---
# <a name="tzutil"></a>tzutil

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza l'utilità fuso orario di Windows. 
## <a name="syntax"></a>Sintassi
```
tzutil [/?] [/g] [/s <timeZoneID>[_dstoff]] [/l]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|/?|Visualizza la guida al prompt dei comandi.|
|/g|Visualizza l'ID del fuso orario corrente.|
|/s \<timeZoneID > [_dstoff]|Imposta il fuso orario corrente usando l'ID del fuso orario specificato. Il suffisso **_dstoff** Disabilita le regolazioni dell'ora legale per il fuso orario (se applicabile).|
|/l|Elenca tutti gli ID di fuso orario e i nomi visualizzati validi. L'output sarà:<br /><br />-   \<nome visualizzato ><br />-   \<ID fuso orario >|

## <a name="remarks"></a>Osservazioni
Un codice di uscita pari a **0** indica che il comando è stato completato correttamente.

## <a name="BKMK_Examples"></a>Esempi
Per visualizzare l'ID del fuso orario corrente, digitare:
```
tzutil /g
```
Per impostare il fuso orario corrente sull'ora solare Pacifico, digitare:
```
tzutil /s Pacific Standard time
```
Per impostare il fuso orario corrente sull'ora solare Pacifico e disabilitare le regolazioni dell'ora legale, digitare:
```
tzutil /s Pacific Standard time_dstoff
```
## <a name="additional-references"></a>riferimenti aggiuntivi
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

