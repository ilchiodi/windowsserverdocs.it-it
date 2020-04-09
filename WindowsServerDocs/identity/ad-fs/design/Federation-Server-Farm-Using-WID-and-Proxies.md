---
ms.assetid: f0464182-56a2-4bfa-a8c8-7e39c1bd62d3
title: Server farm federativa che usa Database interno di Windows e proxy
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 523e076ad9593f09ac2f9db5c45fa8c2e82f05bb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853104"
---
# <a name="federation-server-farm-using-wid-and-proxies"></a>Server farm federativa che usa Database interno di Windows e proxy

Questa topologia di distribuzione per Active Directory Federation Services \(AD FS\) è identica alla Federazione server farm con database interno di Windows \(topologia di\) WID, ma aggiunge i computer proxy alla rete perimetrale per supportare gli utenti esterni. Questi proxy reindirizzano le richieste di autenticazione client che provengono dall'esterno della rete aziendale alla Federazione server farm. Nelle versioni precedenti di AD FS, questi proxy erano denominati proxy server federativi.  
  
> [!IMPORTANT]  
> In Active Directory Federation Services \(AD FS\) in Windows Server 2012 R2, il ruolo di proxy server federativo viene gestito da un nuovo servizio ruolo di accesso remoto denominato proxy applicazione Web. Per abilitare la AD FS per l'accessibilità dall'esterno della rete aziendale, che era lo scopo della distribuzione di un proxy server federativo nelle versioni legacy di AD FS, ad esempio AD FS 2,0 e AD FS in Windows Server 2012, è possibile distribuire uno o più proxy applicazione Web per AD FS in Windows Server 2012 R2.  
>   
> Nel contesto di AD FS, proxy applicazione Web funge da AD FS proxy server federativo. Oltre a questo, Proxy applicazione Web rende disponibili funzionalità di proxy inverso per le applicazioni Web all'interno della rete aziendale, in modo da consentire agli utenti con qualsiasi dispositivo di accedere a tali applicazioni dall'esterno della rete aziendale. Per altre informazioni sul servizio ruolo Proxy applicazione Web, vedere la panoramica di Proxy applicazione Web.  
>   
> Per pianificare la distribuzione di Proxy applicazione Web, è possibile consultare le informazioni negli argomenti seguenti:  
>   
> -   [Pianificare l'infrastruttura del proxy dell'applicazione Web (WAP)](https://technet.microsoft.com/library/dn383648.aspx)  
> -   [Pianificare il server proxy applicazione Web](https://technet.microsoft.com/library/dn383647.aspx)  
  
## <a name="deployment-considerations"></a>Considerazioni sulla distribuzione  
Questa sezione vengono descritte varie considerazioni sui destinatari, vantaggi e limitazioni di cui è associate a questa topologia di distribuzione.  
  
### <a name="who-should-use-this-topology"></a>Chi dovrebbe utilizzare questa topologia?  
  
-   Le organizzazioni con un massimo di 100 relazioni di trust configurato che è necessario specificare sia i relativi utenti interni e gli utenti esterni \(sono connessi ai computer che si trovano fisicamente all'esterno della rete aziendale\) con single sign-\-in \(SSO\) accesso alle applicazioni federate o servizi  
  
-   Organizzazioni che devono fornire i relativi utenti interni e gli utenti esterni l'accesso SSO a Microsoft Office 365  
  
-   Organizzazioni più piccole parte di utenti esterni che richiedono servizi scalabili ridondanti  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quali sono i vantaggi dell'utilizzo di questa topologia?  
  
-   Lo stesso avvantaggia elencati per il [Federation Server Farm utilizzando WID](Federation-Server-Farm-Using-WID.md) topologia, oltre al vantaggio di fornire accesso aggiuntivo per gli utenti esterni  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quali sono le limitazioni dell'utilizzo di questa topologia?  
  
-   Le stesse limitazioni come elencato per il [Federation Server Farm utilizzando WID](Federation-Server-Farm-Using-WID.md) topologia  

||1 \- 100 attendibilità della relying Party|Più di 100 attendibilità della relying Party 
| ----- |-----| ------ |
|1 \- 30 AD FS nodi|Database interno di Windows supportati|Non supportato utilizzando WID \- SQL necessarie 
|Nodi più di 30 AD FS|Non supportato utilizzando WID \- SQL necessarie|Non supportato utilizzando WID \- SQL necessarie  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Consigli di layout posizionamento e la rete di server  
Per distribuire questa topologia, oltre ad aggiungere due proxy applicazione Web, è necessario assicurarsi che la rete perimetrale possa anche fornire l'accesso a un Domain Name System \(server DNS\) e a un secondo bilanciamento carico di rete \(NLB\) host. Il secondo host NLB deve essere configurato con un cluster di bilanciamento carico di RETE che utilizza un Internet\-indirizzo IP del cluster accessibile e utilizzino la stessa impostazione di nome DNS cluster del cluster di bilanciamento carico di RETE precedente configurata nella rete aziendale \(fs.fabrikam.com\). I proxy dell'applicazione Web devono essere configurati anche con indirizzi IP accessibili\-Internet.  
  
Nella figura seguente viene illustrata la server farm federativa esistente con la topologia WID descritta in precedenza e il modo in cui l'azienda fittizia Fabrikam, Inc., fornisce l'accesso a un server DNS perimetrale, aggiunge un secondo host NLB con lo stesso nome DNS del cluster \(fs.fabrikam.com\)e aggiunge due proxy applicazione Web \(WAP1 e WAP2\) alla rete perimetrale.  
  
![Farm e proxy WID](media/WIDFarmADFSBlue.gif)  
  
Per ulteriori informazioni su come configurare l'ambiente di rete per l'utilizzo con i server federativi o i proxy applicazione Web, vedere la sezione relativa ai requisiti di risoluzione dei nomi in [ad FS requisiti](AD-FS-Requirements.md) e [pianificare l'infrastruttura del proxy dell'applicazione Web (WAP)](https://technet.microsoft.com/library/dn383648.aspx).  
  
## <a name="see-also"></a>Vedi anche  
[Pianificare la topologia di distribuzione di AD FS](Plan-Your-AD-FS-Deployment-Topology.md)  
[Guida alla progettazione di AD FS in Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

