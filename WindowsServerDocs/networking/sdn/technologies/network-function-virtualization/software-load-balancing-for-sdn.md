---
title: Bilanciamento del carico software (SLB) per SDN
description: È possibile utilizzare questo argomento per scoprire il bilanciamento del carico Software per Software Defined Networking di Windows Server 2016.
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
ms.openlocfilehash: 26fb4aa21e80618c4c63bd9edbf8731bf886db62
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853762"
---
# <a name="software-load-balancing-slb-for-sdn"></a>Software Load Balancing \(SLB\) per SDN

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per scoprire il bilanciamento del carico Software per Software Defined Networking di Windows Server 2016.  

Provider di servizi cloud (CSP) e le aziende che distribuiscono reti SDN (Software Defined) in Windows Server 2016 è possono usare il bilanciamento del carico Software (SLB) per distribuire uniformemente il traffico di rete cliente tenant tra le risorse della rete virtuale e del tenant. Windows Server SLB consente di abilitare più server per l'hosting dello stesso carico di lavoro, offrendo disponibilità e scalabilità elevate.
  
Bilanciamento del carico software di Windows Server include le funzionalità seguenti.  
  
-   Servizi di bilanciamento del carico per il traffico TCP/UDP 'East-West' e 'Nord-Sud' carico di livello 4 (L4).  
  
-   Pubblici e interni di bilanciamento del carico del traffico di rete.  
  
-   Supporta gli indirizzi IP dinamici (DIP) in reti locali virtuali (VLAN) e nelle reti virtuali create mediante virtualizzazione rete Hyper-V.  
  
-   Supporto di probe di integrità.  
  
-   Pronti per la scalabilità cloud, tra cui funzionalità di scalabilità orizzontale e aumentare la funzionalità per multiplexer e gli agenti Host.  
  
Per altre informazioni, vedere [funzionalità di bilanciamento del carico Software](#bkmk_features) in questo argomento.  
  
> [!NOTE]  
> Multi-tenancy per le VLAN non è supportata dal Controller di rete, tuttavia è possibile usare reti VLAN con bilanciamento del carico software per provider di servizi gestiti i carichi di lavoro, ad esempio l'infrastruttura del Data Center e i server Web ad alta densità.  
  
Usa bilanciamento del carico software di Windows Server, è possibile scalare orizzontalmente le funzionalità usando le macchine virtuali SLB negli stessi server di calcolo Hyper-V che utilizzano per altri carichi di lavoro della macchina virtuale di bilanciamento del carico. Per questo motivo, bilanciamento del carico software supporta la creazione rapida e l'eliminazione dell'endpoint di bilanciamento del carico che è necessaria per operazioni di CSP. Inoltre, SLB di Windows Server supporta decine di GB per ogni cluster, fornisce un semplice modello di provisioning ed è semplice applicare la scalabilità orizzontale.  
  
**Funzionamento del bilanciamento del carico software**  
  
Bilanciamento del carico software funziona eseguendo il mapping di indirizzi IP virtuali (VIP) per gli indirizzi IP dinamici (DIP) che fanno parte di un set di servizi cloud di risorse nel Data Center.  
  
Gli indirizzi VIP sono singoli indirizzi IP che forniscono l'accesso pubblico a un pool di carico bilanciato le macchine virtuali. Ad esempio, gli indirizzi VIP sono indirizzi IP che sono esposte in Internet in modo che i tenant e i clienti tenant possono connettersi alle risorse del tenant nel data center cloud.  
  
DIP sono gli indirizzi IP delle macchine virtuali di un pool con carico bilanciato dietro l'indirizzo VIP del membro. Dedicati vengono assegnati all'interno dell'infrastruttura cloud per le risorse del tenant.  
  
Indirizzi IP virtuali si trovano nel Multiplexer (MUX SLB).  Il MUX è costituito da uno o più macchine virtuali (VM).  Controller di rete offre ogni MUX con ogni indirizzo IP virtuale e ogni MUX per annunciare ogni indirizzo IP virtuale per i router sulla rete fisica come/32 equivale a sua volta utilizza protocollo BGP (Border Gateway) route.  BGP consente i router di rete fisica per:  
  
-   Informazioni che un indirizzo VIP è disponibile in ogni MUX, anche se i MUX si trovano in subnet diverse in una rete di livello 3.  
  
-   Distribuire il carico per ogni indirizzo IP virtuale tra tutti i MUX disponibili con uguale costo percorsi multipli ECMP () di routing.  
  
-   Automaticamente di MUX guasto o rimozione di rilevare e arrestare l'invio di traffico per il MUX non riuscito.  
  
-   Dividere il carico dal MUX non riuscito o rimosso tra i MUX integro.  
  
Quando si riceve il traffico pubblico da Internet, il MUX SLB esamina il traffico, che contiene l'indirizzo VIP come destinazione e viene eseguito il mapping e riscrive il traffico in modo che arriverà a un DIP singoli. Per il traffico di rete in ingresso, questa transazione viene eseguita in un processo in due passaggi che è suddiviso tra le macchine virtuali MUX (VM) e l'host Hyper-V in cui si trova il DIP di destinazione:  
  
-   Il bilanciamento del carico - l'indirizzo VIP per selezionare un DIP, viene utilizzato il MUX incapsula il pacchetto e inoltra il traffico all'host Hyper-V in cui si trova il DIP.  
  
-   Network Address Translation (NAT): l'host Hyper-V rimuove incapsulamento dal pacchetto, converte l'indirizzo VIP a un DIP, esegue un nuovo mapping di porte e inoltra il pacchetto per il DIP della macchina virtuale.  
  
Il MUX sa come eseguire il mapping di indirizzi VIP per il DIP corrette a causa di criteri che definiscono usando Controller di rete di bilanciamento del carico. Queste regole includono protocollo, porta front-end, porta Back-end e l'algoritmo di distribuzione (2, 3 o 5 tuple).  
  
Quando rispondono macchine virtuali tenant e trasmissione in uscita il traffico di rete alla Internet o posizioni remote tenant, perché viene eseguito il NAT dall'host Hyper-V, il traffico ignora il MUX e passa direttamente al router perimetrale dall'host Hyper-V. Questo processo di bypass MUX è denominato Direct Server Return (DSR).  
  
E dopo aver stabilito il flusso del traffico di rete iniziale, il traffico di rete in ingresso ignora completamente il MUX SLB.  
  
Nella figura seguente, un computer client esegue una query DNS per l'indirizzo IP di un società sito di SharePoint: in questo caso, una società fittizia denominata Contoso. Si verifica quanto segue.  
  
-   Il server DNS restituisce l'indirizzo VIP 107.105.47.60 al client.  
  
-   Il client invia una richiesta HTTP all'indirizzo VIP.  
  
-   La rete fisica dispone di più percorsi disponibili per raggiungere l'indirizzo VIP che si trova in qualsiasi MUX.  Ogni router lungo il percorso Usa ECMP per prelevare il segmento successivo del percorso fino a quando la richiesta arriva un MUX.  
  
-   Controlla i criteri configurati, il MUX che riceve la richiesta e vede che non vi siano due flessioni disponibili, 10.10.10.5 e 10.10.20.5, in una rete virtuale per gestire la richiesta all'indirizzo VIP 107.105.47.60  
  
-   Il MUX seleziona il DIP 10.10.10.5 e incapsula i pacchetti utilizzando VXLAN, pertanto è possibile inviarlo all'host che contiene il DIP usando gli host indirizzo di rete fisica.  
  
-   L'host riceve il pacchetto incapsulato e si controlla.  Rimuove l'incapsulamento e riscrive il pacchetto in modo che la destinazione è ora il DIP 10.10.10.5 anziché l'indirizzo VIP e invia il traffico al DIP della macchina virtuale.  
  
-   La richiesta ha raggiunto ora il sito Contoso SharePoint nella Server Farm 2. Il server genera una risposta e lo invia al client, usando il proprio indirizzo IP come origine.  
  
-   L'host intercetta il pacchetto in uscita nel commutatore virtuale che ricorda che il client, a questo punto, la destinazione ha effettuato la richiesta originale all'indirizzo VIP.  L'host riscrive l'origine del pacchetto sia l'indirizzo VIP, in modo che il client non vede l'indirizzo DIP.  
  
-   L'host inoltra direttamente il pacchetto del gateway predefinito per la rete fisica che usa la tabella di routing standard per inoltrare il pacchetto al client che viene ricevuta la risposta alla fine.  
  
![Processo di bilanciamento del carico software](../../../media/Software-Load-Balancing--SLB--for-SDN/slb_process.jpg)  
  
**Il carico del traffico interno Data Center**  
  
Quando il carico del traffico di rete interno al Data Center, ad esempio tra risorse del tenant in esecuzione in server diversi e sono membri della stessa rete virtuale, il commutatore virtuale Hyper-V in cui le macchine virtuali sono connesse esegue NAT.  
  
Con il bilanciamento del carico di traffico interno, la prima richiesta viene inviata a ed elaborata dal MUX, che seleziona il DIP appropriato e lo indirizza il traffico verso il DIP. In questo modo, il flusso del traffico stabiliti ignora il MUX e passa direttamente dalla macchina virtuale alla macchina virtuale.  
  
**Probe di integrità**  
  
Bilanciamento del carico software include i probe di integrità per convalidare l'integrità dell'infrastruttura di rete, inclusi i seguenti.  
  
-   Probe TCP alla porta  
  
-   Probe HTTP alla porta e URL  
  
A differenza di un accessorio bilanciamento del carico tradizionale in cui il probe ha origine nell'appliance e viaggia attraverso la rete per il DIP, il probe di bilanciamento del carico software ha origine nell'host in cui si trova il DIP e si svolge direttamente tra l'agente host di bilanciamento del carico software per il DIP, ulteriormente la distribuzione di funziona tra gli host.  
  
## <a name="bkmk_infrastructure"></a>Infrastruttura di bilanciamento del carico software  
Per distribuire SLB di Windows Server, è necessario distribuire Controller di rete in Windows Server 2016 e uno o più macchine virtuali MUX SLB.  
  
Inoltre, è necessario configurare gli host Hyper-V con il commutatore virtuale Hyper-V abilitato SDN e assicurarsi che l'agente Host di bilanciamento del carico software è in esecuzione.  I router che servono l'host devono supportare il routing di costo uguale multipath (ECMP) e protocollo BGP (Border Gateway) e devono essere configurati per accettare richieste di peering BGP dalle istanze MUX SLB.  
  
Di seguito è fornita una panoramica dell'infrastruttura di bilanciamento del carico software.  

![Infrastruttura di bilanciamento del carico software](../../../media/Software-Load-Balancing--SLB--for-SDN/slb_overview1.png)  
  
Le sezioni seguenti forniscono altre informazioni su questi elementi dell'infrastruttura di bilanciamento del carico software.  
  
### <a name="scvmm"></a>SCVMM  
Con System Center 2016, è possibile configurare i Controller di rete in Windows Server 2016, tra cui gestione SLB e monitoraggio dell'integrità. È anche possibile usare System Center per distribuire SLB MUXs e per installare gli agenti Host di bilanciamento del carico software nei computer che eseguono Hyper-V e Windows Server 2016.  
  
Per altre informazioni su System Center 2016, vedere [System Center 2016](https://www.microsoft.com/server-cloud/products/system-center-2016/).  
  
> [!NOTE]  
> Se non vuoi usare System Center 2016, è possibile usare Windows PowerShell o un'altra applicazione di gestione per installare e configurare Controller di rete e altra infrastruttura di bilanciamento del carico software. Per altre informazioni, vedere [distribuire Controller di rete tramite Windows PowerShell](../../../sdn/deploy/Deploy-Network-Controller-using-Windows-PowerShell.md).  
  
### <a name="network-controller"></a>Controller di rete  
Controller di rete ospita il gestore bilanciamento del carico software ed esegue le azioni seguenti per SLB.  
  
-   Elabora i comandi di bilanciamento del carico software tramite l'API Northbound provengono da System Center, Windows PowerShell o un'altra applicazione di gestione di rete.  
  
-   Calcola i criteri per la distribuzione di host Hyper-V e le istanze MUX SLB.  
  
-   Fornisce lo stato di integrità dell'infrastruttura di bilanciamento del carico software.  
  
### <a name="slb-mux"></a>SLB MUX  
Il MUX SLB elabora il traffico di rete in ingresso ed esegue il mapping di indirizzi VIP in DIP, quindi inoltra il traffico per il DIP corretto. Ogni MUX Usa anche il protocollo BGP per pubblicare le route di VIP per i router perimetrali. BGP Keep-Alive i MUX di notifica quando un MUX ha esito negativo, che consente i MUX active ridistribuire il carico in caso di errore MUX - essenzialmente fornendo bilanciamento del carico per servizi di bilanciamento del carico.  
  
### <a name="hosts-that-are-running-hyper-v"></a>Host che eseguono Hyper-V  
È possibile usare SLB con i computer che eseguono Hyper-V e Windows Server 2016. Le macchine virtuali nell'host Hyper-V possono eseguire qualsiasi sistema operativo supportato da Hyper-V.  
  
### <a name="slb-host-agent"></a>Agente Host di bilanciamento del carico software  
Quando si distribuisce SLB, è necessario utilizzare System Center, Windows PowerShell o un'altra applicazione di gestione per distribuire l'agente Host di bilanciamento del carico software in ogni computer host Hyper-V. È possibile installare l'agente Host di bilanciamento del carico software su tutte le versioni di Windows Server 2016 che forniscono il supporto di Hyper-V, inclusi Nano Server.  
  
L'agente Host di bilanciamento del carico software è in ascolto per gli aggiornamenti dei criteri di bilanciamento del carico software dal Controller di rete. Inoltre, l'agente host programmi regole di bilanciamento del carico software nei commutatori virtuali Hyper-V SDN abilitato che sono configurati nel computer locale.  
  
### <a name="sdn-enabled-hyper-v-virtual-switch"></a>SDN abilitato commutatore virtuale Hyper-V  
Per un commutatore virtuale sia compatibile con bilanciamento del carico software, è necessario usare i comandi di gestione commutatori virtuali Hyper-V o Windows PowerShell per creare il commutatore e quindi è necessario abilitare virtuale filtro piattaforma (VFP) per il commutatore virtuale.  
  
Per informazioni su come abilitare VFP affidano ai commutatori virtuali, vedere i comandi di Windows PowerShell [Get-VMSystemSwitchExtension](https://technet.microsoft.com/library/hh848603.aspx) e [Enable-VMSwitchExtension](https://technet.microsoft.com/library/hh848541.aspx?f=255&MSPPError=-2147217396).  
  
SDN abilitato commutatore virtuale Hyper-V esegue le azioni seguenti per SLB.  
  
-   Elabora il percorso dei dati per SLB.  
  
-   Riceve il traffico di rete in ingresso dal MUX.  
  
-   Consente di ignorare il MUX per il traffico di rete in uscita, inviarlo al router utilizzando DSR.  
  
-   Viene eseguito sulle istanze di Nano Server di Hyper-V.  
  
### <a name="bgp-enabled-router"></a>BGP abilitato Router  
Il router BGP esegue le azioni seguenti per SLB.  
  
-   Instrada il traffico in ingresso per il MUX con ECMP.  
  
-   Per il traffico di rete in uscita, viene utilizzata la route fornita dall'host.  
  
-   È in ascolto per gli aggiornamenti delle route per gli indirizzi VIP da MUX SLB.  
  
-   Rimuove le istanze MUX SLB dalla rotazione del bilanciamento del carico software se non riesce Keep-Alive.  
  
## <a name="bkmk_features"></a>Funzionalità di bilanciamento del carico software  
Di seguito sono alcune delle funzionalità e le funzionalità di bilanciamento del carico software.  
  
**Funzionalità di base**  
  
-   Bilanciamento del carico software fornisce servizi per il traffico TCP/UDP 'East-West' e 'Nord-Sud' di bilanciamento del carico di livello 4  
  
-   È possibile utilizzare Bilanciamento del carico software in una rete basata su virtualizzazione rete Hyper-V  
  
-   È possibile usare SLB con una rete basata su VLAN per le macchine virtuali DIP connesso a una rete SDN abilitato Hyper-V Virtual Switch.  
  
-   Un'istanza di bilanciamento del carico software può gestire più tenant  
  
-   Bilanciamento del carico software e DIP supporta un percorso di ritorno scalabile e a bassa latenza, come implementato dalla Direct Server Return (DSR)  
  
-   Funzioni di bilanciamento del carico software quando si usa anche Switch Embedded Teaming (SET) o Single Root Input/Output Virtualization (SR-IOV)  
  
-   Bilanciamento del carico software include protocollo Internet versione 4 (IPv4) supportano  
  
-   Per gli scenari di gateway da sito a sito, bilanciamento del carico software fornisce la funzionalità NAT per abilitare tutte le connessioni site-to-site utilizzare un singolo indirizzo IP pubblico  
  
-   È possibile installare Bilanciamento del carico software, tra cui l'agente Host e i MUX, in Windows Server 2016, Full, Core e installazione di Nano.  
  
**Scalabilità e prestazioni**  
  
-   Pronti per la scalabilità cloud, tra cui funzionalità di scalabilità orizzontale e aumentare la funzionalità per gli agenti Host e i MUX.  
  
-   Uno dei moduli Controller di rete di gestione SLB active può supportare 8 istanze MUX  
  
**disponibilità elevata**  
  
-   È possibile distribuire SLB a più di 2 nodi in una configurazione attivo/attivo  
  
-   I MUX possono essere aggiunti e rimossi dal pool di MUX senza conseguenze per il servizio di bilanciamento del carico software. Questo consente di mantenere la disponibilità SLB quando   
    i singoli MUX sono in corso patch.  
  
-   Le istanze MUX singole hanno un tempo di attività del 99%  
  
-   È disponibile per le entità di gestione dati di monitoraggio dell'integrità  
  
**Allineamento**  
  
-   È possibile distribuire e configurare SLB con SCVMM  
  
-   Bilanciamento del carico software offre un margine unificato multi-tenant grazie alla perfetta integrazione con le Appliance di Microsoft, ad esempio il Gateway multi-tenant RAS Firewall del centro dati e riflettore delle Route.  
  

