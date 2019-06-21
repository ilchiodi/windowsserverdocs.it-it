---
title: Panoramica di virtualizzazione rete Hyper-V in Windows Server 2016
description: In questo argomento viene fornita una panoramica di virtualizzazione rete Hyper-V in Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0115b7ad-d229-4c69-9d7e-a3f5fbaa3b2f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 40ebd62d66c5a1d1a985a35a9adf179f07a75b3c
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284065"
---
# <a name="hyper-v-network-virtualization-overview-in-windows-server-2016"></a>Panoramica di virtualizzazione rete Hyper-V in Windows Server 2016

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In Windows Server 2016 e Virtual Machine Manager, Microsoft offre una soluzione di virtualizzazione di rete end-to-end.  Esistono cinque componenti principali che costituiscono una soluzione di virtualizzazione di rete Microsoft:  

-   **Windows Azure Pack per Windows Server** offre una per il portale per creare le reti virtuali e un portale di amministrazione per gestire le reti virtuali, il tenant.  

-   **Virtual Machine Manager** (VMM) consente una gestione centralizzata dell'infrastruttura di rete.  

-   **Controller di rete Microsoft** fornisce un punto programmabile e centralizzato di automazione per gestire, configurare, monitorare e risolvere i problemi dell'infrastruttura di rete fisica e virtuale nel proprio Data Center.  

-   **Virtualizzazione rete Hyper-V** offre l'infrastruttura necessaria per virtualizzare il traffico di rete.  

-   **Gateway di virtualizzazione rete Hyper-V** forniscono connessioni tra reti virtuali e fisiche.  

Questo argomento vengono introdotti i concetti e vengono illustrati i principali vantaggi e funzionalità di virtualizzazione di rete Hyper-V (una parte della soluzione di virtualizzazione di rete globale) in Windows Server 2016. Vengono inoltre illustrati i vantaggi che la virtualizzazione di rete offre sia ai cloud privati per il consolidamento del carico di lavoro aziendale che ai provider di servizi IaaS (Infrastructure as a Service) nel cloud pubblico.  

Per informazioni tecniche più dettagliate sulla virtualizzazione di rete in Windows Server 2016, vedere [dettagli tecnici sulla virtualizzazione rete di Hyper-V in Windows Server 2016](../../../sdn/technologies/hyper-v-network-virtualization/hyperv-network-virtualization-technical-details-windows-server.md).  

**Si intendeva**  

-   [Panoramica di virtualizzazione rete Hyper-V](assetId:///bf1dba9d-1960-4dd2-a5e2-99466a02044b) (Windows Server 2012 R2)  

-   [Panoramica di Hyper-V](assetId:///5aad349f-ef06-464a-b36f-366fbb040143)  

-   [Panoramica sul commutatore virtuale Hyper-V](assetId:///e6ec46af-6ef4-49b3-b1f1-5268dc03f05b)  

## <a name="BKMK_OVER"></a>Descrizione funzionalità  
Virtualizzazione rete Hyper-V fornisce "reti virtuali" (detta una rete VM) alle macchine virtuali allo stesso modo in cui server (hypervisor) consente una virtualizzazione di "macchine virtuali" per il sistema operativo. La virtualizzazione di rete disaccoppia le reti virtuali dall'infrastruttura di rete fisica ed elimina dal provisioning delle macchine virtuali i vincoli di VLAN e di assegnazione di indirizzi IP gerarchici. Questa flessibilità consente ai clienti di spostarsi agevolmente nei cloud IaaS e ai provider di servizi di hosting e amministratori di centri dati di gestire con maggiore efficienza la propria infrastruttura, mantenendo invariati il necessario isolamento multi-tenant e i requisiti di sicurezza e garantendo il supporto degli indirizzi IP di macchine virtuali sovrapposti.  

I clienti desiderano poter estendere i propri centri dati nel cloud in modo semplice. Attualmente, la realizzazione di architetture cloud ibride di questo tipo presenta alcune difficoltà tecniche. Tra i principali ostacoli affrontati dai clienti è riutilizzo delle topologie rete esistenti (subnet, IP indirizzi, i servizi di rete e così via.) nel cloud e il bridging tra le risorse locali e le risorse cloud.  Virtualizzazione rete Hyper-V si basa sul concetto di una rete VM indipendente dalla rete fisica sottostante. Grazie a questo concetto di una rete VM, costituita da una o più subnet virtuali, la posizione fisica esatta occupata nella rete fisica dalle macchine virtuali collegate a una rete virtuale è disaccoppiata dalla topologia della rete virtuale. Di conseguenza, i clienti possono spostare agevolmente le subnet virtuali nel cloud mantenendo gli indirizzi IP e la topologia esistenti nel cloud, in modo che i servizi esistenti possano continuare a funzionare anche senza conoscere la posizione fisica delle subnet. Virtualizzazione rete Hyper-V pertanto consente la realizzazione di un cloud ibrido di facile attuazione.  

Oltre al cloud ibrido, molte organizzazioni stanno consolidando i propri centri dati e creando cloud privati che consentano loro di sfruttare internamente i vantaggi offerti dalle architetture cloud in termini di efficienza e scalabilità. Virtualizzazione rete Hyper-V consente la maggiore flessibilità ed efficienza nei cloud privati, una business unit separando la topologia di rete (da resa virtuale) dalla topologia di rete fisica effettiva. In questo modo, le business unit possono condividere facilmente un cloud interno privato restando comunque isolate una dall'altra e continuare a mantenere le topologie di rete esistenti. Il team operativo del centro dati può contare sulla flessibilità di distribuire e spostare in modo dinamico i carichi di lavoro in qualsiasi punto del centro dati senza interruzioni dei server, migliorando le efficienze operative e la funzionalità complessiva del centro dati.  

Per i proprietari del carico di lavoro, il vantaggio principale è che è ora possibile passare le "topologie" del carico di lavoro nel cloud senza modificare gli indirizzi IP o riscrivere l'applicazioni. Ad esempio, una tipica applicazione line-of-business a tre livelli è composta da livello front end, livello della logica di business e livello di database. Tramite il criterio Virtualizzazione rete Hyper-V i clienti possono caricare nel cloud tutti i livelli o parte di essi mantenendo invariati la topologia di routing e gli indirizzi IP dei servizi (vale a dire gli indirizzi IP della macchina virtuale), senza alcuna necessità di modificare le applicazioni.  

La maggiore flessibilità nella selezione host per la macchina virtuale consente ai proprietari dell'infrastruttura di spostare i carichi di lavoro in qualsiasi punto del centro dati senza modificare le macchine virtuali o riconfigurare le reti. Ad esempio, Virtualizzazione rete Hyper-V consente la migrazione in tempo reale tra subnet, in questo modo una macchina virtuale può effettuare la migrazione in tempo reale in qualsiasi punto del centro dati senza alcuna interruzione del servizio. In precedenza, la migrazione in tempo reale era limitata alla stessa subnet, con conseguente limitazione del posizionamento delle macchine virtuali. La migrazione in tempo reale tra subnet consente agli amministratori di consolidare i carichi di lavoro in base ai requisiti di risorse dinamiche e di efficienza energetica, oltre a permettere la manutenzione dell'infrastruttura senza interrompere il tempo di attività del carico di lavoro dei clienti.  

## <a name="BKMK_APP"></a>Applicazioni pratiche  
Con la diffusione dei centri dati virtualizzati, le organizzazioni IT e i provider di servizi di hosting (ovvero provider che offrono servizi di collocazione o noleggio di server fisici) hanno incominciato a offrire infrastrutture virtualizzate più flessibili per facilitare l'offerta ai clienti di istanze di server su richiesta. Questo nuovo tipo di servizio è denominato IaaS (Infrastructure as a Service). Windows Server 2016 offre tutte le funzionalità di piattaforma necessarie per consentire ai clienti aziendali di creare cloud privati e transizione a un IT come un modello operativo di servizio. Windows Server 2016 2016 consente inoltre di hosting di realizzare cloud pubblici e offrire ai propri clienti soluzioni IaaS. Se combinato con Virtual Machine Manager e Windows Azure Pack per gestire i criteri di virtualizzazione rete Hyper-V, Microsoft offre una soluzione cloud avanzate.  

Virtualizzazione rete Hyper-V di Windows Server 2016 offre la virtualizzazione di rete basata su criteri e controllata da software che consente di ridurre l'overhead che colpisce le aziende impegnate nell'espansione di cloud IaaS dedicati di gestione e fornisce un hoster cloud migliore flessibilità e scalabilità nella gestione delle macchine virtuali per un maggiore utilizzo delle risorse.  

In uno scenario IaaS costituito da macchine virtuali provenienti da diverse divisioni aziendali (cloud dedicato) o da diversi clienti (cloud in hosting) è necessario provvedere a un isolamento sicuro. Soluzione di oggi, reti locali virtuali (VLAN), può presentare svantaggi significativi in questo scenario.  

**VLAN**  

Attualmente, le reti VLAN costituiscono il meccanismo utilizzato la maggior parte delle organizzazioni per supportare il riutilizzo dello spazio di indirizzi e l'isolamento dei tenant. Le VLAN utilizzano la codifica esplicita (ID VLAN) nelle intestazioni dei frame Ethernet e si affidano ai commutatori Ethernet per applicare l'isolamento e limitare il traffico verso i nodi di rete aventi lo stesso ID VLAN. I principali svantaggi delle VLAN sono i seguenti:  

-   Maggiore rischio di interruzione accidentale dovuto alle difficoltà di riconfigurazione dei commutatori di produzione ogniqualvolta si spostano le macchine virtuali o i confini di isolamento all'interno del centro dati dinamico.  

-   Scalabilità limitata, poiché sono presenti al massimo 4094 VLAN e i normali commutatori supportano non più di 1000 ID VLAN.  

-   Vincolo di un'unica subnet IP, con conseguente limitazione del numero di nodi entro un'unica VLAN e restrizione del posizionamento delle macchine virtuali in base alle posizioni fisiche. Benché sia possibile espandere le VLAN tra siti, la VLAN deve trovarsi interamente sulla stessa subnet.  

**Assegnazione di indirizzi IP**  

Oltre agli svantaggi presentati dalle VLAN, assegnazione di indirizzi IP di macchine virtuali comporta dei problemi, tra cui:  

-   Gli indirizzi IP delle macchine virtuali sono determinati dalle posizioni fisiche all'interno dell'infrastruttura di rete del centro dati. Di conseguenza, lo spostamento nel cloud richiede in genere la modifica degli indirizzi IP dei carichi di lavoro del servizio.  

-   I criteri sono legati agli indirizzi IP, quali regole firewall, individuazione risorse, servizi directory e così via. Per modificare gli indirizzi IP è necessario aggiornare tutti i criteri associati.  

-   Distribuzione della macchina virtuale e isolamento del traffico dipendono dalla topologia.  

Quando gli amministratori di rete del centro dati pianificano il layout fisico del centro dati, devono decidere dove posizionare fisicamente e indirizzare le subnet. Le decisioni sono basate su IP e tecnologia Ethernet, che determinano i potenziali indirizzi IP consentiti per l'esecuzione delle macchine virtuali su un dato server o un server blade collegato a un determinato rack del centro dati. Il provisioning e il posizionamento di una macchina virtuale nel centro dati richiede che questa rifletta le scelte e le limitazioni riguardanti gli indirizzi IP sopra descritte. Pertanto, in genere gli amministratori del centro dati non possono che assegnare nuovi indirizzi IP alle macchine virtuali.  

Il problema, in questo caso, è che gli indirizzi IP, oltre a essere tali, sono associati a informazioni di carattere semantico. Ad esempio, una subnet può contenere determinati servizi o trovarsi in una posizione fisica diversa. Sono comunemente associati agli indirizzi IP regole firewall, criteri di controllo degli accessi e associazioni di sicurezza IPsec. La modifica degli indirizzi IP costringe i proprietari della macchina virtuale a regolare i criteri basati sull'indirizzo IP originario. Il sovraccarico di renumerazione è talmente elevato che molte aziende preferiscono distribuire nel cloud solo i servizi nuovi, lasciando perdere le applicazioni legacy.  

Virtualizzazione di rete Hyper-V disaccoppia le reti virtuali delle macchine virtuali dei clienti dall'infrastruttura di rete fisica, consentendo alle macchine virtuali dei clienti di mantenere il proprio indirizzo IP originario, mentre gli amministratori del centro dati potrebbero eseguire il provisioning delle macchine virtuali dei clienti in qualsiasi punto del centro dati senza alcuna necessità di riconfigurare gli indirizzi IP fisici o gli ID VLAN. Nella sezione successiva sono riepilogate le funzionalità principali.  

## <a name="BKMK_NEW"></a>Funzionalità importanti  
Di seguito è riportato un elenco delle principali funzionalità, vantaggi e funzionalità di virtualizzazione rete Hyper-V in Windows Server 2016:  

-   **Consente il posizionamento flessibile del carico di lavoro - isolamento di rete e IP riutilizzo degli indirizzi senza VLAN**  

    Virtualizzazione rete Hyper-V disaccoppia le reti virtuali del cliente dall'infrastruttura di rete fisica di servizi di hosting, consentendo maggiore libertà per selezioni host per il carico di lavoro all'interno di Data Center. Il posizionamento del carico di lavoro delle macchine virtuali non è più limitato dall'assegnazione degli indirizzi IP o dai requisiti di isolamento VLAN della rete fisica perché viene applicato all'interno degli host Hyper-V basati su criteri di virtualizzazione multi-tenant definiti da software.  

    È ora possibile distribuire sul medesimo server host le macchine virtuali di clienti diversi con indirizzi IP sovrapposti senza effettuare complicate configurazioni della VLAN, né violare la gerarchia degli indirizzi IP. In questo modo si velocizza la migrazione dei carichi di lavoro dei clienti verso provider di servizi di hosting IaaS condivisi e i clienti possono spostare i carichi di lavoro senza apportare alcuna modifica, lasciando perciò invariati anche gli indirizzi IP delle macchine virtuali. Per i provider di servizi di hosting, che supportano un gran numero di clienti che desiderano estendere il proprio spazio indirizzi di rete esistente nel centro dati IaaS condiviso, configurare e mantenere VLAN isolate per ogni cliente per garantire la coesistenza di spazi indirizzi potenzialmente sovrapposti non è impresa semplice. Con Virtualizzazione rete Hyper-V il supporto degli indirizzi sovrapposti è semplificato e richiede meno interventi di configurazione della rete da parte del provider di servizi di hosting.  

    Inoltre, si possono effettuare manutenzione e aggiornamenti dell'infrastruttura di rete senza causare l'interruzione dei carichi di lavoro dei clienti. Con Virtualizzazione rete Hyper-V è possibile eseguire la migrazione delle macchine virtuali presenti su host, rack, subnet, VLAN o intero cluster senza alcuna necessità di modifica degli indirizzi IP fisici o di un'estesa riconfigurazione.  

-   **Semplifica lo spostamento dei carichi di lavoro in un cloud IaaS condiviso**  

    Con Virtualizzazione rete Hyper-V indirizzi IP e configurazioni della macchina virtuale restano invariati. In questo modo le organizzazioni IT possono spostare agevolmente i carichi di lavoro dai propri centri dati a un provider di servizi di hosting IaaS condivisi con interventi minimi di riconfigurazione del carico di lavoro o di strumenti e criteri dell'infrastruttura. In caso di connettività tra due centri dati, gli amministratori IT possono continuare a utilizzare i propri strumenti senza riconfigurarli.  

-   **Consente la migrazione in tempo reale tra subnet**  

    Migrazione in tempo reale dei carichi di lavoro di macchina virtuale tradizionalmente limitata alla stessa subnet IP o VLAN perché il passaggio tra subnet necessaria sistema operativo guest della macchina virtuale per modificare il relativo indirizzo IP. Questo cambio di indirizzo tronca la comunicazione esistente e interrompe i servizi in esecuzione nella macchina virtuale. Con virtualizzazione rete Hyper-V, i carichi di lavoro può essere in tempo reale la migrazione dai server che eseguono Windows Server 2016 in una subnet ai server che eseguono Windows Server 2016 in un'altra subnet senza modificarne gli indirizzi IP del carico di lavoro. Virtualizzazione rete Hyper-V assicura aggiornamento e sincronizzazione delle modifiche alla posizione delle macchine virtuali tra gli host in comunicazione continua con la macchina virtuale oggetto della migrazione.  

-   **Semplifica la gestione dell'amministrazione di server e rete disaccoppiato**  

    Il posizionamento del carico di lavoro del server è più semplice, perché migrazione e posizionamento dei carichi di lavoro sono indipendenti dalla configurazione della rete fisica sottostante. Gli amministratori del server possono perciò concentrarsi sulla gestione di servizi e server, mentre gli amministratori di rete possono concentrarsi sulla gestione complessiva dell'infrastruttura e del traffico di rete, consentendo agli amministratori dei server dei centri dati di distribuire le macchine virtuali ed effettuarne la migrazione senza modificarne gli indirizzi IP. Il sovraccarico è ridotto, perché Virtualizzazione rete Hyper-V consente il posizionamento delle macchine virtuali indipendentemente dalla topologia di rete, con conseguente riduzione, da parte degli amministratori di rete, della necessità di affrontare posizionamenti che potrebbero modificare i confini di isolamento.  

-   **Semplifica la rete e migliora l'utilizzo delle risorse di rete/server**  

    La rigidità delle VLAN e la dipendenza del posizionamento delle macchine virtuali da un'infrastruttura di rete fisica provocano provisioning eccessivo e sottoutilizzo. Interrompendo la dipendenza, la maggiore flessibilità di posizionamento delle macchine virtuali può semplificare la gestione di rete e migliorare l'utilizzo delle risorse di rete e del server. Si tenga presente che Virtualizzazione rete Hyper-V supporta le VLAN nel contesto del centro dati fisico. Può capitare, ad esempio, che un centro dati desideri convogliare tutto il traffico di Virtualizzazione rete Hyper-V su una determinata VLAN.  

-   **È compatibile con l'infrastruttura esistente e le tecnologie**  

    Virtualizzazione rete Hyper-V possono essere distribuita nei centri dati odierni, ma è compatibile con le ultimissime tecnologie "flat network" Data Center.  

    Ad esempio, virtualizzazione rete in Windows Server 2016 supporta il formato di incapsulamento VXLAN e il vSwitch aprire Database Management Protocol (OVSDB) come interfaccia SouthBound (SBI)...  

-   **Vengono fornite per l'interoperabilità e l'ecosistema readiness**  

    Virtualizzazione rete Hyper-V supporta più configurazioni per la comunicazione con le risorse esistenti, quali la connettività tra sedi, le reti SAN (Storage Area Network), l'accesso non virtualizzato alle risorse e così via. Microsoft si impegna a collaborare con i propri partner di ecosistema per supportare e migliorare l'esperienza di Virtualizzazione rete Hyper-V in termini di performance, scalabilità e gestione.  

-   **Configurazione basata su criteri**  

    I criteri di virtualizzazione di rete in Windows Server 2016 vengono configurati tramite il Controller di rete Microsoft. Il controller di rete ha un'API RESTful northbound e l'interfaccia di Windows PowerShell per configurare i criteri. Per altre informazioni sui Controller di rete Microsoft, vedere [Controller di rete](../../../sdn/technologies/network-controller/../../../sdn/technologies/network-controller/Network-Controller.md).  

## <a name="BKMK_SOFT"></a>Requisiti software  
Virtualizzazione rete Hyper-V usando il Controller di rete Microsoft richiede Windows Server 2016 e il ruolo Hyper-V.  

## <a name="BKMK_LINKS"></a>Vedere anche  
Per altre informazioni su virtualizzazione rete Hyper-V in Windows Server 2016, vedere i collegamenti seguenti:  


|       Tipo di contenuto       |                                                                                                                                Riferimenti                                                                                                                                |
|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Risorse della community**  |     -   [Blog sull'architettura di Cloud privati](https://blogs.technet.com/b/privatecloud/archive/2012/03/19/cloud-datacenter-network-architecture-in-the-windows-server-8-era.aspx)<br />-Domande: [cloudnetfb@microsoft.com](mailto:%20cloudnetfb@microsoft.com)     |
|         **RFC**          |                                                                                                     -   VXLAN - [RFC 7348](https://www.rfc-editor.org/info/rfc7348)                                                                                                      |
| **Tecnologie correlate** | -   [Controller di rete](../../../sdn/technologies/network-controller/../../../sdn/technologies/network-controller/Network-Controller.md)<br />-   [Panoramica di virtualizzazione rete Hyper-V](assetId:///bf1dba9d-1960-4dd2-a5e2-99466a02044b) (Windows Server 2012 R2) |

