---
ms.assetid: f88238ea-d851-4129-8b4e-a3a62b813614
title: Rivedere il ruolo del server federativo nel partner risorse
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 8cda394800bf0554e00e2897d240e2c08a72a00b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858554"
---
# <a name="review-the-role-of-the-federation-server-in-the-resource-partner"></a>Rivedere il ruolo del server federativo nel partner risorse

Il server federativo nell'organizzazione partner risorse intercetta i token di sicurezza in ingresso inviati da un server federativo di account, li convalida e li firma e quindi rilascia i propri token di sicurezza destinati all'applicazione basata su Web\-.  
  
> [!NOTE]  
> Quando gli utenti federati usano i Web browser per accedere alle applicazioni basate su Web\-, il server federativo nell'organizzazione partner risorse compila un nuovo cookie di autenticazione e lo scrive nel browser. Questo cookie Abilita l'accesso Single\-\-sulle funzionalità di\) \(SSO in modo che gli utenti non debbano accedere di nuovo al server federativo nel partner account quando gli utenti tentano di accedere a diverse applicazioni basate su Web\-nel partner risorse.  
  
Nel progetto Web SSO è necessario installare almeno un server federativo nella rete perimetrale. Nel progetto SSO Web federativo deve essere installato almeno un server federativo nella rete aziendale dell'organizzazione partner account e almeno un server federativo installato nella rete aziendale dell'organizzazione partner risorse.  
  
> [!NOTE]  
> Prima di poter configurare un computer server federativo nell'organizzazione partner risorse, è innanzitutto necessario aggiungere il computer a qualsiasi dominio Active Directory nell'organizzazione partner risorse. Per altre informazioni, vedere [Checklist: Setting Up a Federation Server](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
## <a name="see-also"></a>Vedi anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

