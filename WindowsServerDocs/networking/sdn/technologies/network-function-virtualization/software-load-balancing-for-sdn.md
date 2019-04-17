---
title: Software Load Balancing (SLB) per SDN
description: È possibile utilizzare questo argomento per apprendere il bilanciamento del carico Software per la rete definita dal Software in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 97abf182-4725-4026-801c-122db96964ed
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5e08afddde9c7be8d955a0cfdaf44f0fc31b8155
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="software-load-balancing-slb-for-sdn"></a>Software bilanciamento del carico \(SLB\) per SDN

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per apprendere il bilanciamento del carico Software per la rete definita dal Software in Windows Server 2016.  

Provider di servizi cloud (CSP) e le aziende che distribuiscono reti SDN (Software Defined) in Windows Server 2016 è possono utilizzare Software Load Balancing (SLB) per distribuire uniformemente il traffico di rete cliente tenant tra le risorse di rete virtuale e tenant. Windows Server SLB consente più server ospitare stesso carico di lavoro, offrendo disponibilità e scalabilità elevate.
  
Windows Server SLB include le funzionalità seguenti.  
  
-   Livello 4 (L4) caricare i servizi di bilanciamento del carico per il traffico TCP/UDP 'Ovest orientale' e 'Nord-Sud'.  
  
-   Pubblica e interni bilanciamento carico di traffico di rete.  
  
-   Supporta gli indirizzi IP dinamici (DIP) su reti locali virtuali (VLAN) e sulle reti virtuali create mediante virtualizzazione rete Hyper-V.  
  
-   Supporto di probe dello stato.  
  
-   Pronto per la scalabilità cloud, tra cui la funzionalità di scalabilità orizzontale e applicare la scalabilità verticale funzionalità per multiplexer e gli agenti Host.  
  
Per ulteriori informazioni, vedere [funzionalità di bilanciamento del carico Software](#bkmk_features) in questo argomento.  
  
> [!NOTE]  
> Multi-tenancy per le VLAN non è supportato dal Controller di rete, tuttavia è possibile utilizzare le VLAN con SLB per provider di servizi gestiti carichi di lavoro, ad esempio server Web ad alta densità e infrastruttura di Data Center.  
  
Con Windows Server SLB, è possibile scalare orizzontalmente con macchine virtuali SLB sugli stessi server di calcolo Hyper-V che usi per altri carichi di lavoro VM funzionalità di bilanciamento del carico. Per questo motivo, SLB supporta la creazione rapida e l'eliminazione di bilanciamento del carico endpoint che è richiesto per operazioni di CSP. Inoltre, Windows Server SLB supporta decine di GB per ogni cluster, fornisce un modello di provisioning semplice e facile scale-out e in.  
  
**Come funziona SLB**  
  
SLB funziona eseguendo il mapping di indirizzi IP virtuali (VIP) per gli indirizzi IP dinamici (DIP) che fanno parte di un set di servizi cloud di risorse nel Data Center.  
  
Gli indirizzi VIP sono singoli indirizzi IP che forniscono l'accesso pubblico a un pool di carico bilanciato macchine virtuali. Ad esempio, gli indirizzi VIP sono indirizzi IP che vengono esposte su Internet in modo che i tenant e i clienti tenant possono connettersi alle risorse tenant nel data center cloud.  
  
DIP sono gli indirizzi IP di macchine virtuali di un pool di bilanciamento del carico dietro l'indirizzo VIP il membro. All'interno dell'infrastruttura cloud per le risorse del tenant vengono assegnati DIP.  
  
Gli indirizzi VIP si trovano in SLB Multiplexer (MUX).  MUX è costituito da uno o più macchine virtuali (VM).  Controller di rete rappresenta ogni MUX con ogni indirizzo VIP e ogni MUX per annunciare ogni VIP ai router sulla rete fisica come una subnet /32 a sua volta utilizza protocollo BGP (Border Gateway) route.  BGP consente il router di rete fisica per:  
  
-   Scopri che un indirizzo VIP è disponibile in ogni MUX, anche se il MUXes si trovano in subnet diverse in una rete di livello 3.  
  
-   Distribuire il carico per ogni indirizzo VIP tra tutti MUXes disponibili con uguale costo Multi-Path ECMP () di routing.  
  
-   Rileva un errore MUX o la rimozione e interrompere l'invio del traffico a MUX non riuscito automaticamente.  
  
-   Distribuire il carico dal MUX rimosso o non riuscita tra il MUXes integro.  
  
Quando arriva traffico pubblico da Internet, MUX SLB esamina il traffico, che contiene l'indirizzo VIP come una destinazione, mappe e ricrea il traffico in modo che arriveranno un DIP singoli. Per il traffico di rete in ingresso, l'operazione viene eseguita in un processo in due passaggi che è suddivisa tra le macchine virtuali MUX (VM) e l'host Hyper-V in cui si trova la destinazione DIP:  
  
-   Bilanciamento del carico - l'indirizzo VIP per selezionare un DIP, viene utilizzato il MUX incapsula il pacchetto e inoltra il traffico all'host Hyper-V in cui si trova il DIP.  
  
-   Network Address Translation (NAT) - l'host Hyper-V rimuove incapsulamento dal pacchetto, converte l'indirizzo VIP a un DIP, esegue un nuovo mapping delle porte e inoltra il pacchetto per la macchina virtuale DIP.  
  
MUX sa come eseguire il mapping di indirizzi VIP per il DIP corretto a causa di criteri che è possibile definire utilizzando i Controller di rete di bilanciamento del carico. Queste regole sono protocollo, porta front-end, porta di Back-end e algoritmo di distribuzione (2, 3 o 5 tuple).  
  
Quando rispondono macchine virtuali tenant e in uscita invia il traffico di rete a Internet o percorsi remoti tenant, perché NAT viene eseguita dall'host di Hyper-V, il traffico ignora il MUX e passa direttamente al router perimetrale dall'host Hyper-V. Questo processo bypass MUX viene chiamato diretta Server restituire DSR ().  
  
E, dopo aver stabilito il flusso di traffico di rete iniziale, il traffico di rete in entrata Ignora MUX SLB completamente.  
  
Nella figura seguente, un computer client esegue una query DNS per l'indirizzo IP di un società sito di Sharepoint - in questo caso, una società fittizia denominato Contoso. Si verifica quanto segue.  
  
-   Il server DNS restituisce l'indirizzo VIP 107.105.47.60 al client.  
  
-   Il client invia una richiesta HTTP all'indirizzo VIP.  
  
-   La rete fisica è disponibile per raggiungere l'indirizzo VIP in qualsiasi MUX più percorsi.  Ogni router lungo il percorso utilizza ECMP per selezionare il segmento successivo del percorso fino a quando non raggiunge la richiesta di un MUX.  
  
-   Controlla i criteri configurati MUX che riceve la richiesta e vede sono presenti due DIP disponibile, 10.10.10.5 e 10.10.20.5, in una rete virtuale per gestire la richiesta all'indirizzo VIP 107.105.47.60  
  
-   MUX seleziona il DIP 10.10.10.5 e incapsula i pacchetti utilizzando VXLAN, pertanto è possibile inviarlo a host contenente l'indirizzo DIP con gli host indirizzo di rete fisica.  
  
-   L'host riceve il pacchetto incapsulato e la esamina.  Rimuove l'incapsulamento e ricrea il pacchetto in modo che la destinazione è ora il DIP 10.10.10.5 anziché l'indirizzo VIP e invia il traffico di macchina virtuale DIP.  
  
-   La richiesta ora ha raggiunto il sito di Contoso Sharepoint nella Server Farm 2. Il server genera una risposta e lo invia al client, utilizzando il proprio indirizzo IP come origine.  
  
-   L'host intercetta il pacchetto in uscita nel commutatore virtuale che ricorda che il client, ora di destinazione ha effettuato la richiesta originale all'indirizzo VIP.  L'host riscrive l'origine del pacchetto sia l'indirizzo VIP, in modo che il client non visualizzare l'indirizzo DIP.  
  
-   L'host invia il pacchetto direttamente al gateway predefinito per la rete fisica che usa la tabella di routing standard per inoltrare il pacchetto al client che alla fine riceve la risposta.  
  
![Processo di bilanciamento del carico software](../../../media/Software-Load-Balancing--SLB--for-SDN/slb_process.jpg)  
  
**Il carico del traffico datacenter interna**  
  
Quando carico del traffico di rete interno per il Data Center, ad esempio tra le risorse di tenant che sono in esecuzione in server diversi e sono membri della stessa rete virtuale, il commutatore virtuale Hyper-V in cui le macchine virtuali connessi esegue NAT.  
  
Con bilanciamento del carico di traffico interna, la prima richiesta viene inviata al ed elaborata MUX, che seleziona il DIP appropriato e lo instrada il traffico per il DIP. Da quel punto, il flusso di traffico stabilita Ignora MUX e passa direttamente dalla macchina virtuale a macchina virtuale.  
  
**Probe dello stato**  
  
SLB include controlli di integrità per convalidare l'integrità dell'infrastruttura di rete, inclusi i seguenti.  
  
-   Probe TCP alla porta  
  
-   Probe HTTP alla porta e URL  
  
A differenza di un accessorio di bilanciamento del carico tradizionale in cui il probe ha il dispositivo e passa attraverso la rete per il DIP il probe SLB ha origine nell'host in cui si trova il DIP e passa direttamente dall'agente host SLB a DIP, distribuendo ulteriormente le attività tra gli host.  
  
## <a name="bkmk_infrastructure"></a>Infrastruttura di bilanciamento del carico software  
Per distribuire Windows Server SLB, è necessario distribuire Controller di rete in Windows Server 2016 e uno o più macchine virtuali MUX SLB.  
  
Inoltre, è necessario configurare gli host Hyper-V con il commutatore virtuale Hyper-V abilitato SDN e assicurarsi che sia in esecuzione l'agente Host SLB.  I router che servono gli host devono supportare il routing di costo uguale multipath (ECMP) e protocollo BGP (Border Gateway) e devono essere configurati per accettare le richieste di peering BGP dal MUXes SLB.  
  
Di seguito è fornita una panoramica dell'infrastruttura SLB.  

![Infrastruttura di bilanciamento del carico software](../../../media/Software-Load-Balancing--SLB--for-SDN/slb_overview1.png)  
  
Le sezioni seguenti forniscono ulteriori informazioni su questi elementi dell'infrastruttura SLB.  
  
### <a name="scvmm"></a>SCVMM  
Con System Center 2016, è possibile configurare i Controller di rete in Windows Server 2016, incluse le SLB Manager e il monitoraggio dello stato. È anche possibile utilizzare System Center SLB MUXs di distribuire e installare gli agenti Host SLB nei computer che eseguono Hyper-V e Windows Server 2016.  
  
Per ulteriori informazioni su System Center 2016, vedere [System Center 2016](https://www.microsoft.com/en-us/server-cloud/products/system-center-2016/).  
  
> [!NOTE]  
> Se non si desidera utilizzare System Center 2016, è possibile utilizzare Windows PowerShell o un'altra applicazione di gestione per installare e configurare i Controller di rete e un'altra infrastruttura SLB. Per ulteriori informazioni, vedere [distribuire Controller di rete tramite Windows PowerShell](../../../sdn/deploy/Deploy-Network-Controller-using-Windows-PowerShell.md).  
  
### <a name="network-controller"></a>Controller di rete  
Controller di rete ospita il servizio di gestione SLB ed esegue le azioni seguenti per SLB.  
  
-   Elabora i comandi SLB fornite in tramite l'API Northbound da System Center, Windows PowerShell o un'altra applicazione di gestione di rete.  
  
-   Calcola i criteri per la distribuzione di host Hyper-V e SLB MUXes.  
  
-   Fornisce lo stato di integrità dell'infrastruttura SLB.  
  
### <a name="slb-mux"></a>MUX SLB  
MUX SLB elabora il traffico di rete in ingresso ed esegue il mapping di indirizzi VIP a DIP, quindi inoltra il traffico per il DIP corretto. Ogni MUX Usa anche il protocollo BGP per pubblicare le route VIP router perimetrali. BGP Keep-Alive MUXes notifica quando un MUX ha esito negativo, che consente di MUXes active ridistribuire il carico in caso di errore MUX - essenzialmente che fornisce il bilanciamento del carico per servizi di bilanciamento del carico.  
  
### <a name="hosts-that-are-running-hyper-v"></a>Host che eseguono Hyper-V  
È possibile utilizzare SLB con i computer che eseguono Hyper-V e Windows Server 2016. Le macchine virtuali nell'host Hyper-V è possono eseguire qualsiasi sistema operativo supportato da Hyper-V.  
  
### <a name="slb-host-agent"></a>Agente Host SLB  
Quando si distribuisce SLB, è necessario utilizzare System Center, Windows PowerShell o un'altra applicazione di gestione per distribuire l'agente Host SLB su ogni computer host Hyper-V. È possibile installare l'agente Host SLB in tutte le versioni di Windows Server 2016 che forniscono il supporto di Hyper-V, tra cui Nano Server.  
  
L'agente Host SLB rimane in ascolto per gli aggiornamenti dei criteri SLB dal Controller di rete. Inoltre, l'agente host programmi regole per SLB nei commutatori virtuali Hyper-V abilitato SDN configurati nel computer locale.  
  
### <a name="sdn-enabled-hyper-v-virtual-switch"></a>SDN abilitato commutatore virtuale Hyper-V  
Per un commutatore virtuale essere compatibile con SLB, è necessario utilizzare comandi gestione commutatori virtuali Hyper-V o Windows PowerShell per creare il commutatore e quindi è necessario abilitare virtuale filtro piattaforma (VFP) per il commutatore virtuale.  
  
Per informazioni sull'abilitazione VFP in commutatori virtuali, vedere i comandi di Windows PowerShell [Get-VMSystemSwitchExtension](https://technet.microsoft.com/en-us/library/hh848603.aspx) e [Enable-VMSwitchExtension](https://technet.microsoft.com/en-us/library/hh848541.aspx?f=255&MSPPError=-2147217396).  
  
Il SDN abilitato commutatore virtuale Hyper-V esegue le azioni seguenti per SLB.  
  
-   Elabora il percorso dati per SLB.  
  
-   Riceve il traffico di rete in ingresso da MUX.  
  
-   Ignora il MUX per il traffico di rete in uscita, inviarlo al router utilizzando DSR.  
  
-   Viene eseguito sulle istanze di Nano Server di Hyper-V.  
  
### <a name="bgp-enabled-router"></a>Il protocollo BGP abilitato Router  
Il router BGP esegue le azioni seguenti per SLB.  
  
-   Route il traffico in ingresso con ECMP MUX.  
  
-   Per il traffico di rete in uscita, viene utilizzata la route fornita dall'host.  
  
-   È in ascolto per gli aggiornamenti delle route per gli indirizzi VIP da SLB MUX.  
  
-   Rimuove SLB MUXes dalla rotazione SLB in caso di errore Keep-Alive.  
  
## <a name="bkmk_features"></a>Funzionalità di bilanciamento del carico software  
Di seguito sono alcune delle funzionalità e capacità di SLB.  
  
**Funzionalità di base**  
  
-   SLB fornisce Layer 4 bilanciamento del carico servizi per il traffico TCP/UDP 'Ovest orientale' e 'Nord-Sud'  
  
-   È possibile utilizzare SLB in una rete basata su virtualizzazione rete Hyper-V  
  
-   È possibile utilizzare SLB con una rete basata su VLAN per le macchine virtuali DIP connesso a un SDN abilitato Hyper-V commutatore virtuale.  
  
-   Un'istanza SLB può gestire più tenant  
  
-   SLB e DIP supportano un percorso restituito scalabile e bassa latenza, come implementato da diretta Server restituire DSR)  
  
-   Funzioni SLB quando si utilizza anche Switch Embedded Teaming (SET) o Single Root Input/Output Virtualization (SR-IOV)  
  
-   SLB include protocollo Internet versione 4 (IPv4) supportano  
  
-   Per gli scenari di gateway da sito a sito, SLB offre funzionalità NAT per abilitare tutte le connessioni da sito a sito utilizzare un singolo indirizzo IP pubblico  
  
-   È possibile installare SLB, tra cui l'agente Host e MUX, in Windows Server 2016, completo dei componenti di base e installazione di Nano.  
  
**Scalabilità e prestazioni**  
  
-   Pronto per la scalabilità cloud, tra cui la funzionalità di scalabilità orizzontale e applicare la scalabilità verticale funzionalità per MUXes e gli agenti Host.  
  
-   Un modulo di Controller di rete SLB Manager attivo può supportare istanze MUX 8  
  
**Disponibilità elevata**  
  
-   È possibile distribuire SLB in più di 2 nodi in una configurazione attivo/attivo  
  
-   MUXes possono essere aggiunti e rimossi dal pool di MUX senza alcun impatto il servizio SLB. Questo consente di mantenere la disponibilità SLB quando   
    MUXes singoli sono viene corretto.  
  
-   Le singole istanze MUX hanno un tempo di attività del 99%  
  
-   Dati di monitoraggio dello stato è disponibile per le entità di gestione  
  
**Allineamento**  
  
-   È possibile distribuire e configurare SLB con SCVMM  
  
-   SLB fornisce un bordo unificato multi-tenant grazie alla perfetta integrazione con accessori Microsoft come Gateway multi-tenant RAS, il Firewall del centro dati e riflettore delle Route.  
  

