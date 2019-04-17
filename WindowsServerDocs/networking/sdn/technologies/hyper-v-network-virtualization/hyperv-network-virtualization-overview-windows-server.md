---
title: Panoramica di virtualizzazione rete Hyper-V in Windows Server 2016
description: In questo argomento viene fornita una panoramica di virtualizzazione rete Hyper-V in Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0115b7ad-d229-4c69-9d7e-a3f5fbaa3b2f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c7c20f9b2d81ac2d49ed0bbea5f3aca48dea0c50
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="hyper-v-network-virtualization-overview-in-windows-server-2016"></a>Panoramica di virtualizzazione rete Hyper-V in Windows Server 2016

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

In Windows Server 2016 e Virtual Machine Manager, Microsoft offre una soluzione di virtualizzazione di rete end-to-end.  Esistono cinque principali componenti che costituiscono una soluzione di virtualizzazione di rete Microsoft:  
  
-   **Windows Azure Pack per Windows Server** offre un tenant portale per creare reti virtuali e un portale di amministrazione per gestire le reti virtuali.  
  
-   **Virtual Machine Manager** (VMM) consente una gestione centralizzata dell'infrastruttura di rete.  
  
-   **Controller di rete Microsoft** offre un punto programmabile e centralizzato di automazione per gestire, configurare, monitorare e risolvere i problemi dell'infrastruttura di rete fisica e virtuale nel centro dati.  
  
-   **Virtualizzazione rete Hyper-V** offre l'infrastruttura necessaria per virtualizzare il traffico di rete.  
  
-   **Gateway di virtualizzazione rete Hyper-V** forniscono connessioni tra reti virtuali e fisiche.  
  
In questo argomento vengono presentati i concetti e le funzionalità di virtualizzazione di rete Hyper-V (una parte della soluzione di virtualizzazione di rete globale) in Windows Server 2016 e vantaggi principali. Spiega come virtualizzazione di rete vantaggi sia ai cloud privati cercando consolidamento del carico di lavoro aziendale e provider di servizi cloud pubblico dell'infrastruttura come servizio (IaaS).  
  
Per ulteriori dettagli tecnici sulla virtualizzazione di rete in Windows Server 2016, vedere [Hyper-V rete dettagli tecnici sulla virtualizzazione in Windows Server 2016](../../../sdn/technologies/hyper-v-network-virtualization/hyperv-network-virtualization-technical-details-windows-server.md).  
  
**Si intendeva**  
  
-   [Panoramica di virtualizzazione rete Hyper-V](assetId:///bf1dba9d-1960-4dd2-a5e2-99466a02044b) (Windows Server 2012 R2)  
  
-   [Panoramica di Hyper-V](assetId:///5aad349f-ef06-464a-b36f-366fbb040143)  
  
-   [Panoramica sul commutatore virtuale Hyper-V](assetId:///e6ec46af-6ef4-49b3-b1f1-5268dc03f05b)  
  
## <a name="BKMK_OVER"></a>Descrizione delle funzionalità  
Virtualizzazione rete Hyper-V fornisce "reti virtuali" (denominate reti VM) alle macchine virtuali simile a come virtualizzazione di server (hypervisor) fornisce "macchine virtuali" per il sistema operativo. Virtualizzazione di rete disaccoppia le reti virtuali dall'infrastruttura di rete fisica e rimuove i vincoli della VLAN e assegnazione di indirizzi IP gerarchico dal provisioning delle macchine virtuali. Questa flessibilità è semplice per i clienti possono spostare nei cloud IaaS efficace per l'hosting e amministratori del centro dati gestire l'infrastruttura, mantenendo il necessario isolamento multi-tenant, i requisiti di sicurezza e gli indirizzi IP di macchine virtuali sovrapposti supporto.  
  
I clienti desiderano estendere facilmente i propri centri dati nel cloud. Rendere tali architetture cloud ibride di oggi sono disponibili alcune difficoltà tecniche. Uno dei principali ostacoli del affrontati dai clienti è riutilizzare le topologie di rete esistenti (subnet IP indirizzi, servizi di rete e così via) nel cloud e il bridging tra le risorse locali e le risorse cloud.  Virtualizzazione rete Hyper-V fornisce il concetto di una rete VM indipendente dalla rete fisica sottostante. Con questo concetto di una rete VM, costituito da uno o più subnet virtuali, la posizione esatta nella rete fisica di macchine virtuali collegate a una rete virtuale è disaccoppiata dalla topologia della rete virtuale. Di conseguenza, i clienti possono spostare agevolmente le subnet virtuali nel cloud mantenendo gli indirizzi IP e relative topologia nel cloud in modo che i servizi esistenti di continuare a funzionare anche senza conoscere la posizione fisica delle subnet. Virtualizzazione rete Hyper-V, consente un cloud ibrido di facile attuazione.  
  
Oltre al cloud ibrido, molte organizzazioni sono consolidare i propri centri dati e la creazione di cloud privati per ottenere internamente il vantaggio di efficienza e scalabilità di architetture cloud. Virtualizzazione rete Hyper-V consente maggiore flessibilità ed efficienza per cloud privati tramite il disaccoppiamento della business unit topologia di rete (da resa virtuale) dalla topologia della rete fisica effettiva. In questo modo, le business unit possono condividere un cloud interno privato restando comunque isolate una da altra facilmente e continuare a mantenere le topologie di rete esistente. Il team operativo Data Center dispone di flessibilità di distribuire e spostare in modo dinamico i carichi di lavoro all'interno del centro dati senza interruzioni di server fornendo efficienze operative e un complessiva del centro dati.  
  
Per i proprietari del carico di lavoro, il vantaggio principale è che è possibile spostare le "topologie" del carico di lavoro nel cloud senza modificare gli indirizzi IP o riscrivere l'applicazioni. Ad esempio, una tipica applicazione LOB a tre livelli è composta da livello front end, un livello di logica di business e livello di database. Tramite criteri di virtualizzazione rete Hyper-V consente cliente onboarding tutti o parte di tre livelli nel cloud mantenendo la topologia di routing e gli indirizzi IP dei servizi (vale a dire macchina virtuale gli indirizzi IP), senza richiedere le applicazioni da modificare.  
  
Per i proprietari dell'infrastruttura, la maggiore flessibilità nella selezione host per macchina virtuale consente di spostare i carichi di lavoro in qualsiasi punto del centro dati senza modificare le macchine virtuali o riconfigurare le reti. Ad esempio virtualizzazione rete Hyper-V consente di migrazione in tempo reale tra subnet in modo che una macchina virtuale può migrazione in tempo reale in qualsiasi punto del centro dati senza alcuna interruzione del servizio. In precedenza, migrazione in tempo reale era limitata alla stessa subnet limitazione in cui è possibile trovare le macchine virtuali. Tra subnet migrazione in tempo reale consente agli amministratori di consolidare i carichi di lavoro in base ai requisiti di risorse dinamiche, efficienza energetica e permettere la manutenzione dell'infrastruttura senza interrompere i carichi di lavoro dei clienti tempo di attività.  
  
## <a name="BKMK_APP"></a>Applicazioni pratiche  
Con la diffusione dei centri dati virtualizzati, le organizzazioni IT e ai provider di hosting (ovvero provider che offrono servizi di collocazione o noleggio di server fisici) hanno incominciato a offrire infrastrutture virtualizzate più flessibili che rendono più semplice offrire le istanze di server su richiesta ai propri clienti. Questa nuova classe di servizio è detta infrastruttura distribuita come servizio (IaaS). Windows Server 2016 fornisce tutte le funzionalità di piattaforma necessarie per consentire ai clienti aziendali di creare cloud privati ed eseguire la transizione a un oggetto come un modello operativo di servizio. Windows Server 2016 2016 consente inoltre di hosting di realizzare cloud pubblici e offrire soluzioni IaaS ai propri clienti. In combinazione con Virtual Machine Manager e Windows Azure Pack per gestire criteri di virtualizzazione rete Hyper-V, Microsoft offre una soluzione cloud potente.  
  
Virtualizzazione rete Hyper-V di Windows Server 2016 fornisce la virtualizzazione di rete basata su criteri e controllata software che consente di ridurre il sovraccarico colpisce le aziende impegnate nell'espansione di cloud IaaS dedicati e fornisce hoster cloud migliori flessibilità e scalabilità per la gestione di macchine virtuali per un utilizzo ottimale delle risorse di gestione.  
  
Uno scenario IaaS costituito da macchine virtuali da diverse divisioni aziendali (cloud dedicato) o da diversi clienti (cloud in hosting) è necessario un isolamento sicuro. Soluzione di oggi, reti locali virtuali (VLAN), può presentare svantaggi significativi in questo scenario.  
  
**VLAN**  
  
Attualmente, le reti VLAN costituiscono il meccanismo utilizzato da gran parte delle organizzazioni per supportare il riutilizzo dello spazio di indirizzi e l'isolamento del tenant. Una VLAN Usa la codifica esplicita (ID VLAN) nelle intestazioni dei frame Ethernet e si affidano ai commutatori Ethernet per applicare l'isolamento e limitare il traffico ai nodi di rete con lo stesso ID VLAN. I principali svantaggi delle VLAN sono i seguenti:  
  
-   Maggiore rischio di interruzione accidentale dovuto alle difficoltà di riconfigurazione dei commutatori di produzione ogni volta che sposta le macchine virtuali o i confini di isolamento del centro dati dinamico.  
  
-   Scalabilità limitata poiché non esiste un massimo di 4094 VLAN e normali commutatori supportano non più di 1000 ID VLAN.  
  
-   Vincolo di una singola subnet IP, che limita il numero di nodi entro un'unica VLAN e limitazione del posizionamento delle macchine virtuali in base alle posizioni fisiche. Anche se è possibile espandere le VLAN tra siti, la VLAN deve trovarsi interamente sulla stessa subnet.  
  
**Assegnazione di indirizzi IP**  
  
Oltre agli svantaggi presentati dalle VLAN, virtual machine assegnazione di indirizzi IP presenta problemi, quali:  
  
-   Le posizioni fisiche nell'infrastruttura di rete di Data Center determinano gli indirizzi IP di macchina virtuale. Di conseguenza, lo spostamento nel cloud in genere richiede la modifica degli indirizzi IP dei carichi di lavoro del servizio.  
  
-   I criteri sono legati agli indirizzi IP, ad esempio le regole del firewall, risorse individuazione e servizi di directory e così via. Modifica degli indirizzi IP, è necessario aggiornare tutti i criteri associati.  
  
-   Distribuzione della macchina virtuale e isolamento del traffico dipendono la topologia.  
  
Quando gli amministratori di rete di centri dati pianificano il layout fisico del centro dati, devono decidere su in subnet verranno fisicamente inserite e indirizzate. Queste decisioni sono basate su IP e tecnologia Ethernet che influenzano i potenziali indirizzi IP consentiti per le macchine virtuali in esecuzione in un determinato server o una blade che è connesso a un determinato rack del centro dati. Quando una macchina virtuale è stato eseguito il provisioning e inserita nel Data Center, deve rispettare le scelte e le limitazioni riguardanti l'indirizzo IP. Di conseguenza, il risultato tipico è che gli amministratori di datacenter assegnano nuovi indirizzi IP alle macchine virtuali.  
  
Il problema con questo requisito è che oltre a essere un indirizzo, sia le informazioni di carattere semantico associate a un indirizzo IP. Ad esempio, una subnet può contenere determinati servizi o trovarsi in una posizione fisica diversa. Le regole del firewall, criteri di controllo di accesso e associazioni di sicurezza IPsec sono comunemente associate con gli indirizzi IP. Modifica degli indirizzi IP impone i proprietari della macchina virtuale a regolare i criteri basati sull'indirizzo IP originario. Sovraccarico di renumerazione è talmente elevato che molte aziende preferiscono distribuire solo nuovi servizi cloud, lasciare le applicazioni legacy.  
  
Virtualizzazione rete Hyper-V disaccoppia le reti virtuali per le macchine virtuali dei clienti dall'infrastruttura di rete fisica. Di conseguenza, consente di macchine virtuali dei clienti mantenere gli indirizzi IP originali, consentendo agli amministratori di datacenter di eseguire il provisioning delle macchine virtuali dei clienti in un punto qualsiasi nel centro dati senza riconfigurare fisico gli indirizzi IP o gli ID VLAN. La sezione successiva sono riepilogate le funzionalità principali.  
  
## <a name="BKMK_NEW"></a>Funzionalità importanti  
Di seguito è riportato un elenco delle funzionalità di virtualizzazione rete Hyper-V in Windows Server 2016, principali funzionalità e vantaggi:  
  
-   **Consente il posizionamento flessibile del carico di lavoro - isolamento rete e riutilizzo degli indirizzi IP senza VLAN**  
  
    Virtualizzazione rete Hyper-V disaccoppia le reti virtuali dall'infrastruttura di rete fisica di hosting, consentendo maggiore libertà per il posizionamento del carico di lavoro all'interno dei centri dati del cliente. Posizionamento del carico di lavoro delle macchine virtuali non è più limitato dall'assegnazione di indirizzi IP o requisiti di isolamento VLAN della rete fisica perché viene applicato all'interno di host Hyper-V in base ai criteri di virtualizzazione multi-tenant definiti da software.  
  
    Le macchine virtuali di clienti diversi con indirizzi IP sovrapposti ora può essere distribuite nello stesso server host senza richiedere complicate configurazioni della VLAN, né violare la gerarchia di indirizzi IP. In questo modo si velocizza la migrazione dei carichi di lavoro dei clienti in provider, i clienti possono spostare i carichi di lavoro senza alcuna modifica, che include mantenendo gli indirizzi IP di macchina virtuale invariati di hosting IaaS condivisi. Per il provider di hosting, supporto gran numero di clienti che desiderano estendere gli spazi degli indirizzi di rete esistente per il centro dati IaaS condiviso è un'impresa di configurazione e mantenere VLAN isolate per ogni cliente per garantire la coesistenza di spazi degli indirizzi potenzialmente sovrapposti. Con virtualizzazione rete Hyper-V, supporto degli indirizzi sovrapposti è reso più semplice e richiede meno riconfigurazione della rete dal provider di hosting.  
  
    Inoltre, gli aggiornamenti e la manutenzione dell'infrastruttura fisica possono essere eseguiti senza causare l'interruzione dei carichi di lavoro dei clienti. Con virtualizzazione rete Hyper-V, è possibile migrare le macchine virtuali in un host specifico, rack, subnet, VLAN o intero cluster senza richiedere una modifica dell'indirizzo IP fisico o estesa riconfigurazione.  
  
-   **Semplifica lo spostamento dei carichi di lavoro in un cloud IaaS condiviso**  
  
    Con virtualizzazione rete Hyper-V, gli indirizzi IP e configurazioni di macchine virtuali restano invariate. In questo modo le organizzazioni IT possono spostare agevolmente i carichi di lavoro dai propri centri dati a un provider di hosting IaaS condiviso con interventi minimi di riconfigurazione del carico di lavoro o gli strumenti di infrastruttura e i criteri. Nei casi in cui vi sia connettività tra due centri dati, gli amministratori IT possono continuare a utilizzare i propri strumenti senza riconfigurarli.  
  
-   **Consente di migrazione in tempo reale tra subnet**  
  
    Migrazione in tempo reale dei carichi di lavoro macchine virtuali era tradizionalmente limitata alla stessa subnet IP o VLAN perché il passaggio tra subnet richiedeva sistema operativo di guest della macchina virtuale per modificare l'indirizzo IP. Questa modifica indirizzo interruzioni di comunicazione esistente e interrompe i servizi in esecuzione nella macchina virtuale. Con virtualizzazione rete Hyper-V, i carichi di lavoro possono essere animati migrati dal server che eseguono Windows Server 2016 in una subnet ai server che eseguono Windows Server 2016 in un'altra subnet senza modificarne gli indirizzi IP di carico di lavoro. Virtualizzazione rete Hyper-V assicura aggiornamento e sincronizzazione tra gli host in comunicazione continua con la macchina virtuale migrata che modifica del percorso di macchina virtuale a causa di migrazione in tempo reale.  
  
-   **Semplifica la gestione dell'amministrazione di server e rete disaccoppiato**  
  
    Posizionamento del carico di lavoro server è più semplice, perché migrazione e posizionamento dei carichi di lavoro sono indipendenti delle configurazioni di rete fisica sottostante. Gli amministratori del server è possibile concentrarsi sulla gestione di servizi e i server e gli amministratori di rete possono concentrarsi sulla gestione di infrastruttura e il traffico di rete globale. In questo modo gli amministratori di datacenter server distribuire e migrare le macchine virtuali senza modificarne gli indirizzi IP delle macchine virtuali. Sovraccarico è ridotto, perché virtualizzazione rete Hyper-V consente di posizionamento delle macchine virtuali indipendentemente dalla topologia di rete, riducendo la necessità per gli amministratori di rete da affrontare posizionamenti che potrebbero modificare i confini di isolamento.  
  
-   **Semplifica la rete e migliora l'utilizzo delle risorse di rete/del server**  
  
    La rigidità delle VLAN e la dipendenza del posizionamento delle macchine virtuali in un'infrastruttura di rete fisica provocano provisioning eccessivo e sottoutilizzo. Interrompendo la dipendenza, la maggiore flessibilità di posizionamento del carico di lavoro delle macchine virtuali può semplificare la gestione di rete e migliorare i server e utilizzo delle risorse di rete. Tieni presente che virtualizzazione rete Hyper-V supporta le VLAN nel contesto del centro dati fisico. Ad esempio, un centro dati potrebbe essere tutto il traffico di virtualizzazione rete Hyper-V in una determinata VLAN.  
  
-   **È compatibile con l'infrastruttura esistente e le tecnologie più nuove**  
  
    Virtualizzazione rete Hyper-V può essere distribuita nei centri dati odierni, ma è compatibile con tecnologie "flat network" datacenter emergenti.  
  
    Ad esempio, virtualizzazione rete in Windows Server 2016 supporta il formato di incapsulamento VXLAN e il vSwitch aprire Database Management Protocol (OVSDB) come interfaccia SouthBound (SBI).  
  
-   **Offre interoperabilità ed ecosistema idoneità**  
  
    Virtualizzazione rete Hyper-V supporta più configurazioni per la comunicazione con le risorse esistenti, ad esempio cross-premise connettività, rete di archiviazione (SAN), l'accesso alle risorse non virtualizzati e così via. Microsoft si impegna a collaborare con partner di ecosistema per supportare e migliorare l'esperienza di virtualizzazione rete Hyper-V in termini di gestibilità, scalabilità e prestazioni.  
  
-   **Configurazione basata su criteri**  
  
    Criteri di virtualizzazione di rete in Windows Server 2016 sono configurati tramite il Controller di rete Microsoft. Il controller di rete ha una RESTful northbound API e l'interfaccia di Windows PowerShell per configurare i criteri. Per ulteriori informazioni sui Controller di rete Microsoft, vedere [Controller di rete](../../../sdn/technologies/network-controller/../../../sdn/technologies/network-controller/Network-Controller.md).  
  
## <a name="BKMK_SOFT"></a>Requisiti software  
Virtualizzazione rete Hyper-V tramite il Controller di rete Microsoft richiede Windows Server 2016 e il ruolo Hyper-V.  
  
## <a name="BKMK_LINKS"></a>Vedere anche  
Per ulteriori informazioni sulla virtualizzazione rete Hyper-V in Windows Server 2016, vedere i collegamenti seguenti:  
  
|Tipo di contenuto|Riferimenti|  
|----------------|--------------|  
|**Risorse della community**|-   [Blog sull'architettura di Cloud privato](http://blogs.technet.com/b/privatecloud/archive/2012/03/19/cloud-datacenter-network-architecture-in-the-windows-server-8-era.aspx)<br />-Porre domande:[cloudnetfb@microsoft.com](mailto:%20cloudnetfb@microsoft.com)|  
|**RFC**|-VXLAN - [7348 RFC](http://www.rfc-editor.org/info/rfc7348)|  
|**Tecnologie correlate**|-   [Controller di rete](../../../sdn/technologies/network-controller/../../../sdn/technologies/network-controller/Network-Controller.md)<br />-   [Panoramica di virtualizzazione rete Hyper-V](assetId:///bf1dba9d-1960-4dd2-a5e2-99466a02044b) (Windows Server 2012 R2)|  
  


