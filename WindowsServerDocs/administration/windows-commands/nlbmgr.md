---
title: nlbmgr
description: Argomento di riferimento per il comando Nlbmgr, che consente di configurare e gestire i cluster di bilanciamento carico di rete e tutti gli host del cluster da un singolo computer, utilizzando Gestione bilanciamento carico di rete.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 89cb8590-b7cf-4a27-89fa-0fa62ea1a1ca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2eb7f802944260f7274e7ace30b7b55e9c4b27ad
ms.sourcegitcommit: 99d548141428c964facf666c10b6709d80fbb215
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/12/2020
ms.locfileid: "84721504"
---
# <a name="nlbmgr"></a>nlbmgr

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configurare e gestire i cluster di bilanciamento carico di rete e tutti gli host del cluster da un singolo computer, utilizzando Gestione bilanciamento carico di rete. È anche possibile usare questo comando per replicare la configurazione del cluster in altri host.

È possibile avviare Gestione bilanciamento carico di rete dalla riga di comando usando il comando **nlbmgr.exe**, che viene installato nella cartella **systemroot\System32**

## <a name="syntax"></a>Sintassi

```
nlbmgr [/noping][/hostlist <filename>][/autorefresh <interval>][/help | /?]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| /noping | Impedisce a Gestione bilanciamento carico di rete di effettuare il ping degli host prima di provare a contattarli tramite Strumentazione gestione Windows (WMI). Utilizzare questa opzione se si è disabilitato Internet Control Message Protocol (ICMP) in tutte le schede di rete disponibili. Se Gestione bilanciamento carico di rete tenta di contattare un host che non è disponibile, si verificherà un ritardo durante l'uso di questa opzione. |
| /hostlist`<filename>` | Carica gli host specificati in filename in Gestione bilanciamento carico di rete. |
| /AutoRefresh`<interval>` | Consente a Gestione bilanciamento carico di rete di aggiornare le informazioni relative a host e cluster ogni `<interval>` secondo. Se non viene specificato alcun intervallo, le informazioni vengono aggiornate ogni 60 secondi. |
| /? | Visualizza la guida al prompt dei comandi. |
| /help | Visualizza la guida al prompt dei comandi. |

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Guida di riferimento ai cmdlet di NetworkLoadBalancingClusters](https://docs.microsoft.com/powershell/module/networkloadbalancingclusters)
