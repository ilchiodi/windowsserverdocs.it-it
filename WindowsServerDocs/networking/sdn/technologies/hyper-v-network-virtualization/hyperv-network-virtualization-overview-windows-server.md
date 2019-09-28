---
title: Panoramica di virtualizzazione rete Hyper-V in Windows Server 2016
description: Questo argomento fornisce una panoramica di virtualizzazione rete Hyper-V in Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0115b7ad-d229-4c69-9d7e-a3f5fbaa3b2f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0eda30b0980f2080f1603eb906fd308440316248
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405949"
---
# <a name="hyper-v-network-virtualization-overview-in-windows-server-2016"></a>Panoramica di virtualizzazione rete Hyper-V in Windows Server 2016

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In Windows Server 2016 e Virtual Machine Manager Microsoft offre una soluzione di virtualizzazione di rete end-to-end.  Sono disponibili cinque componenti principali che comprendono la soluzione di virtualizzazione di rete Microsoft:  

-   **Windows Azure Pack per Windows Server** fornisce un portale per il tenant per la creazione di reti virtuali e un portale di amministrazione per la gestione delle reti virtuali.  

-   **Virtual Machine Manager** (VMM) fornisce una gestione centralizzata dell'infrastruttura di rete.  

-   Il **controller di rete Microsoft** offre un punto di automazione programmabile e centralizzato per gestire, configurare, monitorare e risolvere i problemi dell'infrastruttura di rete fisica e virtuale nel Data Center.  

-   **Virtualizzazione rete Hyper-V** offre l'infrastruttura necessaria per virtualizzare il traffico di rete.  

-   I **gateway di virtualizzazione rete Hyper-V** forniscono connessioni tra reti virtuali e fisiche.  

In questo argomento vengono presentati i concetti e vengono illustrati i vantaggi e le funzionalità principali di virtualizzazione rete Hyper-V (una parte della soluzione di virtualizzazione di rete complessiva) in Windows Server 2016. Vengono inoltre illustrati i vantaggi che la virtualizzazione di rete offre sia ai cloud privati per il consolidamento del carico di lavoro aziendale che ai provider di servizi IaaS (Infrastructure as a Service) nel cloud pubblico.  

Per altri dettagli tecnici sulla virtualizzazione di rete in Windows Server 2016, vedere la pagina relativa ai [Dettagli tecnici di virtualizzazione rete Hyper-V in Windows server 2016](../../../sdn/technologies/hyper-v-network-virtualization/hyperv-network-virtualization-technical-details-windows-server.md).  

**Si intendeva**  

-   [Panoramica di virtualizzazione rete Hyper-V](assetId:///bf1dba9d-1960-4dd2-a5e2-99466a02044b) (Windows Server 2012 R2)  

-   [Panoramica di Hyper-V](assetId:///5aad349f-ef06-464a-b36f-366fbb040143)  

-   [Panoramica sul commutatore virtuale Hyper-V](assetId:///e6ec46af-6ef4-49b3-b1f1-5268dc03f05b)  

## <a name="BKMK_OVER"></a>Descrizione della funzionalità  
Virtualizzazione rete Hyper-V fornisce "reti virtuali" (denominata rete VM) alle macchine virtuali in modo analogo al modo in cui la virtualizzazione del server (hypervisor) fornisce "macchine virtuali" al sistema operativo. La virtualizzazione di rete disaccoppia le reti virtuali dall'infrastruttura di rete fisica ed elimina dal provisioning delle macchine virtuali i vincoli di VLAN e di assegnazione di indirizzi IP gerarchici. Questa flessibilità consente ai clienti di spostarsi agevolmente nei cloud IaaS e ai provider di servizi di hosting e amministratori di centri dati di gestire con maggiore efficienza la propria infrastruttura, mantenendo invariati il necessario isolamento multi-tenant e i requisiti di sicurezza e garantendo il supporto degli indirizzi IP di macchine virtuali sovrapposti.  

I clienti desiderano poter estendere i propri centri dati nel cloud in modo semplice. Attualmente, la realizzazione di architetture cloud ibride di questo tipo presenta alcune difficoltà tecniche. Uno dei principali ostacoli che i clienti affrontano è il riutilizzo delle topologie di rete esistenti (subnet, indirizzi IP, servizi di rete e così via) nel cloud e il bridging tra le risorse locali e le risorse cloud.  Virtualizzazione rete Hyper-V si basa sul concetto di una rete VM indipendente dalla rete fisica sottostante. Grazie a questo concetto di una rete VM, costituita da una o più subnet virtuali, la posizione fisica esatta occupata nella rete fisica dalle macchine virtuali collegate a una rete virtuale è disaccoppiata dalla topologia della rete virtuale. Di conseguenza, i clienti possono spostare agevolmente le subnet virtuali nel cloud mantenendo gli indirizzi IP e la topologia esistenti nel cloud, in modo che i servizi esistenti possano continuare a funzionare anche senza conoscere la posizione fisica delle subnet. Virtualizzazione rete Hyper-V pertanto consente la realizzazione di un cloud ibrido di facile attuazione.  

Oltre al cloud ibrido, molte organizzazioni stanno consolidando i propri centri dati e creando cloud privati che consentano loro di sfruttare internamente i vantaggi offerti dalle architetture cloud in termini di efficienza e scalabilità. Virtualizzazione rete Hyper-V consente una maggiore flessibilità ed efficienza per i cloud privati, disaccoppiando la topologia di rete di una business unit (rendendola virtuale) dalla topologia di rete fisica effettiva. In questo modo, le business unit possono condividere facilmente un cloud interno privato restando comunque isolate una dall'altra e continuare a mantenere le topologie di rete esistenti. Il team operativo del centro dati può contare sulla flessibilità di distribuire e spostare in modo dinamico i carichi di lavoro in qualsiasi punto del centro dati senza interruzioni dei server, migliorando le efficienze operative e la funzionalità complessiva del centro dati.  

Per i proprietari del carico di lavoro, il vantaggio principale è che ora possono spostare le "topologie" del carico di lavoro nel cloud senza modificare gli indirizzi IP o riscrivere le applicazioni. Ad esempio, una tipica applicazione line-of-business a tre livelli è composta da livello front end, livello della logica di business e livello di database. Tramite il criterio Virtualizzazione rete Hyper-V i clienti possono caricare nel cloud tutti i livelli o parte di essi mantenendo invariati la topologia di routing e gli indirizzi IP dei servizi (vale a dire gli indirizzi IP della macchina virtuale), senza alcuna necessità di modificare le applicazioni.  

La maggiore flessibilità nella selezione host per la macchina virtuale consente ai proprietari dell'infrastruttura di spostare i carichi di lavoro in qualsiasi punto del centro dati senza modificare le macchine virtuali o riconfigurare le reti. Ad esempio, Virtualizzazione rete Hyper-V consente la migrazione in tempo reale tra subnet, in questo modo una macchina virtuale può effettuare la migrazione in tempo reale in qualsiasi punto del centro dati senza alcuna interruzione del servizio. In precedenza, la migrazione in tempo reale era limitata alla stessa subnet, con conseguente limitazione del posizionamento delle macchine virtuali. La migrazione in tempo reale tra subnet consente agli amministratori di consolidare i carichi di lavoro in base ai requisiti di risorse dinamiche e di efficienza energetica, oltre a permettere la manutenzione dell'infrastruttura senza interrompere il tempo di attività del carico di lavoro dei clienti.  

## <a name="BKMK_APP"></a>Applicazioni pratiche  
Con la diffusione dei centri dati virtualizzati, le organizzazioni IT e i provider di servizi di hosting (ovvero provider che offrono servizi di collocazione o noleggio di server fisici) hanno incominciato a offrire infrastrutture virtualizzate più flessibili per facilitare l'offerta ai clienti di istanze di server su richiesta. Questo nuovo tipo di servizio è denominato IaaS (Infrastructure as a Service). Windows Server 2016 offre tutte le funzionalità della piattaforma necessarie per consentire ai clienti aziendali di creare cloud privati e di passare a un modello operativo IT come servizio. Windows Server 2016 2016 consente inoltre ai provider di hosting di creare cloud pubblici e offrire soluzioni IaaS ai clienti. In combinazione con Virtual Machine Manager e Windows Azure Pack per gestire i criteri di virtualizzazione rete Hyper-V, Microsoft offre una potente soluzione cloud.  

Virtualizzazione rete Hyper-V di Windows Server 2016 offre una virtualizzazione di rete basata su criteri e controllata dal software che riduce il sovraccarico di gestione affrontato dalle aziende quando si espandono Cloud IaaS dedicati e offre migliori funzionalità per gli host cloud flessibilità e scalabilità per la gestione delle macchine virtuali per ottenere un utilizzo più elevato delle risorse.  

In uno scenario IaaS costituito da macchine virtuali provenienti da diverse divisioni aziendali (cloud dedicato) o da diversi clienti (cloud in hosting) è necessario provvedere a un isolamento sicuro. La soluzione odierna, Virtual Local Area Network (VLAN), può presentare svantaggi significativi in questo scenario.  

**VLAN**  

Attualmente, le VLAN sono il meccanismo usato dalla maggior parte delle organizzazioni per supportare il riutilizzo dello spazio di indirizzi e l'isolamento del tenant. Le VLAN utilizzano la codifica esplicita (ID VLAN) nelle intestazioni dei frame Ethernet e si affidano ai commutatori Ethernet per applicare l'isolamento e limitare il traffico verso i nodi di rete aventi lo stesso ID VLAN. I principali svantaggi delle VLAN sono i seguenti:  

-   Maggiore rischio di interruzione accidentale dovuto alle difficoltà di riconfigurazione dei commutatori di produzione ogniqualvolta si spostano le macchine virtuali o i confini di isolamento all'interno del centro dati dinamico.  

-   Scalabilità limitata, poiché sono presenti al massimo 4094 VLAN e i normali commutatori supportano non più di 1000 ID VLAN.  

-   Vincolo di un'unica subnet IP, con conseguente limitazione del numero di nodi entro un'unica VLAN e restrizione del posizionamento delle macchine virtuali in base alle posizioni fisiche. Benché sia possibile espandere le VLAN tra siti, la VLAN deve trovarsi interamente sulla stessa subnet.  

**Assegnazione di indirizzi IP**  

Oltre agli svantaggi presentati dalle VLAN, l'assegnazione di indirizzi IP della macchina virtuale presenta problemi che includono:  

-   Gli indirizzi IP delle macchine virtuali sono determinati dalle posizioni fisiche all'interno dell'infrastruttura di rete del centro dati. Di conseguenza, lo spostamento nel cloud richiede in genere la modifica degli indirizzi IP dei carichi di lavoro del servizio.  

-   I criteri sono legati agli indirizzi IP, quali regole firewall, individuazione risorse, servizi directory e così via. Per modificare gli indirizzi IP è necessario aggiornare tutti i criteri associati.  

-   Distribuzione della macchina virtuale e isolamento del traffico dipendono dalla topologia.  

Quando gli amministratori di rete del centro dati pianificano il layout fisico del centro dati, devono decidere dove posizionare fisicamente e indirizzare le subnet. Le decisioni sono basate su IP e tecnologia Ethernet, che determinano i potenziali indirizzi IP consentiti per l'esecuzione delle macchine virtuali su un dato server o un server blade collegato a un determinato rack del centro dati. Il provisioning e il posizionamento di una macchina virtuale nel centro dati richiede che questa rifletta le scelte e le limitazioni riguardanti gli indirizzi IP sopra descritte. Pertanto, in genere gli amministratori del centro dati non possono che assegnare nuovi indirizzi IP alle macchine virtuali.  

Il problema, in questo caso, è che gli indirizzi IP, oltre a essere tali, sono associati a informazioni di carattere semantico. Ad esempio, una subnet può contenere determinati servizi o trovarsi in una posizione fisica diversa. Sono comunemente associati agli indirizzi IP regole firewall, criteri di controllo degli accessi e associazioni di sicurezza IPsec. La modifica degli indirizzi IP costringe i proprietari della macchina virtuale a regolare i criteri basati sull'indirizzo IP originario. Il sovraccarico di renumerazione è talmente elevato che molte aziende preferiscono distribuire nel cloud solo i servizi nuovi, lasciando perdere le applicazioni legacy.  

Virtualizzazione di rete Hyper-V disaccoppia le reti virtuali delle macchine virtuali dei clienti dall'infrastruttura di rete fisica, consentendo alle macchine virtuali dei clienti di mantenere il proprio indirizzo IP originario, mentre gli amministratori del centro dati potrebbero eseguire il provisioning delle macchine virtuali dei clienti in qualsiasi punto del centro dati senza alcuna necessità di riconfigurare gli indirizzi IP fisici o gli ID VLAN. Nella sezione successiva sono riepilogate le funzionalità principali.  

## <a name="BKMK_NEW"></a>Funzionalità importanti  
Di seguito è riportato un elenco delle funzionalità chiave, dei vantaggi e delle funzionalità di virtualizzazione rete Hyper-V in Windows Server 2016:  

-   **Abilita il posizionamento flessibile del carico di lavoro: isolamento della rete e riutilizzo degli indirizzi IP senza VLAN**  

    Virtualizzazione rete Hyper-V separa le reti virtuali del cliente dall'infrastruttura di rete fisica dei provider di servizi di hosting, garantendo la libertà di posizionamento dei carichi di lavoro all'interno dei data center. Il posizionamento del carico di lavoro delle macchine virtuali non è più limitato dall'assegnazione degli indirizzi IP o dai requisiti di isolamento VLAN della rete fisica perché viene applicato all'interno degli host Hyper-V basati su criteri di virtualizzazione multi-tenant definiti da software.  

    È ora possibile distribuire sul medesimo server host le macchine virtuali di clienti diversi con indirizzi IP sovrapposti senza effettuare complicate configurazioni della VLAN, né violare la gerarchia degli indirizzi IP. In questo modo si velocizza la migrazione dei carichi di lavoro dei clienti verso provider di servizi di hosting IaaS condivisi e i clienti possono spostare i carichi di lavoro senza apportare alcuna modifica, lasciando perciò invariati anche gli indirizzi IP delle macchine virtuali. Per i provider di servizi di hosting, che supportano un gran numero di clienti che desiderano estendere il proprio spazio indirizzi di rete esistente nel centro dati IaaS condiviso, configurare e mantenere VLAN isolate per ogni cliente per garantire la coesistenza di spazi indirizzi potenzialmente sovrapposti non è impresa semplice. Con Virtualizzazione rete Hyper-V il supporto degli indirizzi sovrapposti è semplificato e richiede meno interventi di configurazione della rete da parte del provider di servizi di hosting.  

    Inoltre, si possono effettuare manutenzione e aggiornamenti dell'infrastruttura di rete senza causare l'interruzione dei carichi di lavoro dei clienti. Con Virtualizzazione rete Hyper-V è possibile eseguire la migrazione delle macchine virtuali presenti su host, rack, subnet, VLAN o intero cluster senza alcuna necessità di modifica degli indirizzi IP fisici o di un'estesa riconfigurazione.  

-   **Consente di spostare più facilmente i carichi di lavoro in un Cloud IaaS condiviso**  

    Con Virtualizzazione rete Hyper-V indirizzi IP e configurazioni della macchina virtuale restano invariati. In questo modo le organizzazioni IT possono spostare agevolmente i carichi di lavoro dai propri centri dati a un provider di servizi di hosting IaaS condivisi con interventi minimi di riconfigurazione del carico di lavoro o di strumenti e criteri dell'infrastruttura. In caso di connettività tra due centri dati, gli amministratori IT possono continuare a utilizzare i propri strumenti senza riconfigurarli.  

-   **Abilita la migrazione in tempo reale tra subnet**  

    La migrazione in tempo reale dei carichi di lavoro delle macchine virtuali era tradizionalmente limitata alla stessa subnet IP o VLAN perché le subnet che attraversavano il sistema operativo guest della macchina virtuale potevano modificare il proprio indirizzo IP. Questo cambio di indirizzo tronca la comunicazione esistente e interrompe i servizi in esecuzione nella macchina virtuale. Con la virtualizzazione rete Hyper-V, è possibile eseguire la migrazione in tempo reale dei carichi di lavoro dai server che eseguono Windows Server 2016 in una subnet ai server che eseguono Windows Server 2016 in un'altra subnet senza modificare gli indirizzi IP del carico di lavoro. Virtualizzazione rete Hyper-V assicura aggiornamento e sincronizzazione delle modifiche alla posizione delle macchine virtuali tra gli host in comunicazione continua con la macchina virtuale oggetto della migrazione.  

-   **Consente una gestione più semplice del server e dell'amministrazione di rete disaccoppiati**  

    Il posizionamento del carico di lavoro del server è più semplice, perché migrazione e posizionamento dei carichi di lavoro sono indipendenti dalla configurazione della rete fisica sottostante. Gli amministratori del server possono perciò concentrarsi sulla gestione di servizi e server, mentre gli amministratori di rete possono concentrarsi sulla gestione complessiva dell'infrastruttura e del traffico di rete, consentendo agli amministratori dei server dei centri dati di distribuire le macchine virtuali ed effettuarne la migrazione senza modificarne gli indirizzi IP. Il sovraccarico è ridotto, perché Virtualizzazione rete Hyper-V consente il posizionamento delle macchine virtuali indipendentemente dalla topologia di rete, con conseguente riduzione, da parte degli amministratori di rete, della necessità di affrontare posizionamenti che potrebbero modificare i confini di isolamento.  

-   **Semplifica la rete e migliora l'utilizzo delle risorse di rete/server**  

    La rigidità delle VLAN e la dipendenza del posizionamento delle macchine virtuali da un'infrastruttura di rete fisica provocano provisioning eccessivo e sottoutilizzo. Interrompendo la dipendenza, la maggiore flessibilità di posizionamento delle macchine virtuali può semplificare la gestione di rete e migliorare l'utilizzo delle risorse di rete e del server. Si tenga presente che Virtualizzazione rete Hyper-V supporta le VLAN nel contesto del centro dati fisico. Può capitare, ad esempio, che un centro dati desideri convogliare tutto il traffico di Virtualizzazione rete Hyper-V su una determinata VLAN.  

-   **È compatibile con l'infrastruttura esistente e le tecnologie emergenti**  

    Virtualizzazione rete Hyper-V può essere distribuita nel Data Center odierno, ma è compatibile con le tecnologie emergenti di "rete Flat" del Data Center.  

    Ad esempio, HNV in Windows Server 2016 supporta il formato di incapsulamento VXLAN e Open vSwitch Database Management Protocol (OVSDB) come interfaccia Sud (SBI).  

-   **Fornisce l'interoperabilità e la conformità dell'ecosistema**  

    Virtualizzazione rete Hyper-V supporta più configurazioni per la comunicazione con le risorse esistenti, quali la connettività tra sedi, le reti SAN (Storage Area Network), l'accesso non virtualizzato alle risorse e così via. Microsoft si impegna a collaborare con i propri partner di ecosistema per supportare e migliorare l'esperienza di Virtualizzazione rete Hyper-V in termini di performance, scalabilità e gestione.  

-   **Configurazione basata su criteri**  

    I criteri di virtualizzazione di rete in Windows Server 2016 sono configurati tramite il controller di rete Microsoft. Il controller di rete ha un'API a nord RESTful e un'interfaccia di Windows PowerShell per configurare i criteri. Per ulteriori informazioni sul controller di rete Microsoft, vedere [controller di rete](../../../sdn/technologies/network-controller/../../../sdn/technologies/network-controller/Network-Controller.md).  

## <a name="BKMK_SOFT"></a>Requisiti software  
Virtualizzazione rete Hyper-V con il controller di rete Microsoft richiede Windows Server 2016 e il ruolo Hyper-V.  

## <a name="BKMK_LINKS"></a>Vedere anche  
Per ulteriori informazioni su virtualizzazione rete Hyper-V in Windows Server 2016, vedere i collegamenti seguenti:  


|       Tipo di contenuto       |                                                                                                                                Riferimenti                                                                                                                                |
|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Risorse della community**  |     [Blog sull'architettura del cloud privato](https://blogs.technet.com/b/privatecloud/archive/2012/03/19/cloud-datacenter-network-architecture-in-the-windows-server-8-era.aspx) -   <br />-Porre domande: [cloudnetfb@microsoft.com](mailto:%20cloudnetfb@microsoft.com)     |
|         **RFC**          |                                                                                                     -VXLAN- [RFC 7348](https://www.rfc-editor.org/info/rfc7348)                                                                                                      |
| **Tecnologie correlate** | [controller di rete](../../../sdn/technologies/network-controller/../../../sdn/technologies/network-controller/Network-Controller.md) -   <br />[Panoramica di virtualizzazione rete Hyper-V](assetId:///bf1dba9d-1960-4dd2-a5e2-99466a02044b) -    (Windows Server 2012 R2) |

