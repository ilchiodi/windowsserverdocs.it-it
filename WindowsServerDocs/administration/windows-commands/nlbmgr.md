---
title: nlbmgr
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 89cb8590-b7cf-4a27-89fa-0fa62ea1a1ca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 757b218ad3a88cc10c4d1bcfed15a83bfd34cc74
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437031"
---
# <a name="nlbmgr"></a>nlbmgr

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Utilizzando Gestione bilanciamento carico di rete, è possibile configurare e gestire i cluster di bilanciamento del carico di rete e tutti i cluster host da un singolo computer, ed è possibile replicare anche la configurazione del cluster in altri host. È possibile avviare Gestione bilanciamento carico di rete dalla riga di comando utilizzando il comando **nlbmgr.exe**, in cui è installato il **systemroot\System32** cartella.
## <a name="syntax"></a>Sintassi
```
nlbmgr [/help] [/noping] [/hostlist <filename>] [/autorefresh <interval>]
```
### <a name="parameters"></a>Parametri

|        Parametro        |                                                                                                                                                                                                Descrizione                                                                                                                                                                                                |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|          /help          |                                                                                                                                                                                   Visualizza la guida al prompt dei comandi.                                                                                                                                                                                    |
|         /noping         | Impedisce il ping gli host prima di tentare di contattarli tramite Strumentazione gestione Windows (WMI) di gestione del bilanciamento del carico di rete. Usare questa opzione se è stata disabilitata messaggio protocollo ICMP (Internet Control) in tutte le schede di rete disponibile. Se Gestione bilanciamento carico di rete tenta di contattare un host che non è disponibile, si verificherà un ritardo quando si usa questa opzione. |
|  /hostlist <filename>   |                                                                                                                                                                Carica gli host specificati nel nome del file in Gestione bilanciamento carico di rete.                                                                                                                                                                 |
| /autorefresh <interval> |                                                                                                          Fa sì che Gestione bilanciamento carico rete aggiornare le informazioni di host e cluster ogni <interval> secondi. Se non viene specificato alcun intervallo, le informazioni vengono aggiornate ogni 60 secondi.                                                                                                          |
|           /?            |                                                                                                                                                                                   Visualizza la guida al prompt dei comandi.                                                                                                                                                                                    |

## <a name="additional-references"></a>Riferimenti aggiuntivi
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

