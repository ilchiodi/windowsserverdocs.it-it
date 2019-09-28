---
title: Bilanciamento del carico software (SLB) per SDN
description: È possibile usare questo argomento per informazioni sul bilanciamento del carico software per Software Defined Networking in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 97abf182-4725-4026-801c-122db96964ed
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 35743d9e1a25c71a35eed018a4a3882a3d094d76
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355571"
---
# <a name="software-load-balancing-slb-for-sdn"></a>Bilanciamento del carico software \(SLB @ no__t-1 per SDN

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile usare questo argomento per informazioni sul bilanciamento del carico software per Software Defined Networking in Windows Server 2016.  

I provider di servizi cloud (CSP) e le aziende che distribuiscono software defined Networking (SDN) in Windows Server 2016 possono utilizzare il bilanciamento del carico software (SLB) per distribuire in modo uniforme il traffico di rete dei clienti tenant e tenant tra le risorse di rete virtuale. Windows Server SLB consente di abilitare più server per l'hosting dello stesso carico di lavoro, offrendo disponibilità e scalabilità elevate.
  
Windows Server SLB include le funzionalità seguenti.  
  
-   Servizi di bilanciamento del carico di livello 4 (L4) per il traffico TCP/UDP "North-South" e "East-West".  
  
-   Bilanciamento del carico del traffico di rete interno e pubblico.  
  
-   Supporta gli indirizzi IP dinamici (DIP) nelle reti locali virtuali (VLAN) e nelle reti virtuali create tramite la virtualizzazione rete Hyper-V.  
  
-   Supporto del probe di integrità.  
  
-   Pronto per la scalabilità cloud, incluse le funzionalità di scalabilità orizzontale e scalabilità verticale per i multiplexer e gli agenti host.  
  
Per ulteriori informazioni, vedere [funzionalità di bilanciamento del carico software](#bkmk_features) in questo argomento.  
  
> [!NOTE]  
> Il multitenant per le VLAN non è supportato dal controller di rete, ma è possibile usare le VLAN con SLB per i carichi di lavoro gestiti del provider di servizi, ad esempio l'infrastruttura del Data Center e i server Web ad alta densità.  
  
Con Windows Server SLB è possibile scalare in orizzontale le funzionalità di bilanciamento del carico usando le VM SLB negli stessi server di calcolo Hyper-V usati per gli altri carichi di lavoro della macchina virtuale. Per questo motivo, SLB supporta la creazione e l'eliminazione rapide degli endpoint di bilanciamento del carico necessari per le operazioni CSP. Inoltre, Windows Server SLB supporta decine di gigabyte per cluster, fornisce un modello di provisioning semplice ed è facile da scalare in orizzontale e in uscita.  
  
**Funzionamento di SLB**  
  
SLB funziona eseguendo il mapping degli indirizzi IP virtuali (VIP) a indirizzi IP dinamici (DIP) che fanno parte di un set di risorse del servizio cloud nel Data Center.  
  
I VIP sono indirizzi IP singoli che consentono l'accesso pubblico a un pool di macchine virtuali con carico bilanciato. Ad esempio, gli indirizzi VIP sono indirizzi IP esposti su Internet, in modo che i tenant e i clienti tenant possano connettersi alle risorse del tenant nel data center del cloud.  
  
I DIP sono gli indirizzi IP delle VM membro di un pool con bilanciamento del carico dietro l'indirizzo VIP. I dip vengono assegnati all'interno dell'infrastruttura cloud alle risorse del tenant.  
  
Gli indirizzi VIP si trovano in SLB multiplexer (MUX).  Il MUX è costituito da una o più macchine virtuali (VM).  Il controller di rete fornisce ogni MUX con ogni indirizzo VIP e ogni MUX USA a sua volta Border Gateway Protocol (BGP) per annunciare ogni VIP ai router nella rete fisica come Route/32.  BGP consente ai router di rete fisici di:  
  
-   Informazioni su come è disponibile un indirizzo VIP in ogni MUX, anche se le Mux si trovano in subnet diverse in una rete di livello 3.  
  
-   Distribuire il carico per ogni VIP in tutti i Mux disponibili usando il routing ECMP (Equal cost multipath).  
  
-   Rilevare automaticamente un errore o una rimozione MUX e arrestare l'invio del traffico al MUX non riuscito.  
  
-   Suddividere il carico dal MUX non riuscito o rimosso nel Mux integro.  
  
Quando il traffico pubblico arriva da Internet, SLB MUX esamina il traffico, che contiene l'indirizzo VIP come destinazione, e mappa e riscrive il traffico in modo che arrivi a un singolo DIP. Per il traffico di rete in ingresso, questa transazione viene eseguita in un processo in due passaggi diviso tra le macchine virtuali (VM) MUX e l'host Hyper-V in cui si trova il DIP di destinazione:  
  
-   Bilanciamento del carico: il MUX usa l'indirizzo VIP per selezionare un DIP, incapsula il pacchetto e lo trasmette all'host Hyper-V in cui si trova il DIP.  
  
-   NAT (Network Address Translation): l'host Hyper-V rimuove l'incapsulamento dal pacchetto, traduce il VIP in un DIP, ne esegue il mapping e lo trasmette alla VM DIP.  
  
MUX sa come eseguire il mapping degli indirizzi VIP ai DIP corretti a causa dei criteri di bilanciamento del carico definiti usando il controller di rete. Queste regole includono il protocollo, la porta front-end, la porta back-end e l'algoritmo di distribuzione (5, 3 o 2 Tuple).  
  
Quando le macchine virtuali dei tenant rispondono e inviano il traffico di rete in uscita a Internet o a posizioni tenant remote, perché il NAT viene eseguito dall'host Hyper-V, il traffico ignora il MUX e passa direttamente al router perimetrale dall'host Hyper-V. Questo processo di bypass MUX è denominato Direct Server Return (DSR).  
  
Dopo aver stabilito il flusso di traffico di rete iniziale, il traffico di rete in ingresso ignora completamente il MUX SLB.  
  
Nella figura seguente, un computer client esegue una query DNS per l'indirizzo IP di un sito di SharePoint aziendale, in questo caso una società fittizia denominata Contoso. Si verifica il processo seguente.  
  
-   Il server DNS restituisce il 107.105.47.60 VIP al client.  
  
-   Il client invia una richiesta HTTP all'indirizzo VIP.  
  
-   La rete fisica ha più percorsi disponibili per raggiungere l'indirizzo VIP che si trova in qualsiasi MUX.  Ogni router lungo il modo Usa ECMP per selezionare il segmento successivo del percorso fino a quando la richiesta arriva a un MUX.  
  
-   Il MUX che riceve la richiesta controlla i criteri configurati e rileva che sono disponibili due dip, 10.10.10.5 e 10.10.20.5, in una rete virtuale per gestire la richiesta all'indirizzo VIP 107.105.47.60  
  
-   MUX Seleziona il DIP 10.10.10.5 e incapsula i pacchetti usando VXLAN in modo che possano inviarli all'host che contiene il DIP usando l'indirizzo di rete fisico degli host.  
  
-   L'host riceve il pacchetto incapsulato e lo ispeziona.  Rimuove l'incapsulamento e riscrive il pacchetto in modo che la destinazione sia ora il 10.10.10.5 DIP anziché l'indirizzo VIP e invii il traffico alla VM DIP.  
  
-   La richiesta ha ora raggiunto il sito di SharePoint di Contoso in server farm 2. Il server genera una risposta e la invia al client, usando il proprio indirizzo IP come origine.  
  
-   L'host intercetta il pacchetto in uscita nel Commuter virtuale che ricorda che il client, ora la destinazione, ha effettuato la richiesta originale per l'indirizzo VIP.  L'host riscrive l'origine del pacchetto in modo che sia l'indirizzo VIP, in modo che nel client non venga visualizzato l'indirizzo DIP.  
  
-   L'host inoltra il pacchetto direttamente al gateway predefinito per la rete fisica che usa la tabella di routing standard per inoltrare il pacchetto al client che alla fine riceve la risposta.  
  
![Processo di bilanciamento del carico software](../../../media/Software-Load-Balancing--SLB--for-SDN/slb_process.jpg)  
  
**Bilanciamento del carico del traffico interno del Data Center**  
  
Quando si esegue il bilanciamento del carico del traffico di rete interno al Data Center, ad esempio tra le risorse tenant in esecuzione su server diversi e i membri della stessa rete virtuale, il commutere virtuale Hyper-V a cui sono connesse le VM esegue NAT.  
  
Con il bilanciamento del carico del traffico interno, la prima richiesta viene inviata ed elaborata dal MUX, che seleziona il DIP appropriato e instrada il traffico al DIP. Da quel punto in poi, il flusso di traffico stabilito ignora il MUX e passa direttamente dalla macchina virtuale alla macchina virtuale.  
  
**Probe di integrità**  
  
SLB include i probe di integrità per convalidare l'integrità dell'infrastruttura di rete, inclusi i seguenti.  
  
-   Probe TCP per la porta  
  
-   Probe HTTP per la porta e l'URL  
  
A differenza di un appliance di bilanciamento del carico tradizionale in cui il probe ha origine nell'appliance e passa attraverso la rete al DIP, il probe SLB ha origine nell'host in cui si trova il DIP e passa direttamente dall'agente host SLB al DIP, distribuendo ulteriormente il lavorare tra gli host.  
  
## <a name="bkmk_infrastructure"></a>Infrastruttura di bilanciamento del carico software  
Per distribuire Windows Server SLB, è prima di tutto necessario distribuire il controller di rete in Windows Server 2016 e una o più macchine virtuali MUX SLB.  
  
Inoltre, è necessario configurare gli host Hyper-V con il Commuter virtuale Hyper-V abilitato per SDN e verificare che l'agente host SLB sia in esecuzione.  I router che servono gli host devono supportare il routing e la Border Gateway Protocol (BGP) di ECMP (Equal cost multipath) e devono essere configurati per accettare richieste di peering BGP dal Mux SLB.  
  
Di seguito viene riportata una panoramica dell'infrastruttura di SLB.  

![Infrastruttura di bilanciamento del carico software](../../../media/Software-Load-Balancing--SLB--for-SDN/slb_overview1.png)  
  
Le sezioni seguenti forniscono altre informazioni su questi elementi dell'infrastruttura di SLB.  
  
### <a name="scvmm"></a>SCVMM  
Con System Center 2016, è possibile configurare il controller di rete in Windows Server 2016, tra cui SLB Manager e Health Monitor. È anche possibile usare System Center per distribuire SLB MUXs e per installare gli agenti host SLB in computer che eseguono Windows Server 2016 e Hyper-V.  
  
Per ulteriori informazioni su System Center 2016, vedere [System center 2016](https://www.microsoft.com/server-cloud/products/system-center-2016/).  
  
> [!NOTE]  
> Se non si vuole usare System Center 2016, è possibile usare Windows PowerShell o un'altra applicazione di gestione per installare e configurare il controller di rete e altre infrastrutture SLB. Per ulteriori informazioni, vedere [deploy Network controller using Windows PowerShell](../../../sdn/deploy/Deploy-Network-Controller-using-Windows-PowerShell.md).  
  
### <a name="network-controller"></a>Controller di rete  
Il controller di rete ospita SLB Manager ed esegue le azioni seguenti per SLB.  
  
-   Elabora i comandi SLB che passano attraverso l'API in direzione nord da System Center, Windows PowerShell o un'altra applicazione di gestione di rete.  
  
-   Calcola i criteri per la distribuzione in host Hyper-V e SLB MUX.  
  
-   Fornisce lo stato di integrità dell'infrastruttura di SLB.  
  
### <a name="slb-mux"></a>MUX SLB  
Il MUX SLB elabora il traffico di rete in ingresso ed esegue il mapping degli indirizzi VIP ai DIP, quindi invia il traffico al DIP corretto. Ogni MUX usa anche BGP per pubblicare Route VIP nei router perimetrali. BGP Keep Alive invia una notifica a Mux quando un MUX ha esito negativo, che consente a Mux attivo di ridistribuire il carico in caso di errore MUX, in modo da fornire il bilanciamento del carico per i servizi di bilanciamento del carico.  
  
### <a name="hosts-that-are-running-hyper-v"></a>Host che eseguono Hyper-V  
È possibile usare SLB con computer che eseguono Windows Server 2016 e Hyper-V. Le macchine virtuali nell'host Hyper-V possono eseguire qualsiasi sistema operativo supportato da Hyper-V.  
  
### <a name="slb-host-agent"></a>Agente host SLB  
Quando si distribuisce SLB, è necessario usare System Center, Windows PowerShell o un'altra applicazione di gestione per distribuire l'agente host SLB in ogni computer host Hyper-V. È possibile installare l'agente host SLB in tutte le versioni di Windows Server 2016 che forniscono il supporto per Hyper-V, incluso nano server.  
  
L'agente host SLB è in ascolto degli aggiornamenti dei criteri di SLB dal controller di rete. Inoltre, l'agente host programma le regole per SLB nei commutatori virtuali Hyper-V abilitati per SDN configurati nel computer locale.  
  
### <a name="sdn-enabled-hyper-v-virtual-switch"></a>Switch virtuale Hyper-V abilitato per SDN  
Affinché un commutire virtuale sia compatibile con SLB, è necessario utilizzare Gestione commutiri virtuali Hyper-V o i comandi di Windows PowerShell per creare l'opzione, quindi è necessario abilitare la piattaforma filtro virtuale (VFP) per il Commuter virtuale.  
  
Per informazioni sull'abilitazione di VFP su commutatori virtuali, vedere i comandi di Windows PowerShell [Get-VMSystemSwitchExtension](https://technet.microsoft.com/library/hh848603.aspx) e [Enable-VMSwitchExtension](https://technet.microsoft.com/library/hh848541.aspx?f=255&MSPPError=-2147217396).  
  
Il Commuter virtuale Hyper-V abilitato per SDN esegue le azioni seguenti per SLB.  
  
-   Elabora il percorso dati per SLB.  
  
-   Riceve il traffico di rete in ingresso dal MUX.  
  
-   Ignora il MUX per il traffico di rete in uscita, inviarlo al router usando DSR.  
  
-   Viene eseguito in istanze di nano server di Hyper-V.  
  
### <a name="bgp-enabled-router"></a>Router abilitato BGP  
Il router BGP esegue le azioni seguenti per SLB.  
  
-   Instrada il traffico in ingresso verso il MUX usando ECMP.  
  
-   Per il traffico di rete in uscita, utilizza la route fornita dall'host.  
  
-   Ascolta gli aggiornamenti della route per gli indirizzi VIP da SLB MUX.  
  
-   Rimuove SLB Mux dalla rotazione SLB se Keep-Alive non riesce.  
  
## <a name="bkmk_features"></a>Funzionalità di bilanciamento del carico software  
Di seguito sono riportate alcune delle funzionalità e delle funzionalità di SLB.  
  
**Funzionalità di base**  
  
-   SLB fornisce servizi di bilanciamento del carico di livello 4 per il traffico TCP/UDP "North-South" e "East-West"  
  
-   È possibile usare SLB in una rete basata su virtualizzazione rete Hyper-V  
  
-   È possibile usare SLB con una rete basata su VLAN per le macchine virtuali DIP connesse a un Commuter virtuale Hyper-V abilitato per SDN.  
  
-   Un'istanza di SLB può gestire più tenant  
  
-   SLB e DIP supportano un percorso di ritorno scalabile e a bassa latenza, implementato da Direct Server Return (DSR)  
  
-   SLB funziona anche quando si usa switch Embedded Teaming (SET) o single root input/output Virtualization (SR-IOV)  
  
-   SLB include il supporto IPv4 (Internet Protocol versione 4)  
  
-   Per gli scenari di gateway da sito a sito, SLB offre funzionalità NAT per consentire a tutte le connessioni da sito a sito di usare un singolo IP pubblico  
  
-   È possibile installare SLB, tra cui l'agente host e il MUX, in Windows Server 2016, Full, core e nano install.  
  
**Scalabilità e prestazioni**  
  
-   Pronto per la scalabilità cloud, inclusa la funzionalità di scalabilità orizzontale e scalabilità verticale per mux e agenti host.  
  
-   Un modulo Active SLB Manager Network controller può supportare 8 istanze MUX  
  
**Disponibilità elevata**  
  
-   È possibile distribuire SLB a più di 2 nodi in una configurazione attiva/attiva  
  
-   MUX possono essere aggiunti e rimossi dal pool MUX senza alcun effetto sul servizio SLB. Questo mantiene la disponibilità SLB quando   
    è in corso l'applicazione di patch a singoli MUX.  
  
-   Le singole istanze di MUX hanno un tempo di indisponibilità pari al 99%  
  
-   I dati di monitoraggio dell'integrità sono disponibili per le entità di gestione  
  
**Allineamento**  
  
-   È possibile distribuire e configurare SLB con SCVMM  
  
-   SLB offre un perimetro unificato multi-tenant grazie all'integrazione di dispositivi Microsoft, ad esempio RAS multi-tenant gateway, il firewall del Data Center e il riflettore delle route.  
  

