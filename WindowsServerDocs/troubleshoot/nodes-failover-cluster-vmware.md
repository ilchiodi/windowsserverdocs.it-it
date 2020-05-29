---
title: Rimuovere il nodo dall'appartenenza al cluster di failover attivo
description: Questo articolo illustra il problema relativo alla ricerca di nodi rimossi dall'appartenenza al cluster di failover attivo.
ms.prod: windows-server
ms.technology: server-general
ms.date: 05/28/2020
author: Deland-Han
ms.author: delhan
ms.openlocfilehash: 6613bf09c3588637cfe03cb7647e4fed358b760c
ms.sourcegitcommit: ef089864980a1d4793a35cbf4cbdd02ce1962054
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/28/2020
ms.locfileid: "84150329"
---
# <a name="nodes-being-removed-from-failover-cluster-membership-on-vmware-esx"></a>Nodi rimossi dall'appartenenza al cluster di failover in VMWare ESX

Questo articolo illustra il problema relativo alla ricerca di nodi rimossi dall'appartenenza al cluster di failover attivo quando i nodi sono ospitati in VMWare ESX.

## <a name="symptom"></a>Sintomo

Quando si verifica il problema, nel registro eventi di sistema viene visualizzato il Visualizzatore eventi:

![Evento 1135](media/nodes-failover-cluster-vmware/1135.png)

## <a name="resolution"></a>Soluzione

Un problema specifico riguarda gli adapter VMXNET3 che eliminano i pacchetti di rete in ingresso perché il buffer in ingresso è impostato su un livello troppo basso per gestire grandi quantità di traffico. È possibile scoprire facilmente se si tratta di un problema usando Performance Monitor per esaminare il contatore "Interface\Packets di rete ricevuti rimossi".

![Aggiungi contatori](media/nodes-failover-cluster-vmware/0527.png)

Una volta aggiunto questo contatore, esaminare i numeri media, minimo e massimo e se si tratta di un valore maggiore di zero, il buffer di ricezione deve essere regolato per l'adapter. Questo problema è documentato nella Knowledge base di VMWare: [perdita di pacchetti di grandi dimensioni a livello di sistema operativo guest in vmxnet3 vNIC in ESXi 5. x/4. x](https://kb.vmware.com/s/article/2039495).
