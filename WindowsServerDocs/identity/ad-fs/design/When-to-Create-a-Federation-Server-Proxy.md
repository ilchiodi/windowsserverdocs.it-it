---
ms.assetid: aa20c8b3-7f01-4165-8b73-92642bff9676
title: Quando creare un Proxy Server federativo
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1f0253dfb5a690371dae1a2bfcb6b7520077d473
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="when-to-create-a-federation-server-proxy"></a>Quando creare un Proxy Server federativo

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Creazione di un proxy server federativo nell'organizzazione consente di aggiungere ulteriori livelli di sicurezza per la distribuzione di Active Directory Federation Services \(AD FS\). Valutare la distribuzione di un proxy server federativo nella rete perimetrale dell'organizzazione quando si desidera:  
  
-   Impedire l'accesso diretto al server federativi di computer client esterni. Distribuendo un proxy server federativo nella rete perimetrale, isolare in modo efficace i server federativi in modo da essere accessibili solo dal computer client connessi alla rete aziendale tramite i proxy server federativi, che opera per conto dei computer client esterni. I proxy server federativi non hanno accesso alle chiavi private usate per produrre token. Per ulteriori informazioni, vedere [dove posizionare un Proxy Server federativo](Where-to-Place-a-Federation-Server-Proxy.md).  
  
-   Fornire un modo pratico per differenziare l'esperienza di accesso per gli utenti provenienti da Internet rispetto agli utenti provenienti dalla rete aziendale tramite l'autenticazione integrata di Windows. Un proxy server federativo raccoglie le credenziali o i dettagli dell'area di autenticazione dai computer client Internet tramite l'accesso, disconnessione e identità provider individuazione \(homerealmdiscovery.aspx\) pagine che vengono archiviate nel proxy server federativo.  
  
    Al contrario, i computer client provenienti da incontrano la rete aziendale un'esperienza diversa, in base alla configurazione del server federativo. Il server federativo alla rete aziendale è spesso configurato per l'autenticazione integrata di Windows, che offre un'esperienza di accesso trasparente per gli utenti della rete azienda.  
  
Il ruolo svolto un proxy server federativo nell'organizzazione dipende dalla scelta di posizionare il proxy server federativo nell'organizzazione partner account o nell'organizzazione partner risorse. Ad esempio, quando viene inserito un proxy server federativo nella rete perimetrale del partner account, il ruolo è per raccogliere le informazioni sulle credenziali utente da browser client. Quando viene inserito un proxy server federativo nella rete perimetrale del partner risorse, inoltra le richieste di token di sicurezza a un server federativo di risorsa e genera i token di sicurezza aziendali in risposta ai token di sicurezza forniti dai relativi partner account.  
  
Per ulteriori informazioni, vedere [rivedere il ruolo del Proxy Server federativo nel Partner Account](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Account-Partner.md) e [rivedere il ruolo del Proxy Server federativo nel Partner risorse](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Resource-Partner.md)  
  
## <a name="how-to-create-a-federation-server-proxy"></a>Come creare un proxy server federativo  
È possibile creare un proxy server federativo utilizzando la configurazione guidata di AD FS Federation Server Proxy o lo strumento della riga di comando Fsconfig.exe. Per istruzioni su come eseguire questa operazione, vedere [configurare un Computer per il ruolo di Proxy Server federativo](../../ad-fs/deployment/Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md).  
  
Per informazioni generali su come configurare tutti i prerequisiti necessari per distribuire un proxy server federativo, vedere [elenco di controllo: impostazione di un Server Proxy federativo](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server-Proxy.md).  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di ADFS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
