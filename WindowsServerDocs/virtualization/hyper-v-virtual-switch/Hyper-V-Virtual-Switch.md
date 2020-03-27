---
title: Commutatore virtuale Hyper-V
description: Questo argomento fornisce una panoramica del Commuter virtuale Hyper-V in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: article
ms.assetid: 398440ac-5988-41ce-b91e-eab343a255d3
ms.author: lizross
author: eross-msft
ms.openlocfilehash: fb2ebf485b5004e457558fc16d8535662c0c5ff2
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80307986"
---
# <a name="hyper-v-virtual-switch"></a>Commutatore virtuale Hyper-V

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Questo argomento fornisce una panoramica del Commuter virtuale Hyper-V, che offre la possibilità di connettere macchine virtuali \(VM\) a reti esterne all'host Hyper\-V, tra cui la rete Intranet dell'organizzazione e Internet. 

Quando si distribuisce software defined networking \(SDN\), è inoltre possibile connettersi alle reti virtuali sul server che esegue Hyper\-V.

> [!NOTE]  
> Oltre a questo argomento, è disponibile la documentazione seguente relativa al Commuter virtuale Hyper-V.  
>   
> - [Gestire il commutatore virtuale Hyper-V](Manage-Hyper-V-Virtual-Switch.md) 
> - [Accesso diretto a memoria remota (RDMA) e Switch Embedded Teaming (SET)](RDMA-and-Switch-Embedded-Teaming.md)
> - [Cmdlet del team del commutire di rete in Windows PowerShell](https://technet.microsoft.com/library/jj553812.aspx)
> - [Novità di VMM 2016](https://docs.microsoft.com/system-center/vmm/whats-new#networking)
> - [Configurare l'infrastruttura di rete VMM](https://docs.microsoft.com/system-center/vmm/manage-networks)
> - [Creare reti con VMM 2012](https://social.technet.microsoft.com/wiki/contents/articles/3140.create-networks-with-vmm-2012.aspx)  
> - [Hyper-V: configurare le VLAN e la codifica VLAN](https://social.technet.microsoft.com/wiki/contents/articles/1306.hyper-v-configure-vlans-and-vlan-tagging.aspx)  
> - [Hyper-V: è necessario abilitare l'estensione del Commuter virtuale di WFP se è richiesta da estensioni di terze parti](https://social.technet.microsoft.com/wiki/contents/articles/13071.hyper-v-the-wfp-virtual-switch-extension-should-be-enabled-if-it-is-required-by-third-party-extensions.aspx)
>
> Per ulteriori informazioni sulle altre tecnologie di rete, vedere la pagina relativa [alla rete in Windows Server 2016](https://docs.microsoft.com/windows-server/networking/networking).
  
Il Commuter virtuale Hyper\-V è un Commuter di rete Ethernet di livello 2 Basato su software disponibile nella console di gestione di Hyper\-V quando si installa il ruolo server Hyper\-V.

Il Commuter virtuale Hyper-V include funzionalità gestite ed estendibili a livello di codice per connettere le macchine virtuali a entrambe le reti virtuali e alla rete fisica. Il commutatore virtuale Hyper-V supporta inoltre l'imposizione di criteri per sicurezza, l'isolamento e i livelli di servizio.  
  
> [!NOTE]  
> Il commutatore virtuale Hyper-V supporta solo Ethernet e non supporta altre tecnologie di rete locale cablata (LAN), ad esempio Infiniband e Fibre Channel.  
  
Il Commuter virtuale Hyper-V include funzionalità di isolamento dei tenant, shaping del traffico, protezione da macchine virtuali dannose e risoluzione dei problemi semplificata. 

Con il supporto incorporato per la specifica dell'interfaccia di dispositivo di rete \(NDIS\) driver di filtro e Windows Filtering Platform \(WFP\) i driver di callout, il Commuter virtuale Hyper-V consente ai fornitori di software indipendenti \(ISV\) di creare plug-in estendibili, detti estensioni del Commuter virtuale, che possono fornire funzionalità di rete e sicurezza avanzate. Le estensioni del commutatore virtuale aggiunte al commutatore virtuale Hyper-V sono elencate nell'area Gestione commutatori virtuali della console di gestione di Hyper-V.
  
Nella figura seguente, una macchina virtuale dispone di una scheda di interfaccia di rete virtuale connessa al compartitore virtuale Hyper-V tramite una porta di commutazione.  
  
![Connessioni Commuter virtuali](../media/Hyper-V-Virtual-Switch/Vswitch_01.jpg)  
  
Le funzionalità del Commuter virtuale Hyper-V offrono altre opzioni per l'applicazione dell'isolamento dei tenant, il data shaping e il controllo del traffico di rete e l'uso di misure di protezione contro le VM dannose.

>[!NOTE]
> In Windows Server 2016, una VM con una scheda di interfaccia di rete virtuale Visualizza accuratamente la velocità effettiva massima per la scheda di interfaccia di rete virtuale. Per visualizzare la velocità della scheda di interfaccia di rete virtuale in **connessioni di rete**, fare clic con il pulsante **Status**destro del mouse sull'icona della scheda di interfaccia di rete virtuale Verrà visualizzata la finestra di dialogo **stato** scheda di interfaccia di rete virtuale. In **connessione**, il valore di **Speed** corrisponde alla velocità della scheda di interfaccia di rete fisica installata nel server.
  
## <a name="uses-for-hyper-v-virtual-switch"></a><a name="bkmk_apps"></a>Usi per il Commuter virtuale Hyper-V

Di seguito sono riportati alcuni scenari di casi d'uso per il Commuter virtuale Hyper-V.

**Visualizzazione di statistiche**: uno sviluppatore presso un fornitore di cloud ospitati implementa un pacchetto di gestione che visualizza lo stato corrente del Commuter virtuale Hyper-V. Utilizzando WMI, il pacchetto di gestione è in grado di recuperare informazioni su funzionalità correnti dell'intero commutatore, impostazioni di configurazione e statistiche di rete per le singole porte. Mostra quindi lo stato del commutatore per fornire agli amministratori una visualizzazione rapida dello stato del commutatore.  
  
**Monitoraggio delle risorse**: una società di hosting vende servizi di hosting con prezzi a seconda del livello di appartenenza. Ai vari livelli di abbonamento corrispondono livelli diversi di prestazioni della rete. L'amministratore alloca le risorse in base agli SLA, in maniera tale da bilanciare la disponibilità della rete. L'amministratore tiene traccia a livello di codice delle informazioni, ad esempio l'uso corrente della larghezza di banda assegnata e il numero di canali VMQ (Virtual Machine Queue) assegnati alla macchina virtuale (VM) o IOV. Lo stesso programma periodicamente registra anche le risorse in uso, oltre alle risorse assegnate per macchina virtuale, per effettuare un controllo incrociato delle risorse.  
  
**Gestione dell'ordine delle estensioni del Commuter**: un'azienda ha installato nel proprio host Hyper-V estensioni che consentono di monitorare il traffico e segnalare il rilevamento delle intrusioni. Durante la manutenzione alcune estensioni potrebbero essere aggiornate, determinando un cambiamento dell'ordine. Viene eseguito un semplice programma script per riordinare le estensioni dopo gli aggiornamenti.  
  
L' **estensione di avanzamento gestisce l'ID VLAN**: una società di commutazione principale sta creando un'estensione di avanzamento che applica tutti i criteri per la rete. Gli elementi gestiti includono gli ID delle reti locali virtuali (VLAN, Virtual Local Area Network). Il commutatore virtuale cede il controllo della VLAN a un'estensione di inoltro. L'installazione della società del commutatore chiama a livello di codice un Application Programming Interface di Strumentazione gestione Windows (WMI) che attiva la trasparenza, indicando al commutatore virtuale Hyper-V di passare senza eseguire alcuna azione sui tag VLAN.  
  
## <a name="hyper-v-virtual-switch-functionality"></a><a name="bkmk_func"></a>Funzionalità del Commuter virtuale Hyper-V
 
Le principali funzionalità incluse in commutatore virtuale Hyper-V sono:  
  
-   **Protezione da poisoning (spoofing) ARP/ND**: fornisce la protezione contro una macchina virtuale dannosa che usa lo spoofing ARP (Address Resolution Protocol) per rubare gli indirizzi IP da altre macchine virtuali. Protegge il sistema dai potenziali attacchi a IPv6 tramite spoofing ND.  
  
-   **DHCP Guard Protection**: protegge da una macchina virtuale dannosa che rappresenta se stessa come server Dynamic Host Configuration Protocol (DHCP) per gli attacchi man-in-the-Middle.  
  
-   **ACL delle porte**: consente di filtrare il traffico in base agli indirizzi o intervalli Mac (Media Access Control) o IP (Internet Protocol), che consentono di configurare l'isolamento delle reti virtuali.  
  
-   **Modalità trunk per una macchina**virtuale: consente agli amministratori di configurare una macchina virtuale specifica come appliance virtuale e quindi indirizzare il traffico da varie VLAN a tale macchina virtuale.  
  
-   **Monitoraggio del traffico di rete**: consente agli amministratori di esaminare il traffico che attraversa il commutire di rete.  
  
-   **VLAN isolata (privata)** : consente agli amministratori di separare il traffico su più VLAN, per definire più facilmente le community di tenant isolati.  
  
Di seguito sono elencate le funzionalità che migliorano le possibilità di utilizzo del commutatore virtuale Hyper-V:  
  
-   **Limite della larghezza di banda e supporto**di espansione: la larghezza di banda minima garantisce quantità di larghezza di banda riservata La larghezza di banda massima pone un limite alla quantità di larghezza di banda utilizzabile da una macchina virtuale.  
  
-   **Supporto per contrassegno esplicito di congestione (ECN)** : il contrassegno ECN, noto anche come data contrassegni ECN (DCTCP), consente al commutire fisico e al sistema operativo di regolare il flusso del traffico in modo che le risorse del buffer del compasso non vengano sovraccaricate, con conseguente aumento della velocità effettiva del traffico.  
  
-   **Diagnostica**: la diagnostica consente di eseguire facilmente la traccia e il monitoraggio degli eventi e dei pacchetti tramite il Commuter virtuale.
