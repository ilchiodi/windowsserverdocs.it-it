---
title: Bilanciamento del carico di rete
description: In questo argomento viene fornita una panoramica della funzionalità di bilanciamento carico di rete \(NLB\) in Windows Server 2016. È possibile utilizzare NLB per gestire due o più server come un singolo cluster virtuale. Bilanciamento carico di rete consente di migliorare la disponibilità e la scalabilità delle applicazioni server Internet, ad esempio quelle utilizzate in Web, FTP, firewall, proxy, rete privata virtuale \(\)VPN e altri server mission\-critical.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-nlb
ms.topic: article
ms.assetid: 244a4b48-06e5-4796-8750-a50e4f88ac72
ms.author: lizross
author: eross-msft
ms.date: 09/13/2018
ms.openlocfilehash: 80dae16442041e3b46babaca6d163095c1c5e475
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309690"
---
# <a name="network-load-balancing"></a>Bilanciamento del carico di rete

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In questo argomento viene fornita una panoramica della funzionalità di bilanciamento carico di rete \(NLB\) in Windows Server 2016. È possibile utilizzare NLB per gestire due o più server come un singolo cluster virtuale. Bilanciamento carico di rete consente di migliorare la disponibilità e la scalabilità delle applicazioni server Internet, ad esempio quelle utilizzate in Web, FTP, firewall, proxy, rete privata virtuale \(\)VPN e altri server mission\-critical.  

> [!NOTE]
> Windows Server 2016 include un nuovo software ispirato ad Azure Load Balancer \(SLB\) come componente di Software Defined Networking \(SDN\) infrastruttura. Utilizzare SLB anziché bilanciamento carico di rete se si utilizza SDN, si utilizzano carichi di lavoro non Windows, è necessario Network Address Translation in uscita \(\)NAT oppure è necessario il livello 3 \(L3\) o il bilanciamento del carico non basato su TCP. È possibile continuare a usare bilanciamento carico di rete con Windows Server 2016 per le distribuzioni non SDN. Per ulteriori informazioni su SLB, vedere [bilanciamento del carico software (SLB) per Sdn](../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md).

La funzionalità Bilanciamento carico di rete \(NLB\) distribuisce il traffico tra più server utilizzando il protocollo di rete IP\/TCP. Combinando due o più computer che eseguono applicazioni in un singolo cluster virtuale, bilanciamento carico di rete offre affidabilità e prestazioni per i server Web e altri server mission\-critici.  
  
I server inclusi in un cluster Bilanciamento del carico di rete sono denominati *host*e ogni host esegue una copia separata delle applicazioni server. Le richieste client in ingresso vengono distribuite da Bilanciamento carico di rete tra gli host del cluster. È possibile configurare il carico che deve essere gestito da ogni host. È inoltre possibile aggiungere host al cluster in modo dinamico per gestire un incremento del carico. Bilanciamento carico di rete può inoltre indirizzare tutto il traffico a un unico host designato, denominato *host predefinito*.  
  
Bilanciamento carico di rete consente di associare a tutti i computer del cluster lo stesso set di indirizzi IP del cluster, mantenendo un set di indirizzi IP dedicati univoci per ogni host. Per le applicazioni con carico\-bilanciato, quando un host non riesce o passa alla modalità offline, il carico viene ridistribuito automaticamente tra i computer ancora operativi. Quando il computer offline è pronto, può essere aggiunto nuovamente al cluster in modo trasparente e riottenere la propria parte del carico di lavoro, consentendo agli altri computer del cluster di gestire una minore quantità di traffico.  
  
## <a name="practical-applications"></a>Applicazioni pratiche  
Bilanciamento carico di rete è utile per garantire che le applicazioni senza stato, ad esempio i server Web che eseguono Internet Information Services \(IIS\), siano disponibili con tempi di inattività minimi e siano scalabili \(aggiungendo server aggiuntivi man mano che il carico aumenta\). Nelle sezioni seguenti viene descritto il modo in cui Bilanciamento carico di rete supporta la disponibilità elevata, la scalabilità e la gestibilità dei server in cluster che eseguono tali applicazioni.  
  
### <a name="high-availability"></a>Disponibilità elevata  
Un sistema a disponibilità elevata offre un livello accettabile e affidabile di servizio con tempo di inattività minimo. Per garantire un'elevata disponibilità, NLB include\-compilate in funzionalità che possono essere automaticamente:  
  
-   Rilevamento di host del cluster con errori o offline e ripristino.  
  
-   Bilanciamento del carico di rete in caso di aggiunta o rimozione di host.  
  
-   Ripristino e ridistribuzione del carico di lavoro entro dieci secondi.  
  
### <a name="scalability"></a>Scalabilità  
La scalabilità indica la possibilità di espansione di un computer, un servizio o un'applicazione allo scopo di soddisfare crescenti esigenze in termini di prestazioni. Per i cluster di Bilanciamento carico di rete, la scalabilità rappresenta la possibilità di aggiungere in modo incrementale uno o più sistemi a un cluster esistente quando il carico complessivo del cluster ne supera le capacità. Per supportare la scalabilità, Bilanciamento carico di rete consente di eseguire le operazioni seguenti:  
  
-   Bilanciare le richieste di carico nel cluster NLB per i singoli servizi TCP\/IP.  
  
-   Supporto di un massimo di 32 computer in un singolo cluster.  
  
-   Bilanciare più richieste di carico del server \(dallo stesso client o da diversi client\) tra più host del cluster.  
  
-   Aggiunta di host al cluster Bilanciamento carico di rete in base all'aumento del carico, senza provocare errori nel cluster.  
  
-   Rimozione di host dal cluster quando il carico diminuisce.  
  
-   Prestazioni elevate e sovraccarico ridotto grazie a un'implementazione interamente in pipeline. L'utilizzo di una pipeline consente di inviare richieste al cluster di Bilanciamento carico di rete senza attendere la risposta alla richiesta precedentemente inviata.  
  
### <a name="manageability"></a>Gestibilità  
Per supportare la gestibilità, Bilanciamento carico di rete consente di eseguire le operazioni seguenti:  
  
-   Gestire e configurare più cluster NLB e gli host del cluster da un singolo computer utilizzando Gestione bilanciamento carico di rete o i [cmdlet di bilanciamento carico di rete (NLB) in Windows PowerShell](https://technet.microsoft.com/library/hh801274.aspx).
  
-   Impostazione del comportamento di bilanciamento del carico per una singola porta IP o un gruppo di porte utilizzando regole di gestione delle porte.  
  
-   Definizione di regole di porta diverse per ogni sito Web. Se si utilizza lo stesso insieme di server con carico bilanciato\-per più applicazioni o siti Web, le regole di porta sono basate sull'indirizzo IP virtuale di destinazione \(utilizzando cluster virtuali\).  

-   Indirizzare tutte le richieste client a un singolo host usando le regole host facoltative e singole\-. Le richieste client vengono indirizzate da Bilanciamento carico di rete a un determinato host che esegue applicazioni specifiche.  

-   Blocco dell'accesso di rete indesiderato a determinate porte IP.  

-   Abilitare il protocollo di gestione dei gruppi Internet \(IGMP\) supporto per gli host del cluster per controllare il Flood delle porte di commutazione \(in cui i pacchetti di rete in ingresso vengono inviati a tutte le porte nel compartitore\) quando si opera in modalità multicast.  

-   Avvio, arresto e controllo delle azioni di Bilanciamento carico di rete in modalità remota utilizzando i comandi o gli script di Windows PowerShell.  

-   Visualizzazione del registro eventi di Windows per controllare gli eventi di Bilanciamento carico di rete. Tutte le azioni e le modifiche al cluster vengono registrate da Bilanciamento carico di rete nel registro eventi.  

## <a name="important-functionality"></a>Funzionalità importanti  
 
Bilanciamento carico di rete viene installato come componente driver di rete standard di Windows Server. Le relative operazioni sono trasparenti per lo stack di rete TCP\/IP. Nella figura seguente viene illustrata la relazione tra NLB e altri componenti software in una configurazione tipica.  
  
![Bilanciamento carico di rete e altri componenti software](../media/NLB/nlb.jpg)  
  
Di seguito sono riportate le funzionalità principali di NLB.  
  
- Per l'esecuzione di Bilanciamento carico di rete non sono necessarie modifiche all'hardware.  
  
- Sono disponibili gli Strumenti per Bilanciamento carico di rete per la configurazione e la gestione di più cluster e di tutti gli host da un unico computer locale o remoto.  
  
- Consente ai client di accedere al cluster utilizzando un unico nome Internet logico e un indirizzo IP virtuale, noto come indirizzo IP del cluster \(mantiene i singoli nomi per ogni computer\). Bilanciamento carico di rete supporta più indirizzi IP virtuali per server multihomed.  
  
> [!NOTE]  
> Quando si distribuiscono macchine virtuali come cluster virtuali, NLB non richiede che i server siano multihomed per disporre di più indirizzi IP virtuali.  
  
- Bilanciamento carico di rete può essere associato a più schede di rete. In questo modo è possibile configurare più cluster indipendenti in ogni host. Il supporto per più schede di rete è diverso rispetto ai cluster virtuali, poiché i cluster virtuali consentono di configurare più cluster in un'unica scheda di rete.  
  
- Bilanciamento carico di rete non richiede modifiche alle applicazioni server affinché possano essere eseguite in un cluster Bilanciamento carico di rete.  
  
- Può essere configurato per l'aggiunta automatica di un host al cluster se nel cluster host si verifica un errore e viene successivamente riconnesso. L'host aggiunto può iniziare a gestire nuove richieste server dai client.  
  
-   Consente di disconnettere i computer a scopo di manutenzione preventiva senza influenzare le operazioni cluster negli altri host.  
  
## <a name="hardware-requirements"></a>Requisiti hardware  
Di seguito sono riportati i requisiti hardware per l'esecuzione di un cluster NLB.  
  
-   Tutti gli host del cluster devono risiedere nella stessa subnet.  
  
-   Non esistono restrizioni per il numero di schede di rete in ogni host e host diversi possono disporre di un diverso numero di schede.  
  
-   Tutte le schede di rete di ogni cluster devono essere multicast o unicast. Bilanciamento carico di rete non supporta un ambiente misto di modalità unicast e multicast in un singolo cluster.  
  
-   Se si utilizza la modalità unicast, la scheda di rete utilizzata per gestire\-client per\-il traffico del cluster deve supportare la modifica dell'indirizzo\) Media Access Control \(MAC.  
  
## <a name="software-requirements"></a>Requisiti software  
Di seguito sono riportati i requisiti software per l'esecuzione di un cluster NLB.  
  
-   È possibile utilizzare solo TCP\/IP sull'adapter per il quale NLB è abilitato in ogni host. Non aggiungere altri protocolli \(ad esempio, IPX\) a questo adapter.  
  
-   Gli indirizzi IP dei server nel cluster devono essere statici.  
  
> [!NOTE]  
> NLB non supporta Dynamic Host Configuration Protocol \(\)DHCP. Bilanciamento carico di rete disabilita il protocollo DHCP in ogni interfaccia che configura.  
  
## <a name="installation-information"></a>Informazioni sull'installazione  
È possibile installare NLB utilizzando Server Manager o i comandi di Windows PowerShell per NLB.

Facoltativamente, è possibile installare gli Strumenti Bilanciamento carico di rete per gestire un cluster Bilanciamento carico di rete locale o remoto. Gli strumenti includono Gestione bilanciamento carico di rete e i comandi di Windows PowerShell per NLB.

### <a name="installation-with-server-manager"></a>Installazione con Server Manager

In Server Manager, è possibile utilizzare l'aggiunta guidata ruoli e funzionalità per aggiungere la funzionalità **bilanciamento carico di rete** . Quando si completa la procedura guidata, NLB viene installato e non è necessario riavviare il computer.


### <a name="installation-with-windows-powershell"></a>Installazione con Windows PowerShell  

Per installare NLB usando Windows PowerShell, eseguire il comando seguente a un prompt di Windows PowerShell con privilegi elevati nel computer in cui si vuole installare Bilanciamento carico di sistema.

    
    Install-WindowsFeature NLB -IncludeManagementTools
    
Al termine dell'installazione, non è necessario riavviare il computer.

Per altre informazioni, vedere [Install-WindowsFeature](https://docs.microsoft.com/powershell/module/servermanager/install-windowsfeature?view=win10-ps).

### <a name="network-load-balancing-manager"></a>Gestione bilanciamento carico di rete
Per aprire Gestione bilanciamento carico di rete in Server Manager, fare clic su **Strumenti** e quindi fare clic su **Gestione bilanciamento carico di rete**.
  
## <a name="additional-resources"></a>Risorse aggiuntive  
Nella tabella seguente vengono forniti i collegamenti a informazioni aggiuntive sulla funzionalità NLB.  
  
|Tipo di contenuto|Riferimenti|  
|----------------|--------------|  
|Distribuzione|[Guida](https://technet.microsoft.com/library/cc754833(WS.10).aspx) &#124; alla distribuzione di bilanciamento carico [di rete configurazione del bilanciamento del carico di rete con servizi Terminal](https://technet.microsoft.com/library/cc771300(v=WS.10).aspx)|  
|Operazioni|[Gestione dei cluster](https://technet.microsoft.com/library/cc753954(WS.10).aspx) &#124; di bilanciamento carico di rete [impostazione dei parametri](https://technet.microsoft.com/library/cc731619(WS.10).aspx) &#124; [di bilanciamento carico di rete controllo degli host in cluster di bilanciamento carico di rete](https://technet.microsoft.com/library/cc770870(WS.10).aspx)|  
|Risoluzione dei problemi|[Risoluzione dei problemi relativi ai cluster](https://technet.microsoft.com/library/cc732592(WS.10).aspx) &#124; di bilanciamento carico di rete bilanciamento carico [di rete eventi ed errori](https://technet.microsoft.com/library/cc731678(WS.10).aspx)|
|Strumenti e impostazioni|[Cmdlet di Windows PowerShell per bilanciamento carico di rete](https://go.microsoft.com/fwlink/p/?LinkId=238123)|
|Risorse della community|[Forum\) sul clustering di \(a disponibilità elevata](https://go.microsoft.com/fwlink/p/?LinkId=230641)
