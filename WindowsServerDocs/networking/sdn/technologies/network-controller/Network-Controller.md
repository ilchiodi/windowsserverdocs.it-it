---
title: Controller di rete
description: In questo argomento viene fornita una panoramica del Controller di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 31f3fa4e-cd25-4bf3-89e9-a01a6cec7893
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7ace628c6ae9802c0c65d360aedfac8c80ac5537
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875682"
---
# <a name="network-controller"></a>Controller di rete

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Nuove in Windows Server 2016, Controller di rete fornisce un punto programmabile e centralizzato di automazione per gestire, configurare, monitorare e risolvere i problemi dell'infrastruttura di rete fisica e virtuale nel proprio Data Center. 

Il Controller di rete permette di automatizzare la configurazione dell'infrastruttura di rete, invece di eseguire la configurazione manuale dei dispositivi e dei servizi di rete.

> [!NOTE]
> Oltre a questo argomento, la documentazione di Controller di rete seguente è disponibile.
> - [Disponibilità elevata di Controller di rete](network-controller-high-availability.md)
> - [Requisiti di installazione e preparazione per la distribuzione di Controller di rete](../../plan/Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md)  
> - [Distribuire Controller di rete tramite Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)  
> - [Installare il ruolo del server Controller di rete con Server Manager](Install-the-Network-Controller-server-role-using-Server-Manager.md)
> - [Passaggi di post-distribuzione per il Controller di rete](post-deploy-steps-nc.md)
> - [Cmdlet di Controller di rete](https://technet.microsoft.com/library/mt576401.aspx) 

## <a name="bkmk_overview"></a>Cenni preliminari sul Controller di rete

Controller di rete è un ruolo di server scalabile e a disponibilità elevata e fornisce un'interfaccia di programmazione dell'applicazione \(API\) che consente di Controller di rete per comunicare con la rete e una seconda API che consente di comunicare con il Controller di rete.

È possibile distribuire Controller di rete nel dominio sia negli ambienti non di dominio. Negli ambienti di dominio, Controller di rete esegue l'autenticazione di utenti e dispositivi di rete tramite Kerberos; in ambienti non di dominio, è necessario distribuire i certificati per l'autenticazione.

>[!IMPORTANT]
>Non distribuire il ruolo del server Controller di rete in host fisici. Per distribuire Controller di rete, è necessario installare il ruolo del server Controller di rete in una macchina virtuale Hyper-V \(VM\) che viene installato in un host Hyper-V. Dopo aver installato il Controller di rete nelle macchine virtuali in tre diverse Hyper\-host V, è necessario abilitare il Hyper\-host V per Software Defined Networking \(SDN\) mediante l'aggiunta di host al Controller di rete usando il comando di Windows PowerShell **New-NetworkControllerServer**. In questo modo, si sta abilitando il bilanciamento del carico Software SDN alla funzione. Per altre informazioni, vedere [New-NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver).

Il Controller di rete comunica con dispositivi, servizi e componenti di rete mediante l'API Southbound. L'API Southbound permette al Controller di rete di individuare dispositivi di rete, rilevare configurazioni dei servizi e raccogliere tutte le informazioni necessarie sulla rete. L'API Southbound offre anche al Controller di rete un percorso per l'invio di informazioni all'infrastruttura di rete, ad esempio modifiche apportate alla configurazione.

L'API Northbound del Controller di rete permette di raccogliere informazioni sulla rete dal Controller di rete e usarle per monitorare e configurare la rete.

L'API Northbound del Controller di rete consente di configurare, monitorare, risolvere i problemi e distribuire i nuovi dispositivi di rete tramite Windows PowerShell, Representational State Transfer \(REST\) API o un'applicazione di gestione con un'interfaccia utente grafica, ad esempio System Center Virtual Machine Manager.

>[!NOTE]
>L'API Northbound del Controller di rete viene implementata come interfaccia REST.

È possibile gestire la rete di Data Center con Controller di rete usando applicazioni di gestione, ad esempio System Center Virtual Machine Manager \(SCVMM\)e System Center Operations Manager \(SCOM\), Poiché questo Controller di rete consente di configurare, monitorare, programmare e risolvere i problemi di infrastruttura di rete che si trova sotto il controllo.

Se si usa Windows PowerShell, l'API REST o un'applicazione di gestione, sarà possibile usare il Controller di rete per gestire l'infrastruttura di rete fisica e virtuale seguente:

- Macchine virtuali e commutatori virtuali Hyper-V

- Firewall del data center

- Servizio di accesso remoto \(RAS\) gateway multi-tenant, i gateway virtuali e i pool di gateway

- Servizi di bilanciamento del carico software

La figura seguente mostra un amministratore che usa uno strumento di gestione che interagisce direttamente con il Controller di rete. Controller di rete fornisce informazioni sull'infrastruttura di rete, tra cui un'infrastruttura virtuale e fisica, che lo strumento di gestione e apporta modifiche alla configurazione in base alle azioni dell'amministratore quando si usa lo strumento.  

![Panoramica del Controller di rete](../../../media/Network-Controller/NetController_overview.png)  

Se si distribuiscono Controller di rete in un ambiente di testing, è possibile eseguire il ruolo del server Controller di rete in una macchina virtuale Hyper-V \(VM\) che viene installato in un host Hyper-V.

Per la disponibilità elevata in Data Center più grande, è possibile distribuire un cluster usando tre VM installate in tre o più host Hyper-V. Per altre informazioni, vedere [disponibilità elevata di Controller di rete](network-controller-high-availability.md).

## <a name="bkmk_features"></a>Funzionalità di Controller di rete

Le funzionalità seguenti del Controller di rete permettono di configurare e gestire dispositivi e servizi di rete virtuale e fisica.  
  
-   [Gestione del firewall](#bkmk_firewall)  
  
-   [Gestione del bilanciamento del carico software](#bkmk_slb)  
  
-   [Gestione della rete virtuale](#bkmk_virtual)  
  
-   [Gestione del Gateway RAS](#bkmk_gateway)

>[!IMPORTANT]
>Backup del Controller di rete e il ripristino non è attualmente disponibile in Windows Server 2016.
  
### <a name="bkmk_firewall"></a>Gestione del firewall

Questa funzionalità del Controller di rete permette di configurare e gestire le regole di Controllo di accesso del firewall di tipo consenti/nega per le macchine virtuali del carico di lavoro per traffico di rete Est/Ovest e Nord/Sud nel data center. Le regole del firewall sono sottoposte a plumbing nella porta vSwitch delle macchine virtuali del carico di lavoro e vengono quindi distribuite nell'intero carico di lavoro nel data center. Se si usa l'API Northbound, sarà possibile definire le regole del firewall per il traffico in entrata e in uscita dalla VM del carico di lavoro. È anche possibile configurare ogni regola del firewall in modo da registrare il traffico consentito o negato dalla regola.  

Per ulteriori informazioni, vedere [Cenni preliminari sul Firewall Datacenter](../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md).

### <a name="bkmk_slb"></a>Gestione del bilanciamento del carico software

Questa funzionalità del Controller di rete permette di abilitare più server per l'hosting dello stesso carico di lavoro, offrendo disponibilità e scalabilità elevate.  
  
Per ulteriori informazioni, vedere [il bilanciamento del carico Software & #40; SLB & #41; per SDN](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md).  
  
### <a name="bkmk_virtual"></a>Gestione della rete virtuale

Questa funzionalità del Controller di rete permette di distribuire e configurare Virtualizzazione rete Hyper-V, inclusi il commutatore virtuale Hyper-V e le schede di rete virtuali nelle singole macchine virtuali, e di archiviare e distribuire i criteri di rete virtuale.

Il Controller di rete supporta Network Virtualization Generic Routing Encapsulation (NVGRE) e Virtual Extensible Local Area Network (VXLAN).

### <a name="bkmk_gateway"></a>Gestione del Gateway RAS

Questa funzionalità del Controller di rete consente di distribuire, configurare e gestire macchine virtuali (VM) che sono membri di un pool di Gateway RAS, che fornisce servizi gateway ai tenant. Controller di rete consente di distribuire automaticamente le VM che eseguono Gateway RAS con le funzionalità di gateway seguenti:

> [!NOTE]
> In System Center Virtual Machine Manager, Gateway RAS denominato Gateway Windows Server.

- Aggiunta e rimozione di VM gateway dal cluster e definizione del livello di backup necessario.

- Connettività gateway di rete privata virtuale (VPN, Virtual Private Network) da sito a sito tra le reti tenant remote e il data center mediante IPsec.

- Connettività gateway VPN da sito a sito tra le reti tenant remote e il data center mediante GRE (Generic Routing Encapsulation).

- Capacità di inoltro di livello 3.

- Protocollo BGP (Border Gateway) routing, che consente di gestire il routing del traffico di rete tra reti VM dei tenant e i rispettivi siti remoti.

Controller di rete è possibile posizionare diverse connessioni di un tenant su gateway separati. È possibile usare un singolo indirizzo IP pubblico per tutte le connessioni di gateway o impostare diversi indirizzi IP pubblici per un subset delle connessioni. Controller di rete registra tutte le configurazione del gateway e le modifiche di stato, che possono essere usate per il controllo e la risoluzione dei problemi.

Per altre informazioni su BGP, vedere [Border Gateway Protocol &#40;BGP&#41;](../../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md).

Per altre informazioni sul Gateway RAS, vedere [Gateway RAS per SDN](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md).

## <a name="network-controller-deployment-options"></a>Opzioni di distribuzione di Controller di rete

Distribuire Controller di rete con System Center Virtual Machine Manager \(VMM\), vedere [configurare un Controller di rete SDN nell'infrastruttura di VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller).

Per distribuire Controller di rete tramite script, vedere [distribuire un Software di rete dell'infrastruttura usando gli script definiti](../../deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md).

Per distribuire Controller di rete tramite Windows PowerShell, vedere [distribuire Controller di rete tramite Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)
