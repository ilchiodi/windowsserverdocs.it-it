---
title: nlbmgr
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 89cb8590-b7cf-4a27-89fa-0fa62ea1a1ca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a055eb30984bd1832d84bce40607eb0722b175b2
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820851"
---
# <a name="nlbmgr"></a>nlbmgr

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Utilizzando Gestione bilanciamento carico di rete, è possibile configurare e gestire i cluster di bilanciamento carico di rete e tutti gli host del cluster da un singolo computer ed è inoltre possibile replicare la configurazione del cluster in altri host. È possibile avviare Gestione bilanciamento carico di rete dalla riga di comando usando il comando **Nlbmgr. exe**, che viene installato nella cartella **systemroot\System32** .
## <a name="syntax"></a>Sintassi
```
nlbmgr [/help] [/noping] [/hostlist <filename>] [/autorefresh <interval>]
```
#### <a name="parameters"></a>Parametri

|        Parametro        |                                                                                                                                                                                                Descrizione                                                                                                                                                                                                |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|          /help          |                                                                                                                                                                                   Visualizza la guida al prompt dei comandi.                                                                                                                                                                                    |
|         /noping         | Impedisce a Gestione bilanciamento carico di rete di effettuare il ping degli host prima di provare a contattarli tramite Strumentazione gestione Windows (WMI). Utilizzare questa opzione se si è disabilitato Internet Control Message Protocol (ICMP) in tutte le schede di rete disponibili. Se Gestione bilanciamento carico di rete tenta di contattare un host non disponibile, si verificherà un ritardo durante l'uso di questa opzione. |
|  /hostlist<filename>   |                                                                                                                                                                Carica gli host specificati in filename in Gestione bilanciamento carico di rete.                                                                                                                                                                 |
| /AutoRefresh<interval> |                                                                                                          Consente a Gestione bilanciamento carico di rete di aggiornare le informazioni relative a host e cluster ogni <interval> secondo. Se non viene specificato alcun intervallo, le informazioni vengono aggiornate ogni 60 secondi.                                                                                                          |
|           /?            |                                                                                                                                                                                   Visualizza la guida al prompt dei comandi.                                                                                                                                                                                    |

## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

