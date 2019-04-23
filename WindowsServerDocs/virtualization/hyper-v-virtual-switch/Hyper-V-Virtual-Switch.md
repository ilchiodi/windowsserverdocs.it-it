---
title: Commutatore virtuale Hyper-V
description: In questo argomento viene fornita una panoramica del commutatore virtuale Hyper-V in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: article
ms.assetid: 398440ac-5988-41ce-b91e-eab343a255d3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 17962b9bbb90a5ea1b6a4a303c64d72f74be37b5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860922"
---
# <a name="hyper-v-virtual-switch"></a>Commutatore virtuale Hyper-V

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo argomento viene fornita una panoramica del commutatore virtuale Hyper-V, che offre la possibilità di connettere le macchine virtuali \(VMs\) alle reti esterne per l'Hyper\-host V, tra cui la intranet dell'organizzazione e Internet. 

È anche possibile connettersi alle reti virtuali nel server in cui è in esecuzione Hyper\-V quando si distribuisce Software Defined Networking \(SDN\).

> [!NOTE]  
> Oltre a questo argomento, la documentazione seguente di commutatore virtuale Hyper-V è disponibile.  
>   
> - [Gestire il commutatore virtuale Hyper-V](Manage-Hyper-V-Virtual-Switch.md) 
> - [Accesso diretto a memoria remota (RDMA) e Switch Embedded Teaming (SET)](RDMA-and-Switch-Embedded-Teaming.md)
> - [Cmdlet di Team di commutatore di rete in Windows PowerShell](https://technet.microsoft.com/library/jj553812.aspx)
> - [What ' s New in VMM 2016](https://docs.microsoft.com/system-center/vmm/whats-new#networking)
> - [Configurare l'infrastruttura di rete VMM](https://docs.microsoft.com/system-center/vmm/manage-networks)
> - [Creare reti con VMM 2012](https://social.technet.microsoft.com/wiki/contents/articles/3140.create-networks-with-vmm-2012.aspx)  
> - [Hyper-V: Configurare le VLAN e la codifica VLAN](https://social.technet.microsoft.com/wiki/contents/articles/1306.hyper-v-configure-vlans-and-vlan-tagging.aspx)  
> - [Hyper-V: Abilitare l'estensione del commutatore virtuale piattaforma filtro Windows se è necessaria per le estensioni di terze parti](https://social.technet.microsoft.com/wiki/contents/articles/13071.hyper-v-the-wfp-virtual-switch-extension-should-be-enabled-if-it-is-required-by-third-party-extensions.aspx)
>
> Per altre informazioni su altre tecnologie di rete, vedere [funzionalità di rete in Windows Server 2016](https://docs.microsoft.com/windows-server/networking/networking).
  
Hyper\-commutatore virtuale Hyper-V è un software in base al livello 2 Ethernet commutatore di rete che è disponibile in Hyper\-console di gestione durante l'installazione di Hyper V\-ruolo del server V.

Commutatore virtuale Hyper-V include funzionalità estendibili e gestibili a livello di codice per connettere le macchine virtuali a entrambe le reti virtuali e la rete fisica. Il commutatore virtuale Hyper-V supporta inoltre l'imposizione di criteri per sicurezza, l'isolamento e i livelli di servizio.  
  
> [!NOTE]  
> Il commutatore virtuale Hyper-V supporta solo Ethernet e non supporta altre tecnologie di rete locale cablata (LAN), ad esempio Infiniband e Fibre Channel.  
  
Commutatore virtuale Hyper-V include funzionalità di isolamento tenant, la conformazione del traffico, la protezione dalle macchine virtuali dannose e semplificata risoluzione dei problemi. 

Con supporto incorporato per Network Device Interface Specification \(NDIS\) filtrare i driver e Windows Filtering Platform \(WFP\) i driver callout, il commutatore virtuale Hyper-V consente di software indipendenti i fornitori \(ISV\) creare plug-in estendibili, chiamato estensioni del commutatore virtuale, che può fornire funzionalità di rete e di sicurezza avanzate. Le estensioni del commutatore virtuale aggiunte al commutatore virtuale Hyper-V sono elencate nell'area Gestione commutatori virtuali della console di gestione di Hyper-V.
  
Nella figura seguente, una macchina virtuale ha un'interfaccia di rete virtuale connessa al commutatore Hyper-V virtuale tramite una porta di commutazione.  
  
![Connessioni del commutatore virtuale](../media/Hyper-V-Virtual-Switch/Vswitch_01.jpg)  
  
Le funzionalità di commutatore virtuale Hyper-V offrono più opzioni per l'isolamento dei tenant, il traffico di rete data shaping e controllo, e le misure di impiegando protettiva contro le macchine virtuali dannose.

>[!NOTE]
> In Windows Server 2016, una macchina virtuale con una scheda di rete virtuale in modo accurato Visualizza la velocità effettiva massima per l'interfaccia di rete virtuale. Per visualizzare la velocità di interfaccia di rete virtuale in **connessioni di rete**, fare doppio clic sull'icona di interfaccia di rete virtuale desiderata e quindi fare clic su **stato**. L'interfaccia di rete virtuale **stato** verrà visualizzata la finestra di dialogo. Nelle **Connection**, il valore di **velocità** corrisponda a quella dell'interfaccia di rete fisica installata nel server.
  
## <a name="bkmk_apps"></a>Viene utilizzato per il commutatore virtuale Hyper-V

Ecco alcuni casi d'uso per il commutatore virtuale Hyper-V.

**Visualizzazione di statistiche**: uno sviluppatore presso un fornitore di cloud ospitati implementa un pacchetto di gestione che visualizza lo stato attuale del commutatore virtuale Hyper-V. Utilizzando WMI, il pacchetto di gestione è in grado di recuperare informazioni su funzionalità correnti dell'intero commutatore, impostazioni di configurazione e statistiche di rete per le singole porte. Mostra quindi lo stato del commutatore per fornire agli amministratori una visualizzazione rapida dello stato del commutatore.  
  
**Monitoraggio delle risorse**: una società di hosting vende servizi di hosting con prezzo variabile in base al livello dell'abbonamento. Ai vari livelli di abbonamento corrispondono livelli diversi di prestazioni della rete. L'amministratore alloca le risorse in base agli SLA, in maniera tale da bilanciare la disponibilità della rete. L'amministratore a livello di codice tiene traccia delle informazioni, ad esempio l'utilizzo corrente della larghezza di banda assegnata e il numero di macchine virtuali (VM) assegnato coda macchine virtuali (VMQ) o i canali IOV. Lo stesso programma periodicamente registra anche le risorse in uso, oltre alle risorse assegnate per macchina virtuale, per effettuare un controllo incrociato delle risorse.  
  
**Gestione dell'ordine delle estensioni del commutatore**: un'azienda ha installato nel proprio host Hyper-V estensioni che consentono di monitorare il traffico e segnalare le intrusioni rilevate. Durante la manutenzione alcune estensioni potrebbero essere aggiornate, determinando un cambiamento dell'ordine. Viene eseguito un semplice programma script per riordinare le estensioni dopo gli aggiornamenti.  
  
**Estensione di inoltro gestione degli ID delle VLAN**: un'importante società di switch sta realizzando un'estensione di inoltro che applica tutti i criteri per le funzionalità di rete. Gli elementi gestiti includono gli ID delle reti locali virtuali (VLAN, Virtual Local Area Network). Il commutatore virtuale cede il controllo della VLAN a un'estensione di inoltro. Installazione della società di switch chiama a livello di codice una Strumentazione gestione Windows (WMI) API application programming interface () che attiva la trasparenza, indicando il commutatore Hyper-V Virtual di passare i tag VLAN senza eseguire alcuna azione.  
  
## <a name="bkmk_func"></a>Funzionalità dei commutatori virtuali Hyper-V
 
Le principali funzionalità incluse in commutatore virtuale Hyper-V sono:  
  
-   **Protezione da Poisoning ARP/ND (spoofing)**: protegge il sistema da una macchina virtuale dannosa che usa lo spoofing ARP per rubare indirizzi IP da altre macchine virtuali. Protegge il sistema dai potenziali attacchi a IPv6 tramite spoofing ND.  
  
-   **Controllo DHCP**: protegge il sistema da una macchina virtuale dannosa che si identifica come server DHCP (Dynamic Host Configuration Protocol) per gli attacchi man-in-the-middle.  
  
-   **ACL delle porte**: consente di filtrare il traffico in base agli indirizzi o intervalli MAC (Media Access Control) o IP (Internet Protocol), permettendo di configurare l'isolamento delle reti virtuali.  
  
-   **Modalità trunk per una macchina virtuale**: consente agli amministratori di configurare una macchina virtuale specifica come appliance virtuale e quindi indirizzare il traffico di varie VLAN a tale macchina virtuale.  
  
-   **Monitoraggio del traffico di rete**: consente agli amministratori di esaminare il traffico che attraversa il commutatore di rete.  
  
-   **VLAN isolate (private)**: consente agli amministratori di separare il traffico su più VLAN, per creare più facilmente community tenant isolate.  
  
Di seguito sono elencate le funzionalità che migliorano le possibilità di utilizzo del commutatore virtuale Hyper-V:  
  
-   **Limite di larghezza di banda e burst supportano**: la larghezza di banda minima garantisce la quantità di larghezza di banda riservata. La larghezza di banda massima pone un limite alla quantità di larghezza di banda utilizzabile da una macchina virtuale.  
  
-   **Explicit Congestion Notification (ECN) supporto dei contrassegni ECN**:  Dei contrassegni ECN, anche noto o DCTCP (Data), abilita il commutatore fisico e sistema operativo di regolare il flusso del traffico in modo che non sono le risorse di buffer del commutatore, determinando un velocità effettiva di un aumento del traffico.  
  
-   **Diagnostica**: la diagnostica semplifica il tracciamento e il monitoraggio degli eventi e dei pacchetti che attraversano il commutatore virtuale.
