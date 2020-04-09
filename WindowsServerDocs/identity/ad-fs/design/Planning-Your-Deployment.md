---
ms.assetid: bb9b9e18-bf2f-4115-be77-9a165944db41
title: Pianificazione della distribuzione
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 6386aac112fcf936ccdd9772e3d5566d8dd21ad8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858644"
---
# <a name="planning-your-deployment"></a>Pianificazione della distribuzione

Quando si pianifica la collaborazione tra\-\(di Federazione\-basata\) usando Active Directory Federation Services \(ad FS\), determinare prima di tutto se l'organizzazione ospiterà una risorsa Web a cui accedere da altre organizzazioni in Internet o se si fornirà l'accesso alla risorsa Web per i dipendenti dell'organizzazione. Questa determinazione influiscono sul modo in cui si distribuisce AD FS ed è fondamentale per la pianificazione dell'infrastruttura AD FS.  
  
> [!NOTE]  
> Assicurarsi che il ruolo dell'organizzazione nel contratto di federazione sia ben chiaro a tutte le parti.  
  
Per la soluzione di accesso Single Sign-on [Web federativo](Federated-Web-SSO-Design.md), ad FS usa termini quali il *partner account* \(detto anche *provider di identità* nello snap-in di gestione ad FS\-in\) e il *partner risorse* \(relying party ad FS del partner risorse *\-in\)* per distinguere l'organizzazione che ospita gli account \(\) del partner account\-dall'organizzazione che ospita le risorse basate su \(Web\).  
  
Nel [Web SSO Design](Web-SSO-Design.md)l'organizzazione riveste entrambi i ruoli di partner account e di partner risorse, perché fornisce ai suoi utenti l'accesso alle sue applicazioni.  
  
Negli argomenti seguenti vengono illustrati alcuni dei concetti AD FS dell'organizzazione partner. Contengono inoltre collegamenti ad argomenti della Guida alla distribuzione di AD FS che contengono informazioni sulla configurazione e la configurazione di organizzazioni partner account e organizzazioni partner risorse in base agli obiettivi di distribuzione di AD FS.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
-   [Procedure consigliate per la pianificazione e la distribuzione sicure di AD FS](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)  
  
-   [Pianificazione per l'interoperabilità con AD FS 1.x](Planning-for-Interoperability-with-AD-FS-1.x.md)  
  
-   [Quando usare la delega di identità](When-to-Use-Identity-Delegation.md)  
  
-   [Distribuzione di AD FS nell'organizzazione partner account](Deploying-AD-FS-in-the-Account-Partner-Organization-2012.md)  
  
-   [Distribuzione di AD FS nell'organizzazione partner risorse](Deploying-AD-FS-in-the-Resource-Partner-Organization-2012.md)  
  
## <a name="see-also"></a>Vedi anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)


