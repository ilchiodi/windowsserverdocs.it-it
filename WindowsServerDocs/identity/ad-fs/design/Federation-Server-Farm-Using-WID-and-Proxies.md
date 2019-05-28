---
ms.assetid: f0464182-56a2-4bfa-a8c8-7e39c1bd62d3
title: Server farm federativa che usa Database interno di Windows e proxy
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d49ae34d83d4a0b912bd92dbb9de16e18cc5b7ff
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191344"
---
# <a name="federation-server-farm-using-wid-and-proxies"></a>Server farm federativa che usa Database interno di Windows e proxy

Questa topologia di distribuzione di Active Directory Federation Services \(ADFS\) è identica alla server farm federativa con Database interno di Windows \(WID\) topologia, ma aggiunge i computer proxy per il rete perimetrale per supportare gli utenti esterni. Questi proxy reindirizza le richieste di autenticazione client provengono dall'esterno della rete aziendale per la server farm federativa. Nelle versioni precedenti di AD FS, chiamati questi proxy server federativi.  
  
> [!IMPORTANT]  
> In Active Directory Federation Services \(ADFS\) in Windows Server 2012 R2, il ruolo di un proxy server federativo viene gestito da un nuovo servizio ruolo Accesso remoto denominato Proxy applicazione Web. Per rendere ADFS accessibile dall'esterno della rete aziendale, che lo scopo della distribuzione di un proxy server federativo nelle versioni legacy di ADFS, ad esempio AD FS 2.0 e ADFS in Windows Server 2012, è possibile distribuire uno o più proxy applicazione web per oggetto D ADFS in Windows Server 2012 R2.  
>   
> Nel contesto di AD FS, Proxy applicazione Web funziona come un proxy server federativo AD FS. Oltre a questo, Proxy applicazione Web rende disponibili funzionalità di proxy inverso per le applicazioni Web all'interno della rete aziendale, in modo da consentire agli utenti con qualsiasi dispositivo di accedere a tali applicazioni dall'esterno della rete aziendale. Per altre informazioni sul servizio ruolo Proxy applicazione Web, vedere la panoramica di Proxy applicazione Web.  
>   
> Per pianificare la distribuzione di Proxy applicazione Web, è possibile consultare le informazioni negli argomenti seguenti:  
>   
> -   [Pianificare l'infrastruttura del Proxy applicazione Web (WAP)](https://technet.microsoft.com/library/dn383648.aspx)  
> -   [Pianificare il Server Proxy applicazione Web](https://technet.microsoft.com/library/dn383647.aspx)  
  
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
Per distribuire questa topologia, oltre ad aggiungere due proxy applicazioni web, è necessario assicurarsi che nella rete perimetrale può anche fornire l'accesso a un sistema di nome di dominio \(DNS\) server e al bilanciamento del carico di rete secondo \( Bilanciamento carico di rete\) host. Il secondo host NLB deve essere configurato con un cluster di bilanciamento carico di RETE che utilizza un Internet\-indirizzo IP del cluster accessibile e utilizzino la stessa impostazione di nome DNS cluster del cluster di bilanciamento carico di RETE precedente configurata nella rete aziendale \(fs.fabrikam.com\). Il proxy applicazione web deve essere configurato anche con Internet\-indirizzi IP accessibili.  
  
La figura seguente mostra la server farm federativa esistente con topologia database interno di Windows che è stato descritto in precedenza e fornisce l'accesso a un server DSN perimetrale, come la società fittizia Fabrikam, Inc., aggiunge un secondo host NLB con lo stesso nome DNS cluster \(fs.fabrikam.com\)e aggiunge due proxy applicazioni web \(wap1 e wap2\) alla rete perimetrale.  
  
![Farm WID e proxy](media/WIDFarmADFSBlue.gif)  
  
Per ulteriori informazioni su come configurare l'ambiente di rete per l'utilizzo con server federativo o proxy applicazione web, vedere "Requisiti di risoluzione dei nomi" sezione [requisiti per ADFS](AD-FS-Requirements.md) e [pianificare l'infrastruttura di Proxy applicazione Web (WAP)](https://technet.microsoft.com/library/dn383648.aspx).  
  
## <a name="see-also"></a>Vedere anche  
[Pianificare la topologia di distribuzione di AD FS](Plan-Your-AD-FS-Deployment-Topology.md)  
[Guida alla progettazione di AD FS in Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

