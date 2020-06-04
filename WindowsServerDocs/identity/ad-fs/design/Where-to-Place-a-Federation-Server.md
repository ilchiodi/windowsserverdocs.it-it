---
ms.assetid: 935ea7c2-4678-4033-b50f-2036a0359c5d
title: Dove posizionare un server federativo
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 503b1f206786d36364f9f0033ec0ea88330c444f
ms.sourcegitcommit: 2cc251eb5bc3069bf09bc08e06c3478fcbe1f321
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/03/2020
ms.locfileid: "84333862"
---
# <a name="where-to-place-a-federation-server"></a>Dove posizionare un server federativo

Come procedura di sicurezza consigliata, inserire Active Directory Federation Services \( ad FS \) server federativi protetti da un firewall e connetterli alla rete aziendale per impedire l'esposizione a Internet. Questo aspetto è importante perché i server federativi hanno l'autorizzazione completa per concedere i token di sicurezza. Di conseguenza, dovranno avere la stessa protezione di un controller di dominio. Se un server federativo è compromesso, un utente malintenzionato ha la possibilità di rilasciare token di accesso completo a tutte le applicazioni Web e ai server federativi protetti da Active Directory Federation Services \( ad FS \) in tutte le organizzazioni partner risorse.  
  
> [!NOTE]  
> Come procedura di sicurezza consigliata, evitare che i server federativi siano direttamente accessibili su Internet. Si consiglia di concedere ai server federativi l'accesso diretto a Internet solo quando si configura un ambiente lab di test o quando l'organizzazione non dispone di una rete perimetrale.  
  
Per le reti aziendali tipiche, viene stabilito un firewall con connessione alla Intranet \- tra la rete aziendale e la rete perimetrale e \- viene spesso stabilito un firewall con connessione Internet tra la rete perimetrale e Internet. In questa situazione, il server federativo si trova all'interno della rete aziendale e non è direttamente accessibile dai client Internet.  
  
> [!NOTE]  
> I computer client connessi alla rete aziendale possono comunicare direttamente con il server federativo tramite l'autenticazione integrata di Windows.  
  
È necessario inserire un proxy server federativo nella rete perimetrale prima di configurare i server del firewall per l'uso con AD FS. Per altre informazioni, vedere [Dove posizionare un Proxy Server federativo](Where-to-Place-a-Federation-Server-Proxy.md).  
  
## <a name="configuring-your-firewall-servers-for-a-federation-server"></a>Configurazione dei server del firewall per un server federativo  
In modo che i server federativi possano comunicare direttamente con i proxy server federativi, il server del firewall Intranet deve essere configurato per consentire il \( traffico HTTPS protetto Hypertext Transfer Protocol \) dal proxy server federativo al server federativo. Questo è un requisito perché il server del firewall Intranet deve pubblicare il server federativo utilizzando la porta 443, in modo che il proxy server federativo nella rete perimetrale possa accedere al server federativo.  
  
Inoltre, il server del firewall con connessione alla Intranet \- , ad esempio un server che esegue ISA Server per Internet Security and Acceleration \( \) , usa un processo noto come pubblicazione del server per distribuire le richieste client Internet ai server federativi aziendali appropriati. Ciò significa che è necessario creare manualmente una regola di pubblicazione del server nel server Intranet che esegue ISA Server che pubblica l'URL del server federativo cluster, ad esempio, http: \/ \/ FS.fabrikam.com.  
  
Per altre informazioni su come configurare la pubblicazione del server in una rete perimetrale, vedere [Where to Place a Federation Server Proxy](Where-to-Place-a-Federation-Server-Proxy.md). Per informazioni su come configurare ISA Server per pubblicare un server, vedere [creare una regola di pubblicazione Web sicura](https://go.microsoft.com/fwlink/?LinkId=75182).  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
