---
ms.assetid: 935ea7c2-4678-4033-b50f-2036a0359c5d
title: Dove posizionare un server federativo
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 376cec7f3a4fb1f988ac5d458b05220c7b9de970
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857692"
---
# <a name="where-to-place-a-federation-server"></a>Dove posizionare un server federativo

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Per una sicurezza ottimale, sul posto Active Directory Federation Services \(ADFS\)server federativi davanti a un firewall e connetterle alla rete aziendale per impedire l'esposizione su Internet. Questo è importante perché i server federativi hanno autorizzazioni complete per concedere i token di sicurezza. Di conseguenza, dovranno avere la stessa protezione di un controller di dominio. Se viene compromesso un server federativo, un utente malintenzionato ha la possibilità di rilasciare token di accesso completo a tutte le applicazioni Web e per i server federativi che sono protetti da Active Directory Federation Services \(ADFS\) in tutte le risorse organizzazioni partner.  
  
> [!NOTE]  
> Come procedura di sicurezza consigliata, evitare che i server federativi direttamente accessibili su Internet. È consigliabile assegnare i server federativi accesso diretto a Internet solo durante l'impostazione di un ambiente lab di test o quando l'organizzazione non dispone di una rete perimetrale.  
  
Per le reti aziendali tipiche, una rete intranet\-rivolta firewall viene stabilita tra la rete aziendale e la rete perimetrale e Internet\-rivolta firewall spesso viene stabilito tra la rete perimetrale e il Internet. In questo caso, si trova il server federativo nella rete aziendale e non è direttamente accessibile dai client Internet.  
  
> [!NOTE]  
> I computer client connessi alla rete aziendale possono comunicare direttamente con il server di federazione tramite l'autenticazione integrata di Windows.  
  
Un proxy server federativo deve essere posizionato nella rete perimetrale prima di configurare il server del firewall per l'uso con AD FS. Per altre informazioni, vedere [Where to Place a Federation Server Proxy](Where-to-Place-a-Federation-Server-Proxy.md).  
  
## <a name="configuring-your-firewall-servers-for-a-federation-server"></a>Configurazione dei server del firewall per un server federativo  
In modo che i server federativi possono comunicare direttamente con i server federativi, il server del firewall intranet deve essere configurato per consentire Secure Hypertext Transfer Protocol \(HTTPS\) il traffico proveniente da proxy server federativo per il server federativo. Questo è un requisito perché il server del firewall intranet deve pubblicare il server federativo utilizzando la porta 443 in modo che il proxy server federativo nella rete perimetrale possa accedere al server federativo.  
  
Inoltre, la rete intranet\-rivolto verso server del firewall, ad esempio un server che esegue Internet Security and Acceleration \(ISA\) Server, Usa un processo noto come pubblicazione del server per distribuire le richieste client Internet per il server federativi di aziendale appropriati. Ciò significa che è necessario creare manualmente una regola di pubblicazione del server nel server intranet che esegue ISA Server che pubblica l'URL del server federativo in cluster, ad esempio, http:\/\/fs.fabrikam.com.  
  
Per altre informazioni su come configurare la pubblicazione del server in una rete perimetrale, vedere [Where to Place a Federation Server Proxy](Where-to-Place-a-Federation-Server-Proxy.md). Per informazioni su come configurare ISA Server per pubblicare un server, vedere [creare una regola di pubblicazione Web sicura](https://go.microsoft.com/fwlink/?LinkId=75182).  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
