---
ms.assetid: 9aaca9c5-ce44-495c-aad6-61aede87a83f
title: Distribuzione di AD FS nell'organizzazione partner account
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: bf21860603b3055c2ef2c9e7b77bb106eb06e238
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191617"
---
# <a name="deploying-ad-fs-in-the-account-partner-organization"></a>Distribuzione di AD FS nell'organizzazione partner account

Un partner account in Active Directory Federation Services \(ADFS\) rappresenta l'organizzazione nella relazione di trust federativa che archivia fisicamente gli account utente in un archivio di attributi supportati. Per altre informazioni su quale attributo archivi sono supportati, vedere [il ruolo degli archivi attributi](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md).  
  
Il server federativo nell'organizzazione partner account autentica gli utenti locali e crea i token di sicurezza utilizzati dal partner risorse per poter prendere decisioni di autorizzazione. È quindi in grado di registrarsi con il server federativo e usare i token emessi per l'autenticazione e controllo di accesso facilmente le relying party, ad esempio siti Web e servizi Web.  
  
Negli scenari in cui è necessario assegnare agli utenti con accesso a più applicazioni federate o servizi, ovvero quando ogni applicazione o il servizio è ospitato da un'altra organizzazione, è possibile configurare il server federativo del partner account in modo da poter distribuire più relying party.  
  
Per altre informazioni su come installare e configurare un'organizzazione partner account, vedere [elenco di controllo: Configurazione dell'organizzazione Partner Account](../../ad-fs/deployment/Checklist--Configuring-the-Account-Partner-Organization.md).  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
-   [Rivedere il ruolo del server federativo nel partner account](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md)  
  
-   [Rivedere il ruolo del proxy server federativo nel partner account](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Account-Partner.md)  
  
-   [Preparare i computer Client nel Partner Account](Prepare-Client-Computers-in-the-Account-Partner.md)  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
