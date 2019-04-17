---
title: Controller di rete
description: In questo argomento viene fornita una panoramica di Controller di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 31f3fa4e-cd25-4bf3-89e9-a01a6cec7893
ms.author: pashort
author: shortpatti
ms.openlocfilehash: acb9abbd716e9930fb01431e7004abb72a7da10c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="network-controller"></a>Controller di rete

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

In Windows Server 2016, nuovi Controller di rete fornisce un punto programmabile e centralizzato di automazione per gestire, configurare, monitorare e risolvere i problemi dell'infrastruttura di rete fisica e virtuale nel centro dati. 

Controller di rete è possibile automatizzare la configurazione dell'infrastruttura di rete invece di eseguire la configurazione manuale dei dispositivi di rete e dei servizi.

> [!NOTE]
> Oltre a questo argomento, è disponibile la documentazione seguente Controller di rete.
> - [Disponibilità elevata del Controller di rete](network-controller-high-availability.md)
> - [Installazione e i requisiti di preparazione per la distribuzione di Controller di rete](../../plan/Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md)  
> - [Distribuire Controller di rete tramite Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)  
> - [Installare il ruolo server di Controller di rete con Server Manager](Install-the-Network-Controller-server-role-using-Server-Manager.md)
> - [Passaggi di post-distribuzione per i Controller di rete](post-deploy-steps-nc.md)
> - [Cmdlet di Controller di rete](https://technet.microsoft.com/library/mt576401.aspx) 

## <a name="bkmk_overview"></a>Panoramica del Controller di rete

Controller di rete è un ruolo del server a disponibilità e scalabilità elevate e offre una programmazione delle applicazioni interfaccia \(API\) che permette al Controller di rete comunicare con la rete e una seconda API che consente di comunicare con il Controller di rete.

È possibile distribuire Controller di rete nel dominio sia gli ambienti non di dominio. Negli ambienti di dominio, Controller di rete esegue l'autenticazione di utenti e dispositivi di rete tramite Kerberos; in ambienti non di dominio, è necessario distribuire i certificati di autenticazione.

>[!IMPORTANT]
>Non distribuire il ruolo server di Controller di rete in host fisici. Per distribuire Controller di rete, è necessario installare il ruolo server di Controller di rete in una macchina virtuale Hyper-V \(VM\) che viene installato in un host Hyper-V. Dopo aver installato il Controller di rete nelle macchine virtuali in tre host Hyper\ V diversi, è necessario attivare gli host Hyper\-V per la rete definita dal Software \(SDN\) mediante l'aggiunta di host al Controller di rete utilizzando il comando di Windows PowerShell **New-NetworkControllerServer**. In questo modo, si desidera abilitare il bilanciamento del carico Software SDN alla funzione. Per ulteriori informazioni, vedere [New-NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver).

Controller di rete comunica con i dispositivi di rete, servizi e componenti mediante l'API Southbound. Con l'API Southbound, Controller di rete può individuare i dispositivi di rete, rilevare configurazioni dei servizi e raccogliere tutte le informazioni che necessarie sulla rete. Inoltre, l'API Southbound offre Controller di rete un percorso per l'invio di informazioni per l'infrastruttura di rete, ad esempio modifiche alla configurazione apportate.

L'API Northbound del Controller di rete offre la possibilità di raccogliere informazioni sulla rete dal Controller di rete e utilizzarlo per monitorare e configurare la rete.

L'API Northbound del Controller di rete consente di configurare, monitorare, risolvere i problemi e distribuire nuovi dispositivi di rete utilizzando Windows PowerShell, l'API Representational State Transfer \(REST\) o un'applicazione di gestione con un'interfaccia utente grafica, ad esempio System Center Virtual Machine Manager.

>[!NOTE]
>L'API Northbound del Controller di rete viene implementata come interfaccia REST.

È possibile gestire la rete di Data Center con Controller di rete tramite applicazioni di gestione, ad esempio \(SCVMM\) System Center Virtual Machine Manager e System Center Operations Manager \(SCOM\), poiché questo Controller di rete consente di configurare, monitorare, programmare e risolvere i problemi di infrastruttura di rete che si trova sotto il controllo.

Usa Windows PowerShell, l'API REST o un'applicazione di gestione, è possibile utilizzare Controller di rete per gestire l'infrastruttura di rete fisica e virtuale seguente:

- Le macchine virtuali Hyper-V e i commutatori virtuali

- Firewall del centro dati

- Gateway di multi-tenant \(RAS\) del servizio di accesso remoto, gateway virtuale e pool di gateway

- Servizi di bilanciamento del carico software

Nella figura seguente, un amministratore utilizza uno strumento di gestione che interagisce direttamente con i Controller di rete. Controller di rete fornisce informazioni sull'infrastruttura di rete, tra cui un'infrastruttura fisica e virtuale, che lo strumento di gestione e apporta modifiche alla configurazione in base alle azioni dell'amministratore quando si utilizza lo strumento.  

![Panoramica del Controller di rete](../../../media/Network-Controller/NetController_overview.png)  

Se si distribuiscono Controller di rete in un ambiente di testing, è possibile eseguire il ruolo server di Controller di rete in una macchina virtuale Hyper-V \(VM\) che viene installato in un host Hyper-V.

Per la disponibilità elevata in Data Center di dimensioni maggiori, è possibile distribuire un cluster usando tre VM installate in tre o più host Hyper-V. Per ulteriori informazioni, vedere [la disponibilità elevata del Controller di rete](network-controller-high-availability.md).

## <a name="bkmk_features"></a>Funzionalità di Controller di rete

Le seguenti funzionalità di Controller di rete consente di configurare e gestire virtuali e i dispositivi di rete fisica e servizi.  
  
-   [Gestione firewall](#bkmk_firewall)  
  
-   [Gestione del bilanciamento del carico di software](#bkmk_slb)  
  
-   [Gestione di rete virtuale](#bkmk_virtual)  
  
-   [Gestione Gateway RAS](#bkmk_gateway)

>[!IMPORTANT]
>Backup di Controller di rete e il ripristino non è attualmente disponibile in Windows Server 2016.
  
### <a name="bkmk_firewall"></a>Gestione firewall

Questa funzionalità del Controller di rete consente di configurare e gestire consentire o negare le regole di controllo di accesso di firewall per il carico di lavoro macchine virtuali per EST/ovest e il traffico di rete nord/sud nel Data Center. Le regole del firewall sono sottoposte a plumbing nella porta vSwitch del carico di lavoro macchine virtuali e vengono quindi distribuite tra il carico di lavoro nel Data Center. Usa l'API Northbound, è possibile definire le regole del firewall per il traffico in ingresso e in uscita dal carico di lavoro di macchina virtuale. È inoltre possibile configurare ogni regola del firewall per registrare il traffico consentito o negato dalla regola.  

Per ulteriori informazioni, vedere [Cenni preliminari sul Firewall Datacenter](../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md).

### <a name="bkmk_slb"></a>Gestione del bilanciamento del carico di software

Questa funzionalità del Controller di rete consente di abilitare più server ospitare stesso carico di lavoro, offrendo disponibilità e scalabilità elevate.  
  
Per ulteriori informazioni, vedere [il bilanciamento del carico Software & #40; SLB & #41; per SDN](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md).  
  
### <a name="bkmk_virtual"></a>Gestione di rete virtuale

Questa funzionalità del Controller di rete consente di distribuire e configurare virtualizzazione rete Hyper-V, inclusi il commutatore virtuale Hyper-V e schede di rete virtuali nelle singole macchine virtuali e di archiviare e distribuire i criteri di rete virtuale.

Controller di rete supporta Network Virtualization Generic Routing Encapsulation (NVGRE) e Virtual Extensible Local Area Network (VXLAN).

### <a name="bkmk_gateway"></a>Gestione Gateway RAS

Questa funzionalità del Controller di rete consente di distribuire, configurare e gestire macchine virtuali (VM) che sono membri di un pool di Gateway RAS, fornire servizi gateway ai tenant. Controller di rete consente di distribuire automaticamente le macchine virtuali con le seguenti funzionalità di gateway RAS Gateway:

> [!NOTE]
> In System Center Virtual Machine Manager, Gateway RAS denominato Gateway Windows Server.

- Aggiunta e rimozione di VM gateway dal cluster e specificare il livello di backup necessario.

- Connettività gateway di rete privata virtuale (VPN) Site-to-site tra le reti tenant remote e il Data Center mediante IPsec.

- Site-to-site VPN gateway connettività tra le reti tenant remote e il Data Center mediante Generic Routing Encapsulation (GRE).

- Livello 3 capacità di inoltro.

- Protocollo BGP (Border Gateway) routing, che consente di gestire il routing del traffico di rete tra reti VM dei tenant e i rispettivi siti remoti.

Controller di rete può inserire diverse connessioni di un tenant su gateway separati. È possibile utilizzare un singolo indirizzo IP pubblico per tutte le connessioni a gateway o avere diversi IP pubblico per un sottoinsieme delle connessioni. Controller di rete registra tutti configurazione del gateway e le modifiche di stato, che possono essere utilizzate per il controllo e la risoluzione dei problemi.

Per ulteriori informazioni su BGP, vedere [Border Gateway Protocol & #40; BGP & #41;](../../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md).

Per ulteriori informazioni sul Gateway RAS, vedere [Gateway RAS per SDN](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md).

## <a name="network-controller-deployment-options"></a>Opzioni di distribuzione di Controller di rete

Per distribuire Controller di rete utilizzando \(VMM\) System Center Virtual Machine Manager, vedere [configurare un Controller di rete SDN nell'infrastruttura VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller).

Per distribuire Controller di rete tramite script, vedere [distribuire un Software definito dell'infrastruttura utilizzando script di rete](../../deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md).

Per distribuire Controller di rete tramite Windows PowerShell, vedere [distribuire Controller di rete tramite Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)
