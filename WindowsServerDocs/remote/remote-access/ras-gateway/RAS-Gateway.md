---
title: Gateway RAS
description: Questo argomento, destinato ai professionisti IT (Information Technology), fornisce informazioni generali sul Gateway RAS, incluse le modalità di distribuzione di Gateway RAS e funzionalità in Windows Server 2016.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: acaa46b7-09b1-4707-9562-116df8db17eb
ms.author: pashort
author: shortpatti
ms.date: 05/23/2018
ms.openlocfilehash: d61dcdbb61449bd2af57b8e2c99ced6235c4deca
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281258"
---
# <a name="ras-gateway"></a>Gateway RAS

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Gateway RAS è un router software e gateway che è possibile usare in modalità a tenant singolo o in modalità multi-tenant.  
  
- **Singolo tenant** modalità consente alle organizzazioni di qualsiasi dimensione per la distribuzione del gateway come un anello esterno, o rete privata virtuale con connessione Internet edge (VPN) e server DirectAccess. In modalità a tenant singolo, è possibile distribuire Gateway RAS su un server fisico o macchina virtuale (VM) che esegue Windows Server 2016.  
  
- **Modalità multi-tenant** consente a provider di servizi Cloud (CSP) e alle aziende di usare il Gateway RAS per abilitare Data Center e cloud il routing del traffico di rete tra reti virtuali e fisiche, Internet incluso. Per la modalità multi-tenant, è consigliabile distribuire il Gateway RAS su macchine virtuali che eseguono Windows Server 2016.  
  
> [!NOTE]  
> Il Gateway RAS supporta IPv4 e IPv6, incluso l'inoltro IPv4 e IPv6. Quando si configura Gateway RAS con Network Address Translation (NAT), è supportato solo NAT44.  
  
## <a name="who-will-be-interested-in-ras-gateway"></a>Chi sarà interessato in Gateway RAS?
  
Se sei un amministratore di sistema, architetti di rete o altri professionisti IT, Gateway RAS può rivelarsi utile in uno o più delle circostanze seguenti:  
  
-   Se si progetta o si supporta l'infrastruttura IT per un'organizzazione che utilizza o intende utilizzare Hyper-V per distribuire macchine virtuali in reti virtuali.  
  
-   Se si progetta o si supporta l'infrastruttura IT per un'organizzazione che ha distribuito o intende distribuire tecnologie cloud.  
  
-   Se si desidera fornire la connettività di rete completa tra le reti fisiche e le reti virtuali.  
  
-   Si desidera fornire ai clienti della propria organizzazione l'accesso alle loro reti virtuali tramite Internet.  
  
-   Si desidera fornire ai dipendenti dell'organizzazione con accesso remoto alla rete dell'organizzazione.  
  
-   Si desidera connettere gli uffici in posizioni fisiche diverse tramite Internet.  
 
Questo argomento, destinato ai professionisti IT (Information Technology), fornisce informazioni generali sul Gateway RAS, che include funzionalità e le modalità di distribuzione di Gateway RAS. 
  
In questo argomento sono incluse le sezioni seguenti.  
  
  
-   [Modalità di distribuzione di Gateway RAS](#bkmk_modes)  
  
-   [Il servizio cluster di Gateway RAS per la disponibilità elevata](#bkmk_clustering)  
  
-   [Funzionalità del Gateway RAS](#bkmk_features)  
  
-   [Scenari di distribuzione di Gateway RAS](#bkmk_deploy)  
  
-   [Strumenti di Gestione Gateway RAS](#bkmk_manage)  
  


  
## <a name="bkmk_modes"></a>Modalità di distribuzione di Gateway RAS  
Gateway RAS include le seguenti modalità di distribuzione.  
  
### <a name="single-tenant-mode"></a>Modalità a tenant singolo  
Per la maggior parte delle organizzazioni, l'utilizzo di Gateway RAS in modalità a tenant singolo è la tipica configurazione. In modalità a tenant singolo, è possibile distribuire contemporaneamente RAS Gateway come un server VPN edge, un server DirectAccess perimetrale o entrambi. In questa configurazione, Gateway RAS fornisce i dipendenti remoti con connettività alla rete tramite connessioni VPN o DirectAccess. Modalità a tenant singolo, inoltre, consente di connettere gli uffici in posizioni fisiche diverse tramite Internet.  
  
### <a name="multitenant-mode"></a>Modalità multi-tenant  
Se l'organizzazione è un CSP o un'organizzazione con più tenant, è possibile distribuire Gateway RAS in modalità multi-tenant per fornire rete il routing del traffico da e verso reti virtuali e fisiche.  
  
Multi-tenancy è la capacità di un'infrastruttura cloud per supportare i carichi di lavoro di macchine virtuali di più tenant, ancora isolarle tra loro, anche se tutti i carichi di lavoro eseguiti nella stessa infrastruttura. I carichi di lavoro multipli di un singolo tenant possono connettersi tra loro ed essere gestiti in modalità remota, ma questi sistemi non sono collegati con i carichi di lavoro di altri tenant né altri tenant possono gestirli in modalità remota.  
  
Un'azienda potrebbe ad esempio avere molte subnet distinte, ciascuna delle quali dedicata ad un reparto specifico, come Ricerca e sviluppo o Contabilità. Oppure, un CSP potrebbe avere molti tenant con subnet virtuali isolate che si trovano nello stesso centro dati fisico. In entrambi i casi RAS Gateway può instradare il traffico da e verso ogni tenant, mantenendo l'isolamento di ogni tenant progettato. Questa funzionalità rende il Gateway RAS-tenant.  
  
Le reti virtuali vengono create mediante virtualizzazione rete Hyper-V, che è una tecnologia che è stata introdotta in Windows Server 2012 ed è stata migliorata in Windows Server 2016. Gateway RAS è integrato con virtualizzazione rete Hyper-V ed è in grado di instradare il traffico di rete in modo efficace nelle situazioni in cui sono presenti molti clienti - o - tenant che dispongono di isolati virtual networks dello stesso Data Center.  
  
Virtualizzazione rete Hyper-V offre la possibilità di distribuire una rete di macchina virtuale (VM) che è indipendente dalla rete fisica sottostante. Con le reti VM, che sono costituite da uno o più subnet virtuali, la posizione fisica esatta di una subnet IP è disaccoppiata dalla topologia della rete virtuale. Di conseguenza, è possibile spostare facilmente le subnet locale via per il cloud, mentre mantenendo l'indirizzo IP di indirizzi e della topologia nel cloud. La possibilità di conservare l'infrastruttura consente ai servizi esistenti di continuare a funzionare indipendentemente dalla posizione fisica delle subnet. Virtualizzazione rete Hyper-V pertanto consente la realizzazione di un cloud ibrido di facile attuazione.  
  
> [!NOTE]  
> Virtualizzazione rete Hyper-V è una tecnologia di overlay della rete tramite Network Virtualization Generic Routing Encapsulation ([NVGRE](https://tools.ietf.org/html/draft-sridharan-virtualization-nvgre-00)) che consente ai tenant di mantenere i propri spazi degli indirizzi CSP di ottenere una scalabilità migliore rispetto a è possibile eseguire usando le reti VLAN per l'isolamento dei tenant.  
  
In Windows Server 2016 RAS Gateway instrada il traffico di rete tra la rete fisica e le risorse di rete della macchina virtuale, indipendentemente dalla posizione le risorse. È possibile usare Gateway RAS per instradare il traffico di rete tra reti fisiche e virtuali nella stessa posizione fisica o in numerose posizioni fisiche diverse.  
  
Ad esempio, se si dispone di una rete fisica e una rete virtuale nella stessa posizione fisica, è possibile distribuire un computer che esegue Hyper-V configurato con una macchina virtuale Gateway RAS di funzionare come gateway di inoltro e instradare il traffico tra la fisica e virtuale reti.  
  
In un altro esempio, se le reti virtuali si trovano nel cloud, il CSP può distribuire un Gateway RAS in modo che sia possibile creare una connessione di rete privata virtuale (VPN) site-to-site tra il server VPN e di CSP RAS Gateway; Quando questo collegamento viene stabilito tramite la connessione VPN è possibile connettersi alle risorse virtuali nel cloud.  
  
Per ulteriori informazioni, vedere [RAS Gateway un'elevata disponibilità](../../../networking/sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md).  
  
## <a name="bkmk_clustering"></a>Il servizio cluster di Gateway RAS per la disponibilità elevata  
Gateway RAS è distribuito in un computer dedicato che esegue Hyper-V e che sia configurato con una macchina virtuale. La macchina virtuale viene quindi configurata come un Gateway RAS.  
  
Per la disponibilità elevata di risorse di rete, è possibile distribuire Gateway RAS con il failover con due server host fisici che eseguono Hyper-V che eseguono ogni anche una macchina virtuale (VM) che è stata configurata come gateway. Le macchine virtuali del gateway vengono quindi configurate come cluster per assicurare la protezione del failover nel caso si verifichino interruzioni della rete o errori hardware.  
  
Ad esempio, se l'organizzazione è un'azienda con una distribuzione cloud privata, si potrebbe essere necessario solo due macchine virtuali Gateway RAS, ognuno dei quali è installato in un altro computer che esegue Hyper-V. In questo scenario, le macchine virtuali Gateway RAS vengono aggiunti a un cluster per garantire un'elevata disponibilità.  
  
In un altro esempio, se l'organizzazione è un Provider del servizio Cloud (CSP) con duecento tenant nel tuo Data Center, è possibile usare otto macchine virtuali Gateway RAS, con ogni coppia di macchine virtuali in Gateway RAS cluster che fornisce servizi di routing per cinquanta tenant. In questo scenario, due computer che eseguono Hyper-V ogni dispone di quattro macchine virtuali sono configurate come gateway RAS. Quindi si configurano i quattro cluster macchina virtuale Gateway RAS, ogni cluster che contiene una macchina virtuale da ogni computer che esegue Hyper-V.  
  
Quando si distribuisce Gateway RAS, i server host che eseguono Hyper-V e le macchine virtuali configurate come gateway devono eseguire Windows Server 2012 R2 o Windows Server 2016.  
  
## <a name="bkmk_features"></a>Funzionalità del Gateway RAS  
Gateway RAS include le funzionalità seguenti.  
  
-   **Site-to-site VPN**. Questa funzionalità di Gateway RAS consente di connettere due reti in posizioni fisiche diverse tramite Internet usando una connessione VPN site-to-site. Se si dispone di una sede centrale e più filiali, è possibile distribuire un Gateway RAS in ogni posizione del bordo e creazione di connessioni site-to-site per fornire il flusso del traffico tra i percorsi di rete. Per i CSP che ospitano molti tenant nel proprio Data Center, Gateway RAS offre una soluzione gateway multi-tenant che consente i tenant potranno accedere alle risorse e gestirle tramite connessioni VPN site-to-site da siti remoti e che consente di flusso del traffico di rete tra risorse virtuali nel tuo Data Center e la relativa rete fisica.  
  
-   **Point-to-site VPN**. Questa funzionalità di Gateway RAS consente i dipendenti dell'organizzazione o agli amministratori di connettersi alla rete dell'organizzazione da posizioni remote. Per distribuzioni a tenant singolo di Gateway RAS, i dipendenti remoti possono connettersi alla rete dell'organizzazione usando una connessione VPN. Questa connessione consente loro di usare risorse di rete interna, ad esempio siti web intranet e file server. Per le distribuzioni multi-tenant, gli amministratori di rete tenant possono usare connessioni VPN point-to-site per accedere alle risorse di rete virtuale nel Data Center CSP.  
  
-   **Routing dinamico con protocollo BGP (Border Gateway)** . BGP, essendo un protocollo di routing dinamico che apprende automaticamente le route tra i siti connessi da connessioni VPN da sito a sito, riduce la necessità di configurare manualmente le route sui router. Se l'organizzazione dispone di più siti connessi mediante router BGP abilitato, ad esempio Gateway RAS, BGP consente i router calcolare automaticamente e usare le route valide tra loro in caso di interruzione della rete o di errore. Per altre informazioni, vedere [RFC 4271](https://tools.ietf.org/html/rfc4271).  
  
-   **Network Address Translation (NAT)** . Il protocollo NAT (NAT) consente di condividere una connessione alla rete Internet pubblica attraverso una singola interfaccia con un singolo indirizzo IP pubblico. I computer nella rete privata usano indirizzi privati, non instradabile. NAT esegue il mapping di indirizzi privati all'indirizzo pubblico. Questa funzionalità di Gateway RAS consente ai dipendenti dell'organizzazione con distribuzioni a tenant singolo accedere alle risorse Internet da dietro il gateway. Per i CSP, questa funzionalità consente alle applicazioni in esecuzione nel tenant alle macchine virtuali di accedere a Internet. Ad esempio, un macchina virtuale configurata come server Web del tenant possa rivolgersi alle risorse di finanziari esterne per l'elaborazione di transazioni con carta di credito.  

  
## <a name="bkmk_deploy"></a>Scenari di distribuzione di Gateway RAS  
Di seguito sono gli scenari di distribuzione consigliato per il Gateway RAS.  
  
-   **Enterprise Edge - deployment Single-Tenant**. Con tenant singolo di distribuzione aziendale, è possibile connettersi uno fisico più altri percorsi fisici nella rete Internet tramite la connessione VPN site-to-site - delle funzionalità e protocollo BGP (Border Gateway) consente di usare il routing dinamico. È anche possibile fornire ai dipendenti remoti l'accesso alla rete dell'organizzazione con le connessioni VPN da punto a sito e le connessioni DirectAccess. (Le connessioni DirectAccess sono sempre attive e forniscono anche il vantaggio che è possibile gestire facilmente i computer connessi tramite DirectAccess, in quanto sono connesse ogni volta che sono su e connesso a Internet.) È anche possibile configurare i gateway RAS Enterprise Single-tenant con NAT, in modo che i computer sulla rete Intranet possono comunicare facilmente con Internet.  
  
-   **Cloud Service Provider Edge - distribuzione multi-tenant**. Distribuzione di multi-tenant RAS Gateway per i CSP consente di offrire ai tenant tutte le funzionalità disponibili con la distribuzione di Enterprise Edge single-tenant. Connessioni VPN Site-to-site tra reti virtuali dei tenant nel proprio Data Center e i percorsi di rete tenant attraverso la media di Internet che i tenant hanno accesso trasparente alle risorse cloud continuamente. Accesso VPN da punto a sito per i tenant significa che gli amministratori tenant, è comunque possono connettersi alle reti virtuali nel tuo Data Center per gestire le risorse. BGP offre il routing dinamico e mantiene i tenant connessi per le proprie risorse anche quando si verificano problemi di rete in Internet o altrove. E NAT consente tenant alle macchine virtuali di connettersi alle risorse su Internet, ad esempio carta di credito, le risorse di elaborazione.  
  
## <a name="bkmk_manage"></a>Strumenti di Gestione Gateway RAS  
Di seguito sono gli strumenti di gestione per il Gateway RAS.  
  
-   In Windows Server 2016, per distribuire un router RAS Gateway, è necessario usare i comandi di Windows PowerShell. Per altre informazioni, vedere [cmdlet di accesso remoto](https://docs.microsoft.com/powershell/module/remoteaccess) per Windows Server 2016 e Windows 10.  
  
-   In System Center 2012 R2 Virtual Machine Manager (VMM), il Gateway RAS denominato Gateway Windows Server. Un set limitato di opzioni di configurazione del protocollo BGP (Border Gateway) sono disponibili nell'interfaccia del software VMM, inclusi **indirizzo IP BGP locale** e **numeri sistema autonomo (ASN)** ,  **Elenco di indirizzi IP Peer BGP**, e **i valori ASN**. È comunque possibile utilizzare i comandi BGP di Windows PowerShell per l'accesso remoto per configurare tutte le altre funzionalità di Windows Server Gateway. Per altre informazioni, vedere [Virtual Machine Manager (VMM)](https://technet.microsoft.com/system-center-docs/vmm/vmm) e [cmdlet di accesso remoto](https://technet.microsoft.com/library/hh918399.aspx) per Windows Server 2016 e Windows 10.  
  
## <a name="related-topics"></a>Argomenti correlati
- [Disponibilità elevata del Gateway RAS](../../../networking/sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)  
- [Tunneling GRE in Windows Server](gre-tunneling-windows-server.md)
- [Prestazioni e velocità effettiva del tunnel GRE di Gateway RAS](RAS-Gateway-GRE-Perf.md)
