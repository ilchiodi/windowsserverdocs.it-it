---
ms.assetid: 8c3536b7-d091-4ee6-ad04-24713f070862
title: Distribuzione di ADFS nell'organizzazione Partner Account
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 5b4ba00aa9fed1022d9c0137d05ac6240b44b276
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="deploying-ad-fs-in-the-account-partner-organization"></a>Distribuzione di ADFS nell'organizzazione Partner Account

>Si applica a: Windows Server 2016, Windows Server 2012 R2

Un partner account in Active Directory Federation Services \(AD FS\) rappresenta l'organizzazione nella relazione di trust federativa che archivia fisicamente gli account utente in un archivio di attributi supportato. Per ulteriori informazioni sull'attributo archivi sono supportati, vedere [il ruolo degli archivi attributi](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md).  
  
Il server federativo nell'organizzazione partner account autentica gli utenti locali e crea i token di sicurezza usati da partner risorse per prendere decisioni di autorizzazione. Ad esempio siti Web e i servizi Web sono in grado di registrarsi con il server federativo e utilizzare facilmente relying party emesso token per il controllo di accesso e autenticazione.  
  
Negli scenari in cui è necessario consentire agli utenti di accedere a più applicazioni federate o servizi, ovvero quando ogni applicazione o servizio è ospitato da un'altra organizzazione, è possibile configurare il server federativo del partner account in modo che è possibile distribuire più relying party.  
  
Per ulteriori informazioni su come installare e configurare un'organizzazione partner account, vedere [elenco di controllo: configurazione dell'organizzazione Partner Account](../../ad-fs/deployment/Checklist--Configuring-the-Account-Partner-Organization.md).  
  
## <a name="in-this-section"></a>In questa sezione  
  
-   [Rivedere il ruolo del Server federativo nel Partner Account](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md)  
  
-   [Rivedere il ruolo del Proxy Server federativo nel Partner Account](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Account-Partner.md)  
  
-   [Preparazione dei computer Client nel Partner Account](Prepare-Client-Computers-in-the-Account-Partner.md)  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di ADFS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
