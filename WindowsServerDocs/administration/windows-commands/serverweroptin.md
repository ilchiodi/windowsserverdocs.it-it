---
title: serverWerOptin
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: a7d5791e059d31e416f848f6e8df648c8f9bd27d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371024"
---
# <a name="serverweroptin"></a>serverWerOptin

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente di attivare Segnalazione errori.
## <a name="syntax"></a>Sintassi
```
serverweroptin [/query] [/detailed] [/summary]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|/query|Verifica l'impostazione corrente.|
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
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

