---
title: Scenari di rete supportati in Windows Server
description: Questo argomento fornisce informazioni sui nuovi scenari di rete supportati in Windows Server 2016 e versioni successive
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.date: ''
ms.assetid: 6de4232d-b0b3-4e43-8735-ebae35ae4f9f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f338ddf0a7d3a4fe41277ddbf49b0c3db34ae11b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395703"
---
# <a name="windows-server-supported-networking-scenarios"></a>Scenari di rete supportati in Windows Server

>Si applica a: Windows Server \(Semi-canale annuale @ no__t-1, Windows Server 2016

In questo argomento vengono fornite informazioni sugli scenari supportati e non supportati che è possibile o non è possibile eseguire con questa versione di Windows Server 2016.  
>[!IMPORTANT]
>Per tutti gli scenari di produzione, usare i driver hardware firmati più recenti del produttore di apparecchiature originale \(OEM @ no__t-1 o un fornitore di hardware indipendente \(IHV @ no__t-3.
  
## <a name="bkmk_supp"></a>Scenari di rete supportati

In questa sezione sono incluse informazioni sugli scenari di rete supportati per Windows Server 2016 e sono incluse le categorie di scenari seguenti.  
  
-   [Scenari SDN (Software Defined Networking)](#bkmk_sdn)  
  
-   [Scenari della piattaforma di rete](#bkmk_netp)  
  
-   [Scenari di server DNS](#bkmk_dns)  
  
-   [Scenari di gestione indirizzi IP con DHCP e DNS](#bkmk_ipam)  
  
-   [Scenari di gruppo NIC](#bkmk_nicteam)

- [Switch Embedded Teaming \(SET @ no__t-2 scenari](#bkmk_set)
  
### <a name="bkmk_sdn"></a>Scenari SDN (Software Defined Networking)
 
È possibile usare la documentazione seguente per distribuire scenari SDN con Windows Server 2016.  
  
  
-   [Distribuire un'infrastruttura software defined Network usando gli script](sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)  
  
Per altre informazioni, vedere [Software Defined Networking &#40;Sdn&#41;](sdn/software-defined-networking.md).  
  
#### <a name="bkmk_netc"></a>Scenari del controller di rete

Gli scenari del controller di rete consentono di:  
  
-   Distribuire e gestire un'istanza a più nodi del controller di rete. Per ulteriori informazioni, vedere [deploy Network controller using Windows PowerShell](sdn/deploy/Deploy-Network-Controller-using-Windows-PowerShell.md).  
  
-   Usare il controller di rete per definire i criteri di rete a livello di codice tramite l'API REST in Nord.  
  
-   Usare il controller di rete per creare e gestire reti virtuali con la virtualizzazione rete Hyper-V, usando l'incapsulamento NVGRE o VXLAN.  
  
Per altre informazioni, vedere [Controller di rete](sdn/technologies/network-controller/Network-Controller.md).  
  
#### <a name="bkmk_netf"></a>Scenari di Network Function Virtualization (NFV)  
Gli scenari NFV consentono di:  
  
-   Distribuire e usare un servizio di bilanciamento del carico software per distribuire il traffico in direzione nord-sud.  
  
-   Distribuire e usare un servizio di bilanciamento del carico software per distribuire il traffico in direzione est e in direzione ovest per le reti virtuali create con la virtualizzazione rete Hyper-V.  
  
-   Distribuire e usare un servizio di bilanciamento del carico software NAT per le reti virtuali create con la virtualizzazione rete Hyper-V.  
  
-   Distribuire e usare un gateway di invio di livello 3  
  
-   Distribuire e usare un gateway di rete privata virtuale (VPN) per i tunnel IPsec da sito a sito (IKEv2)  
  
-   Distribuire e usare un gateway GRE (Generic Routing Encapsulation).  
  
-   Distribuire e configurare il routing dinamico e il routing di transito tra i siti usando Border Gateway Protocol (BGP).  
  
-   Configurare la ridondanza M + N per i gateway di livello 3 e da sito a sito e per il routing BGP.  
  
-   Usare il controller di rete per specificare gli ACL nelle reti virtuali e nelle interfacce di rete.  
  
Per ulteriori informazioni, vedere [virtualizzazione delle funzioni di rete](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md).  
  
### <a name="bkmk_netp"></a>Scenari della piattaforma di rete

Per gli scenari in questa sezione il team di rete di Windows Server supporta l'utilizzo di qualsiasi driver Windows Server 2016 Certified. Per assicurarsi di disporre degli aggiornamenti più recenti dei driver, verificare con la scheda di interfaccia di rete \(NIC @ no__t-1.
  
Gli scenari della piattaforma di rete consentono di:  
  
-   Usare una scheda di interfaccia di rete convergente per combinare il traffico RDMA e Ethernet con una singola scheda di rete.  
  
-   Creare un percorso dati a bassa latenza utilizzando pacchetti diretti, abilitati nel Commuter virtuale Hyper-V e una singola scheda di rete.  
  
-   Configurare SET per la distribuzione di flussi di traffico SMB diretto e RDMA tra un massimo di due schede di rete.  
  
Per ulteriori informazioni, vedere [Remote Direct Memory Access & #40; RDMA & #41; e passare al gruppo incorporato & #40; IMP & #41;](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).  
  
#### <a name="bkmk_switch"></a>Scenari di Commuter virtuali Hyper-V

Gli scenari del Commuter virtuale Hyper-V consentono di:  
  
-   Creazione di un Commuter virtuale Hyper-V con accesso diretto a memoria remota (RDMA) vNIC  
  
-   Creare un Commuter virtuale Hyper-V con switch Embedded Teaming (SET) e RDMA schede  
  
-   Creare un gruppo di SET nel Commuter virtuale Hyper-V  
  
-   Gestire un team di SET usando i comandi di Windows PowerShell  
  
Per altre informazioni, vedere [accesso &#40;diretto a memoria remota&#41; RDMA e switch Embedded &#40;Teaming set&#41; ](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)  
  
### <a name="bkmk_dns"></a>Scenari di server DNS

Gli scenari di server DNS consentono di:  
  
-   Specificare la gestione del traffico basata sulla posizione geografica usando i criteri DNS  
  
-   Configurare DNS split-brain usando i criteri DNS  
  
-   Applicare filtri alle query DNS usando i criteri DNS  
  
-   Configurare il bilanciamento del carico dell'applicazione usando i criteri DNS  
  
-   Specificare risposte DNS intelligenti in base all'ora del giorno  
  
-   Configurare i criteri di trasferimento di zona DNS  
  
-   Configurare i criteri del server DNS nelle zone integrate Active Directory Domain Services (AD DS)  
  
-   Configurare la limitazione della velocità di risposta  
  
-   Specificare l'autenticazione basata su DNS di entità denominate (DANE)  
  
-   Configurare il supporto per record sconosciuti in DNS  
  
Per ulteriori informazioni, vedere gli argomenti [novità del client DNS in Windows server 2016](dns/What-s-New-in-DNS-Client.md) e novità [di server DNS in Windows Server 2016](dns/What-s-New-in-DNS-Server.md).  
  
### <a name="bkmk_ipam"></a>Scenari di gestione indirizzi IP con DHCP e DNS

Gli scenari di gestione indirizzi IP consentono di:  
  
-   Individuare e amministrare i server DNS e DHCP e gli indirizzi IP in più foreste Active Directory federate  
  
-   Usare Gestione indirizzi IP per la gestione centralizzata delle proprietà DNS, incluse le zone e i record di risorse.  
  
-   Definire i criteri granulari per il controllo degli accessi in base al ruolo e delegare gli utenti o i gruppi di gestione indirizzi IP per gestire il set di proprietà DNS specificato.  
  
-   Usare i comandi di Windows PowerShell per gestione indirizzi IP per automatizzare la configurazione del controllo di accesso per DHCP e DNS.  
  
    Per altre informazioni, vedere [Manage IPAM](technologies/ipam/Manage-IPAM.md).  
  
### <a name="bkmk_nicteam"></a>Scenari di gruppo NIC

Gli scenari di gruppo NIC consentono di:  
  
-   Creare un gruppo NIC in una configurazione supportata  
  
-   Eliminare un gruppo NIC  
  
-   Aggiungere schede di rete al gruppo NIC in una configurazione supportata  
  
-   Rimuovere le schede di rete dal gruppo NIC  
  
> [!NOTE]  
> In Windows Server 2016 è possibile usare il gruppo NIC in Hyper-V, tuttavia in alcuni casi le code di macchine virtuali (VMQ) potrebbero non essere abilitate automaticamente sulle schede di rete sottostanti quando si crea un gruppo NIC. In tal caso, è possibile usare il comando di Windows PowerShell seguente per verificare che VMQ sia abilitato nelle schede membro del gruppo NIC: `Set-NetAdapterVmq -Name <NetworkAdapterName> -Enable`  

Per ulteriori informazioni, vedere [Gruppo NIC](technologies/nic-teaming/NIC-Teaming.md). 

### <a name="bkmk_set"></a>Switch Embedded Teaming \(SET @ no__t-2 scenari

SET è una soluzione di gruppo NIC alternativa che è possibile usare in ambienti che includono Hyper-V e lo stack SDN (Software Defined Networking) in Windows Server 2016. SET integra alcune funzionalità di gruppo NIC nel Commuter virtuale Hyper-V. 

Per ulteriori informazioni, vedere [accesso diretto a memoria remota (RDMA) e switch Embedded Teaming (set)](https://technet.microsoft.com/windows-server-docs/networking/technologies/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming)
  
 
  
## <a name="bkmk_unsupp"></a>Scenari di rete non supportati  
Gli scenari di rete seguenti non sono supportati in Windows Server 2016.  
  
-   Reti virtuali tenant basate su VLAN.  
  
-   IPv6 non è supportato né nel sottoposto né nella sovrapposizione.  
  


