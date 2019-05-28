---
ms.assetid: bb9b9e18-bf2f-4115-be77-9a165944db41
title: Pianificazione della distribuzione
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0206197b24f13d80019cbc864057e99e195ebc4b
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191142"
---
# <a name="planning-your-deployment"></a>Pianificazione della distribuzione

Quando si pianifica la cross\-dell'organizzazione \(federation\-basata\) collaborazione usando Active Directory Federation Services \(ADFS\), determinare innanzitutto se l'organizzazione ospiterà una risorsa Web a cui accedere da altre organizzazioni su Internet o se si intende fornire l'accesso alla risorsa Web per i dipendenti dell'organizzazione. Questo aspetto influisce sul modo in cui si distribuisce ADFS ed è determinante per la pianificazione dell'infrastruttura AD FS.  
  
> [!NOTE]  
> Assicurarsi che il ruolo dell'organizzazione nel contratto di federazione sia ben chiaro a tutte le parti.  
  
Per il [Federated Web SSO Design](Federated-Web-SSO-Design.md), AD FS Usa, ad esempio termini *partner account* \(noto anche come *provider di identità* nello snap di gestione di AD FS\-nelle\) e *partner risorse* \(noto anche come *relying party* nello snap di gestione di AD FS\-in\) a distinguere l'organizzazione che ospita gli account \(partner account\) dall'organizzazione che ospita Web\-risorse basate su \(partner risorse\).  
  
Nel [Web SSO Design](Web-SSO-Design.md)l'organizzazione riveste entrambi i ruoli di partner account e di partner risorse, perché fornisce ai suoi utenti l'accesso alle sue applicazioni.  
  
Gli argomenti seguenti illustrano che alcune di AD FS concetti di organizzazione del partner. Contengono anche collegamenti ad argomenti della Guida alla distribuzione di AD FS che contengono informazioni sull'impostazione e configurazione di organizzazioni partner account e le organizzazioni partner risorse in base agli obiettivi di distribuzione di AD FS.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
-   [Procedure consigliate per la pianificazione e la distribuzione sicure di AD FS](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)  
  
-   [Pianificazione per l'interoperabilità con AD FS 1.x](Planning-for-Interoperability-with-AD-FS-1.x.md)  
  
-   [Quando usare la delega dell'identità](When-to-Use-Identity-Delegation.md)  
  
-   [Distribuzione di AD FS nell'organizzazione partner account](Deploying-AD-FS-in-the-Account-Partner-Organization-2012.md)  
  
-   [Distribuzione di AD FS nell'organizzazione partner risorse](Deploying-AD-FS-in-the-Resource-Partner-Organization-2012.md)  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)


