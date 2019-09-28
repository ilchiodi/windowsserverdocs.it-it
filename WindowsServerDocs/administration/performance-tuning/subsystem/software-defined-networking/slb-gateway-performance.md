---
title: Ottimizzazione delle prestazioni del gateway SLB in reti definite da software
description: Linee guida per l'ottimizzazione delle prestazioni del gateway SLB in reti SDN
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: grcusanz; AnPaul
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 9a0d239da2ca321333ec757db22bbaf9a9b8ba30
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383464"
---
# <a name="slb-gateway-performance-tuning-in-software-defined-networks"></a>Ottimizzazione delle prestazioni del gateway SLB in reti definite da software

Il bilanciamento del carico software è fornito da una combinazione di una gestione del servizio di bilanciamento del carico nelle macchine virtuali del controller di rete, il Commuter virtuale Hyper-V e un set di macchine virtuali Load Balancer Multixplexor (MUX).

Non è necessaria alcuna ottimizzazione delle prestazioni aggiuntiva per configurare il controller di rete o l'host Hyper-V per il bilanciamento del carico oltre a quanto descritto nella sezione [Software Defined Networking](index.md) , a meno che non si usi SR-IOV per Mux, come descritto di seguito.

## <a name="slb-mux-vm-configuration"></a>Configurazione della macchina virtuale Mux SLB

Le macchine virtuali SLB Mux vengono distribuite in una configurazione Active-Active.  Ciò significa che ogni macchina virtuale Mux distribuita e aggiunta al controller di rete è in grado di elaborare le richieste in ingresso.  Quindi, la velocità effettiva complessiva aggregata di tutte le connessioni è limitata solo dal numero di macchine virtuali Mux distribuite.  

Una singola connessione a un indirizzo IP virtuale (VIP) verrà sempre inviata allo stesso Mux, supponendo che il numero di MUX rimanga costante e, di conseguenza, la velocità effettiva sarà limitata alla velocità effettiva di una singola macchina virtuale MUX.  MUX elabora solo il traffico in ingresso destinato a un indirizzo VIP.  I pacchetti di risposta passano direttamente dalla macchina virtuale che invia la risposta al Commuter fisico che lo inoltra al client.

In alcuni casi, quando l'origine della richiesta ha origine da un host SDN aggiunto allo stesso controller di rete che gestisce il VIP, viene anche eseguita un'ulteriore ottimizzazione del percorso in ingresso per la richiesta, che consente la corsa della maggior parte dei pacchetti direttamente dalla dal client al server, ignorando completamente la macchina virtuale MUX.  Per eseguire questa ottimizzazione non è necessaria alcuna configurazione aggiuntiva.

Ogni VM Mux di SLB deve essere ridimensionata in base alle linee guida fornite nella sezione requisiti del ruolo macchina virtuale dell'infrastruttura SDN dell'argomento [pianificare un'infrastruttura software defined Network](../../../../networking/sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md) .

## <a name="single-root-io-virtualization-sr-iov"></a>Single root IO Virtualization (SR-IOV)

Quando si usa 40Gbit Ethernet, la possibilità per il commutire virtuale di elaborare i pacchetti per la VM Mux diventa il fattore di limitazione per la velocità effettiva della macchina virtuale MUX.  Per questo motivo è consigliabile abilitare SR-IOV sulla scheda di rete VM della VM SLB per assicurarsi che il Commuter virtuale non sia il collo di bottiglia.

Per abilitare SR-IOV, è necessario abilitarlo nel commutire virtuale quando viene creato il Commuter virtuale.  In questo esempio si sta creando un Commuter virtuale con switch Embedded Teaming (SET) e SR-IOV:
``` syntax
    new-vmswitch -Name SDNSwitch -EnableEmbeddedTeaming $true -NetAdapterName @("NIC1", "NIC2") -EnableIOV $true
```
Deve quindi essere abilitata nelle schede di rete virtuali della macchina virtuale Mux SLB che elabora il traffico dei dati.  In questo esempio SR-IOV viene abilitato in tutte le schede:
``` syntax
    get-vmnetworkadapter -VMName SLBMUX1 | set-vmnetworkadapter -IovWeight 50
```
