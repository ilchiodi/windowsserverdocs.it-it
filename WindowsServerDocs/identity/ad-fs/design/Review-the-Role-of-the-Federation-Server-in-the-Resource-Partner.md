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
ms.openlocfilehash: b2ed7a09bbc50c83d3bf6f8f2688152ed5202abc
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190825"
---
# <a name="review-the-role-of-the-federation-server-in-the-resource-partner"></a>Rivedere il ruolo del server federativo nel partner risorse

Il server federativo nell'organizzazione partner risorse intercetta i token di sicurezza in ingresso inviati da un server federativo di account, convalida e li firma e quindi rilascia i propri token di sicurezza destinati a Web\-basato su applicazione.  
  
> [!NOTE]  
> Quando gli utenti federati usano i Web browser per accedere alle Web\-le applicazioni basate su, il server federativo nell'organizzazione partner risorse crea un nuovo cookie di autenticazione e lo scrive nel browser. Questo cookie consente single\-sign\-sul \(SSO\) funzionalità in modo che gli utenti non debbano accedere di nuovo al server federativo nel partner account quando provano ad accedere a vari Web\- applicazioni basate su nel partner risorse.  
  
Nella progettazione Web SSO federativo almeno un server deve essere installato nella rete perimetrale. Nella progettazione di Web SSO federativo, deve essere presente almeno un server federativo installato nella rete aziendale dell'organizzazione partner account e almeno un server federativo installato nella rete aziendale dell'organizzazione partner risorse.  
  
> [!NOTE]  
> Prima di impostare backup di un computer server federativo nell'organizzazione partner risorse, è innanzitutto necessario aggiungere il computer in un dominio di Active Directory nell'organizzazione partner risorse. Per ulteriori informazioni, vedere [Elenco di controllo: Configurazione di un Server federativo](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

