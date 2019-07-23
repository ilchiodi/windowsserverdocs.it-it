---
ms.assetid: aa20c8b3-7f01-4165-8b73-92642bff9676
title: Quando creare un proxy server federativo
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b41c2194940c85e39e5a3724f747dd12c2544259
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190641"
---
# <a name="when-to-create-a-federation-server-proxy"></a>Quando creare un proxy server federativo

Creazione di un proxy server federativo nell'organizzazione aggiunge ulteriori livelli di sicurezza per l'Active Directory Federation Services \(ADFS\) distribuzione. È consigliabile distribuire un proxy server federativo nella rete perimetrale dell'organizzazione quando si desidera:  
  
-   Impedire i computer client esterni che accedono direttamente ai server federativi. Tramite la distribuzione di un proxy server federativo nella rete perimetrale, isolare in modo efficace i server federativi, in modo che siano accessibili solo dai computer client connessi alla rete aziendale tramite il proxy server federativi, che agiscono per conto altrui i computer client esterni. I proxy server federativi non hanno accesso alle chiavi private usate per la generazione dei token. Per altre informazioni, vedere [Where to Place a Federation Server Proxy](Where-to-Place-a-Federation-Server-Proxy.md).  
  
-   Fornire un modo pratico per differenziare il segno\-nell'esperienza per gli utenti provenienti da Internet rispetto agli utenti provenienti dalla rete aziendale tramite l'autenticazione integrata di Windows. Un proxy server federativo raccoglie le credenziali o i dettagli dell'area di autenticazione dai computer client Internet tramite l'individuazione di provider di accesso, disconnessione e identità \(homerealmdiscovery. aspx\) pagine che vengono archiviate nella federazione proxy del server.  
  
    Al contrario, i computer client che provengono da rilevato la rete aziendale un'esperienza diversa, in base alla configurazione del server federativo. Il server federativo alla rete aziendale è spesso configurato per l'autenticazione integrata di Windows, che fornisce un'accesso trasparente\-nell'esperienza per gli utenti nella rete aziendale.  
  
Il ruolo svolto da un proxy server federativo nell'organizzazione varia a seconda di dove viene collocato il proxy server federativo nell'organizzazione partner account o nell'organizzazione partner risorse. Ad esempio, quando viene inserito un proxy server federativo nella rete perimetrale del partner account, il suo ruolo è raccogliere le informazioni sulle credenziali utente dai browser client. Quando viene inserito un proxy server federativo nella rete perimetrale del partner risorse, inoltra le richieste a un server federativo di risorsa di token di sicurezza e genera i token di sicurezza dell'organizzazione in risposta ai token di sicurezza forniti dal relativo partner account.  
  
Per altre informazioni, vedere [rivedere il ruolo del Proxy Server federativo nel Partner Account](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Account-Partner.md) e [rivedere il ruolo del Proxy Server federativo nel Partner risorse](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Resource-Partner.md)  
  
## <a name="how-to-create-a-federation-server-proxy"></a>Come creare un proxy server federativo  
È possibile creare un proxy server federativo utilizzando la configurazione guidata di AD FS Federation Server Proxy o il comando Fsconfig.exe\-strumento della riga. Per istruzioni su come eseguire questa operazione, vedere [configurare un Computer per il ruolo Proxy Server federativo](../../ad-fs/deployment/Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md).  
  
Per informazioni generali su come configurare tutti i prerequisiti necessari per distribuire un proxy server federativo, vedere [elenco di controllo: Configurazione di un Proxy Server federativo](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server-Proxy.md).  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
