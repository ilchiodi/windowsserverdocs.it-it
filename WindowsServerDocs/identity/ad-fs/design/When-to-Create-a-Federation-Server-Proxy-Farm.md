---
ms.assetid: ad0bf21d-2ace-4565-b1f5-ce57c8eb2689
title: Quando creare una farm di proxy server federativi
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c33475d7420383448439e2b769562e55127c7b0e
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190634"
---
# <a name="when-to-create-a-federation-server-proxy-farm"></a>Quando creare una farm di proxy server federativi

Si consiglia di installare i proxy server federativo aggiuntivo quando si dispone di un grande Active Directory Federation Services \(ADFS\) distribuzione e si vuole fornire tolleranza di errore, il carico\-bilanciamento del carico e scalabilità per la distribuzione di proxy. L'atto di creazione della federazione di due o più i proxy server federativi nella stessa rete perimetrale e la configurazione di ognuno di essi per proteggere il servizio federativo ADFS stesso crea una farm proxy server federativo.  
  
È possibile creare una farm proxy server federativo o installare altri proxy server federativi per una farm esistente usando la configurazione guidata di AD FS Federation Server Proxy. Per altre informazioni, vedere [When to Create a Federation Server Proxy](When-to-Create-a-Federation-Server-Proxy.md).  
  
Prima di tutti i server federativi possono funzionare insieme come farm, è necessario prima di tutto cluster li in un indirizzo IP e una Domain Name System \(DNS\) il nome di dominio completo \(FQDN\). È possibile riunire in cluster i server tramite la distribuzione di bilanciamento del carico di rete Microsoft \(NLB\) all'interno della rete perimetrale. Le attività nella tabella seguente richiedono bilanciamento carico di rete siano configurati in modo appropriato per i proxy server federativi della farm del cluster.  
  
Per altre informazioni su come configurare un FQDN per un cluster mediante la tecnologia Microsoft NLB, vedere [specificando i parametri del Cluster](https://go.microsoft.com/fwlink/?linkid=74651).  
  
## <a name="configuring-federation-server-proxies-for-a-farm"></a>Configurazione di proxy server federativi per una farm  
Nella tabella seguente vengono descritte le attività che devono essere completate in modo che ciascun proxy server federativo può far parte di una farm.  
  
|Attività|Descrizione|  
|--------|---------------|  
|Puntare tutti i proxy della farm per lo stesso nome servizio federativo AD FS|Quando si crea la federazione i proxy server federativi, è necessario digitare lo stesso nome servizio federativo nella configurazione guidata di AD FS Federation Server Proxy per tutti i server federativi che faranno parte della farm. Il proxy server federativo Usa l'URL che costituisce questo nome host DNS per determinare quale istanza del servizio federativo AD FS, i contatti.<br /><br />Per altre informazioni, vedere [Configure a Computer for the Federation Server Proxy Role](../../ad-fs/deployment/Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md).|  
|Ottenere e condividere i certificati|È possibile ottenere un server di certificato di autenticazione da un'autorità di certificazione pubblica \(autorità di certificazione\), ad esempio VeriSign e quindi configurare il certificato in modo che tutti i server federativi condividano la stessa parte di chiave privata di stesso certificato nel sito Web predefinito per ogni proxy server federativo. Per condividere il certificato, è necessario installare il medesimo certificato di autenticazione server nel sito Web predefinito per ogni proxy server federativo. Per altre informazioni, vedere [importare un certificato di autenticazione Server nel sito Web predefinito](../../ad-fs/deployment/Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md).<br /><br />Per altre informazioni, vedere [Certificate Requirements for Federation Server Proxies](Certificate-Requirements-for-Federation-Server-Proxies.md).|  
  
Per altre informazioni sull'aggiunta di nuovi server federativi per creare una farm proxy server federativo, vedere [elenco di controllo: Configurazione di un Proxy Server federativo](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server-Proxy.md).  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
