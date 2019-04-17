---
title: Scenari di rete di Windows Server supportati
description: Questo argomento fornisce informazioni sulle nuove funzionalità di rete gli scenari supportati in Windows Server 2016 e versioni successive
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.date: ''
ms.assetid: 6de4232d-b0b3-4e43-8735-ebae35ae4f9f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 70198f97c4ec39de4b78de28ab196dc3e86a684c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="windows-server-supported-networking-scenarios"></a>Scenari di rete di Windows Server supportati

>Si applica a: Windows Server \(Semi-Annual Channel\), Windows Server 2016

Questo argomento fornisce informazioni sugli scenari supportati e non supportati che è possibile o non può eseguire con questa versione di Windows Server 2016.  
>[!IMPORTANT]
>Per tutti gli scenari di produzione, utilizzare i driver più recente firmato hardware dal produttore \(OEM\) o fornitore di hardware indipendenti \(IHV\).
  
## <a name="bkmk_supp"></a>Scenari di rete supportate

In questa sezione include informazioni sugli scenari di rete supportati per Windows Server 2016 e include le seguenti categorie di scenario.  
  
-   [Scenari di software definito reti SDN)](#bkmk_sdn)  
  
-   [Scenari di piattaforma di rete](#bkmk_netp)  
  
-   [Scenari di Server DNS](#bkmk_dns)  
  
-   [Scenari di gestione indirizzi IP con DHCP e DNS](#bkmk_ipam)  
  
-   [Scenari di gruppo NIC](#bkmk_nicteam)

- [Scenari di switch Embedded Teaming \(SET\)](#bkmk_set)
  
### <a name="bkmk_sdn"></a>Scenari di software definito reti SDN)
 
È possibile utilizzare la seguente documentazione per distribuire gli scenari di SDN con Windows Server 2016.  
  
  
-   [Distribuire un'infrastruttura Software Defined Network usando gli script](sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)  
  
Per ulteriori informazioni, vedere [rete definita dal Software & #40; SDN & #41;](sdn/software-defined-networking.md).  
  
#### <a name="bkmk_netc"></a>Scenari di Controller di rete

Gli scenari di Controller di rete consentono di:  
  
-   Distribuire e gestire un'istanza di più nodi di Controller di rete. Per ulteriori informazioni, vedere [distribuire Controller di rete tramite Windows PowerShell](sdn/deploy/Deploy-Network-Controller-using-Windows-PowerShell.md).  
  
-   Utilizzare Controller di rete a livello di codice definire criteri di rete usando l'API Northbound del resto.  
  
-   Per creare e gestire le reti virtuali con virtualizzazione rete Hyper-V - utilizzando incapsulamento NVGRE o VXLAN, utilizzare Controller di rete.  
  
Per ulteriori informazioni, vedere [Controller di rete](sdn/technologies/network-controller/Network-Controller.md).  
  
#### <a name="bkmk_netf"></a>Scenari di funzione virtualizzazione (NFV) di rete  
Gli scenari NFV consentono di:  
  
-   Distribuire e utilizzare un servizio di bilanciamento del carico software per distribuire il traffico northbound sia southbound.  
  
-   Distribuire e utilizzare un servizio di bilanciamento del carico software per distribuire il traffico sull'e westbound per le reti virtuali creati con virtualizzazione rete Hyper-V.  
  
-   Distribuire e utilizzare un bilanciamento del carico software NAT per le reti virtuali create con virtualizzazione rete Hyper-V.  
  
-   Distribuire e utilizzare un gateway di inoltro di livello 3  
  
-   Distribuire e utilizzare un gateway di rete privata virtuale (VPN) per i tunnel IPsec (IKEv2) site-to-site  
  
-   Distribuire e utilizzare un gateway Generic Routing Encapsulation (GRE).  
  
-   Distribuire e configurare il routing dinamico e il routing di transito tra siti mediante protocollo BGP (Border Gateway).  
  
-   Configurare la ridondanza N + M per livello 3 e i gateway da sito a sito e per il routing BGP.  
  
-   Utilizzare Controller di rete per specificare gli ACL in reti virtuali e le interfacce di rete.  
  
Per ulteriori informazioni, vedere [virtualizzazione delle funzioni di rete](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md).  
  
### <a name="bkmk_netp"></a>Scenari di piattaforma di rete

Per gli scenari descritti in questa sezione, la rete di Windows Server team supporta l'utilizzo di qualsiasi driver certificati di Windows Server 2016. Contattare il produttore \(NIC\) della scheda interfaccia di rete per assicurarsi di disporre gli aggiornamenti dei driver più recente.
  
Gli scenari di piattaforma di rete consentono di:  
  
-   Utilizzare una scheda di rete convergente per combinare il traffico sia RDMA ed Ethernet tramite una singola scheda di rete.  
  
-   Creare un percorso dati a bassa latenza utilizzando pacchetti diretti, abilitato nel commutatore virtuale Hyper-V e una sola scheda di rete.  
  
-   Configurare insieme per distribuire i flussi di traffico SMB diretto e RDMA tra fino a due schede di rete.  
  
Per ulteriori informazioni, vedere [Remote Direct Memory Access & #40; RDMA & #41; e passare gruppo incorporato & #40; Imposta & #41;](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).  
  
#### <a name="bkmk_switch"></a>Scenari di commutatore virtuale Hyper-V

Gli scenari di commutatore virtuale Hyper-V consentono di:  
  
-   Creare un commutatore virtuale Hyper-V con una scheda di rete diretta accesso memoria remota (RDMA)  
  
-   Creare un commutatore virtuale Hyper-V con Switch Embedded Teaming (SET) e RDMA  
  
-   Creare un gruppo SET nel commutatore virtuale Hyper-V  
  
-   Gestire un SET di gruppo utilizzando i comandi di Windows PowerShell  
  
Per ulteriori informazioni, vedere [Remote Direct Memory Access & #40; RDMA & #41; e passare gruppo incorporato & #40; Imposta & #41;](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)  
  
### <a name="bkmk_dns"></a>Scenari di Server DNS

Scenari di Server DNS consentono di:  
  
-   Specificare la posizione geografica basato su gestione del traffico tramite i criteri DNS  
  
-   Configurare DNS "split Brain" utilizzando criteri DNS  
  
-   Applicare filtri alle query DNS utilizzando criteri DNS  
  
-   Configurare Bilanciamento carico di applicazioni tramite criteri di DNS  
  
-   Specificare che le risposte DNS intelligente in base all'ora del giorno  
  
-   Configurare i criteri di trasferimento di zona DNS  
  
-   Configurare le zone criteri su servizi di dominio Active Directory (AD DS) integrati di server DNS  
  
-   Configurare la velocità di risposta limitazione  
  
-   Specificare l'autenticazione basata su DNS di entità denominate (DANE)  
  
-   Configurare il supporto per i record sconosciuti in DNS  
  
Per ulteriori informazioni, vedere gli argomenti [novità di Client DNS in Windows Server 2016](dns/What-s-New-in-DNS-Client.md) e [What's New in Server DNS in Windows Server 2016](dns/What-s-New-in-DNS-Server.md).  
  
### <a name="bkmk_ipam"></a>Scenari di gestione indirizzi IP con DHCP e DNS

Gli scenari di gestione indirizzi IP consentono di:  
  
-   Individuare e amministrare i server DNS e DHCP e gli indirizzi IP in più foreste di Active Directory federati  
  
-   Utilizzare Gestione indirizzi IP per la gestione centralizzata delle proprietà DNS, tra cui le zone e record di risorse.  
  
-   Definire i criteri di controllo granulare di accesso in base al ruolo e delegare agli utenti di gestione indirizzi IP o gruppi di utenti per gestire il set di proprietà DNS specificato.  
  
-   Utilizzare i comandi di Windows PowerShell per gestione indirizzi IP per automatizzare la configurazione di controllo di accesso per server DHCP e DNS.  
  
    Per ulteriori informazioni, vedere [gestire gestione indirizzi IP](technologies/ipam/Manage-IPAM.md).  
  
### <a name="bkmk_nicteam"></a>Scenari di gruppo NIC

Gli scenari con gruppo NIC consentono di:  
  
-   Creare un gruppo NIC in una configurazione supportata  
  
-   Eliminare un gruppo NIC  
  
-   Aggiungere schede di rete al gruppo NIC in una configurazione supportata  
  
-   Rimuovere le schede di rete dal gruppo NIC  
  
> [!NOTE]  
> In Windows Server 2016, è possibile utilizzare gruppo NIC in Hyper-V, ma in alcuni casi le code di macchine virtuali (VMQ) potrebbe non attivare automaticamente alle schede di rete sottostante quando si crea un gruppo NIC. In questo caso, è possibile utilizzare il comando di Windows PowerShell seguente per verificare che sia abilitato coda macchine Virtuali nelle schede membro del team NIC: `Set-NetAdapterVmq -Name <NetworkAdapterName> -Enable`  

Per ulteriori informazioni, vedere [gruppo NIC](technologies/nic-teaming/NIC-Teaming.md). 

### <a name="bkmk_set"></a>Scenari di switch Embedded Teaming \(SET\)

SET è una soluzione alternativa gruppo NIC che è possibile utilizzare in ambienti che includono Hyper-V e lo stack di rete SDN (Software Defined) in Windows Server 2016. SET di alcune funzionalità gruppo NIC si integra il commutatore virtuale Hyper-V. 

Per ulteriori informazioni, vedere [diretta accesso memoria remota (RDMA) e Switch Embedded Teaming (SET)](https://technet.microsoft.com/windows-server-docs/networking/technologies/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming)
  
 
  
## <a name="bkmk_unsupp"></a>Scenari di rete non supportata  
I seguenti scenari di rete non sono supportati in Windows Server 2016.  
  
-   Reti virtuali tenant basata su VLAN.  
  
-   IPv6 non è supportato nel Metti sotto o sovrimpressione.  
  


