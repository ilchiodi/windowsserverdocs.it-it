---
title: Configurazione del Commuter fisico per NIC convergente
description: In questo argomento vengono fornite le linee guida per la configurazione dei commutatori fisici.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 6d53c797-fb67-4b9e-9066-1c9a8b76d2aa
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/14/2018
ms.openlocfilehash: d10e8ca6e4689b89a8b9532f77613f17280282b1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355478"
---
# <a name="physical-switch-configuration-for-converged-nic"></a>Configurazione del Commuter fisico per NIC convergente

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In questo argomento vengono fornite le linee guida per la configurazione dei commutatori fisici. 


Questi sono solo i comandi e i rispettivi usi; è necessario determinare le porte a cui sono connesse le schede di rete nell'ambiente in uso. 

>[!IMPORTANT]
>Verificare che i criteri VLAN e no-drop siano impostati per la priorità di configurazione di SMB.

## <a name="arista-switch-dcs-7050s-64-eos-4137m"></a>Arista switch \(dcs @ no__t-17050s @ no__t-264, EOS @ no__t-34.13.7 M @ no__t-4

1.  en \(go alla modalità di amministrazione, in genere richiede una password @ no__t-1
2.  config \(to immettere in modalità di configurazione @ no__t-1
3.  Mostra Run \(shows Current running Configuration @ no__t-1
4.  individuare le porte di commutazione a cui sono connesse le schede di rete. In questi esempi, sono 14/1, 15/1, 16/1, 17/1.
5.  int ETH 14/1, 15/1, 16/1, 17/1 \(enter in modalità di configurazione per queste porte @ no__t-1
6.  IEEE in modalità DCBX
7.  priorità-modalità di controllo del flusso in
8.  VLAN nativa trunk switchport 225
9.  switchport trunk consentito 100-225 VLAN
10. Trunk modalità switchport
11. priorità-flusso-controllo priorità 3 no-drop
12. cos attendibilità QoS
13. Mostra eseguire \(verify che la configurazione sia correttamente impostata sulle porte @ no__t-1
14. WR \(to fare in modo che le impostazioni vengano mantenute tra switch reboot @ no__t-1

### <a name="tips"></a>Consigli
1.  No #command # nega un comando
2.  Come aggiungere una nuova VLAN: int VLAN 100 \(If Storage Network è on VLAN 100 @ no__t-1
3.  Come verificare le VLAN esistenti: Mostra VLAN
4.  Per ulteriori informazioni sulla configurazione dell'opzione Arista, cercare online per: Manuale di Arista EOS
5.  Usare questo comando per verificare le impostazioni di PFC: Mostra priorità-dettagli dei contatori di controllo del flusso

--- 

## <a name="dell-switch-s4810-ftos-99-00"></a>Switch Dell \(S4810, FTOS 9,9 \(0.0 @ no__t-2 @ no__t-3

    
    !
    dcb enable
    ! put pfc control on qos class 3
    configure
    dcb-map dcb-smb
    priority group 0 bandwidth 90 pfc on
    priority group 1 bandwidth 10 pfc off
    priority-pgid 1 1 1 0 1 1 1 1
    exit
    ! apply map to ports 0-31
    configure
    interface range ten 0/0-31
    dcb-map dcb-smb
    exit
    
--- 

## <a name="cisco-switch-nexus-3132-version-602u61"></a>Switch Cisco \(Nexus 3132, versione 6.0 @ no__t-12 @ no__t-2U6 @ no__t-31 @ no__t-4 @ no__t-5

### <a name="global"></a>Globale
    
    class-map type qos match-all RDMA
    match cos 3
    class-map type queuing RDMA
    match qos-group 3
    policy-map type qos QOS_MARKING
    class RDMA
    set qos-group 3
    class class-default
    policy-map type queuing QOS_QUEUEING
    class type queuing RDMA
    bandwidth percent 50
    class type queuing class-default
    bandwidth percent 50
    class-map type network-qos RDMA
    match qos-group 3
    policy-map type network-qos QOS_NETWORK
    class type network-qos RDMA
    mtu 2240
    pause no-drop
    class type network-qos class-default
    mtu 9216
    system qos
    service-policy type qos input QOS_MARKING
    service-policy type queuing output QOS_QUEUEING
    service-policy type network-qos QOS_NETWORK
    

### <a name="port-specific"></a>Specifico per la porta

    
    switchport mode trunk
    switchport trunk native vlan 99
    switchport trunk allowed vlan 99,2000,2050   çuse VLANs that already exists
    spanning-tree port type edge
    flowcontrol receive on (not supported with PFC in Cisco NX-OS)
    flowcontrol send on (not supported with PFC in Cisco NX-OS)
    no shutdown
    priority-flow-control mode on
    
--- 

## <a name="related-topics"></a>Argomenti correlati

- [Configurazione NIC convergente con una singola scheda di rete](cnic-single.md)
- [Configurazione NIC convergente con gruppo NIC](cnic-datacenter.md)
- [Risoluzione dei problemi relativi alle configurazioni NIC convergenti](cnic-app-troubleshoot.md)

--- 