---
title: SLB Gateway ottimizzazione delle prestazioni nel Software definivano reti
description: Linee guida sulle reti SDN l'ottimizzazione delle prestazioni di Gateway di bilanciamento del carico software
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: grcusanz; AnPaul
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: fede7d404ddbb4f465eff435cc340db1907ce9d2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829932"
---
# <a name="slb-gateway-performance-tuning-in-software-defined-networks"></a>SLB Gateway ottimizzazione delle prestazioni nel Software definivano reti

Il bilanciamento del carico software viene fornito da una combinazione di una gestione del servizio di bilanciamento di carico in macchine virtuali del Controller di rete, il commutatore virtuale Hyper-V e un set di macchine virtuali di carico bilanciamento Multixplexor (Mux).

Alcuna ottimizzazione aggiuntiva delle prestazioni non è necessario configurare il Controller di rete o l'host Hyper-V per il bilanciamento del carico oltre ciò che è descritto nel [Software Defined Networking](index.md) sezione, a meno che non si intende utilizzare SR-IOV per il MUX come descritto di seguito.

## <a name="slb-mux-vm-configuration"></a>Configurazione della macchina virtuale Mux SLB

Le macchine virtuali Mux di bilanciamento del carico software vengono distribuite in una configurazione attivo / attivo.  Ciò significa che ogni VM Mux distribuita e aggiunto al Controller di rete possa elaborare le richieste in ingresso.  Di conseguenza, la velocità effettiva aggregata totale di tutte le connessioni è limitata solo dal numero di macchine virtuali Mux distribuita.  

Una singola connessione a un IP virtuale (VIP) verrà sempre inviata per il Mux stesso, presupponendo che il numero dei MUX rimane costante e di conseguenza la velocità effettiva sarà limitata alla velocità effettiva di una singola VM Mux.  I MUX di elaborano solo il traffico in ingresso che è destinato a un indirizzo VIP.  Pacchetti di risposta passare direttamente dalla macchina virtuale che invia la risposta al commutatore fisico che li inoltra al client.

In alcuni casi quando l'origine della richiesta proviene da un host SDN che viene aggiunto allo stesso Controller di rete che gestisce l'indirizzo VIP, un'ulteriore ottimizzazione del percorso in ingresso per la richiesta viene eseguita anche che permette di spostarsi direttamente dalla maggior parte dei pacchetti di client al server, ignorando completamente le VM Mux.  È necessaria per l'ottimizzazione avvenire alcuna configurazione aggiuntiva.

Ogni macchina virtuale Mux SLB deve vengano ridimensionati secondo le linee guida fornite nella sezione requisiti di ruolo macchina virtuale di infrastruttura SDN del [pianificare un'infrastruttura Software Defined Network](../../../../networking/sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md) argomento.

## <a name="single-root-io-virtualization-sr-iov"></a>Singola virtualizzazione i/o radice (SR-IOV)

Quando si usa Ethernet da 40 Gbit, la possibilità per il commutatore virtuale per il processo di pacchetti per la VM Mux diventa il fattore di limitazione per la velocità effettiva di VM Mux.  Per questo motivo, è consigliabile abilitato SR-IOV nella scheda di rete della macchina virtuale di SLB VM per assicurarsi che il commutatore virtuale non sia un collo di bottiglia.

Per abilitare SR-IOV, è necessario abilitarlo nel commutatore virtuale quando viene creato il commutatore virtuale.  In questo esempio viene creato un commutatore virtuale con SR-IOV e switch embedded teaming (SET):
``` syntax
    new-vmswitch -Name SDNSwitch -EnableEmbeddedTeaming $true -NetAdapterName @("NIC1", "NIC2") -EnableIOV $true
```
Quindi, deve essere abilitata su schede di rete virtuali della macchina virtuale Mux SLB che elaborano il traffico di dati.  In questo esempio, SR-IOV è in corso abilitata in tutte le schede:
``` syntax
    get-vmnetworkadapter -VMName SLBMUX1 | set-vmnetworkadapter -IovWeight 50
```
