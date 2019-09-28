---
ms.assetid: 8890ccc9-068d-4da2-bd51-8a2964173ff1
title: Server farm federativa che usa Database interno di Windows e proxy
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 60072037aea4ecd81376e1334f3a89b7bb2ff851
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408089"
---
# <a name="federation-server-farm-using-wid-and-proxies"></a>Server farm federativa che usa Database interno di Windows e proxy

Questa topologia di distribuzione per Active Directory Federation Services \(AD FS @ no__t-1 è identica alla server farm federativa con la topologia di database interno di Windows \(WID @ no__t-3, ma aggiunge i proxy server federativi alla rete perimetrale a supportare utenti esterni. I proxy server federativi reindirizzano le richieste di autenticazione client provengono dall'esterno della rete aziendale per la server farm federativa.  
  
## <a name="deployment-considerations"></a>Considerazioni sulla distribuzione  
Questa sezione vengono descritte varie considerazioni sui destinatari, vantaggi e limitazioni di cui è associate a questa topologia di distribuzione.  
  
### <a name="who-should-use-this-topology"></a>Chi dovrebbe utilizzare questa topologia?  
  
-   Le organizzazioni con un massimo di 100 relazioni di trust configurato che è necessario specificare sia i relativi utenti interni e gli utenti esterni \(sono connessi ai computer che si trovano fisicamente all'esterno della rete aziendale\) con single sign-\-in \(SSO\) accesso alle applicazioni federate o servizi  
  
-   Organizzazioni che devono fornire i relativi utenti interni e gli utenti esterni l'accesso SSO a Microsoft Office 365  
  
-   Organizzazioni più piccole parte di utenti esterni che richiedono servizi scalabili ridondanti  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quali sono i vantaggi dell'utilizzo di questa topologia?  
  
-   Lo stesso avvantaggia elencati per il [Federation Server Farm utilizzando WID](Federation-Server-Farm-Using-WID-2012.md) topologia, oltre al vantaggio di fornire accesso aggiuntivo per gli utenti esterni  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quali sono le limitazioni dell'utilizzo di questa topologia?  
  
-   Le stesse limitazioni come elencato per il [Federation Server Farm utilizzando WID](Federation-Server-Farm-Using-WID-2012.md) topologia  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Consigli di layout posizionamento e la rete di server  
Per distribuire la topologia, oltre ad aggiungere due proxy server federativo, è necessario assicurarsi che la rete perimetrale può anche fornire accesso a un sistema di nome di dominio \(DNS\) server e al bilanciamento del carico di rete secondo \(Bilanciamento carico di RETE\) host. Il secondo host NLB deve essere configurato con un cluster di bilanciamento carico di RETE che utilizza un Internet\-indirizzo IP del cluster accessibile e utilizzino la stessa impostazione di nome DNS cluster del cluster di bilanciamento carico di RETE precedente configurata nella rete aziendale \(fs.fabrikam.com\). I proxy server federativi devono inoltre essere configurati con Internet\-indirizzi IP accessibili.  
  
La figura seguente mostra la server farm federativa esistente con topologia database interno di Windows che è stato descritto in precedenza e fornisce l'accesso a un server DSN perimetrale, come la società fittizia Fabrikam, Inc., aggiunge un secondo host di bilanciamento carico di RETE con lo stesso nome DNS cluster \(fs.fabrikam.com\), e aggiunge due proxy server federativo \(fsp1 e fsp2\) alla rete perimetrale.  
  
![server farm con database interno di Windows](media/FarmWIDProxies.gif)  
  
Per ulteriori informazioni su come configurare l'ambiente di rete per l'utilizzo con i server federativi o server federativi, vedere [requisiti di risoluzione dei nomi per i server federativi](Name-Resolution-Requirements-for-Federation-Servers.md) o [nome requisiti di risoluzione per i proxy Server federativi](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
