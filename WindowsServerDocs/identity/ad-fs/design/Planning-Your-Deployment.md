---
ms.assetid: bb9b9e18-bf2f-4115-be77-9a165944db41
title: Pianificazione della distribuzione
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 607dc34c8f44d8d96a8dc0c9d1ed004edc799167
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407991"
---
# <a name="planning-your-deployment"></a>Pianificazione della distribuzione

Quando si pianifica la collaborazione tra @ no__t-0organizational \(federation @ no__t-2based @ no__t-3 con Active Directory Federation Services \(AD FS @ no__t-5, determinare innanzitutto se l'organizzazione ospiterà una risorsa Web a cui accedere da altri organizzazioni in Internet o se si fornirà l'accesso alla risorsa Web per i dipendenti dell'organizzazione. Questa determinazione influiscono sul modo in cui si distribuisce AD FS ed è fondamentale per la pianificazione dell'infrastruttura AD FS.  
  
> [!NOTE]  
> Assicurarsi che il ruolo dell'organizzazione nel contratto di federazione sia ben chiaro a tutte le parti.  
  
Per il [progetto SSO Web federativo](Federated-Web-SSO-Design.md), ad FS usa termini quali il *partner account* \(also definito provider di *identità* nello snap-in di gestione ad FS @ no__t-4in @ no__t-5 e il *partner risorse* \(also indicato come  *relying party* nello snap-in gestione ad FS @ no__t-9in @ no__t-10 per distinguere l'organizzazione che ospita gli account 1The account partner @ no__t-12 dall'organizzazione che ospita la risorsa Web @ no__t-13based risorse 4a partner @ no__t-15.  
  
Nel [Web SSO Design](Web-SSO-Design.md)l'organizzazione riveste entrambi i ruoli di partner account e di partner risorse, perché fornisce ai suoi utenti l'accesso alle sue applicazioni.  
  
Negli argomenti seguenti vengono illustrati alcuni dei concetti AD FS dell'organizzazione partner. Contengono inoltre collegamenti ad argomenti della Guida alla distribuzione di AD FS che contengono informazioni sulla configurazione e la configurazione di organizzazioni partner account e organizzazioni partner risorse in base agli obiettivi di distribuzione di AD FS.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
-   [Procedure consigliate per la pianificazione e la distribuzione sicure di AD FS](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)  
  
-   [Pianificazione per l'interoperabilità con AD FS 1.x](Planning-for-Interoperability-with-AD-FS-1.x.md)  
  
-   [Quando usare la delega di identità](When-to-Use-Identity-Delegation.md)  
  
-   [Distribuzione di AD FS nell'organizzazione partner account](Deploying-AD-FS-in-the-Account-Partner-Organization-2012.md)  
  
-   [Distribuzione di AD FS nell'organizzazione partner risorse](Deploying-AD-FS-in-the-Resource-Partner-Organization-2012.md)  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)


