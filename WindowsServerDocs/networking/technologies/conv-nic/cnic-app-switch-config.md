---
title: Configurazione di commutatore fisico per scheda di rete convergente
description: In questo argomento fa parte di convergenza NIC Configuration Guide per Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 6d53c797-fb67-4b9e-9066-1c9a8b76d2aa
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 98a2e249aea38bd4d07dc1bcbc9b1ca98b98b6d6
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="physical-switch-configuration-for-converged-nic"></a>Configurazione di commutatore fisico per scheda di rete convergente

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare le sezioni seguenti come linee guida per configurare i commutatori fisici.

Questi sono solo i comandi e dei relativi utilizzi; è necessario determinare le porte a cui le schede NIC sono connesse nell'ambiente in uso. 

>[!IMPORTANT]
>Assicurarsi che la VLAN e i criteri non rilascio è impostata per la priorità su cui è configurato SMB.

## <a name="arista-switch-dcs-7050s-64-eos-4137m"></a>Commutatore arista \ (dcs\ 7050s\-64, EOS\-4.13.7M\)

1.  En \ (passare alla modalità di amministrazione, in genere richiede un password\)
2.  Configurazione \ (a stipulare mode\ configurazione)
3.  Mostra esecuzione \ (Mostra corrente in esecuzione configurazione)
4.  Scopri porte del commutatore a cui le schede di rete connessi. Questo esempio, sono 14/1,15/1,16/1,17/1.
5.  Int alfabeto 14/1,15/1,16/1,17/1 \ (immettere in modalità di configurazione per questi ports\)
6.  Modalità dcbx ieee
7.  Modalità di controllo di flusso prioritario in
8.  Switchport trunk vlan nativa 225
9.  Switchport trunk vlan 100-225 consentito
10. Trunk modalità switchport
11. Controllo di flusso prioritario priorità 3 non trascinamento
12. QoS trust CO
13. Mostra esecuzione \ (verificare che la configurazione è impostata correttamente sul ports\)
14. WR \ (per verificare le impostazioni viene mantenuta in commutatore reboot\)

### <a name="tips"></a>Suggerimenti:
1.  Nessun command # # e l'annullamento di un comando
2.  Come aggiungere un nuovo VLAN: vlan int 100 \ (se rete di archiviazione in 100\ VLAN)
3.  Come controllare le VLAN esistente: Mostra vlan
4.  Per ulteriori informazioni sulla configurazione di commutatore Arista, cercare online: Arista manuale di fine del flusso
5.  Utilizzare questo comando per verificare le impostazioni di base alla priorità: visualizzare i contatori di controllo di flusso prioritario dettagli

## <a name="dell-switch-s4810-ftos-99-00"></a>Commutatore Dell \ (S4810, FTOS 9.9 \(0.0\)\)

    
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
    

## <a name="cisco-switch-nexus-3132-version-602u61"></a>Commutatore Cisco \ (Nexus 3132, versione 6.0\(2\)U6\(1\)\)

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
    

### <a name="port-specific"></a>Porte specifiche

    
    switchport mode trunk
    switchport trunk native vlan 99
    switchport trunk allowed vlan 99,2000,2050   çuse VLANs that already exists
    spanning-tree port type edge
    flowcontrol receive on (not supported with PFC in Cisco NX-OS)
    flowcontrol send on (not supported with PFC in Cisco NX-OS)
    no shutdown
    priority-flow-control mode on
    

## <a name="all-topics-in-this-guide"></a>Tutti gli argomenti in questa Guida

Questa guida contiene gli argomenti seguenti.

- [Configurazione di rete convergente con una sola scheda di rete](cnic-single.md)
- [Configurazione di rete convergente le NIC combinate](cnic-datacenter.md)
- [Configurazione di commutatore fisico per scheda di rete convergente](cnic-app-switch-config.md)
- [Risoluzione dei problemi convergente configurazioni NIC](cnic-app-troubleshoot.md)
