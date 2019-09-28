---
title: Controller di rete
description: Questo argomento fornisce una panoramica del controller di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 31f3fa4e-cd25-4bf3-89e9-a01a6cec7893
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 13f535b9a91f26b30600b637b46817cfa33ccd7b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355645"
---
# <a name="network-controller"></a>Controller di rete

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Una novità di Windows Server 2016, il controller di rete offre un punto di automazione programmabile e centralizzato per gestire, configurare, monitorare e risolvere i problemi dell'infrastruttura di rete fisica e virtuale nel Data Center. 

Il Controller di rete permette di automatizzare la configurazione dell'infrastruttura di rete, invece di eseguire la configurazione manuale dei dispositivi e dei servizi di rete.

> [!NOTE]
> Oltre a questo argomento, è disponibile la seguente documentazione del controller di rete.
> - [Disponibilità elevata del controller di rete](network-controller-high-availability.md)
> - [Requisiti di installazione e preparazione per la distribuzione del controller di rete](../../plan/Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md)  
> - [Distribuire controller di rete tramite Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)  
> - [Installare il ruolo del server del controller di rete con Server Manager](Install-the-Network-Controller-server-role-using-Server-Manager.md)
> - [Passaggi post-distribuzione per il controller di rete](post-deploy-steps-nc.md)
> - [Cmdlet del controller di rete](https://technet.microsoft.com/library/mt576401.aspx) 

## <a name="bkmk_overview"></a>Panoramica sul controller di rete

Il controller di rete è un ruolo del server a disponibilità elevata e scalabile e fornisce un Application Programming Interface \(API @ no__t-1 che consente al controller di rete di comunicare con la rete e una seconda API che consente di comunicare con Controller di rete.

È possibile distribuire il controller di rete in ambienti di dominio e non di dominio. Negli ambienti di dominio, il controller di rete autentica gli utenti e i dispositivi di rete mediante Kerberos. negli ambienti non di dominio, è necessario distribuire i certificati per l'autenticazione.

>[!IMPORTANT]
>Non distribuire il ruolo del server del controller di rete negli host fisici. Per distribuire il controller di rete, è necessario installare il ruolo del server del controller di rete in una macchina virtuale Hyper-V \(VM @ no__t-1 installato in un host Hyper-V. Dopo aver installato il controller di rete nelle macchine virtuali in tre host Hyper @ no__t-0V diversi, è necessario abilitare gli host Hyper @ no__t-1V per Software Defined Networking \(SDN @ no__t-3 aggiungendo gli host al controller di rete usando Windows PowerShell comando **New-NetworkControllerServer**. In questo modo si Abilita il funzionamento del software SDN Load Balancer. Per ulteriori informazioni, vedere [New-NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver).

Il Controller di rete comunica con dispositivi, servizi e componenti di rete mediante l'API Southbound. L'API Southbound permette al Controller di rete di individuare dispositivi di rete, rilevare configurazioni dei servizi e raccogliere tutte le informazioni necessarie sulla rete. L'API Southbound offre anche al Controller di rete un percorso per l'invio di informazioni all'infrastruttura di rete, ad esempio modifiche apportate alla configurazione.

L'API Northbound del Controller di rete permette di raccogliere informazioni sulla rete dal Controller di rete e usarle per monitorare e configurare la rete.

Il controller di rete API in Nord consente di configurare, monitorare, risolvere i problemi e distribuire nuovi dispositivi in rete usando Windows PowerShell, l'API Representational State Transfer \(REST @ no__t-1 o un'applicazione di gestione con un'interfaccia grafica interfaccia utente, ad esempio System Center Virtual Machine Manager.

>[!NOTE]
>L'API Northbound del Controller di rete viene implementata come interfaccia REST.

È possibile gestire la rete del data center con il controller di rete usando le applicazioni di gestione, ad esempio System Center Virtual Machine Manager \(SCVMM @ no__t-1 e System Center Operations Manager \(SCOM @ no__t-3, perché il controller di rete consente di configurare, monitorare, programmare e risolvere i problemi relativi all'infrastruttura di rete controllata.

Se si usa Windows PowerShell, l'API REST o un'applicazione di gestione, sarà possibile usare il Controller di rete per gestire l'infrastruttura di rete fisica e virtuale seguente:

- Macchine virtuali e commutatori virtuali Hyper-V

- Firewall del data center

- Servizio di accesso remoto \(RAS @ no__t-1 gateway multi-tenant, gateway virtuali e pool di gateway

- Bilanciamento del carico software

La figura seguente mostra un amministratore che usa uno strumento di gestione che interagisce direttamente con il Controller di rete. Il controller di rete fornisce informazioni sull'infrastruttura di rete, tra cui l'infrastruttura fisica e virtuale, allo strumento di gestione e apporta modifiche di configurazione in base alle azioni dell'amministratore quando si utilizza lo strumento.  

![Panoramica sul controller di rete](../../../media/Network-Controller/NetController_overview.png)  

Se si distribuisce il controller di rete in un ambiente di testing, è possibile eseguire il ruolo del server del controller di rete in una macchina virtuale Hyper-V \(VM @ no__t-1 installato in un host Hyper-V.

Per la disponibilità elevata in data center di grandi dimensioni, è possibile distribuire un cluster con tre VM installate in tre o più host Hyper-V. Per altre informazioni, vedere [disponibilità elevata del controller di rete](network-controller-high-availability.md).

## <a name="bkmk_features"></a>Funzionalità del controller di rete

Le funzionalità seguenti del Controller di rete permettono di configurare e gestire dispositivi e servizi di rete virtuale e fisica.  
  
-   [Gestione del firewall](#bkmk_firewall)  
  
-   [Gestione Load Balancer software](#bkmk_slb)  
  
-   [Gestione della rete virtuale](#bkmk_virtual)  
  
-   [Gestione gateway RAS](#bkmk_gateway)

>[!IMPORTANT]
>Il backup e il ripristino del controller di rete non sono attualmente disponibili in Windows Server 2016.
  
### <a name="bkmk_firewall"></a>Gestione del firewall

Questa funzionalità del Controller di rete permette di configurare e gestire le regole di Controllo di accesso del firewall di tipo consenti/nega per le macchine virtuali del carico di lavoro per traffico di rete Est/Ovest e Nord/Sud nel data center. Le regole del firewall sono sottoposte a plumbing nella porta vSwitch delle macchine virtuali del carico di lavoro e vengono quindi distribuite nell'intero carico di lavoro nel data center. Se si usa l'API Northbound, sarà possibile definire le regole del firewall per il traffico in entrata e in uscita dalla VM del carico di lavoro. È anche possibile configurare ogni regola del firewall in modo da registrare il traffico consentito o negato dalla regola.  

Per ulteriori informazioni, vedere [Cenni preliminari sul Firewall Datacenter](../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md).

### <a name="bkmk_slb"></a>Gestione Load Balancer software

Questa funzionalità del Controller di rete permette di abilitare più server per l'hosting dello stesso carico di lavoro, offrendo disponibilità e scalabilità elevate.  
  
Per ulteriori informazioni, vedere [il bilanciamento del carico Software & #40; SLB & #41; per SDN](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md).  
  
### <a name="bkmk_virtual"></a>Gestione della rete virtuale

Questa funzionalità del Controller di rete permette di distribuire e configurare Virtualizzazione rete Hyper-V, inclusi il commutatore virtuale Hyper-V e le schede di rete virtuali nelle singole macchine virtuali, e di archiviare e distribuire i criteri di rete virtuale.

Il Controller di rete supporta Network Virtualization Generic Routing Encapsulation (NVGRE) e Virtual Extensible Local Area Network (VXLAN).

### <a name="bkmk_gateway"></a>Gestione gateway RAS

Questa funzionalità del controller di rete consente di distribuire, configurare e gestire macchine virtuali (VM) che sono membri di un pool di gateway RAS, offrendo servizi gateway ai tenant. Il controller di rete consente di distribuire automaticamente le macchine virtuali che eseguono il gateway RAS con le funzionalità del gateway seguenti:

> [!NOTE]
> In System Center Virtual Machine Manager il gateway RAS è denominato gateway Windows Server.

- Aggiunta e rimozione di VM gateway dal cluster e definizione del livello di backup necessario.

- Connettività gateway di rete privata virtuale (VPN, Virtual Private Network) da sito a sito tra le reti tenant remote e il data center mediante IPsec.

- Connettività gateway VPN da sito a sito tra le reti tenant remote e il data center mediante GRE (Generic Routing Encapsulation).

- Capacità di inoltro di livello 3.

- Border Gateway Protocol (BGP) routing, che consente di gestire il routing del traffico di rete tra le reti VM dei tenant e i rispettivi siti remoti.

Il controller di rete può inserire connessioni diverse di un tenant in gateway distinti. È possibile usare un singolo indirizzo IP pubblico per tutte le connessioni del gateway o avere indirizzi IP pubblici diversi per un subset delle connessioni. Il controller di rete registra tutte le modifiche di stato e configurazione del gateway, che possono essere usate a scopo di controllo e risoluzione dei problemi.

Per altre informazioni su BGP, vedere [Border Gateway Protocol &#40;BGP&#41;](../../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md).

Per altre informazioni sul gateway RAS, vedere [gateway RAS per Sdn](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md).

## <a name="network-controller-deployment-options"></a>Opzioni di distribuzione del controller di rete

Per distribuire il controller di rete usando System Center Virtual Machine Manager \(VMM @ no__t-1, vedere [configurare un controller di rete Sdn nell'infrastruttura VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller).

Per distribuire il controller di rete usando gli script, vedere [distribuire un'infrastruttura software defined Network usando gli script](../../deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md).

Per distribuire il controller di rete con Windows PowerShell, vedere [distribuire un controller di rete con Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)
