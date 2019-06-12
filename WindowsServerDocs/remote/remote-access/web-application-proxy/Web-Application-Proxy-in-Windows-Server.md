---
ms.assetid: 0b3587b2-219f-43d8-88b4-1254eaa8b910
title: Proxy applicazione Web in Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.service: ''
ms.suite: na
ms.technology: web-app-proxy
ms.tgt_pltfrm: na
ms.topic: article
author: kgremban
ms.openlocfilehash: 760b0fa11d8d0b77c2a44a8696d199bc378da947
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446796"
---
# <a name="web-application-proxy-in-windows-server"></a>Proxy applicazione Web in Windows Server

>Si applica a: Windows Server&reg; 2016

**Questo contenuto è rilevante per la versione locale del Proxy applicazione Web. Per abilitare l'accesso sicuro alle applicazioni locali tramite il cloud, vedere la [contenuto di Azure AD Application Proxy](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/).**  
  
In questa sezione vengono descritte le nuove e modificate nel Proxy applicazione Web per Windows Server 2016. Le nuove funzionalità e modifiche elencate di seguito sono quelli più probabilmente avranno il massimo impatto mentre si lavora con l'anteprima.  
  
## <a name="web-application-proxy-new-features"></a>Nuove funzionalità Proxy applicazione Web  
  
- Preautenticazione per la pubblicazione delle applicazioni di base HTTP  
  
  Base HTTP è il protocollo di autorizzazione utilizzato da numerosi protocolli, tra cui ActiveSync, per la connessione rich client, tra cui Smartphone, la cassetta postale di Exchange. Proxy applicazione Web in genere interagisce con AD FS mediante reindirizzamenti che non è supportato nei client di ActiveSync. Questa nuova versione di Proxy applicazione Web fornisce il supporto per pubblicare un'app tramite HTTP di base, consentendo all'app HTTP di ricevere un non-attestazioni trust della relying party per l'applicazione per il servizio federativo.  
  
  Per ulteriori informazioni sulla pubblicazione di base HTTP, vedere [pubblicazione di applicazioni mediante la preautenticazione di ADFS](../web-application-proxy/../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md)  
  
- Pubblicazione del dominio con caratteri jolly di applicazioni  
  
  Per supportare scenari, ad esempio SharePoint 2013, l'URL esterno per l'applicazione può ora include un carattere jolly che consente di pubblicare più applicazioni dall'interno di un dominio specifico, ad esempio, https://*.sp-apps.contoso.com. Ciò semplifica la pubblicazione di applicazioni SharePoint.  
  
- HTTP per il reindirizzamento HTTPS  
  
  Per assicurarsi che gli utenti possano accedere all'app, anche se essi trascura digitare l'URL HTTPS, Proxy applicazione Web supporta HTTP per il reindirizzamento HTTPS.  
  
- Pubblicazione HTTP  
  
  È ora possibile pubblicare le applicazioni HTTP mediante la preautenticazione pass-through  
  
- Pubblicazione di applicazioni di Gateway Desktop remoto  
  
  Per ulteriori informazioni su RDG nel Proxy di applicazione Web, vedere [pubblicazione di applicazioni con SharePoint, Exchange e RDG](../web-application-proxy/Publishing-Applications-with-SharePoint,-Exchange-and-RDG.md)  
  
- Nuovo registro di debug per una migliore risoluzione dei problemi e migliorare il servizio Registro di itinerario di controllo completo e la gestione degli errori migliorata  
  
  Per ulteriori informazioni sulla risoluzione dei problemi, vedere [risoluzione dei problemi di Proxy applicazione Web](https://technet.microsoft.com/library/dn770156.aspx)  
  
- Miglioramenti di interfaccia Utente della Console di amministrazione  
  
- Propagazione dell'indirizzo IP del client per applicazioni back-end  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Novità di Windows Server 2016](https://technet.microsoft.com/library/dn765472.aspx)  
  
-   [Pubblicazione delle applicazioni con la preautenticazione di AD FS](../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md)  
  
-   [Risoluzione dei problemi relativi a Proxy applicazione Web](https://technet.microsoft.com/library/dn770156.aspx)  
  


