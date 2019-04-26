---
title: Bilanciamento carico di rete
description: In questo argomento, ti offriamo una panoramica del bilanciamento carico di rete \(NLB\) funzionalità in Windows Server 2016. È possibile utilizzare Bilanciamento carico di rete per gestire due o più server come un singolo cluster virtuale. Bilanciamento carico di rete consente di migliorare la disponibilità e scalabilità delle applicazioni server Internet come quelli usati sul web, FTP, firewall, proxy, rete privata virtuale \(VPN\)e altri mission\-server critici.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-nlb
ms.topic: article
ms.assetid: 244a4b48-06e5-4796-8750-a50e4f88ac72
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: d0cf1e1d6b1681a0f18908b08cd17572159e0462
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881752"
---
# <a name="network-load-balancing"></a>Bilanciamento carico di rete

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo argomento, ti offriamo una panoramica del bilanciamento carico di rete \(NLB\) funzionalità in Windows Server 2016. È possibile utilizzare Bilanciamento carico di rete per gestire due o più server come un singolo cluster virtuale. Bilanciamento carico di rete consente di migliorare la disponibilità e scalabilità delle applicazioni server Internet come quelli usati sul web, FTP, firewall, proxy, rete privata virtuale \(VPN\)e altri mission\-server critici.  

>[!NOTE]
>Windows Server 2016 include un nuovo ispirata Azure Software Load Balancer \(SLB\) come componente di Software Defined Networking \(SDN\) dell'infrastruttura. Usare SLB invece di bilanciamento carico di rete se si usa SDN, Usa i carichi di lavoro non-Windows, necessario NAT in uscita \(NAT\), o livello 3 è necessario \(L3\) o il bilanciamento del carico non basati su TCP. È possibile continuare a usare bilanciamento carico di rete con Windows Server 2016 per le distribuzioni non SDN. Per altre informazioni sul bilanciamento del carico software, vedere [bilanciamento del carico Software (SLB) per SDN](../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md).

Il bilanciamento del carico di rete \(NLB\) funzionalità distribuisce il traffico tra più server utilizzando il protocollo TCP\/protocollo di rete IP. Combinando due o più computer che eseguono le applicazioni in un singolo cluster virtuale, Bilanciamento carico di rete offre affidabilità e prestazioni per server web e altri mission\-server critici.  
  
I server inclusi in un cluster Bilanciamento del carico di rete sono denominati *host*e ogni host esegue una copia separata delle applicazioni server. Le richieste client in ingresso vengono distribuite da Bilanciamento carico di rete tra gli host del cluster. È possibile configurare il carico che deve essere gestito da ogni host. È inoltre possibile aggiungere host al cluster in modo dinamico per gestire un incremento del carico. Bilanciamento carico di rete può inoltre indirizzare tutto il traffico a un unico host designato, denominato *host predefinito*.  
  
Bilanciamento carico di rete consente di associare a tutti i computer del cluster lo stesso set di indirizzi IP del cluster, mantenendo un set di indirizzi IP dedicati univoci per ogni host. Per il carico\-bilanciato delle applicazioni, quando un host ha esito negativo o è offline, il carico viene automaticamente ridistribuito tra i computer ancora operativi. Quando il computer offline è pronto, può essere aggiunto nuovamente al cluster in modo trasparente e riottenere la propria parte del carico di lavoro, consentendo agli altri computer del cluster di gestire una minore quantità di traffico.  
  
## <a name="practical-applications"></a>Applicazioni pratiche  
Bilanciamento carico di rete è utile per garantire che senza stato applicazioni, ad esempio server web che eseguono Internet Information Services \(IIS\), sono disponibile con tempi di inattività minimi e che siano scalabili \(aggiungendo altri server come l'aumento del carico\). Nelle sezioni seguenti viene descritto il modo in cui Bilanciamento carico di rete supporta la disponibilità elevata, la scalabilità e la gestibilità dei server in cluster che eseguono tali applicazioni.  
  
### <a name="high-availability"></a>Disponibilità elevata  
Un sistema a disponibilità elevata offre un livello accettabile e affidabile di servizio con tempo di inattività minimo. Per garantire la disponibilità elevata, Bilanciamento carico di rete include compilato\-nella funzionalità che può essere automaticamente:  
  
-   Rilevamento di host del cluster con errori o offline e ripristino.  
  
-   Bilanciamento del carico di rete in caso di aggiunta o rimozione di host.  
  
-   Ripristino e ridistribuzione del carico di lavoro entro dieci secondi.  
  
### <a name="scalability"></a>Scalabilità  
La scalabilità indica la possibilità di espansione di un computer, un servizio o un'applicazione allo scopo di soddisfare crescenti esigenze in termini di prestazioni. Per i cluster di Bilanciamento carico di rete, la scalabilità rappresenta la possibilità di aggiungere in modo incrementale uno o più sistemi a un cluster esistente quando il carico complessivo del cluster ne supera le capacità. Per supportare la scalabilità, Bilanciamento carico di rete consente di eseguire le operazioni seguenti:  
  
-   Bilanciare le richieste di carico nel cluster di bilanciamento carico di rete per TCP singole\/servizi IP.  
  
-   Supporto di un massimo di 32 computer in un singolo cluster.  
  
-   Bilanciare le richieste di carico più server \(dallo stesso client o tramite svariati client\) tra più host nel cluster.  
  
-   Aggiunta di host al cluster Bilanciamento carico di rete in base all'aumento del carico, senza provocare errori nel cluster.  
  
-   Rimozione di host dal cluster quando il carico diminuisce.  
  
-   Prestazioni elevate e sovraccarico ridotto grazie a un'implementazione interamente in pipeline. L'utilizzo di una pipeline consente di inviare richieste al cluster di Bilanciamento carico di rete senza attendere la risposta alla richiesta precedentemente inviata.  
  
### <a name="manageability"></a>Gestibilità  
Per supportare la gestibilità, Bilanciamento carico di rete consente di eseguire le operazioni seguenti:  
  
-   Gestire e configurare più cluster di bilanciamento carico di rete e i cluster host da un singolo computer utilizzando Gestione bilanciamento carico di rete o la [cmdlet di bilanciamento del carico di rete (NLB) in Windows PowerShell](https://technet.microsoft.com/library/hh801274.aspx).
  
-   Impostazione del comportamento di bilanciamento del carico per una singola porta IP o un gruppo di porte utilizzando regole di gestione delle porte.  
  
-   Definizione di regole di porta diverse per ogni sito Web. Se si usa lo stesso set di carico\-bilanciate server per più applicazioni o siti Web, le regole di porta sono basati su indirizzo IP virtuale di destinazione \(utilizzando cluster virtuali\).  

-   Indirizzare tutte le richieste client a un singolo host usando facoltativo, singolo\-ospitare le regole. Le richieste client vengono indirizzate da Bilanciamento carico di rete a un determinato host che esegue applicazioni specifiche.  

-   Blocco dell'accesso di rete indesiderato a determinate porte IP.  

-   Abilitare Internet Group Management Protocol \(IGMP\) supporto nei cluster host per controllare la porta di commutazione \(in cui vengono inviati i pacchetti di rete in ingresso a tutte le porte del commutatore\) quando si opera in modalità multicast.  

-   Avvio, arresto e controllo delle azioni di Bilanciamento carico di rete in modalità remota utilizzando i comandi o gli script di Windows PowerShell.  

-   Visualizzazione del registro eventi di Windows per controllare gli eventi di Bilanciamento carico di rete. Tutte le azioni e le modifiche al cluster vengono registrate da Bilanciamento carico di rete nel registro eventi.  

## <a name="important-functionality"></a>Funzionalità importanti  
 
Bilanciamento carico di rete viene installato come un componente driver di rete standard di Windows Server. Le operazioni sono trasparenti per il protocollo TCP\/stack di rete IP. La figura seguente mostra la relazione tra Bilanciamento carico di rete e altri componenti software in una configurazione tipica.  
  
![Bilanciamento del carico di rete e altri componenti software](../media/NLB/nlb.jpg)  
  
Di seguito sono le principali funzionalità di bilanciamento carico di rete.  
  
- Per l'esecuzione di Bilanciamento carico di rete non sono necessarie modifiche all'hardware.  
  
- Sono disponibili gli Strumenti per Bilanciamento carico di rete per la configurazione e la gestione di più cluster e di tutti gli host da un unico computer locale o remoto.  
  
- Consente ai client di accedere al cluster usando un unico nome Internet logico e un indirizzo IP virtuale, che è noto come indirizzo IP del cluster \(mantiene i singoli nomi di ogni computer\). Bilanciamento carico di rete supporta più indirizzi IP virtuali per server multihomed.  
  
> [!NOTE]  
> Quando si distribuiscono le macchine virtuali come cluster virtuale, Bilanciamento carico di rete richiede che i server siano multihomed per disporre di più indirizzi IP virtuali.  
  
- Bilanciamento carico di rete può essere associato a più schede di rete. In questo modo è possibile configurare più cluster indipendenti in ogni host. Il supporto per più schede di rete è diverso rispetto ai cluster virtuali, poiché i cluster virtuali consentono di configurare più cluster in un'unica scheda di rete.  
  
- Bilanciamento carico di rete non richiede modifiche alle applicazioni server affinché possano essere eseguite in un cluster Bilanciamento carico di rete.  
  
- Può essere configurato per l'aggiunta automatica di un host al cluster se nel cluster host si verifica un errore e viene successivamente riconnesso. L'host aggiunto può iniziare a gestire nuove richieste server dai client.  
  
-   Consente di disconnettere i computer a scopo di manutenzione preventiva senza influenzare le operazioni cluster negli altri host.  
  
## <a name="hardware-requirements"></a>Requisiti hardware  
Di seguito sono i requisiti hardware per eseguire un cluster Bilanciamento carico di rete.  
  
-   Tutti gli host del cluster devono risiedere nella stessa subnet.  
  
-   Non esistono restrizioni per il numero di schede di rete in ogni host e host diversi possono disporre di un diverso numero di schede.  
  
-   Tutte le schede di rete di ogni cluster devono essere multicast o unicast. Bilanciamento carico di rete non supporta un ambiente misto di modalità unicast e multicast in un singolo cluster.  
  
-   Se si usa la modalità unicast, la scheda di rete che viene utilizzata per gestire i client\-al\-traffico del cluster deve supportare modificandone il controllo di accesso multimediale \(MAC\) indirizzo.  
  
## <a name="software-requirements"></a>Requisiti software  
Di seguito sono i requisiti software per l'esecuzione di un cluster Bilanciamento carico di rete.  
  
-   Solo il protocollo TCP\/IP può essere usato nella scheda per il quale bilanciamento carico di rete è abilitata in ogni host. Non aggiungere altri protocolli \(ad esempio IPX\) a questo adapter.  
  
-   Gli indirizzi IP dei server nel cluster devono essere statici.  
  
> [!NOTE]  
> Bilanciamento carico di rete non supporta Dynamic Host Configuration Protocol \(DHCP\). Bilanciamento carico di rete disabilita il protocollo DHCP in ogni interfaccia che configura.  
  
## <a name="installation-information"></a>Informazioni sull'installazione  
È possibile installare Bilanciamento carico di rete con Server Manager o i comandi di Windows PowerShell per Bilanciamento carico di rete.

Facoltativamente, è possibile installare gli Strumenti Bilanciamento carico di rete per gestire un cluster Bilanciamento carico di rete locale o remoto. Gli strumenti includono Gestione bilanciamento carico di rete e i comandi di PowerShell di Windows di bilanciamento carico di rete.

### <a name="installation-with-server-manager"></a>Installazione con Server Manager

In Server Manager, è possibile usare l'aggiunta guidata ruoli e funzionalità per aggiungere il **bilanciamento carico di rete** funzionalità. Dopo aver completato la procedura guidata, Bilanciamento carico di rete viene installato e non è necessario riavviare il computer.


### <a name="installation-with-windows-powershell"></a>Installazione con Windows PowerShell  

Per installare Bilanciamento carico di rete tramite Windows PowerShell, eseguire il comando seguente al prompt di Windows PowerShell con privilegi elevati nel computer in cui si vuole installare Bilanciamento carico di rete.

    
    Install-WindowsFeature NLB -IncludeManagementTools
    
Al termine dell'installazione, non è necessario alcun riavvio del computer.

Per altre informazioni, vedere [Install-WindowsFeature](https://docs.microsoft.com/powershell/module/servermanager/install-windowsfeature?view=win10-ps).

### <a name="network-load-balancing-manager"></a>Gestione del bilanciamento del carico di rete
Per aprire Gestione bilanciamento carico di rete in Server Manager, fare clic su **Strumenti** e quindi fare clic su **Gestione bilanciamento carico di rete**.
  
## <a name="additional-resources"></a>Risorse aggiuntive  
Nella tabella seguente vengono forniti collegamenti a informazioni aggiuntive sulla funzionalità Bilanciamento carico di rete.  
  
|Tipo di contenuto|Riferimenti|  
|----------------|--------------|  
|Distribuzione|[Guida alla distribuzione di bilanciamento del carico di rete](https://technet.microsoft.com/library/cc754833(WS.10).aspx) &#124; [configurazione di bilanciamento carico di rete con servizi Terminal](https://technet.microsoft.com/library/cc771300(v=WS.10).aspx)|  
|Operazioni|[Gestione dei cluster di bilanciamento carico di rete](https://technet.microsoft.com/library/cc753954(WS.10).aspx) &#124; [impostando i parametri di bilanciamento del carico di rete](https://technet.microsoft.com/library/cc731619(WS.10).aspx) &#124; [controllare gli host in cluster di bilanciamento carico di rete](https://technet.microsoft.com/library/cc770870(WS.10).aspx)|  
|Risoluzione dei problemi|[Risoluzione dei problemi dei cluster di bilanciamento del carico di rete](https://technet.microsoft.com/library/cc732592(WS.10).aspx) &#124; [NLB Cluster Events and Errors](https://technet.microsoft.com/library/cc731678(WS.10).aspx)|
|Strumenti e impostazioni|[I cmdlet di PowerShell di Windows di bilanciamento del carico di rete](https://go.microsoft.com/fwlink/p/?LinkId=238123)|
|Risorse della community|[Disponibilità elevata \(Clustering\) Forum](https://go.microsoft.com/fwlink/p/?LinkId=230641)
