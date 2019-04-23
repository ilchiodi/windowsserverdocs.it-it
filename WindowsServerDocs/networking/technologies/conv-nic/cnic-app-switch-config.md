---
title: Configurazione di commutatore fisico di interfaccia di rete convergente
description: In questo argomento, Microsoft fornisce le linee guida per la configurazione dei commutatori fisici.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 6d53c797-fb67-4b9e-9066-1c9a8b76d2aa
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/14/2018
ms.openlocfilehash: e31d7b83fee84d9055d938f77b49389205786244
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829402"
---
# <a name="physical-switch-configuration-for-converged-nic"></a>Configurazione di commutatore fisico di interfaccia di rete convergente

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo argomento, Microsoft fornisce le linee guida per la configurazione dei commutatori fisici. 


Si tratta solo i comandi e il relativo utilizzo; è necessario determinare le porte a cui sono connesse le schede di rete nell'ambiente in uso. 

>[!IMPORTANT]
>Verificare che la VLAN e i criteri di non trascinamento è impostato per la priorità su cui è configurato SMB.

## <a name="arista-switch-dcs-7050s-64-eos-4137m"></a>Commutatore arista \(controller di dominio\-7050s\-EOS 64,\-4.13.7M\)

1.  en \(passare alla modalità di amministrazione, in genere richiede una password\)
2.  configurazione \(immettere in modalità di configurazione\)
3.  Visualizza esecuzione \(Visualizza configurazione corrente in esecuzione\)
4.  informazioni su porte di commutazione a cui le schede di rete connessi. In questi esempio, sono 14/1,15/1,16/1,17/1.
5.  int eth 14/1,15/1,16/1,17/1 \(immettere in modalità di configurazione per queste porte\)
6.  modalità dcbx ieee
7.  modalità di controllo di flusso prioritario a
8.  switchport trunk vlan nativa 225
9.  switchport trunk vlan. 100 e 225 è consentita
10. trunk modalità switchport
11. controllo di flusso prioritario con priorità 3 non rilascio
12. QoS trust cos
13. Visualizza esecuzione \(verificare che la configurazione sia impostata correttamente su porte\)
14. WR \(per configurare le impostazioni vengono mantenute per switch riavvio\)

### <a name="tips"></a>Suggerimenti:
1.  Nessun command # # Annulla un comando
2.  Come aggiungere una nuovo VLAN: vlan int 100 \(se rete di archiviazione si trova in 100 VLAN\)
3.  Come controllare le VLAN esistente: Mostra vlan
4.  Per altre informazioni sulla configurazione di commutatore Arista, eseguire una ricerca online: Arista EOS manuale
5.  Usare questo comando per verificare le impostazioni di base alla priorità: mostrare i dettagli dei contatori di controllo di flusso prioritario

--- 

## <a name="dell-switch-s4810-ftos-99-00"></a>Commutatore Dell \(S4810, 9.9 FTOS \(0,0\)\)

    
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

## <a name="cisco-switch-nexus-3132-version-602u61"></a>Commutatore Cisco \(Nexus 3132, versione 6.0\(2\)U6\(1\)\)

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
    

### <a name="port-specific"></a>Porta specifica

    
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

- [Configurazione scheda di rete convergente con una sola scheda di rete](cnic-single.md)
- [Configurazione di interfaccia di rete convergente le NIC](cnic-datacenter.md)
- [Risoluzione dei problemi di configurazioni di interfaccia di rete convergente](cnic-app-troubleshoot.md)

--- 