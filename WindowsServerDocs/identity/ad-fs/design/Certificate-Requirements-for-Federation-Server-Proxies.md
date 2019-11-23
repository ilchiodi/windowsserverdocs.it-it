---
ms.assetid: dc24adb7-385d-4a92-ab81-78ba73df0118
title: Requisiti dei certificati per i proxy server federativi
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: dab77c3e3226e89eb3ac9b74e7db9b6df8f181bf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408144"
---
# <a name="certificate-requirements-for-federation-server-proxies"></a>Requisiti dei certificati per i proxy server federativi

I server che sono in esecuzione nel ruolo proxy server federativo in Active Directory Federation Services \(AD FS\) sono necessari per utilizzare Secure Sockets Layer certificati di autenticazione server SSL \(\). I proxy server federativi usano i certificati di autenticazione server per proteggere le comunicazioni tra il traffico dei server Web e i client Web.  
  
I proxy server federativi sono in genere esposti a computer in Internet che non sono inclusi nell'infrastruttura a chiave pubblica dell'organizzazione \(\)PKI. Usare quindi un certificato di autenticazione server emesso da un \(di terze parti\-\) autorità di certificazione \(CA\), ad esempio VeriSign.  
  
Quando si dispone di una farm di proxy server federativi, tutti i computer proxy server federativi devono utilizzare lo stesso certificato di autenticazione server. Per altre informazioni, vedere [When to Create a Federation Server Proxy Farm](When-to-Create-a-Federation-Server-Proxy-Farm.md).  
  
È importante verificare che il nome del soggetto nel certificato di autenticazione server corrisponda al valore del nome del Servizio federativo specificato nello snap-in di gestione AD FS\-in. Per individuare questo valore, aprire lo snap\-in, fare clic con il pulsante\-destro del mouse su **servizio**, fare clic su **modifica proprietà servizio federativo**, quindi individuare il valore nella casella di testo **nome servizio federativo** .  
  
Per informazioni generali sull'uso dei certificati SSL, vedere Configurazione di Secure Sockets Layer in IIS 7,0 \([http:\/\/go.microsoft.com\/fwlink\/? LinkID\=108544](https://go.microsoft.com/fwlink/?LinkID=108544)\) e configurazione dei certificati del server in IIS 7,0 \([http:\/\/go.Microsoft.com\/fwlink\/? LinkID\=108545](https://go.microsoft.com/fwlink/?LinkID=108545)\).  
  
> [!NOTE]  
> I certificati di autenticazione client non sono necessari per AD FS proxy server federativi.  
  
Se uno dei certificati utilizzati dispone di elenchi di revoche di certificati \(CRL\), il server con il certificato configurato deve essere in grado di contattare il server che distribuisce i CRL. Il tipo di CRL determina quali porte vengono utilizzate.  
  
## <a name="see-also"></a>Vedi anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
