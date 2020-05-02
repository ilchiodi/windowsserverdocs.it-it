---
title: serverWerOptin
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f3c0b0af-cafb-4f09-8b36-5a357ddf392d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a3acba57aa012c57c5c6109ed948ce6bb5b28078
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721947"
---
# <a name="serverweroptin"></a>serverWerOptin

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente di attivare Segnalazione errori.
## <a name="syntax"></a>Sintassi
```
serverweroptin [/query] [/detailed] [/summary]
```
#### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|/query|Verifica l'impostazione corrente.|
|/ dettagliate|Invia report dettagliati automaticamente.|
|/Summary|Invia automaticamente rapporti brevi.|
|/?|Visualizza la guida al prompt dei comandi.|
## <a name="examples"></a>Esempi
Per verificare l'impostazione corrente, digitare:
```
serverweroptin /query
```
Per inviare automaticamente rapporti dettagliati, digitare:
```
serverweroptin /detailed
```
Per inviare automaticamente report di riepilogo, digitare
```
serverweroptin /summary
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

