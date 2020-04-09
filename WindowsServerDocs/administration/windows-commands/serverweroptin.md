---
title: serverWerOptin
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f3c0b0af-cafb-4f09-8b36-5a357ddf392d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 18b4a56888b3f23bf3bac4a12b2dba7079b50923
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834634"
---
# <a name="serverweroptin"></a>serverWerOptin

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
## <a name="examples"></a><a name=BKMK_Examples></a>Esempi
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
## <a name="additional-references"></a>Altre informazioni di riferimento
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

