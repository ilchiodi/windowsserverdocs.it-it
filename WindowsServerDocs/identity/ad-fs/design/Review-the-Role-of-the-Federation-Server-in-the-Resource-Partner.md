---
ms.assetid: f88238ea-d851-4129-8b4e-a3a62b813614
title: Rivedere il ruolo del server federativo nel partner risorse
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6d4c7763d7204dd2340d10a234e48f47c96788dc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841842"
---
# <a name="review-the-role-of-the-federation-server-in-the-resource-partner"></a>Rivedere il ruolo del server federativo nel partner risorse

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Il server federativo nell'organizzazione partner risorse intercetta i token di sicurezza in ingresso inviati da un server federativo di account, convalida e li firma e quindi rilascia i propri token di sicurezza destinati a Web\-basato su applicazione.  
  
> [!NOTE]  
> Quando gli utenti federati usano i Web browser per accedere alle Web\-le applicazioni basate su, il server federativo nell'organizzazione partner risorse crea un nuovo cookie di autenticazione e lo scrive nel browser. Questo cookie consente single\-sign\-sul \(SSO\) funzionalità in modo che gli utenti non debbano accedere di nuovo al server federativo nel partner account quando provano ad accedere a vari Web\- applicazioni basate su nel partner risorse.  
  
Nella progettazione Web SSO federativo almeno un server deve essere installato nella rete perimetrale. Nella progettazione di Web SSO federativo, deve essere presente almeno un server federativo installato nella rete aziendale dell'organizzazione partner account e almeno un server federativo installato nella rete aziendale dell'organizzazione partner risorse.  
  
> [!NOTE]  
> Prima di impostare backup di un computer server federativo nell'organizzazione partner risorse, è innanzitutto necessario aggiungere il computer in un dominio di Active Directory nell'organizzazione partner risorse. Per ulteriori informazioni, vedere [Elenco di controllo: Configurazione di un Server federativo](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

