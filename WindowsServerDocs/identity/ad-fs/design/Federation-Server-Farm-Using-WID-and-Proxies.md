---
ms.assetid: f0464182-56a2-4bfa-a8c8-7e39c1bd62d3
title: Server farm federativa che usa Database interno di Windows e proxy
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 6a123afaebba002b8ee4fb98d5cee5aded286a96
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359128"
---
# <a name="federation-server-farm-using-wid-and-proxies"></a>Server farm federativa che usa Database interno di Windows e proxy

Questa topologia di distribuzione per Active Directory Federation Services \(AD FS @ no__t-1 è identica alla server farm federativa con la topologia di database interno di Windows \(WID @ no__t-3, ma aggiunge i computer proxy alla rete perimetrale per supportare utenti esterni. Questi proxy reindirizzano le richieste di autenticazione client che provengono dall'esterno della rete aziendale alla Federazione server farm. Nelle versioni precedenti di AD FS, questi proxy erano denominati proxy server federativi.  
  
> [!IMPORTANT]  
> In Active Directory Federation Services \(AD FS @ no__t-1 in Windows Server 2012 R2, il ruolo di proxy server federativo viene gestito da un nuovo servizio ruolo di accesso remoto denominato proxy applicazione Web. Per abilitare la AD FS per l'accessibilità dall'esterno della rete aziendale, che era lo scopo della distribuzione di un proxy server federativo nelle versioni legacy di AD FS, ad esempio AD FS 2,0 e AD FS in Windows Server 2012, è possibile distribuire uno o più proxy applicazione Web per un D FS in Windows Server 2012 R2.  
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
Per distribuire questa topologia, oltre ad aggiungere due proxy applicazione Web, è necessario assicurarsi che la rete perimetrale possa anche fornire l'accesso a un Domain Name System \(DNS @ no__t-1 server e a un secondo host di bilanciamento carico di rete \(NLB @ no__t-3. Il secondo host NLB deve essere configurato con un cluster di bilanciamento carico di RETE che utilizza un Internet\-indirizzo IP del cluster accessibile e utilizzino la stessa impostazione di nome DNS cluster del cluster di bilanciamento carico di RETE precedente configurata nella rete aziendale \(fs.fabrikam.com\). I proxy dell'applicazione Web devono essere configurati anche con indirizzi IP Internet @ no__t-0accessible.  
  
Nella figura seguente viene illustrato il server farm della Federazione esistente con la topologia WID descritta in precedenza e il modo in cui l'azienda fittizia Fabrikam, Inc., fornisce l'accesso a un server DNS perimetrale, aggiunge un secondo host NLB con lo stesso nome DNS del cluster @no__t -0FS. fabrikam. com @ no__t-1 e aggiunge due proxy applicazione Web \(wap1 e WAP2 @ no__t-3 alla  
  
![Farm e proxy WID](media/WIDFarmADFSBlue.gif)  
  
Per ulteriori informazioni su come configurare l'ambiente di rete per l'utilizzo con server federativo o proxy applicazione web, vedere "Requisiti di risoluzione dei nomi" sezione [requisiti per ADFS](AD-FS-Requirements.md) e [pianificare l'infrastruttura di Proxy applicazione Web (WAP)](https://technet.microsoft.com/library/dn383648.aspx).  
  
## <a name="see-also"></a>Vedere anche  
[Pianificare la topologia di distribuzione di AD FS](Plan-Your-AD-FS-Deployment-Topology.md)  
[Guida alla progettazione di AD FS in Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

