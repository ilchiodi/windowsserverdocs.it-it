---
title: Accesso remoto
description: Questo argomento fornisce una panoramica del ruolo del server accesso remoto in Windows Server 2016.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: pashort
author: shortpatti
ms.date: 05/18/2018
ms.openlocfilehash: d2fa9c82c4cab05b2a60916fee3f09c1ea48a472
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388915"
---
# <a name="remote-access"></a>Accesso remoto

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

La Guida di accesso remoto offre una panoramica del ruolo del server accesso remoto in Windows Server 2016 e comprende gli argomenti seguenti:

- [Guida alla distribuzione di Always On VPN](vpn/always-on-vpn/deploy/always-on-vpn-deploy.md)
- [Border Gateway Protocol &#40;BGP&#41;](bgp/Border-Gateway-Protocol-BGP.md)
- [Gateway RAS](ras-gateway/RAS-Gateway.md) 
- [Documentazione sul ruolo del server di accesso remoto](ras/Remote-Access-Server-Role-Documentation.md)
- [Gateway RAS per SDN](../../networking/sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)
- [Rete privata virtuale (VPN)](vpn/vpn-top.md)
 
Per ulteriori informazioni sulle altre tecnologie di rete, vedere la pagina relativa [alla rete in Windows Server 2016](https://docs.microsoft.com/windows-server/networking/networking).

Il ruolo del server accesso remoto è un raggruppamento logico di queste tecnologie di accesso alla rete, ovvero il [servizio di accesso remoto (RAS)](#bkmk_da), il [routing](#bkmk_rras)e il [proxy dell'applicazione Web](#bkmk_proxy). Queste tecnologie sono i *servizi ruolo* del ruolo del server Accesso remoto. Quando si installa il ruolo del server accesso remoto con l' **Aggiunta guidata ruoli e funzionalità** o Windows PowerShell, è possibile installare uno o più di questi tre servizi ruolo.

>[!IMPORTANT]
>Non tentare di distribuire accesso remoto in una macchina virtuale \(\) VM in Microsoft Azure. L'uso di accesso remoto in Microsoft Azure non è supportato. Non è possibile usare accesso remoto in una macchina virtuale di Azure per distribuire VPN, DirectAccess o qualsiasi altra funzionalità di accesso remoto in Windows Server 2016 o versioni precedenti di Windows Server. Per ulteriori informazioni, vedere [supporto del software server Microsoft per Microsoft Azure macchine virtuali](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).

## <a name="bkmk_da"></a>Servizio di accesso remoto \(RAS\)-gateway RAS

Quando si installa il servizio ruolo **DirectAccess e VPN (RAS)** , si distribuisce il gateway del servizio di accesso remoto \(\)**gateway RAS** . È possibile distribuire il gateway RAS una rete privata virtuale del gateway RAS a tenant singolo \(VPN\) server, un server VPN gateway RAS multi-tenant e come server DirectAccess.

- **Gateway RAS-tenant singolo**. Tramite il gateway RAS è possibile distribuire connessioni VPN per offrire agli utenti finali accesso remoto alla rete e alle risorse dell'organizzazione. Se i client eseguono Windows 10, è possibile distribuire Always On VPN, che mantiene una connessione permanente tra i client e la rete dell'organizzazione ogni volta che i computer remoti sono connessi a Internet. Con il gateway RAS è inoltre possibile creare una connessione VPN da sito a sito tra due server in posizioni diverse, ad esempio tra l'ufficio principale e una succursale, e utilizzare Network Address Translation \(NAT\) in modo che gli utenti all'interno della rete possano accedere a risorse esterne, ad esempio Internet. Il gateway RAS supporta inoltre Border Gateway Protocol (BGP), che fornisce servizi di routing dinamico quando i percorsi di ufficio remoto hanno anche gateway perimetrali che supportano BGP.

- **Gateway RAS-multi-tenant**. Quando si usa la virtualizzazione rete Hyper\-V o si hanno reti VM distribuite con reti locali virtuali \(VLAN\), è possibile distribuire gateway RAS come gateway e router perimetrale basato su software multi-tenant. Con il gateway RAS, i provider di servizi cloud \(CSP\) e le aziende possono abilitare il routing del traffico di rete di data center e cloud tra reti virtuali e fisiche, incluso Internet. Con il gateway RAS, i tenant possono usare connessioni VPN Point-to-site per accedere alle risorse di rete VM nel Data Center da qualsiasi posizione. È anche possibile fornire ai tenant connessioni VPN da sito a sito tra i siti remoti e il data center CSP. Inoltre, è possibile configurare il gateway RAS con BGP per il routing dinamico ed è possibile abilitare Network Address Translation \(NAT\) per fornire l'accesso a Internet per le macchine virtuali nelle reti VM.

>[!IMPORTANT]
> Il gateway RAS con funzionalità multi-tenant è disponibile anche in Windows Server 2012 R2.

- **VPN always on**. Always On VPN consente agli utenti remoti di accedere in modo sicuro a risorse condivise, siti Web Intranet e applicazioni in una rete interna senza connettersi a una rete VPN. 

Per altre informazioni, vedere [gateway RAS](ras-gateway/RAS-Gateway.md) e [Border Gateway Protocol (BGP)](bgp/Border-Gateway-Protocol-BGP.md).

## <a name="bkmk_rras"></a>Routing

È possibile usare accesso remoto per instradare il traffico di rete tra subnet nella rete locale. Il routing fornisce il supporto per i router NAT (Network Address Translation), i router LAN che eseguono BGP, Routing Information Protocol (RIP) e i router che supportano il multicast tramite il protocollo IGMP (Internet Group Management Protocol). Come router completo, è possibile distribuire RAS in un computer server o come macchina virtuale (VM) in un computer che esegue Hyper-V.

Per installare accesso remoto come router LAN, utilizzare l'aggiunta guidata ruoli e funzionalità in Server Manager e selezionare il ruolo del server **accesso remoto** e il servizio ruolo di **routing** . in alternativa, digitare il comando seguente al prompt di Windows PowerShell, quindi premere INVIO.

```  
Install-RemoteAccess -VpnType RoutingOnly
```  

## <a name="bkmk_proxy"></a>Proxy applicazione Web

Proxy applicazione Web è un servizio ruolo di accesso remoto in Windows Server 2016. Proxy applicazione Web rende disponibili funzionalità di proxy inverso per le applicazioni Web all'interno della rete aziendale, in modo da consentire agli utenti con qualsiasi dispositivo di accedere a tali applicazioni dall'esterno della rete aziendale. Proxy applicazione Web Preautentica l'accesso alle applicazioni Web tramite Active Directory Federation Services (AD FS) e funge anche da proxy AD FS.

Per installare accesso remoto come proxy applicazione Web, utilizzare l'aggiunta guidata ruoli e funzionalità in Server Manager e selezionare il ruolo del server **accesso remoto** e il servizio ruolo **proxy applicazione Web** . in alternativa, digitare il comando seguente al prompt di Windows PowerShell, quindi premere INVIO.  

```  
Install-RemoteAccess -VpnType SstpProxy  
```  

Per ulteriori informazioni, vedere [proxy applicazione Web](https://technet.microsoft.com/windows-server-docs/identity/web-application-proxy/web-application-proxy-windows-server).


---