---
ms.assetid: bb9b9e18-bf2f-4115-be77-9a165944db41
title: Pianificazione della distribuzione
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 5c7cec9ad92605f3dc98f8ce8fb7853a7ae61299
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="planning-your-deployment"></a>Pianificazione della distribuzione

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Quando si pianifica la collaborazione \(federation\-based\) cross\ organizzazioni utilizzando Active Directory Federation Services \(AD FS\), determinare innanzitutto se la propria organizzazione ospiterà una risorsa Web a cui devono accedere altre organizzazioni tramite Internet o se si fornirà l'accesso alla risorsa Web per i dipendenti nell'organizzazione. Questo aspetto influisce come distribuire ADFS ed è determinante per la pianificazione dell'infrastruttura di ADFS.  
  
> [!NOTE]  
> Assicurarsi che il ruolo dell'organizzazione nel contratto di federazione sia ben chiaro a tutte le parti.  
  
Per il [Federated Web SSO Design](Federated-Web-SSO-Design.md), ADFS Usa termini come *partner account* \ (detta anche *provider di identità* nella gestione di ADFS snap-attive) e *partner risorse* \ (detta anche *relying party* nella gestione di ADFS snap-attive) per distinguere l'organizzazione che ospita gli account \(the account partner\) dall'organizzazione che ospita le risorse basata sul Web \(the resource partner\).  
  
Nel [progettazione di Web SSO](Web-SSO-Design.md), l'organizzazione riveste entrambi i partner e risorse ruoli account partner perché fornisce ai suoi utenti con accesso alle sue applicazioni.  
  
Gli argomenti seguenti illustrano che alcuni di AD FS concetti relativi alle organizzazioni di partner. Contengono anche collegamenti ad argomenti della Guida alla distribuzione di ADFS che contengono informazioni sull'installazione e configurazione di organizzazioni partner account e le organizzazioni partner risorse in base agli obiettivi di distribuzione di ADFS.  
  
## <a name="in-this-section"></a>In questa sezione  
  
-   [Procedure consigliate per la pianificazione sicuro e distribuzione di ADFS](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)  
  
-   [Pianificazione per l'interoperabilità con AD FS 1. x](Planning-for-Interoperability-with-AD-FS-1.x.md)  
  
-   [Quando usare la delega dell'identità](When-to-Use-Identity-Delegation.md)  
  
-   [Distribuzione di ADFS nell'organizzazione Partner Account](Deploying-AD-FS-in-the-Account-Partner-Organization-2012.md)  
  
-   [Distribuzione di ADFS nell'organizzazione Partner risorse](Deploying-AD-FS-in-the-Resource-Partner-Organization-2012.md)  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di ADFS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)


