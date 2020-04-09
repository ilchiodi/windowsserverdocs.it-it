---
title: nlbmgr
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 89cb8590-b7cf-4a27-89fa-0fa62ea1a1ca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dab1a2eff2c0c2e039e67e9c3271a967b802ac7c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838874"
---
# <a name="nlbmgr"></a>nlbmgr

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Utilizzando Gestione bilanciamento carico di rete, è possibile configurare e gestire i cluster di bilanciamento carico di rete e tutti gli host del cluster da un singolo computer ed è inoltre possibile replicare la configurazione del cluster in altri host. È possibile avviare Gestione bilanciamento carico di rete dalla riga di comando usando il comando **Nlbmgr. exe**, che viene installato nella cartella **systemroot\System32** .
## <a name="syntax"></a>Sintassi
```
nlbmgr [/help] [/noping] [/hostlist <filename>] [/autorefresh <interval>]
```
#### <a name="parameters"></a>Parametri

|        Parametro        |                                                                                                                                                                                                Descrizione                                                                                                                                                                                                |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|          /Help          |                                                                                                                                                                                   Visualizza la guida al prompt dei comandi.                                                                                                                                                                                    |
|         /noping         | Impedisce a Gestione bilanciamento carico di rete di effettuare il ping degli host prima di provare a contattarli tramite Strumentazione gestione Windows (WMI). Utilizzare questa opzione se si è disabilitato Internet Control Message Protocol (ICMP) in tutte le schede di rete disponibili. Se Gestione bilanciamento carico di rete tenta di contattare un host non disponibile, si verificherà un ritardo durante l'uso di questa opzione. |
|  <filename>/hostlist   |                                                                                                                                                                Carica gli host specificati in filename in Gestione bilanciamento carico di rete.                                                                                                                                                                 |
| <interval>/AutoRefresh |                                                                                                          Consente a Gestione bilanciamento carico di rete di aggiornare le informazioni relative a host e cluster ogni <interval> secondi. Se non viene specificato alcun intervallo, le informazioni vengono aggiornate ogni 60 secondi.                                                                                                          |
|           /?            |                                                                                                                                                                                   Visualizza la guida al prompt dei comandi.                                                                                                                                                                                    |

## <a name="additional-references"></a>Altre informazioni di riferimento
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

