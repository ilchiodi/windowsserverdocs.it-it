---
ms.assetid: f0464182-56a2-4bfa-a8c8-7e39c1bd62d3
title: Server Farm federativa con WID e proxy
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e372f066fc82b9857d438234b491732a177e24fa
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="federation-server-farm-using-wid-and-proxies"></a>Server Farm federativa con WID e proxy

>Si applica a: Windows Server 2016, Windows Server 2012 R2

Questa topologia di distribuzione di Active Directory Federation Services \(AD FS\) è identica per la server farm federativa con topologia Database interno di Windows \(WID\), ma aggiunge i computer proxy alla rete perimetrale per supportare gli utenti esterni. Questi proxy reindirizzare le richieste di autenticazione client provengono dall'esterno della rete aziendale per la server farm federativa. Nelle versioni precedenti di ADFS, chiamati questi proxy server federativi.  
  
> [!IMPORTANT]  
> In Active Directory Federation Services \(AD FS\) in Windows Server 2012 R2, il ruolo di un proxy server federativo viene gestito da un nuovo servizio ruolo Accesso remoto denominato Proxy applicazione Web. Per rendere ADFS accessibile dall'esterno della rete aziendale, che lo scopo della distribuzione di un proxy server federativo nelle versioni legacy di ADFS, ad esempio ADFS 2.0 e AD FS in Windows Server 2012, è possibile distribuire uno o più proxy applicazione web per ADFS in Windows Server 2012 R2.  
>   
> Nel contesto di ADFS, Proxy applicazione Web funziona come un proxy server federativo di ADFS. Oltre a questo, Proxy applicazione Web fornisce funzionalità di proxy inverso per le applicazioni web all'interno della rete aziendale consentire agli utenti in qualsiasi dispositivo per accedervi dall'esterno della rete aziendale. Per ulteriori informazioni sul servizio ruolo Proxy applicazione Web, vedere Panoramica di Proxy applicazione Web.  
>   
> Per pianificare la distribuzione di proxy applicazione Web, è possibile esaminare le informazioni negli argomenti seguenti:  
>   
> -   [Pianificare l'infrastruttura di Proxy applicazione Web (WAP)](https://technet.microsoft.com/library/dn383648.aspx)  
> -   [Pianificare il Server di Proxy applicazione Web](https://technet.microsoft.com/library/dn383647.aspx)  
  
## <a name="deployment-considerations"></a>Considerazioni sulla distribuzione di  
In questa sezione vengono descritte varie considerazioni sui destinatari, vantaggi e limitazioni di cui è associate a questa topologia di distribuzione.  
  
### <a name="who-should-use-this-topology"></a>Chi dovrebbe utilizzare questa topologia?  
  
-   Le organizzazioni con un massimo di 100 relazioni di trust configurato che è necessario per fornire i relativi utenti interni e gli utenti esterni \ (che sono connessi ai computer che si trovano fisicamente all'esterno di isolamento di rete aziendale) con una sola di accesso \(SSO\) accesso alle applicazioni federate o servizi  
  
-   Organizzazioni che devono fornire i relativi utenti interni e gli utenti esterni con accesso SSO a Microsoft Office 365  
  
-   Organizzazioni più piccole parte di utenti esterni che richiedono servizi scalabili ridondanti  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quali sono i vantaggi dell'uso di questa topologia?  
  
-   Lo stesso avvantaggia elencati per il [Federation Server Farm utilizzando WID](Federation-Server-Farm-Using-WID.md) topologia, oltre al vantaggio di fornire accesso aggiuntivo per gli utenti esterni  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quali sono i limiti di utilizzo di questa topologia?  
  
-   Le stesse limitazioni come elencato per il [Federation Server Farm utilizzando WID](Federation-Server-Farm-Using-WID.md) topologia  

||1 \-100 attendibilità della Relying party|Più di 100 attendibilità della Relying party 
| ----- |-----| ------ |
|1 \-30 AD FS nodi|Database interno di Windows supportati|Non supportato utilizzando WID \-SQL necessarie 
|Più di 30 AD FS nodi|Non supportato utilizzando WID \-SQL necessarie|Non supportato utilizzando WID \-SQL necessarie  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Consigli di layout posizionamento e la rete di server  
Per distribuire la topologia, oltre ad aggiungere due proxy applicazione web, è necessario assicurarsi che la rete perimetrale può anche fornire accesso a un server Domain Name System \(DNS\) e in un secondo host di bilanciamento carico di rete \(NLB\). Il secondo host NLB deve essere configurato con un cluster di bilanciamento carico di rete che utilizza un indirizzo IP del cluster accessibile da Internet, ed è necessario utilizzare la stessa impostazione di nome DNS cluster del cluster di bilanciamento carico di rete precedente configurata sul \(fs.fabrikam.com\) rete aziendale. I proxy applicazione web devono essere configurati anche con indirizzi IP accessibili su Internet.  
  
Nella figura seguente mostra la server farm federativa esistente con topologia database interno di Windows che è stato descritto in precedenza e consente di accedere a un server DSN perimetrale, come la società fittizia Fabrikam, Inc., aggiunge un secondo host di bilanciamento carico di rete con il nome \(fs.fabrikam.com\) stesso cluster DNS e aggiunge due proxy applicazione web \(wap1 and wap2\) alla rete perimetrale.  
  
![Proxy e Farm database interno di Windows](media/WIDFarmADFSBlue.gif)  
  
Per ulteriori informazioni su come configurare l'ambiente di rete per l'utilizzo con server federativo o proxy applicazione web, vedere "Requisiti di risoluzione dei nomi" sezione [requisiti per ADFS](AD-FS-Requirements.md) e [pianificare l'infrastruttura di Proxy applicazione Web (WAP)](https://technet.microsoft.com/library/dn383648.aspx).  
  
## <a name="see-also"></a>Vedere anche  
[Pianificare la topologia di distribuzione AD FS](Plan-Your-AD-FS-Deployment-Topology.md)  
[Guida alla progettazione di ADFS in Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

