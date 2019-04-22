---
title: Accesso remoto
description: In questo argomento viene fornita una panoramica del ruolo del server Accesso remoto in Windows Server 2016.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: pashort
author: shortpatti
ms.date: 05/18/2018
ms.openlocfilehash: faf12ad22678fa58ea933613759e3e8414966bca
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818362"
---
# <a name="remote-access"></a>Accesso remoto

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

La Guida di accesso remoto offre una panoramica del ruolo del server Accesso remoto in Windows Server 2016 e vengono illustrati gli argomenti seguenti:

- [Sempre nella Guida alla distribuzione VPN](vpn/always-on-vpn/deploy/always-on-vpn-deploy.md)
- [Border Gateway Protocol &#40;BGP&#41;](bgp/Border-Gateway-Protocol-BGP.md)
- [Gateway RAS](ras-gateway/RAS-Gateway.md) 
- [Documentazione relativa al ruolo Server accesso remoto](ras/Remote-Access-Server-Role-Documentation.md)
- [Gateway RAS per SDN](../../networking/sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)
- [Rete privata virtuale (VPN)](vpn/vpn-top.md)
 
Per altre informazioni su altre tecnologie di rete, vedere [funzionalità di rete in Windows Server 2016](https://docs.microsoft.com/windows-server/networking/networking).

Il ruolo server Accesso remoto è un raggruppamento logico di queste tecnologie di accesso di rete correlati: [Il servizio di accesso remoto (RAS)](#bkmk_da), [Routing](#bkmk_rras), e [Proxy applicazione Web](#bkmk_proxy). Queste tecnologie sono i *servizi ruolo* del ruolo del server Accesso remoto. Quando si installa il ruolo server Accesso remoto con la **Aggiunta guidata ruoli e funzionalità** oppure Windows PowerShell, è possibile installare uno o più di questi servizi tre ruolo.

>[!IMPORTANT]
>Non tentare di distribuire accesso remoto in una macchina virtuale \(VM\) in Microsoft Azure. Uso di accesso remoto in Microsoft Azure non è supportato. È possibile utilizzare l'accesso remoto in una VM di Azure per distribuire VPN, DirectAccess o qualsiasi altra funzionalità di accesso remoto in Windows Server 2016 o versioni precedenti di Windows Server. Per altre informazioni, vedere [supporto di software server Microsoft per le macchine virtuali di Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).

## <a name="bkmk_da"></a>Servizio di accesso remoto \(RAS\) -Gateway RAS

Quando si installa il **DirectAccess e VPN (RAS)** servizio ruolo, si sta distribuendo il Gateway di servizi di accesso remoto \( **Gateway RAS**\). È possibile distribuire il Gateway RAS una rete privata virtuale single-tenant RAS Gateway \(VPN\) , un server multi-tenant RAS Gateway VPN e come server DirectAccess.

- **Gateway RAS - Single-Tenant**. Tramite il Gateway RAS, è possibile distribuire le connessioni VPN per fornire agli utenti finali con accesso remoto alla rete e le risorse dell'organizzazione. Se i client sono in esecuzione Windows 10, è possibile distribuire VPN Always On, che mantiene una connessione permanente tra i client e rete dell'organizzazione ogni volta che i computer remoti connessi a Internet. Con il Gateway RAS, è anche possibile creare una connessione VPN site-to-site tra due server in posizioni diverse, ad esempio tra la sede principale e una succursale e Usa Network Address Translation \(NAT\) in modo che gli utenti all'interno di rete possa accedere alle risorse esterne, ad esempio Internet. Inoltre, Gateway RAS supporta protocollo BGP (Border Gateway), che fornisce servizi di routing dinamici quando le sedi remote dispone anche di gateway edge che supportano il protocollo BGP.

- **Gateway RAS - Multitenant**. È possibile distribuire RAS Gateway come un router e gateway edge basata su software, multi-tenant quando si usa Hyper\-V rete virtualizzazione o non si hanno reti VM distribuite con le reti locali virtuali \(VLAN\). Con il Gateway RAS, provider di servizi Cloud \(CSP\) e le aziende possono abilitare Data Center e cloud il routing del traffico di rete tra reti virtuali e fisiche, Internet incluso. Con il Gateway RAS, i tenant possono usare connessioni VPN da punto sito ad per accedere alle risorse di rete della macchina virtuale nel Data Center da qualsiasi posizione. È anche possibile fornire tenant con connessioni VPN site-to-site tra i rispettivi siti remoti e il tuo Data Center CSP. Inoltre, è possibile configurare il Gateway RAS con il protocollo BGP per il routing dinamico ed è possibile abilitare Network Address Translation \(NAT\) per fornire l'accesso a Internet per le macchine virtuali nelle reti VM.

>[!IMPORTANT]
> Il Gateway RAS con funzionalità multi-tenant è anche disponibile in Windows Server 2012 R2.

- **VPN Always On**. VPN Always On consente agli utenti remoti di accedere in modo sicuro le risorse condivise, siti Web intranet e le applicazioni in una rete interna senza connettersi a una rete VPN. 

Per altre informazioni, vedere [Gateway RAS](ras-gateway/RAS-Gateway.md) e [protocollo BGP (Border Gateway)](bgp/Border-Gateway-Protocol-BGP.md).

## <a name="bkmk_rras"></a>Routing

È possibile usare l'accesso remoto per instradare il traffico di rete tra subnet nella rete locale. Routing offre supporto per i router Network Address Translation (NAT), i router LAN con BGP, Routing Information Protocol (RIP) e router multicast con gruppo di gestione IGMP (Internet Protocol). Come un router completa, è possibile distribuire server di accesso remoto in un computer server o come macchina virtuale (VM) in un computer che esegue Hyper-V.

Per installare accesso remoto come router LAN, utilizzare l'aggiunta guidata ruoli e funzionalità in Server Manager e selezionare il **accesso remoto** ruolo del server e il **Routing** servizio ruolo; o digitare quanto segue comando al prompt di Windows PowerShell e quindi premere INVIO.

```  
Install-RemoteAccess -VpnType RoutingOnly
```  

## <a name="bkmk_proxy"></a>Proxy applicazione Web

Proxy applicazione Web è un servizio ruolo Accesso remoto in Windows Server 2016. Proxy applicazione Web rende disponibili funzionalità di proxy inverso per le applicazioni Web all'interno della rete aziendale, in modo da consentire agli utenti con qualsiasi dispositivo di accedere a tali applicazioni dall'esterno della rete aziendale. Proxy applicazione Web Preautentica l'accesso alle applicazioni web usando Active Directory Federation Services (ADFS) e funge anche da un proxy AD FS.

Per installare accesso remoto come un Proxy applicazione Web, usare l'aggiunta guidata ruoli e funzionalità in Server Manager e selezionare il **accesso remoto** ruolo del server e il **Proxy dell'applicazione Web** servizio ruolo; o digitare il comando seguente al prompt di Windows PowerShell e quindi premere INVIO.  

```  
Install-RemoteAccess -VpnType SstpProxy  
```  

Per altre informazioni, vedere [Proxy applicazione Web](https://technet.microsoft.com/windows-server-docs/identity/web-application-proxy/web-application-proxy-windows-server).


---