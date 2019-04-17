---
ms.assetid: 935ea7c2-4678-4033-b50f-2036a0359c5d
title: Dove posizionare un Server federativo
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 376cec7f3a4fb1f988ac5d458b05220c7b9de970
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="where-to-place-a-federation-server"></a>Dove posizionare un Server federativo

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Come procedura di sicurezza meglio, posizionare i server federativi \(AD FS\) di Active Directory Federation Services usato un firewall e collegarli alla tua rete aziendale per impedirne l'esposizione su Internet. Questo è importante perché i server federativi dispongano di autorizzazioni complete per concedere i token di sicurezza. Di conseguenza, devono avere la stessa protezione di un controller di dominio. Se viene compromesso un server federativo, un utente malintenzionato ha la possibilità di rilasciare token di accesso completo a tutte le applicazioni Web e per i server federativi che sono protetti da Active Directory Federation Services \(AD FS\) in tutte le organizzazioni partner risorse.  
  
> [!NOTE]  
> Come procedura di sicurezza consigliata, evitare che i server federativi direttamente accessibile su Internet. È consigliabile fornire i server federativi accesso diretto a Internet solo quando si impostano su un ambiente di testing o quando l'organizzazione non dispone di una rete perimetrale.  
  
Per le reti aziendali tipiche, viene stabilito un firewall con connessione intranet\ tra la rete aziendale e la rete perimetrale e spesso viene stabilito un firewall di con connessione Internet tra la rete perimetrale e Internet. In questo caso, il server federativo si trova all'interno della rete azienda e non è direttamente accessibile dai client Internet.  
  
> [!NOTE]  
> I computer client connessi alla rete aziendale possono comunicare direttamente con il server federativo tramite autenticazione integrata di Windows.  
  
Un proxy server federativo deve essere inserito nella rete perimetrale prima di configurare i server del firewall per l'utilizzo con AD FS. Per ulteriori informazioni, vedere [dove posizionare un Proxy Server federativo](Where-to-Place-a-Federation-Server-Proxy.md).  
  
## <a name="configuring-your-firewall-servers-for-a-federation-server"></a>Configurazione dei server del firewall per un server federativo  
In modo che i server federativi possono comunicare direttamente con i proxy server federativi, il server del firewall intranet deve essere configurato per consentire il traffico Secure Hypertext Transfer Protocol \(HTTPS\) da proxy server federativo al server federativo. Questo è un requisito perché il server del firewall intranet deve pubblicare il server federativo tramite la porta 443 in modo che il proxy server federativo nella rete perimetrale può accedere al server federativo.  
  
Inoltre, con il connessione intranet\ firewall server, ad esempio un server che esegue Internet Security and Acceleration \(ISA\) Server, utilizza un processo noto come pubblicazione del server per distribuire le richieste client Internet per i server federativi aziendale appropriati. Ciò significa che è necessario creare manualmente una regola di pubblicazione server nel server intranet che esegue ISA Server che pubblica l'URL del server federativo in cluster, ad esempio, http:///\/fs.fabrikam.com.  
  
Per ulteriori informazioni su come configurare la pubblicazione del server in una rete perimetrale, vedere [dove posizionare un Proxy Server federativo](Where-to-Place-a-Federation-Server-Proxy.md). Per informazioni su come configurare ISA Server per pubblicare un server, vedere [creare una regola di pubblicazione Web sicura](https://go.microsoft.com/fwlink/?LinkId=75182).  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di ADFS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
