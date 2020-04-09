---
ms.assetid: ad0bf21d-2ace-4565-b1f5-ce57c8eb2689
title: Quando creare una farm di proxy server federativi
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: d4b2b889159dee9f3b93a54a2b1924be286792f4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858494"
---
# <a name="when-to-create-a-federation-server-proxy-farm"></a>Quando creare una farm di proxy server federativi

Si consiglia di installare altri proxy server federativi quando si dispone di un Active Directory Federation Services di grandi dimensioni \(AD FS distribuzione\) e si desidera fornire tolleranza di errore, bilanciamento del carico\-e scalabilità per la distribuzione del proxy. L'atto di creare due o più proxy server federativi nella stessa rete perimetrale e di configurare ognuno di essi per proteggere lo stesso AD FS Servizio federativo crea una farm di proxy server federativi.  
  
È possibile creare una farm di proxy server federativi o installare altri proxy server federativi in una farm esistente usando la configurazione guidata di AD FS proxy server federativo. Per altre informazioni, vedere [When to Create a Federation Server Proxy](When-to-Create-a-Federation-Server-Proxy.md).  
  
Prima che tutti i proxy server federativi possano funzionare insieme come farm, è necessario innanzitutto raggrupparli in un indirizzo IP e uno Domain Name System \(DNS\) nome di dominio completo \(FQDN\). È possibile eseguire il clustering dei server distribuendo bilanciamento carico di rete Microsoft \(NLB\) all'interno della rete perimetrale. Le attività nella tabella seguente richiedono che NLB sia configurato in modo appropriato per eseguire il clustering dei proxy server federativi nella farm.  
  
Per ulteriori informazioni su come configurare un FQDN per un cluster mediante la tecnologia NLB Microsoft, vedere [specifica dei parametri del cluster](https://go.microsoft.com/fwlink/?linkid=74651).  
  
## <a name="configuring-federation-server-proxies-for-a-farm"></a>Configurazione di proxy server federativi per una farm  
Nella tabella seguente vengono descritte le attività che è necessario completare in modo che ogni proxy server federativo possa partecipare a una farm.  
  
|Attività|Descrizione|  
|--------|---------------|  
|Puntare tutti i proxy nella farm allo stesso nome di Servizio federativo AD FS|Quando si creano i proxy server federativi, è necessario digitare lo stesso nome di Servizio federativo nella configurazione guidata del proxy server federativo di AD FS per tutti i proxy server federativi che parteciperanno alla farm. Il proxy server federativo utilizza l'URL che costituisce questo nome host DNS per determinare quale AD FS Servizio federativo istanza contattata.<p>Per altre informazioni, vedere [Configure a Computer for the Federation Server Proxy Role](../../ad-fs/deployment/Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md).|  
|Ottenere e condividere i certificati|È possibile ottenere un certificato di autenticazione server da un'autorità di certificazione pubblica \(CA\), ad esempio VeriSign, e quindi configurare il certificato in modo che tutti i proxy server federativi condividano la stessa parte di chiave privata dello stesso certificato nel sito Web predefinito per ogni proxy server federativo. Per condividere il certificato, è necessario installare lo stesso certificato di autenticazione server nel sito Web predefinito per ogni proxy server federativo. Per ulteriori informazioni, vedere [importare un certificato di autenticazione server nel sito Web predefinito](../../ad-fs/deployment/Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md).<p>Per altre informazioni, vedere [Certificate Requirements for Federation Server Proxies](Certificate-Requirements-for-Federation-Server-Proxies.md).|  
  
Per ulteriori informazioni sull'aggiunta di nuovi proxy server federativi per creare una farm proxy server federativo, vedere [Checklist: Setting up a Federation Server proxy](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server-Proxy.md).  
  
## <a name="see-also"></a>Vedi anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
