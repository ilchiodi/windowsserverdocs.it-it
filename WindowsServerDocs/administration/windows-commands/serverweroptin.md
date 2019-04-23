---
title: serverweroptin
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f3c0b0af-cafb-4f09-8b36-5a357ddf392d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 29545be99b14042d16a6f3a4118e0746f18b14ab
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869642"
---
# <a name="serverweroptin"></a>serverweroptin

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente di attivare Segnalazione errori.
## <a name="syntax"></a>Sintassi
```
serverweroptin [/query] [/detailed] [/summary]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|/query|verifica l'impostazione corrente.|
|/ dettagliate|Invia report dettagliati automaticamente.|
|/Summary|Invia automaticamente rapporti brevi.|
|/?|Visualizza la guida al prompt dei comandi.|
## <a name="BKMK_Examples"></a>Esempi
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
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)

