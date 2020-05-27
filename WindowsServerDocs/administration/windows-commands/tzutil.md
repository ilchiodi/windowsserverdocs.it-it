---
title: tzutil
description: Argomento di riferimento per tzutil, che Visualizza l'utilità fuso orario di Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bcf6e007-c9b6-4df5-83c5-ed7b4b1b5913
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5d4a94fa9f776af1ae21ad9057b6f9225691b516
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83821301"
---
# <a name="tzutil"></a>tzutil

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza l'utilità fuso orario di Windows.

## <a name="syntax"></a>Sintassi
```
tzutil [/?] [/g] [/s <timeZoneID>[_dstoff]] [/l]
```
#### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|/?|Visualizza la guida al prompt dei comandi.|
|/g|Visualizza l'ID del fuso orario corrente.|
|/s \< timeZoneID> [_dstoff]|Imposta il fuso orario corrente usando l'ID del fuso orario specificato. Il suffisso **_dstoff** Disabilita le regolazioni dell'ora legale per il fuso orario (se applicabile).|
|/l|Elenca tutti gli ID di fuso orario e i nomi visualizzati validi. L'output sarà:<p>-   \<nome visualizzato><br />-   \<ID fuso orario>|

## <a name="remarks"></a>Osservazioni
Un codice di uscita pari a **0** indica che il comando è stato completato correttamente.

## <a name="examples"></a>Esempi
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
## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

