---
ms.assetid: d8adcb68-18e0-41bf-a817-d57344bf2e7d
title: Proxy applicazione Web in Windows Server 2016
description: ''
author: kgremban
manager: femila
ms.date: 07/13/2016
ms.topic: article
ms.prod: windows-server
ms.technology: web-app-proxy
ms.openlocfilehash: 4f2827f02ec13d187cdf360637882c6c9d4b2441
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387966"
---
# <a name="web-application-proxy-in-windows-server-2016"></a>Proxy applicazione Web in Windows Server 2016

>Si applica a: Windows Server 2016

il contenuto **Stanziamento è pertinente per la versione locale del proxy applicazione Web. Per abilitare l'accesso sicuro alle applicazioni locali tramite il cloud, vedere il [Azure ad contenuto del proxy dell'applicazione](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/).**  
  
In questa sezione vengono descritte le nuove e modificate nel Proxy applicazione Web per Windows Server 2016. Le nuove funzionalità e modifiche elencate di seguito sono quelli più probabilmente avranno il massimo impatto mentre si lavora con l'anteprima.  
  
## <a name="web-application-proxy-new-features-in-windows-server-2016"></a>Nuove funzionalità del proxy dell'applicazione Web in Windows Server 2016
  
- Preautenticazione per la pubblicazione delle applicazioni di base HTTP  
  
  Base HTTP è il protocollo di autorizzazione utilizzato da numerosi protocolli, tra cui ActiveSync, per la connessione rich client, tra cui Smartphone, la cassetta postale di Exchange. Proxy applicazione Web in genere interagisce con AD FS mediante reindirizzamenti che non è supportato nei client di ActiveSync. Questa nuova versione di Proxy applicazione Web fornisce il supporto per pubblicare un'app tramite HTTP di base, consentendo all'app HTTP di ricevere un non-attestazioni trust della relying party per l'applicazione per il servizio federativo.  
  
  Per ulteriori informazioni sulla pubblicazione di base HTTP, vedere [pubblicazione di applicazioni mediante la preautenticazione di ADFS](Publishing-Applications-using-AD-FS-Preauthentication.md#publish-an-application-that-uses-http-basic)  
  
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
  


