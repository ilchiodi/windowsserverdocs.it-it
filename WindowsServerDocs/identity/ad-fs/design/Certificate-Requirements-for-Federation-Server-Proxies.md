---
ms.assetid: dc24adb7-385d-4a92-ab81-78ba73df0118
title: Requisiti dei certificati per i proxy server federativi
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ca0b25480eedfc6471837ab8ae83b0d1d522e61e
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191661"
---
# <a name="certificate-requirements-for-federation-server-proxies"></a>Requisiti dei certificati per i proxy server federativi

I server che eseguono il ruolo proxy server federativo in Active Directory Federation Services \(ADFS\) sono necessari per utilizzare Secure Sockets Layer \(SSL\) certificati di autenticazione server. I proxy server federativi usano i certificati di autenticazione server per proteggere le comunicazioni tra il traffico dei server Web e i client Web.  
  
Server federativi sono in genere esposti a computer in Internet che non sono inclusi nell'infrastruttura a chiave pubblica azienda \(PKI\). Pertanto, utilizzare un certificato di autenticazione server rilasciato da un pubblico \(terzo\-party\) autorità di certificazione \(CA\), ad esempio VeriSign.  
  
Quando si dispone di una farm proxy server federativo, tutti i computer proxy server federativo è necessario usare lo stesso certificato di autenticazione server. Per altre informazioni, vedere [Quando creare una farm di proxy server federativi](When-to-Create-a-Federation-Server-Proxy-Farm.md).  
  
È importante verificare che il nome del soggetto di corrisponda al certificato di autenticazione server valore che rappresenta il nome del servizio federativo specificato nello snap di gestione di AD FS\-in. Per individuare questo valore, aprire lo snap\-, pulsante destro del mouse\-fare clic su **Service**, fare clic su **Modifica proprietà servizio federativo**e quindi individuare il valore in **Federation Nome del servizio** casella di testo.  
  
Per informazioni generali sull'uso dei certificati SSL, vedere configurazione di Secure Sockets Layer in IIS 7.0 \( [http:\/\/go.microsoft.com\/fwlink\/? LinkID\=108544](https://go.microsoft.com/fwlink/?LinkID=108544) \) e configurazione dei certificati del Server in IIS 7.0 \( [http:\/\/go.microsoft.com\/fwlink\/? LinkID\=108545](https://go.microsoft.com/fwlink/?LinkID=108545)\).  
  
> [!NOTE]  
> Certificati di autenticazione client non sono necessari per proxy server federativo di ADFS.  
  
Se qualsiasi certificato che si usa con gli elenchi di revoche di certificati \(CRL\), il server con il certificato configurato deve essere in grado di contattare il server che distribuisce i CRL. Il tipo di CRL determina quali porte vengono utilizzate.  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
