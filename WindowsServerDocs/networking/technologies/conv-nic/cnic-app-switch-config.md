---
title: Configurazione del Commuter fisico per NIC convergente
description: In questo argomento vengono fornite le linee guida per la configurazione dei commutatori fisici.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 6d53c797-fb67-4b9e-9066-1c9a8b76d2aa
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/14/2018
ms.openlocfilehash: 57fc944461254e78635913ac298bacc26a0789f2
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309612"
---
# <a name="physical-switch-configuration-for-converged-nic"></a>Configurazione del Commuter fisico per NIC convergente

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In questo argomento vengono fornite le linee guida per la configurazione dei commutatori fisici. 


Questi sono solo i comandi e i rispettivi usi; è necessario determinare le porte a cui sono connesse le schede di rete nell'ambiente in uso. 

>[!IMPORTANT]
>Verificare che i criteri VLAN e no-drop siano impostati per la priorità di configurazione di SMB.

## <a name="arista-switch-dcs-7050s-64-eos-4137m"></a>Arista switch \(controller di dominio\-7050s\-64, EOS\-4.13.7 M\)

1.  in \(passare alla modalità di amministrazione, in genere richiede una password\)
2.  configurare \(per accedere alla modalità di configurazione\)
3.  Mostra esecuzione \(Mostra la configurazione in esecuzione corrente\)
4.  individuare le porte di commutazione a cui sono connesse le schede di rete. In questi esempi, sono 14/1, 15/1, 16/1, 17/1.
5.  int ETH 14/1, 15/1, 16/1, 17/1 \(attivare la modalità di configurazione per queste porte\)
6.  IEEE in modalità DCBX
7.  priorità-modalità di controllo del flusso in
8.  VLAN nativa trunk switchport 225
9.  switchport trunk consentito 100-225 VLAN
10. Trunk modalità switchport
11. priorità-flusso-controllo priorità 3 no-drop
12. cos attendibilità QoS
13. Mostra esecuzione \(verificare che la configurazione sia correttamente impostata sulle porte\)
14. WR \(fare in modo che le impostazioni vengano mantenute tra il riavvio del cambio\)

### <a name="tips"></a>Suggerimenti
1.  No #command # nega un comando
2.  Come aggiungere una nuova VLAN: int VLAN 100 \(se la rete di archiviazione si trova nella VLAN 100\)
3.  Come verificare le VLAN esistenti: Mostra VLAN
4.  Per ulteriori informazioni sulla configurazione del commutire Arista, cercare online per: Arista EOS Manual
5.  Usare questo comando per verificare le impostazioni di PFC: Mostra priorità-dettagli dei contatori di controllo del flusso

--- 

## <a name="dell-switch-s4810-ftos-99-00"></a>Switch Dell \(S4810, FTOS 9,9 \(0,0\)\)

    
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

## <a name="cisco-switch-nexus-3132-version-602u61"></a>Switch Cisco \(Nexus 3132, versione 6,0\(2\)U6\(1\)\)

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