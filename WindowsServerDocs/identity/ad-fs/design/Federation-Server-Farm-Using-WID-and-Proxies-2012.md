---
ms.assetid: 8890ccc9-068d-4da2-bd51-8a2964173ff1
title: Server Farm federativa con WID e proxy
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4bd815daccdd72a8c612b9b728ce12378c1926e7
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="federation-server-farm-using-wid-and-proxies"></a>Server Farm federativa con WID e proxy

>Si applica a: Windows Server 2012

Questa topologia di distribuzione di Active Directory Federation Services \(AD FS\) è identica alla server farm federativa con topologia Database interno di Windows \(WID\), ma aggiunge i proxy server federativi nella rete perimetrale per supportare gli utenti esterni. I proxy server federativi reindirizzano le richieste di autenticazione client provengono dall'esterno della rete aziendale per la server farm federativa.  
  
## <a name="deployment-considerations"></a>Considerazioni sulla distribuzione di  
In questa sezione vengono descritte varie considerazioni sui destinatari, vantaggi e limitazioni di cui è associate a questa topologia di distribuzione.  
  
### <a name="who-should-use-this-topology"></a>Chi dovrebbe utilizzare questa topologia?  
  
-   Le organizzazioni con un massimo di 100 relazioni di trust configurato che è necessario per fornire i relativi utenti interni e gli utenti esterni \ (che sono connessi ai computer che si trovano fisicamente all'esterno di isolamento di rete aziendale) con una sola di accesso \(SSO\) accesso alle applicazioni federate o servizi  
  
-   Organizzazioni che devono fornire i relativi utenti interni e gli utenti esterni con accesso SSO a Microsoft Office 365  
  
-   Organizzazioni più piccole parte di utenti esterni che richiedono servizi scalabili ridondanti  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quali sono i vantaggi dell'uso di questa topologia?  
  
-   Lo stesso avvantaggia elencati per il [Federation Server Farm utilizzando WID](Federation-Server-Farm-Using-WID-2012.md) topologia, oltre al vantaggio di fornire accesso aggiuntivo per gli utenti esterni  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quali sono i limiti di utilizzo di questa topologia?  
  
-   Le stesse limitazioni come elencato per il [Federation Server Farm utilizzando WID](Federation-Server-Farm-Using-WID-2012.md) topologia  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Consigli di layout posizionamento e la rete di server  
Per distribuire la topologia, oltre ad aggiungere due proxy server federativo, è necessario assicurarsi che la rete perimetrale può anche fornire accesso a un server Domain Name System \(DNS\) e in un secondo host di bilanciamento carico di rete \(NLB\). Il secondo host NLB deve essere configurato con un cluster di bilanciamento carico di rete che utilizza un indirizzo IP del cluster accessibile da Internet, ed è necessario utilizzare la stessa impostazione di nome DNS cluster del cluster di bilanciamento carico di rete precedente configurata sul \(fs.fabrikam.com\) rete aziendale. I proxy server federativo devono essere configurati anche con indirizzi IP accessibili su Internet.  
  
Nella figura seguente mostra la server farm federativa esistente con topologia database interno di Windows che è stato descritto in precedenza e consente di accedere a un server DSN perimetrale, come la società fittizia Fabrikam, Inc., aggiunge un secondo host di bilanciamento carico di rete con il nome \(fs.fabrikam.com\) stesso cluster DNS e aggiunge due proxy server federativo \(fsp1 and fsp2\) alla rete perimetrale.  
  
![Server farm con database interno di Windows](media/FarmWIDProxies.gif)  
  
Per ulteriori informazioni su come configurare l'ambiente di rete per l'utilizzo con i server federativi o server federativi, vedere [requisiti di risoluzione dei nomi per i server federativi](Name-Resolution-Requirements-for-Federation-Servers.md) o [requisiti di risoluzione dei nomi per i proxy Server federativi](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di ADFS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
