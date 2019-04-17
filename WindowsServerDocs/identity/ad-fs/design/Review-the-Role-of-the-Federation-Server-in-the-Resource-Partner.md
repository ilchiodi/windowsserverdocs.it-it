---
ms.assetid: f88238ea-d851-4129-8b4e-a3a62b813614
title: Rivedere il ruolo del Server federativo nel Partner risorse
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6d4c7763d7204dd2340d10a234e48f47c96788dc
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="review-the-role-of-the-federation-server-in-the-resource-partner"></a>Rivedere il ruolo del Server federativo nel Partner risorse

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Il server federativo in risorse partner dell'organizzazione intercettazioni in ingresso token di sicurezza vengono inviati da un server federativo di account, convalida e li firma e quindi rilascia il proprio token di sicurezza destinati per l'applicazione basata sul Web.  
  
> [!NOTE]  
> Quando gli utenti federati usano i Web browser per accedere alle applicazioni basate sul Web, il server federativo nell'organizzazione partner risorse crea un nuovo cookie di autenticazione e la scrive nel browser. Questo cookie consente l'accesso in apartment \(SSO\) funzionalità in modo che gli utenti non debbano accedere di nuovo al server federativo nel partner account quando gli utenti tentano di accedere alle diverse applicazioni basata sul Web nel partner risorse.  
  
Nella progettazione Web SSO federativo almeno un server deve essere installato nella rete perimetrale. Nella progettazione Web SSO federativo deve essere presente almeno un server federativo installato nella rete aziendale dell'organizzazione partner account e almeno un server federativo installato nella rete aziendale dell'organizzazione partner risorse.  
  
> [!NOTE]  
> Prima di impostare un server federativo nell'organizzazione partner risorse, è innanzitutto necessario aggiungere il computer in un dominio di Active Directory nell'organizzazione partner risorse. Per ulteriori informazioni, vedere [Checklist: Setting Up a Federation Server](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di ADFS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

