---
title: Ottimizzazione delle prestazioni di reti software-defined
description: Linee guida per l'ottimizzazione delle prestazioni SDN (Software Defined Network)
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: grcusanz; anpaul
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: cfd166aab7a0ef0383fe4700fdf50cc6d35289b1
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "80851614"
---
# <a name="performance-tuning-software-defined-networks"></a>Ottimizzazione delle prestazioni di reti software-defined

SDN (Software Defined Networking) in Windows Server 2016 è la combinazione di un controller di rete, host Hyper-V, gateway di bilanciamento del carico software e gateway HNV.  Per ottimizzare ognuno di questi componenti, fare riferimento alle sezioni seguenti:

## <a name="network-controller"></a>Controller di rete

Il controller di rete è un ruolo di Windows Server che deve essere abilitato nelle macchine virtuali in esecuzione in host configurati per l'uso di SDN e controllati dal controller di rete.

Per una disponibilità elevata e massime prestazioni sono sufficienti tre macchine virtuali abilitate per il controller di rete.  Ogni macchina virtuale deve essere dimensionata in base alle linee guida fornite nella sezione relativa ai requisiti dei ruoli delle macchine virtuali dell'infrastruttura SDN dell'argomento [Pianificare un'infrastruttura Software Defined Network](../../../../networking/sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md).

### <a name="sdn-quality-of-service-qos"></a>QoS (Quality of Service) SDN

Per essere certi che al traffico delle macchine virtuali venga data priorità in modo efficace e appropriato, è consigliabile configurare QoS SDN nelle macchine virtuali del carico di lavoro.  Per altre informazioni sulla configurazione di QoS SDN, fare riferimento all'argomento [Configurare QoS per una scheda di rete della macchina virtuale tenant](../../../../networking/sdn/manage/Configure-QoS-for-Tenant-VM-Network-Adapter.md).

## <a name="hyper-v-host-networking"></a>Reti host Hyper-V

Le indicazioni fornite nella sezione relativa alle prestazioni di I/O delle reti Hyper-V della guida dedicata all'[ottimizzazione delle prestazioni per i server Hyper-V](../../role/remote-desktop/session-hosts.md) sono applicabili quando è in uso SDN. In questa sezione vengono tuttavia fornite altre linee guida da seguire per garantire prestazioni ottimali quando si usa SDN.

### <a name="physical-network-adapter-nic-teaming"></a>Gruppi di schede di rete fisiche

Per avere prestazioni ottimali e le migliori funzionalità di failover, è consigliabile configurare le schede di rete fisiche in modo che costituiscano un gruppo.  Quando si usa SDN, è necessario creare il gruppo con SET (Switch Embedded Teaming).  

Il numero ottimale di membri del gruppo è due in quanto il traffico virtualizzato verrà distribuito in entrambi i membri sia in ingresso che in uscita.  È possibile avere più di due membri nel gruppo, ma il traffico in ingresso verrà distribuito al massimo a due delle schede.  Il traffico in uscita verrà sempre distribuito in tutte le schede se per il commutatore virtuale rimane configurata l'impostazione predefinita per il bilanciamento del carico dinamico.


### <a name="encapsulation-offloads"></a>Offload di incapsulamento

SDN si basa sull'incapsulamento dei pacchetti per virtualizzare la rete.  Per prestazioni ottimali, è importante che la scheda di rete supporti l'offload hardware per il formato di incapsulamento usato.  Non si riscontrano miglioramenti significativi in termini di prestazioni con l'uno o l'altro formato di incapsulamento.  Il formato di incapsulamento predefinito è VXLAN quando viene usato il controller di rete.

È possibile determinare quale sia il formato di incapsulamento in uso con il controller di rete mediante il cmdlet seguente di PowerShell:

``` syntax
    (Get-NetworkControllerVirtualNetworkConfiguration -connectionuri $uri).properties.networkvirtualizationprotocol
```

Per prestazioni ottimali, se viene restituito VXLAN, è necessario assicurarsi che le schede di rete fisiche supportino l'offload delle attività VXLAN.  Se viene restituito NVGRE, le schede di rete fisiche devono supportare l'offload delle attività NVGRE.

### <a name="mtu"></a>MTU

L'incapsulamento comporta l'aggiunta di ulteriori byte a ogni pacchetto.  Per evitare la frammentazione di tali pacchetti, è necessario che la rete fisica sia configurata per l'uso di frame jumbo.  Un valore MTU pari a 9234 è la dimensione consigliata per VXLAN o NVGRE e deve essere configurato sul commutatore fisico per le interfacce fisiche delle porte host (L2) e le interfacce router (L3) delle VLAN su cui verranno inviati i pacchetti incapsulati.  Sono incluse le reti di transito, di provider HNV e di gestione.

Il valore MTU nell'host Hyper-V viene configurato tramite la scheda di rete e l'agente host del controller di rete in esecuzione nell'host Hyper-V effettuerà automaticamente l'adeguamento per il sovraccarico dell'incapsulamento se supportato dal driver della scheda di rete.  

Quando il traffico esce dalla rete virtuale attraverso un gateway, l'incapsulamento viene rimosso e viene usata l'MTU originale così come viene inviata dalla macchina virtuale.

### <a name="single-root-io-virtualization-sr-iov"></a>Single Root I/O Virtualization (SR-IOV)

SDN viene implementato nell'host Hyper-V usando un'estensione commutatore di inoltro nel commutatore virtuale.  Affinché tale estensione commutatore elabori i pacchetti, non deve essere usata la virtualizzazione SR-IOV nelle interfacce di rete virtuale configurate per l'uso con il controller di rete perché fa in modo che il traffico delle macchine virtuali ignori il commutatore virtuale.

Se lo si desidera, la virtualizzazione SR-IOV può comunque essere abilitata nel commutatore virtuale e usata dalle schede di rete delle macchine virtuali non controllate dal controller di rete.  Queste macchine virtuali SR-IOV possono coesistere nello stesso commutatore virtuale delle macchine virtuali controllate dal controller di rete che non usano SR-IOV.

Se si usano schede di rete da 40 Gbit, è consigliabile abilitare SR-IOV nel commutatore virtuale per i gateway di bilanciamento del carico software per ottenere la massima velocità effettiva.  Per informazioni più dettagliate, vedere la sezione relativa ai [gateway di bilanciamento del carico software](slb-gateway-performance.md).

## <a name="hnv-gateways"></a>Gateway di virtualizzazione rete Hyper-V

È possibile trovare informazioni sull'ottimizzazione dei gateway di virtualizzazione rete Hyper-V (HNV) per l'uso con SDN nella sezione relativa a [tali gateway](hnv-gateway-performance.md).

## <a name="software-load-balancer-slb"></a>Bilanciamento del carico software

I gateway di bilanciamento del carico software possono essere usati solo con il controller di rete e SDN.  È possibile trovare altre informazioni sull'ottimizzazione di SDN per l'uso con tali gateway nella sezione relativa ai [gateway di bilanciamento del carico software](slb-gateway-performance.md).
