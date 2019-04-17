---
ms.assetid: 09f335bb-896a-45dd-adc2-f215b8fba828
title: Progetto Web SSO federativo
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b85f49ac0556bf9b3542a23514d7fcbf82d2d88e
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="federated-web-sso-design"></a>Progetto Web SSO federativo

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La progettazione \(SSO\) Single\-Sign\-On Web federativo in Active Directory Federation Services \(AD FS\) implica una comunicazione protetta che si estende su più firewall, reti perimetrali e server di risoluzione name\, oltre all'intera infrastruttura di routing Internet.  
  
In genere, questa soluzione viene usata quando due organizzazioni si accordano per creare una relazione di trust federativa per consentire agli utenti di un'organizzazione \ (l'account partner organization\) per accedere ad applicazioni basate sul Web o servizi, che sono protetti da ADFS, all'interno dell'organizzazione \ (organization\ il partner risorse).  
  
In altre parole, una relazione di trust federativa è la rappresentazione di un contratto business\ o relazione tra due organizzazioni. Come illustrato nella figura seguente, è possibile stabilire una relazione di trust federativa tra due aziende, determinando in uno scenario federativo end-to-end.  
  
![sso web federativo](media/adfs2_FederatedWebSSODesign.gif)  
  
Freccia vie base nell'illustrazione indica la direzione della federazione attendibile, che, come la direzione del trust Windows, punta sempre al lato account della foresta. Ciò significa che l'autenticazione va dall'organizzazione partner account per l'organizzazione partner risorse.  
  
In questa progettazione di Web SSO federativo, due server federativi \ (uno in Fabrikam e l'altra in Contoso \) indirizzare le richieste di autenticazione dagli account utente in Fabrikam ad applicazioni basata sul Web o servizi in Contoso.  
  
> [!NOTE]  
> Per una maggiore sicurezza, è possibile utilizzare i proxy server federativi per inoltrare le richieste per i server federativi che non sono direttamente accessibili da Internet.  
  
In questo esempio Fabrikam è il provider di identità o di account. La parte Fabrikam di Web SSO federativo utilizza l'obiettivo di distribuzione di ADFS seguenti:  
  
-   [Fornire agli utenti di Active Directory accesso alle applicazioni e servizi di altre organizzazioni](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)  
  
Contoso è il provider di risorse. La parte Contoso di Web SSO federativo raggiunge gli obiettivi di distribuzione di ADFS seguenti:  
  
-   [Fornire agli utenti in un'altra organizzazione l'accesso ai propri servizi e applicazioni in grado di riconoscere attestazioni](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
-   [Fornire agli utenti di Active Directory accesso ai propri servizi e applicazioni in grado di riconoscere attestazioni](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
Per un elenco di attività dettagliate che è possibile utilizzare per pianificare e distribuire la progettazione di Web SSO federativo, vedere [Checklist: Implementing a Federated Web SSO Design ](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md).  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di ADFS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
