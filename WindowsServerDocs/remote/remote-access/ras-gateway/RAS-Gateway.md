---
title: Gateway RAS
description: Questo argomento, destinato ai professionisti IT (Information Technology), fornisce informazioni generali sul gateway RAS, incluse le modalità di distribuzione del gateway RAS e le funzionalità di Windows Server 2016.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: acaa46b7-09b1-4707-9562-116df8db17eb
ms.author: pashort
author: shortpatti
ms.date: 05/23/2018
ms.openlocfilehash: ebf2cc840be771707f23d7976b670baae96c1343
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367489"
---
# <a name="ras-gateway"></a>Gateway RAS

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Il gateway RAS è un router software e un gateway che è possibile usare in modalità single-tenant o multi-tenant.  
  
- La modalità **single-tenant** consente alle organizzazioni di qualsiasi dimensione di distribuire il gateway come una rete privata virtuale (VPN) perimetrale o con connessione Internet e un server DirectAccess. In modalità a tenant singolo è possibile distribuire il gateway RAS in un server fisico o in una macchina virtuale (VM) che esegue Windows Server 2016.  
  
- La **modalità multi-tenant** consente ai provider di servizi cloud (CSP) e alle aziende di usare il gateway RAS per abilitare il routing del traffico di rete di data center e cloud tra reti virtuali e fisiche, incluso Internet. Per la modalità multi-tenant, è consigliabile distribuire il gateway RAS nelle VM che eseguono Windows Server 2016.  
  
> [!NOTE]  
> Il gateway RAS supporta IPv4 e IPv6, tra cui l'invio IPv4 e IPv6. Quando si configura il gateway RAS con NAT (Network Address Translation), è supportato solo NAT44.  
  
## <a name="who-will-be-interested-in-ras-gateway"></a>Chi sarà interessato al gateway RAS?
  
Se si è un amministratore di sistema, un progettista di rete o un altro professionista IT, il gateway RAS potrebbe interessare l'utente in una o più delle seguenti circostanze:  
  
-   Se si progetta o si supporta l'infrastruttura IT per un'organizzazione che utilizza o intende utilizzare Hyper-V per distribuire macchine virtuali in reti virtuali.  
  
-   Se si progetta o si supporta l'infrastruttura IT per un'organizzazione che ha distribuito o intende distribuire tecnologie cloud.  
  
-   Se si desidera fornire la connettività di rete completa tra le reti fisiche e le reti virtuali.  
  
-   Si desidera fornire ai clienti dell'organizzazione l'accesso alle proprie reti virtuali tramite Internet.  
  
-   Si vuole fornire ai dipendenti dell'organizzazione l'accesso remoto alla rete dell'organizzazione.  
  
-   Si desidera connettere uffici in posizioni fisiche diverse su Internet.  
 
Questo argomento, destinato ai professionisti IT (Information Technology), fornisce informazioni generali sul gateway RAS, incluse le modalità e le funzionalità di distribuzione del gateway RAS. 
  
In questo argomento sono incluse le sezioni seguenti.  
  
  
-   [Modalità di distribuzione del gateway RAS](#bkmk_modes)  
  
-   [Clustering del gateway RAS per la disponibilità elevata](#bkmk_clustering)  
  
-   [Funzionalità del gateway RAS](#bkmk_features)  
  
-   [Scenari di distribuzione del gateway RAS](#bkmk_deploy)  
  
-   [Strumenti di gestione gateway RAS](#bkmk_manage)  
  


  
## <a name="bkmk_modes"></a>Modalità di distribuzione del gateway RAS  
Il gateway RAS include le modalità di distribuzione seguenti.  
  
### <a name="single-tenant-mode"></a>Modalità single-tenant  
Per la maggior parte delle organizzazioni, l'uso del gateway RAS in modalità a tenant singolo è la configurazione tipica. In modalità a tenant singolo è possibile distribuire il gateway RAS come server VPN perimetrale, server DirectAccess perimetrale o entrambi contemporaneamente. In questa configurazione il gateway RAS fornisce ai dipendenti remoti la connettività alla rete usando connessioni VPN o DirectAccess. La modalità single-tenant consente inoltre di connettere uffici in posizioni fisiche diverse su Internet.  
  
### <a name="multitenant-mode"></a>Modalità multi-tenant  
Se l'organizzazione è un CSP o un'azienda con più tenant, è possibile distribuire il gateway RAS in modalità multi-tenant per consentire il routing del traffico di rete da e verso reti virtuali e fisiche.  
  
Il multitenant è la capacità di un'infrastruttura cloud di supportare i carichi di lavoro delle macchine virtuali di più tenant, ma di isolarli tra loro, mentre tutti i carichi di lavoro vengono eseguiti nella stessa infrastruttura. I carichi di lavoro multipli di un singolo tenant possono connettersi tra loro ed essere gestiti in modalità remota, ma questi sistemi non sono collegati con i carichi di lavoro di altri tenant né altri tenant possono gestirli in modalità remota.  
  
Un'azienda potrebbe ad esempio avere molte subnet distinte, ciascuna delle quali dedicata ad un reparto specifico, come Ricerca e sviluppo o Contabilità. Oppure, un CSP potrebbe avere molti tenant con subnet virtuali isolate che si trovano nello stesso centro dati fisico. In entrambi i casi, il gateway RAS può instradare il traffico da e verso ogni tenant mantenendo l'isolamento progettato per ogni tenant. Questa funzionalità rende il gateway RAS compatibile con multi-tenant.  
  
Le reti virtuali vengono create con la virtualizzazione rete Hyper-V, una tecnologia introdotta in Windows Server 2012 e migliorata in Windows Server 2016. Il gateway RAS è integrato con virtualizzazione rete Hyper-V ed è in grado di instradare il traffico di rete in modo efficace nei casi in cui sono presenti molti clienti, o tenant, che hanno reti virtuali isolate nello stesso data center.  
  
Virtualizzazione rete Hyper-V offre la possibilità di distribuire una rete di macchine virtuali (VM) indipendente dalla rete fisica sottostante. Con le reti VM, che sono composte da una o più subnet virtuali, la posizione fisica esatta di una subnet IP è disaccoppiata dalla topologia della rete virtuale. Di conseguenza, è possibile spostare facilmente le subnet locali nel cloud mantenendo gli indirizzi IP e la topologia esistenti nel cloud. La possibilità di conservare l'infrastruttura consente ai servizi esistenti di continuare a funzionare indipendentemente dalla posizione fisica delle subnet. Virtualizzazione rete Hyper-V pertanto consente la realizzazione di un cloud ibrido di facile attuazione.  
  
> [!NOTE]  
> Virtualizzazione rete Hyper-V è una tecnologia di sovrapposizione della rete che usa[NVGRE](https://tools.ietf.org/html/draft-sridharan-virtualization-nvgre-00)(Network Virtualization Generic Routing Encapsulation), che consente ai tenant di portare il proprio spazio di indirizzi e consente ai CSP una migliore scalabilità rispetto a quanto possibile con VLAN per l'isolamento dei tenant.  
  
In Windows Server 2016 il gateway RAS instrada il traffico di rete tra la rete fisica e le risorse di rete della macchina virtuale, indipendentemente dalla posizione in cui si trovano le risorse. È possibile usare il gateway RAS per instradare il traffico di rete tra reti fisiche e virtuali nella stessa posizione fisica o in numerose posizioni fisiche diverse.  
  
Se, ad esempio, si dispone sia di una rete fisica che di una rete virtuale nella stessa posizione fisica, è possibile distribuire un computer che esegue Hyper-V configurato con una macchina virtuale del gateway RAS che funga da gateway di inoltro e instradare il traffico tra la rete virtuale e fisica reti.  
  
In un altro esempio, se le reti virtuali sono presenti nel cloud, il CSP può distribuire un gateway RAS in modo che sia possibile creare una connessione da sito a sito di rete privata virtuale (VPN) tra il server VPN e il gateway RAS del CSP. Quando viene stabilito questo collegamento, è possibile connettersi alle risorse virtuali nel cloud tramite la connessione VPN.  
  
Per ulteriori informazioni, vedere [RAS Gateway un'elevata disponibilità](../../../networking/sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md).  
  
## <a name="bkmk_clustering"></a>Clustering del gateway RAS per la disponibilità elevata  
Il gateway RAS viene distribuito in un computer dedicato che esegue Hyper-V e che è configurato con una macchina virtuale. La macchina virtuale viene quindi configurata come gateway RAS.  
  
Per la disponibilità elevata delle risorse di rete, è possibile distribuire il gateway RAS con failover usando due server host fisici che eseguono Hyper-V, ognuno dei quali esegue anche una macchina virtuale (VM) configurata come gateway. Le macchine virtuali del gateway vengono quindi configurate come cluster per assicurare la protezione del failover nel caso si verifichino interruzioni della rete o errori hardware.  
  
Ad esempio, se l'organizzazione è un'azienda con una distribuzione cloud privata, potrebbero essere necessarie solo due VM gateway RAS, ciascuna delle quali è installata in un computer diverso che esegue Hyper-V. In questo scenario, le VM del gateway RAS vengono aggiunte a un cluster per garantire una disponibilità elevata.  
  
In un altro esempio, se l'organizzazione è un provider di servizi cloud (CSP) con 200 tenant nel Data Center, è possibile usare otto macchine virtuali del gateway RAS, con ogni coppia di macchine virtuali del gateway RAS in cluster che forniscono servizi di routing per i tenant 50. In questo scenario, due computer che eseguono Hyper-V hanno quattro VM configurate come gateway RAS. Si configurano quindi quattro cluster di macchine virtuali del gateway RAS, ciascuno dei quali contiene una VM da ogni computer che esegue Hyper-V.  
  
Quando si distribuisce il gateway RAS, i server host che eseguono Hyper-V e le macchine virtuali configurate come gateway devono eseguire Windows Server 2012 R2 o Windows Server 2016.  
  
## <a name="bkmk_features"></a>Funzionalità del gateway RAS  
Il gateway RAS include le funzionalità seguenti.  
  
-   **VPN da sito a sito**. Questa funzionalità del gateway RAS consente di connettere due reti in posizioni fisiche diverse su Internet tramite una connessione VPN da sito a sito. Se si dispone di una sede centrale e di più succursali, è possibile distribuire un gateway RAS perimetrale in ogni posizione e creare connessioni da sito a sito per fornire il flusso del traffico di rete tra le località. Per i CSP che ospitano molti tenant nel proprio Data Center, il gateway RAS fornisce una soluzione gateway multi-tenant che consente ai tenant di accedere alle proprie risorse e gestirle tramite connessioni VPN da sito a sito da siti remoti e che consente il flusso del traffico di rete tra risorse virtuali nel Data Center e nella rete fisica.  
  
-   **VPN da punto a sito**. Questa funzionalità gateway RAS consente ai dipendenti o agli amministratori dell'organizzazione di connettersi alla rete dell'organizzazione da posizioni remote. Per le distribuzioni con tenant singolo del gateway RAS, i dipendenti remoti possono connettersi alla rete aziendale usando una connessione VPN. Questa connessione consente loro di utilizzare le risorse di rete interne, ad esempio siti Web e file server Intranet. Per le distribuzioni multi-tenant, gli amministratori di rete tenant possono usare connessioni VPN da punto a sito per accedere alle risorse di rete virtuale nel data center CSP.  
  
-   **Routing dinamico con Border Gateway Protocol (BGP)** . BGP, essendo un protocollo di routing dinamico che apprende automaticamente le route tra i siti connessi da connessioni VPN da sito a sito, riduce la necessità di configurare manualmente le route sui router. Se l'organizzazione dispone di più siti connessi tramite router abilitati per BGP, ad esempio gateway RAS, BGP consente ai router di calcolare e utilizzare automaticamente route valide in caso di interruzione della rete o di errore. Per ulteriori informazioni, vedere [RFC 4271](https://tools.ietf.org/html/rfc4271).  
  
-   **NAT (Network Address Translation)** . NAT (Network Address Translation) consente di condividere una connessione alla rete Internet pubblica tramite una singola interfaccia con un singolo indirizzo IP pubblico. I computer della rete privata utilizzano indirizzi privati e non instradabili. NAT esegue il mapping degli indirizzi privati all'indirizzo pubblico. Questa funzionalità del gateway RAS consente ai dipendenti dell'organizzazione con distribuzioni con tenant singolo di accedere alle risorse Internet da dietro il gateway. Per i CSP, questa funzionalità consente alle applicazioni in esecuzione nelle macchine virtuali tenant di accedere a Internet. Ad esempio, una VM tenant configurata come server Web può contattare le risorse finanziarie esterne per elaborare le transazioni della carta di credito.  

  
## <a name="bkmk_deploy"></a>Scenari di distribuzione del gateway RAS  
Di seguito sono riportati gli scenari di distribuzione consigliati per il gateway RAS.  
  
-   **Enterprise Edge-distribuzione a tenant singolo**. Con la distribuzione aziendale a tenant singolo, è possibile connettere un computer fisico a più posizioni fisiche in Internet usando la funzionalità VPN da sito a sito e il Border Gateway Protocol (BGP) consente di usare il routing dinamico. È anche possibile fornire ai dipendenti remoti l'accesso alla rete aziendale con connessioni VPN da punto a sito e connessioni DirectAccess. Le connessioni DirectAccess sono sempre attivate e offrono inoltre il vantaggio di poter gestire facilmente i computer connessi tramite DirectAccess, perché sono connessi quando sono connessi e connessi a Internet. È anche possibile configurare gateway RAS Enterprise a tenant singolo con NAT, in modo che i computer della rete Intranet possano comunicare facilmente con Internet.  
  
-   **Edge del provider di servizi cloud-distribuzione multi-tenant**. La distribuzione multi-tenant del gateway RAS per CSP consente di offrire ai tenant tutte le funzionalità disponibili con la distribuzione a tenant singolo di Enterprise Edge. Le connessioni VPN da sito a sito tra reti virtuali tenant nel Data Center e i percorsi di rete tenant attraverso Internet significano che i tenant hanno sempre accesso alle risorse cloud in modo trasparente. L'accesso VPN da punto a sito per i tenant significa che gli amministratori tenant possono sempre connettersi alle reti virtuali nel Data Center per gestire le proprie risorse. BGP fornisce il routing dinamico e mantiene i tenant connessi alle risorse anche quando si verificano problemi di rete in Internet o altrove. E NAT consente alle macchine virtuali tenant di connettersi alle risorse su Internet, ad esempio le risorse di elaborazione della carta di credito.  
  
## <a name="bkmk_manage"></a>Strumenti di gestione gateway RAS  
Di seguito sono riportati gli strumenti di gestione per gateway RAS.  
  
-   In Windows Server 2016, per distribuire un router gateway RAS, è necessario utilizzare i comandi di Windows PowerShell. Per altre informazioni, vedere [cmdlet di accesso remoto](https://docs.microsoft.com/powershell/module/remoteaccess) per windows server 2016 e Windows 10.  
  
-   In System Center 2012 R2 Virtual Machine Manager (VMM) il gateway RAS è denominato gateway Windows Server. Nell'interfaccia del software VMM è disponibile un set limitato di opzioni di configurazione BGP (Border Gateway Protocol), inclusi l' **indirizzo IP BGP locale** e i **numeri di sistema autonomo (ASN)** , l' **elenco di indirizzi IP del peer BGP**e **i valori ASN** . È comunque possibile utilizzare i comandi BGP di Windows PowerShell per l'accesso remoto per configurare tutte le altre funzionalità di Windows Server Gateway. Per ulteriori informazioni, vedere [Virtual Machine Manager (VMM)](https://technet.microsoft.com/system-center-docs/vmm/vmm) e [cmdlet di accesso remoto](https://technet.microsoft.com/library/hh918399.aspx) per Windows Server 2016 e Windows 10.  
  
## <a name="related-topics"></a>Argomenti correlati
- [Disponibilità elevata del gateway RAS](../../../networking/sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)  
- [Tunneling GRE in Windows Server](gre-tunneling-windows-server.md)
- [Prestazioni e velocità effettiva del tunnel GRE di Gateway RAS](RAS-Gateway-GRE-Perf.md)
