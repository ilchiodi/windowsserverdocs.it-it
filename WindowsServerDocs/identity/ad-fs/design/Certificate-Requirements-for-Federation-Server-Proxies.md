---
ms.assetid: dc24adb7-385d-4a92-ab81-78ba73df0118
title: Requisiti dei certificati per i proxy Server federativi
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e7fb8e71afed1c0eb6b55857835d95f2dd0ec9d5
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="certificate-requirements-for-federation-server-proxies"></a>Requisiti dei certificati per i proxy Server federativi

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Sono necessari server che eseguono con il ruolo di proxy server federativo in Active Directory Federation Services \(AD FS\) da utilizzare Secure Sockets Layer \(SSL\) certificati di autenticazione server. I proxy server federativi usano i certificati di autenticazione server SSL per proteggere la comunicazione tra il traffico di server Web con i client del Web.  
  
I proxy server federativi sono in genere esposti ai computer su Internet che non sono inclusi nella tua infrastruttura a chiave pubblica dell'organizzazione \(PKI\). Pertanto, utilizzare un certificato di autenticazione server rilasciato da un'autorità di certificazione pubblica \(third\-party\) \(CA\), ad esempio VeriSign.  
  
Quando si dispone di una farm di proxy server federativo, tutti i computer proxy server federativo è necessario utilizzare lo stesso certificato di autenticazione server. Per ulteriori informazioni, vedere [sulla creazione di una Farm di Proxy Server federativo](When-to-Create-a-Federation-Server-Proxy-Farm.md).  
  
È importante verificare che il nome del soggetto nel certificato di autenticazione server corrisponda al valore di nome servizio federativo specificato nella gestione di AD FS snap-in. Per individuare questo valore, aprire lo snap-in con clic **servizio**, fare clic su **Modifica proprietà servizio federativo**e quindi individuare il valore in **nome servizio federativo** casella di testo.  
  
Per informazioni generali sull'uso dei certificati SSL, vedere configurazione di Secure Sockets Layer in IIS 7.0 \ ([http:///\/go.microsoft.com\/fwlink\/? LinkID\ = 108544](https://go.microsoft.com/fwlink/?LinkID=108544)\) e configurazione di certificati Server in IIS 7.0 \ ([http:///\/go.microsoft.com\/fwlink\/? LinkID\ = 108545](https://go.microsoft.com/fwlink/?LinkID=108545)\).  
  
> [!NOTE]  
> Certificati di autenticazione client non sono necessari per i proxy server federativi AD FS.  
  
Se tutti i certificati che usi ha \(CRLs\) gli elenchi di revoche di certificati, il server con il certificato configurato deve essere in grado di contattare il server che distribuisce i CRL. Il tipo di CRL determina quali porte vengono utilizzate.  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di ADFS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
