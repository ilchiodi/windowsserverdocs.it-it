---
ms.assetid: 09f335bb-896a-45dd-adc2-f215b8fba828
title: Progetto SSO Web federativo
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 6a3e7eb6c42c8190da799c88c1e947e6aef1c29f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408103"
---
# <a name="federated-web-sso-design"></a>Progetto SSO Web federativo

Il progetto federativo Web single @ no__t-0Sign @ no__t-1Sulla Barra \(SSO @ no__t-3 Active Directory Federation Services \(AD FS @ no__t-5 implica una comunicazione sicura che si estende su più firewall, reti perimetrali e nome @ no__t-6resolution Server, oltre all'intera infrastruttura di routing Internet.  
  
Questa progettazione è utilizzata in genere, quando due organizzazioni accettano creare una relazione di trust federativa per consentire agli utenti di un'organizzazione \(organizzazione partner account\) per accedere alle Web\-basato su applicazioni o servizi, che sono protetti da ADFS, all'interno dell'organizzazione \(organizzazione partner risorse\).  
  
In altre parole, una relazione di trust federativa è la rappresentazione di un'azienda\-contratto o relazione tra due organizzazioni. Come illustrato nella figura seguente, è possibile stabilire una relazione di trust federativa tra due aziende, che comporta una fine\-a\-scenario federativo end.  
  
![sso web federativo](media/adfs2_FederatedWebSSODesign.gif)  
  
Quello\-freccia nell'illustrazione indica la direzione della federazione attendibile, che, come la direzione del trust Windows, ovvero fa sempre riferimento al lato account della foresta. Questo significa che la direzione del flusso dell'autenticazione va dall'organizzazione partner account all'organizzazione partner risorse.  
  
In questo progetto Web SSO federativo, due server federativi \(uno in Fabrikam e l'altra in Contoso\) indirizzare le richieste di autenticazione degli account utente Fabrikam Web\-basato su applicazioni o servizi in Contoso.  
  
> [!NOTE]  
> Per una maggiore sicurezza, è possibile utilizzare i proxy server federativi per le richieste di inoltro per i server federativi che non sono direttamente accessibili da Internet.  
  
In questo esempio Fabrikam è il provider di identità (o di account). La parte di Fabrikam di Web SSO federativo utilizza l'obiettivo di distribuzione di ADFS seguente:  
  
-   [Fornire agli utenti di Active Directory l'accesso ai servizi e alle applicazioni di altre organizzazioni](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)  
  
Contoso è il provider di risorse. La parte di Contoso di Web SSO federativo raggiunge gli obiettivi di distribuzione di ADFS seguenti:  
  
-   [Fornire agli utenti di un'altra organizzazione l'accesso ai propri servizi e alle applicazioni in grado di riconoscere attestazioni](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
-   [Fornire agli utenti di Active Directory l'accesso ai servizi e alle applicazioni in grado di riconoscere attestazioni](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
Per un elenco delle attività dettagliate che è possibile usare per pianificare e distribuire il progetto Web SSO federativo, vedere [Checklist: Implementazione di un progetto Web SSO federativo @ no__t-0.  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
