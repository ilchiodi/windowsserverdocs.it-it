---
title: Scenari di rete supportati in Windows Server
description: Questo argomento vengono fornite informazioni sulle nuove funzionalità di rete gli scenari supportati in Windows Server 2016 e versioni successive
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.date: ''
ms.assetid: 6de4232d-b0b3-4e43-8735-ebae35ae4f9f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 85f73f1f7caf833d23d3d693c0d754f52c4aa27d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812232"
---
# <a name="windows-server-supported-networking-scenarios"></a>Scenari di rete supportati in Windows Server

>Si applica a: Windows Server \(canale semestrale\), Windows Server 2016

In questo argomento vengono fornite informazioni sugli scenari supportati e non supportati che è possibile o non è possibile eseguire con questa versione di Windows Server 2016.  
>[!IMPORTANT]
>Per tutti gli scenari di produzione, usare i driver più recente di hardware firmato dall'original equipment manufacturer \(OEM\) o un fornitore di hardware indipendenti \(IHV\).
  
## <a name="bkmk_supp"></a>Scenari di rete supportate

In questa sezione include informazioni sugli scenari di rete supportate per Windows Server 2016 e include le seguenti categorie di scenario.  
  
-   [Scenari di software Defined Networking (SDN)](#bkmk_sdn)  
  
-   [Scenari di piattaforma di rete](#bkmk_netp)  
  
-   [Scenari di Server DNS](#bkmk_dns)  
  
-   [Scenari di gestione indirizzi IP con DHCP e DNS](#bkmk_ipam)  
  
-   [Scenari di gruppo NIC](#bkmk_nicteam)

- [Switch Embedded Teaming \(impostare\) scenari](#bkmk_set)
  
### <a name="bkmk_sdn"></a>Scenari di software Defined Networking (SDN)
 
È possibile usare la documentazione seguente per distribuire gli scenari di rete SDN con Windows Server 2016.  
  
  
-   [Distribuire un'infrastruttura Software Defined Networking tramite script](sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)  
  
Per altre informazioni, vedere [Software Defined Networking &#40;SDN&#41;](sdn/software-defined-networking.md).  
  
#### <a name="bkmk_netc"></a>Scenari di Controller di rete

Gli scenari di Controller di rete consentono di:  
  
-   Distribuire e gestire un'istanza di più nodi di Controller di rete. Per altre informazioni, vedere [distribuire Controller di rete tramite Windows PowerShell](sdn/deploy/Deploy-Network-Controller-using-Windows-PowerShell.md).  
  
-   Usare Controller di rete a livello di codice definire criteri di rete usando l'API Northbound del resto.  
  
-   Usare Controller di rete per creare e gestire le reti virtuali con virtualizzazione rete Hyper-V - utilizzando incapsulamento NVGRE o VXLAN.  
  
Per altre informazioni, vedere [Controller di rete](sdn/technologies/network-controller/Network-Controller.md).  
  
#### <a name="bkmk_netf"></a>Scenari di funzione virtualizzazione (NFV) di rete  
Gli scenari NFV consentono di:  
  
-   Distribuire e usare un servizio di bilanciamento del carico software per distribuire il traffico northbound sia southbound.  
  
-   Distribuire e usare un servizio di bilanciamento del carico software per distribuire il traffico sull'e westbound per le reti virtuali create con virtualizzazione rete Hyper-V.  
  
-   Distribuire e usare un bilanciamento del carico software NAT per le reti virtuali create con virtualizzazione rete Hyper-V.  
  
-   Distribuire e usare un gateway di inoltro di livello 3  
  
-   Distribuire e usare un gateway di rete privata virtuale (VPN) per i tunnel IPsec (IKEv2) site-to-site  
  
-   Distribuire e usare un gateway Generic Routing Encapsulation (GRE).  
  
-   Distribuire e configurare il routing dinamico e il routing di transito tra siti tramite protocollo BGP (Border Gateway).  
  
-   Configurare la ridondanza N + M per livello 3 e i gateway da sito a sito e per il routing BGP.  
  
-   Usare Controller di rete per specificare gli ACL in reti virtuali e le interfacce di rete.  
  
Per ulteriori informazioni, vedere [virtualizzazione delle funzioni di rete](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md).  
  
### <a name="bkmk_netp"></a>Scenari di piattaforma di rete

Per gli scenari in questa sezione di Windows Server Networking team supporta l'utilizzo di qualsiasi driver certificati di Windows Server 2016. Verificare con la scheda di interfaccia di rete \(NIC\) produttore per assicurarsi di aver gli aggiornamenti dei driver più recenti.
  
Gli scenari di piattaforma di rete consentono di:  
  
-   Usare un'interfaccia di rete convergente per combinare il traffico RDMA sia Ethernet con una sola scheda di rete.  
  
-   Creare un percorso dati con bassa latenza usando Packet Direct, abilitata nel commutatore virtuale Hyper-V e una sola scheda di rete.  
  
-   Configurare il SET di distribuire i flussi di traffico SMB diretto e RDMA tra fino a due schede di rete.  
  
Per ulteriori informazioni, vedere [Remote Direct Memory Access & #40; RDMA & #41; e passare al gruppo incorporato & #40; IMP & #41;](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).  
  
#### <a name="bkmk_switch"></a>Scenari di commutatore virtuale Hyper-V

Gli scenari di commutatore virtuale Hyper-V consentono di:  
  
-   Creare un commutatore virtuale Hyper-V con una scheda vNIC Direct accesso memoria remota (RDMA)  
  
-   Creare un commutatore virtuale Hyper-V con Vnic RDMA e Switch Embedded Teaming (SET)  
  
-   Creare un SET di gruppo nel commutatore virtuale Hyper-V  
  
-   Gestire un SET di team usando i comandi di Windows PowerShell  
  
Per altre informazioni, vedere [Remote Direct Memory Access &#40;RDMA&#41; e Switch Embedded Teaming &#40;SET&#41;](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)  
  
### <a name="bkmk_dns"></a>Scenari di Server DNS

Scenari di Server DNS consentono di:  
  
-   Specificare la posizione geografica basato su gestione traffico usando i criteri di DNS  
  
-   Configurare DNS "split Brain" usando i criteri di DNS  
  
-   Applicare filtri alle query DNS tramite i criteri DNS  
  
-   Configurare Bilanciamento del carico dell'applicazione usando i criteri di DNS  
  
-   Specificare che le risposte DNS intelligente basate sull'ora del giorno  
  
-   Configurare i criteri di trasferimento di zona DNS  
  
-   Configurare zone DNS di server dei criteri integrati in Active Directory Domain Services (AD DS)  
  
-   Configurare la velocità di risposta limitazione  
  
-   Specificare l'autenticazione basata su DNS di entità denominate (DANE)  
  
-   Configurare il supporto per record sconosciuti in DNS  
  
Per altre informazioni, vedere gli argomenti [What ' s New in Client DNS in Windows Server 2016](dns/What-s-New-in-DNS-Client.md) e [What ' s New in Server DNS in Windows Server 2016](dns/What-s-New-in-DNS-Server.md).  
  
### <a name="bkmk_ipam"></a>Scenari di gestione indirizzi IP con DHCP e DNS

Gli scenari di gestione indirizzi IP consentono di:  
  
-   Individuare e amministrare i server DNS e DHCP e gli indirizzi IP in più foreste di Active Directory federative  
  
-   Usare Gestione indirizzi IP per la gestione centralizzata delle proprietà DNS, tra cui zone e record di risorse.  
  
-   Definire i criteri di controllo di accesso in base al ruolo granulare e delegare utenti gestione indirizzi IP o i gruppi di utenti per gestire il set di proprietà DNS specificato.  
  
-   Usare i comandi di Windows PowerShell per gestione indirizzi IP per automatizzare la configurazione del controllo di accesso per server DHCP e DNS.  
  
    Per altre informazioni, vedere [gestire gestione indirizzi IP](technologies/ipam/Manage-IPAM.md).  
  
### <a name="bkmk_nicteam"></a>Scenari di gruppo NIC

Gli scenari di gruppo NIC consentono di:  
  
-   Creare un gruppo NIC in una configurazione supportata  
  
-   Eliminare un gruppo NIC  
  
-   Aggiungere schede di rete al gruppo NIC in una configurazione supportata  
  
-   Rimuovere le schede di rete dal team di interfaccia di rete  
  
> [!NOTE]  
> In Windows Server 2016, è possibile usare gruppo NIC in Hyper-V, ma in alcuni casi le code di macchine virtuali (VMQ) potrebbe non attivare automaticamente alle schede di rete sottostante quando si crea un gruppo NIC. In questo caso, è possibile utilizzare il comando Windows PowerShell seguente per assicurarsi che coda macchine Virtuali sono abilitata sulle schede di interfaccia di rete team membro: `Set-NetAdapterVmq -Name <NetworkAdapterName> -Enable`  

Per altre informazioni, vedere [NIC Teaming](technologies/nic-teaming/NIC-Teaming.md). 

### <a name="bkmk_set"></a>Switch Embedded Teaming \(impostare\) scenari

SET è una soluzione alternativa gruppo NIC che è possibile usare in ambienti che includono Hyper-V e lo stack di rete SDN (Software Defined) in Windows Server 2016. SET integra alcune funzionalità gruppo NIC nel commutatore virtuale Hyper-V. 

Per altre informazioni, vedere [Direct accesso memoria remota (RDMA) e Switch Embedded Teaming (SET)](https://technet.microsoft.com/windows-server-docs/networking/technologies/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming)
  
 
  
## <a name="bkmk_unsupp"></a>Scenari di rete non supportata  
Gli scenari di rete seguenti non sono supportati in Windows Server 2016.  
  
-   Reti virtuali tenant basata su VLAN.  
  
-   IPv6 non è supportato in Metti sotto o di sovrapposizione.  
  


