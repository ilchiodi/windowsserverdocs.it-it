---
ms.assetid: ad0bf21d-2ace-4565-b1f5-ce57c8eb2689
title: Quando creare una Farm di Proxy Server federativo
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8935760cad272d5b82edb675cda85caf0456565f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="when-to-create-a-federation-server-proxy-farm"></a>Quando creare una Farm di Proxy Server federativo

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Si consiglia di installare altri proxy server federativi, quando si dispone di una grande distribuzione \(AD FS\) Active Directory Federation Services e si vuole fornire tolleranza di errore, Load bilanciamento del carico e scalabilità per la distribuzione di proxy. L'atto di creare due o più proxy server federativi nella stessa rete perimetrale e la configurazione di ognuno di essi per proteggere il servizio federativo ADFS stesso crea una farm di proxy server federativo.  
  
È possibile creare una farm di proxy server federativi o installare altri proxy server federativi in una farm esistente usando la configurazione guidata di AD FS Federation Server Proxy. Per ulteriori informazioni, vedere [quando creare un Proxy Server federativo](When-to-Create-a-Federation-Server-Proxy.md).  
  
Prima di tutti i server federativi possono funzionare insieme come farm, è necessario innanzitutto cluster in un unico indirizzo IP e dominio completo di uno \(DNS\) Domain Name System \(FQDN\) dei nomi. È possibile cluster i server distribuendo \(NLB\) bilanciamento carico di rete Microsoft nella rete perimetrale. Le attività nella tabella seguente richiedono bilanciamento carico di rete deve essere configurata in modo appropriato per i proxy server federativi della farm del cluster.  
  
Per ulteriori informazioni su come configurare un FQDN per un cluster mediante la tecnologia NLB Microsoft, vedere [specifica dei parametri del Cluster](https://go.microsoft.com/fwlink/?linkid=74651).  
  
## <a name="configuring-federation-server-proxies-for-a-farm"></a>Configurazione di proxy server federativi per una farm  
Nella tabella seguente vengono descritte le attività che devono essere completate in modo che ogni proxy server federativo possa far parte di una farm.  
  
|Attività|Descrizione|  
|--------|---------------|  
|Puntare tutti i proxy nella farm allo stesso nome servizio federativo AD FS|Quando si crea la federazione proxy server federativi, è necessario digitare lo stesso nome servizio federativo nella configurazione guidata di AD FS Federation Server Proxy per tutti i server federativi che faranno parte della farm. Il proxy server federativo utilizza l'URL che costituisce questo nome host DNS per determinare quale istanza del servizio federativo AD FS è contatti.<br /><br />Per ulteriori informazioni, vedere [configurare un Computer per il ruolo di Proxy Server federativo](../../ad-fs/deployment/Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md).|  
|Ottenere e condividere i certificati|È possibile ottenere un server di autenticazione certificato da un'autorità di certificazione pubblica \ (CA\), ad esempio VeriSign e quindi configurare il certificato in modo che tutti i proxy server federativi condividano la stessa parte di chiave privata dello stesso certificato nel sito Web predefinito per ogni proxy server federativo. Per condividere il certificato, è necessario installare lo stesso certificato di autenticazione server nel sito Web predefinito per ogni proxy server federativo. Per ulteriori informazioni, vedere [importare un certificato di autenticazione Server nel sito Web predefinito](../../ad-fs/deployment/Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md).<br /><br />Per ulteriori informazioni, vedere [requisiti dei certificati per i proxy Server federativi](Certificate-Requirements-for-Federation-Server-Proxies.md).|  
  
Per ulteriori informazioni sull'aggiunta di nuovi server federativi per creare una farm di proxy server federativo, vedere [elenco di controllo: impostazione di un Server Proxy federativo](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server-Proxy.md).  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di ADFS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
