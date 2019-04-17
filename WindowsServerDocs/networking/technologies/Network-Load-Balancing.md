---
title: Bilanciamento carico di rete
description: In questo argomento viene fornita una panoramica della funzionalità Bilanciamento carico di rete (NLB) in Windows Server 2016 e include collegamenti a ulteriori istruzioni sulla creazione, configurazione e gestione dei cluster di bilanciamento carico di rete.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-nlb
ms.topic: article
ms.assetid: 244a4b48-06e5-4796-8750-a50e4f88ac72
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b9fd39381316a8bcd06328e7aa75492ed99bc7f1
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="network-load-balancing"></a>Bilanciamento carico di rete

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

In questo argomento viene fornita una panoramica della funzionalità Bilanciamento carico di rete \(NLB\) in Windows Server 2016 e include collegamenti a ulteriori istruzioni sulla creazione, configurazione e gestione dei cluster di bilanciamento carico di rete.

È possibile utilizzare Bilanciamento carico di rete per gestire due o più server come un singolo cluster virtuale. Bilanciamento carico di rete consente di migliorare la disponibilità e scalabilità di applicazioni server Internet come quelle utilizzate in web, FTP, firewall, proxy, virtual private \(VPN\) e altri server mission\ critici di rete.   

>[!NOTE]
>Windows Server 2016 include un \(SLB\) bilanciamento del carico Software ispirazione Azure nuovo come componente dell'infrastruttura di rete definita dal Software \(SDN\). Utilizzare SLB invece di bilanciamento carico di rete se si utilizza SDN, utilizza i carichi di lavoro non Windows, conversione indirizzi di rete in uscita necessario \(NAT\), o necessitano di livello 3 \(L3\) o il bilanciamento del carico non basati su TCP. È possibile continuare a utilizzare Bilanciamento carico di rete con Windows Server 2016 per le distribuzioni non SDN. Per ulteriori informazioni su SLB, vedere [Software Load Balancing (SLB) per SDN](../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md).

La funzionalità Bilanciamento carico di rete \(NLB\) distribuisce il traffico tra più server utilizzando il protocollo di rete TCP/IP. Grazie alla combinazione di due o più computer che eseguono le applicazioni in un singolo cluster virtuale, Bilanciamento carico di rete offre affidabilità e prestazioni per i server web e altri server mission\-critical.  
  
Il server in un cluster di bilanciamento carico di rete sono denominati *host*, e ogni host esegue una copia separata delle applicazioni server. Bilanciamento carico di rete distribuisce le richieste client in ingresso tra gli host nel cluster. È possibile configurare il carico che deve essere gestito da ogni host. È anche possibile aggiungere host in modo dinamico al cluster per gestire l'incremento del carico. Bilanciamento carico di rete può inoltre indirizzare tutto il traffico a un unico host designato, viene chiamato il *host predefinito*.  
  
Bilanciamento carico di rete consente a tutti i computer del cluster da affrontare lo stesso set di indirizzi IP e mantiene un set di indirizzi IP dedicati univoci per ogni host. Per le applicazioni Load bilanciato, quando si verifica un errore o disconnessione di un host il carico viene automaticamente ridistribuito tra i computer ancora operativi. Quando è pronto, il computer offline può ricostituzione del cluster in modo trasparente e riottenere la propria parte del carico di lavoro, consentendo agli altri computer del cluster di gestire una minore quantità di traffico.  
  
## <a name="BKMK_APP"></a>Applicazioni pratiche  
Bilanciamento carico di rete è utile per garantire che le applicazioni, ad esempio server web che eseguono Internet Information Services \(IIS\), sono disponibili con tempo di inattività minimo e siano scalabili \ (aggiungendo altri server come increases\ il carico). Le sezioni seguenti descrivono come bilanciamento carico di rete supporta un'elevata disponibilità, scalabilità e la gestibilità dei server in cluster che eseguono tali applicazioni.  
  
### <a name="high-availability"></a>Disponibilità elevata  
Un sistema a disponibilità elevata in modo affidabile fornisce un livello di servizio con tempo di inattività minimo accettabile. Per garantire una disponibilità elevata, Bilanciamento carico di rete include funzionalità predefinita che può automaticamente:  
  
-   Rilevare un cluster host che si verifica un errore o disconnessione e quindi ripristinare.  
  
-   Bilanciamento del carico di rete quando vengono aggiunti o rimossi gli host.  
  
-   Ripristino e ridistribuzione del carico di lavoro entro dieci secondi.  
  
### <a name="scalability"></a>Scalabilità  
Scalabilità è la misura della come un computer, servizio o applicazione può raggiungere per soddisfare le richieste di prestazioni maggiori. Per i cluster di bilanciamento carico di rete, la scalabilità rappresenta la possibilità di aggiungere in modo incrementale uno o più sistemi a un cluster esistente quando il carico complessivo del cluster ne supera le capacità. Per supportare la scalabilità, è possibile eseguire le operazioni seguenti con bilanciamento carico di rete:  
  
-   Bilanciare le richieste di carico nel cluster di bilanciamento carico di rete per i singoli servizi TCP/IP.  
  
-   Supporta un massimo di 32 computer in un singolo cluster.  
  
-   Bilanciamento di più richieste di carico server \ (dallo stesso client o da diversi clients\) tra più host del cluster.  
  
-   Aggiungere host al cluster di bilanciamento carico di rete come all'aumento del carico, senza provocare errori nel cluster.  
  
-   Rimuovere gli host dal cluster quando il carico diminuisce.  
  
-   Abilitare prestazioni elevate e sovraccarico ridotto grazie a un'implementazione interamente in pipeline. Il pipelining consente di essere inviati al cluster Bilanciamento carico di rete senza attendere una risposta a una precedente richiesta richieste.  
  
### <a name="manageability"></a>Facilità di gestione  
Per supportare la gestibilità, è possibile eseguire le operazioni seguenti con bilanciamento carico di rete:  
  
-   Gestire e configurare più cluster di bilanciamento carico di rete e i cluster host da un singolo computer utilizzando Gestione bilanciamento carico di rete o [cmdlet di bilanciamento carico di rete (NLB) in Windows PowerShell](https://technet.microsoft.com/library/hh801274.aspx).
  
-   Specificare il comportamento per una singola porta IP o un gruppo di porte utilizzando regole di gestione di porta di bilanciamento del carico.  
  
-   Definire regole di porta diverse per ogni sito Web. Se si utilizza lo stesso set di server Load bilanciato per più applicazioni o siti Web, le regole di porta sono basate sull'indirizzo IP virtuale di destinazione \(using virtual clusters\).  

-   Indirizzare tutte le richieste dei client a un unico host mediante facoltativo, regole apartment-host. Bilanciamento carico di rete consente di indirizzare le richieste client a un determinato host che esegue applicazioni specifiche.  

-   Bloccare l'accesso di rete indesiderato a determinate porte IP.  

-   Abilitare il supporto Internet Group Management Protocol \(IGMP\) negli host del cluster per controllare la porta di commutazione \ (in cui i pacchetti di rete in ingresso vengono inviati a tutte le porte di switch\) quando si opera in modalità multicast.  

-   Avviare, arrestare e controllare le azioni di bilanciamento carico di rete in modalità remota utilizzando script o comandi di Windows PowerShell.  

-   Visualizzare il registro eventi di Windows per controllare gli eventi di bilanciamento carico di rete. Bilanciamento carico di rete registra tutte le azioni e delle modifiche del cluster nel registro eventi.  

## <a name="important-functionality"></a>Funzionalità importanti  
 
Bilanciamento carico di rete viene installato come un componente driver di rete standard di Windows Server. Le operazioni sono trasparenti per lo stack di rete TCP/IP. Nella figura seguente mostra la relazione tra Bilanciamento carico di rete e altri componenti software in una configurazione tipica.  
  
![Bilanciamento carico di rete e altri componenti software](../media/NLB/nlb.jpg)  
  
Di seguito sono le principali funzionalità di bilanciamento carico di rete.  
  
- Non richiedono modifiche hardware per l'esecuzione.  
  
- Fornisce strumenti bilanciamento carico di rete per configurare e gestire più cluster e tutti gli host da un unico computer locale o remoto.  
  
- Consente ai client di accedere al cluster utilizzando un unico nome Internet logico e un indirizzo IP virtuale, è noto come indirizzo IP del cluster \ (mantiene i singoli nomi di ogni computer\). Bilanciamento carico di rete consente più indirizzi IP virtuali per server multihomed.  
  
> [!NOTE]  
> Quando si distribuiscono le macchine virtuali come cluster virtuale, Bilanciamento carico di rete non richiede server siano multihomed per disporre di più indirizzi IP virtuali.  
  
- Consente di bilanciamento carico di rete da associare più schede di rete, che consente di configurare più cluster indipendenti in ogni host. Supporto per più schede di rete diversa rispetto ai cluster virtuali in cluster virtuali consentono di configurare più cluster in una singola scheda di rete.  
  
- Non richiede modifiche alle applicazioni server, in modo che possano essere eseguite in un cluster di bilanciamento carico di rete.  
  
- Può essere configurato per l'aggiunta automatica di un host al cluster se tale host del cluster non riesce e viene successivamente riconnesso. L'host aggiunto può iniziare a gestire nuove richieste server dai client.  
  
-   Consente di disconnettere i computer per la manutenzione preventiva senza influenzare le operazioni cluster negli altri host.  
  
## <a name="BKMK_HARD"></a>Requisiti hardware  
Di seguito sono i requisiti hardware per l'esecuzione di un cluster di bilanciamento carico di rete.  
  
-   Tutti gli host nel cluster devono risiedere nella stessa subnet.  
  
-   Non esiste alcuna restrizione al numero di schede di rete in ogni host e host diversi possono disporre di un diverso numero di schede.  
  
-   Ogni cluster, tutte le schede di rete devono essere multicast o unicast. Bilanciamento carico di rete non supporta un ambiente misto di unicast e multicast in un singolo cluster.  
  
-   Se si utilizza la modalità unicast, la scheda di rete che viene utilizzata per gestire il traffico di connessione tra client e da destra-cluster deve supportare la modifica il relativo indirizzo MAC \(MAC\).  
  
## <a name="BKMK_SOFT"></a>Requisiti software  
Di seguito sono i requisiti software per l'esecuzione di un cluster di bilanciamento carico di rete.  
  
-   Può essere utilizzato solo TCP/IP sulla scheda per cui Bilanciamento carico di rete è abilitata in ogni host. Non aggiungere altri protocolli \ (ad esempio, IPX\) a questa scheda.  
  
-   Gli indirizzi IP dei server nel cluster devono essere statici.  
  
> [!NOTE]  
> Bilanciamento carico di rete non supporta Dynamic Host Configuration Protocol \(DHCP\). Bilanciamento carico di rete disabilita il protocollo DHCP in ogni interfaccia che configura.  
  
## <a name="BKMK_INSTALL"></a>Informazioni di installazione  
È possibile installare Bilanciamento carico di rete con Server Manager o i comandi di Windows PowerShell per Bilanciamento carico di rete.

Facoltativamente è possibile installare gli strumenti bilanciamento del carico di rete per gestire un cluster di bilanciamento carico di rete locale o remoto. Gli strumenti includono Gestione bilanciamento carico di rete e i comandi di Windows PowerShell di bilanciamento carico di rete.

### <a name="installation-with-server-manager"></a>Installazione con Server Manager

In Server Manager, è possibile utilizzare l'aggiunta guidata ruoli e funzionalità per aggiungere il **bilanciamento carico di rete** funzionalità. Dopo aver completato la procedura guidata, Bilanciamento carico di rete viene installato e non è necessario riavviare il computer.


### <a name="installation-with-windows-powershell"></a>Installazione con Windows PowerShell  

Per installare Bilanciamento carico di rete utilizzando Windows PowerShell, eseguire il comando seguente al prompt di Windows PowerShell con privilegi elevati nel computer in cui si desidera installare Bilanciamento carico di rete.

    
    Install-WindowsFeature NLB -IncludeManagementTools
    
Al termine dell'installazione, non è necessario alcun riavvio del computer.

Per ulteriori informazioni, vedere [Install-WindowsFeature](https://technet.microsoft.com/library/jj205467.aspx).

### <a name="network-load-balancing-manager"></a>Gestione bilanciamento carico di rete
Per aprire Gestione bilanciamento carico di rete in Server Manager, fare clic su **strumenti**, quindi fare clic su **Gestione bilanciamento carico di rete**.
  
## <a name="BKMK_LINKS"></a>Risorse aggiuntive  
Nella tabella seguente vengono forniti collegamenti a informazioni aggiuntive sulla funzionalità di bilanciamento carico di rete.  
  
|Tipo di contenuto|Riferimenti|  
|----------------|--------------|  
|Distribuzione|[Guida alla distribuzione di bilanciamento carico di rete](https://technet.microsoft.com/library/cc754833(WS.10).aspx) & #124; [La configurazione di bilanciamento carico di rete con i servizi Terminal](https://technet.microsoft.com/library/cc771300(v=WS.10).aspx)|  
|Operazioni|[Gestione cluster di bilanciamento carico di rete](https://technet.microsoft.com/library/cc753954(WS.10).aspx) & #124; [L'impostazione di parametri di bilanciamento carico di rete](https://technet.microsoft.com/library/cc731619(WS.10).aspx) & #124; [Controllo degli host in cluster di bilanciamento carico di rete](https://technet.microsoft.com/library/cc770870(WS.10).aspx)|  
|Risoluzione dei problemi|[Risoluzione dei problemi di cluster di bilanciamento carico di rete](https://technet.microsoft.com/library/cc732592(WS.10).aspx) & #124; [Errori ed eventi Cluster di bilanciamento carico di rete](https://technet.microsoft.com/library/cc731678(WS.10).aspx)|
|Strumenti e impostazioni|[Cmdlet PowerShell di Windows di bilanciamento del carico di rete](https://go.microsoft.com/fwlink/p/?LinkId=238123)|
|Risorse della community|[Forum \(Clustering\) la disponibilità elevata](https://go.microsoft.com/fwlink/p/?LinkId=230641)