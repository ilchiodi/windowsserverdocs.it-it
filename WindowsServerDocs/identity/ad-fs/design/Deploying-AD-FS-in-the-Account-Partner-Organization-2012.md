---
ms.assetid: 9aaca9c5-ce44-495c-aad6-61aede87a83f
title: Distribuzione di AD FS nell'organizzazione partner account
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 94446ddd8d33c18b6166870a34b65c997d1644ac
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853204"
---
# <a name="deploying-ad-fs-in-the-account-partner-organization"></a>Distribuzione di AD FS nell'organizzazione partner account

Un partner account in Active Directory Federation Services \(AD FS\) rappresenta l'organizzazione nella relazione di trust federativa che archivia fisicamente gli account utente in un archivio di attributi supportato. Per ulteriori informazioni sugli archivi di attributi supportati, vedere [ruolo degli archivi di attributi](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md).  
  
Il server federativo nell'organizzazione partner account autentica gli utenti locali e crea i token di sicurezza usati dal partner risorse per prendere decisioni relative alle autorizzazioni. Le relying party, ad esempio siti Web e servizi Web, possono quindi registrarsi facilmente con il server federativo e utilizzare i token emessi per l'autenticazione e il controllo di accesso.  
  
Negli scenari in cui è necessario fornire agli utenti l'accesso a più applicazioni o servizi federati, quando ogni applicazione o servizio è ospitato da un'organizzazione diversa, è possibile configurare il server federativo del partner account per poter distribuire più relying party.  
  
Per altre informazioni su come installare e configurare un'organizzazione partner account, vedere [Checklist: Configuring the Account Partner Organization](../../ad-fs/deployment/Checklist--Configuring-the-Account-Partner-Organization.md).  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
-   [Rivedere il ruolo del server federativo nel partner account](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md)  
  
-   [Rivedere il ruolo del proxy server federativo nel partner account](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Account-Partner.md)  
  
-   [Preparare i computer client nel partner account](Prepare-Client-Computers-in-the-Account-Partner.md)  
  
## <a name="see-also"></a>Vedi anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
