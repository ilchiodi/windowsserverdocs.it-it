---
ms.assetid: aa20c8b3-7f01-4165-8b73-92642bff9676
title: Quando creare un proxy server federativo
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 13ca7d49e8044273c9f40e58c06e705e758c68bf
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858484"
---
# <a name="when-to-create-a-federation-server-proxy"></a>Quando creare un proxy server federativo

La creazione di un proxy server federativo nell'organizzazione consente di aggiungere ulteriori livelli di sicurezza al Active Directory Federation Services \(AD FS distribuzione\). Prendere in considerazione la distribuzione di un proxy server federativo nella rete perimetrale dell'organizzazione quando si desidera:  
  
-   Impedire ai computer client esterni di accedere direttamente ai server federativi. Distribuendo un proxy server federativo nella rete perimetrale, è possibile isolare in modo efficace i server federativi in modo che siano accessibili solo ai computer client che sono connessi alla rete aziendale tramite proxy server federativi, che agiscono per conto dei computer client esterni. I proxy server federativi non hanno accesso alle chiavi private usate per la generazione dei token. Per altre informazioni, vedere [Where to Place a Federation Server Proxy](Where-to-Place-a-Federation-Server-Proxy.md).  
  
-   Fornire un modo pratico per distinguere il segno\-in esperienza per gli utenti provenienti da Internet anziché gli utenti provenienti dalla rete aziendale tramite l'autenticazione integrata di Windows. Un proxy server federativo raccoglie le credenziali o i dettagli dell'area di autenticazione principale dai computer client Internet utilizzando l'individuazione di accesso, disconnessione e provider di identità \(homerealmdiscovery. aspx\) pagine archiviate nel proxy server federativo.  
  
    Al contrario, i computer client provenienti dalla rete aziendale si trovano in un'esperienza diversa, in base alla configurazione del server federativo. Il server federativo della rete aziendale è spesso configurato per l'autenticazione integrata di Windows, che fornisce un segno di\-in modo trasparente per gli utenti della rete aziendale.  
  
Il ruolo svolto da un proxy server federativo nell'organizzazione varia a seconda che si inserisca il proxy server federativo nell'organizzazione partner account o nell'organizzazione partner risorse. Ad esempio, quando un proxy server federativo si trova nella rete perimetrale del partner account, il suo ruolo consiste nel raccogliere le informazioni sulle credenziali utente dai client del browser. Quando un proxy server federativo si trova nella rete perimetrale del partner risorse, inoltra le richieste di token di sicurezza a un server federativo di risorsa e genera i token di sicurezza aziendali in risposta ai token di sicurezza forniti dai relativi partner account.  
  
Per altre informazioni, vedere [Review the Role of the Federation Server Proxy in the Account Partner](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Account-Partner.md) e [Review the Role of the Federation Server Proxy in the Resource Partner](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Resource-Partner.md)  
  
## <a name="how-to-create-a-federation-server-proxy"></a>Come creare un proxy server federativo  
È possibile creare un proxy server federativo utilizzando la configurazione guidata del proxy server federativo di AD FS o lo strumento da riga\-comando Fsconfig. exe. Per informazioni sulle procedure necessarie, vedere [Configure a Computer for the Federation Server Proxy Role](../../ad-fs/deployment/Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md).  
  
Per informazioni generali su come configurare tutti i prerequisiti necessari per distribuire un proxy server federativo, vedere [Checklist: Setting Up a Federation Server Proxy](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server-Proxy.md).  
  
## <a name="see-also"></a>Vedi anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
