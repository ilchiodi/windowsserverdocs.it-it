---
ms.assetid: 9eab8c43-a0f2-4d19-a5a4-e1399f0d5f25
title: Determinare la strategia per le applicazioni federate nel partner risorse
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 0cc7a9202813cd3f8d45a72305d13ad197f5b04d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359159"
---
# <a name="determine-your-federated-application-strategy-in-the-resource-partner"></a>Determinare la strategia per le applicazioni federate nel partner risorse

Una parte importante della progettazione di una nuova infrastruttura Active Directory Federation Services \(AD FS @ no__t-1 nell'organizzazione partner risorse consiste nel determinare il set completo di applicazioni e servizi che verranno usati per partecipare alla Federazione e quali partner account saranno i destinatari di tali risorse. Prima di progettare una strategia per applicazioni e servizi federati, prendere in considerazione le domande seguenti:  
  
-   Verrà abilitata e distribuita un'applicazione ASP.NET o un servizio Windows Communication Foundation \(WCF @ no__t-1 per la Federazione?  
  
-   Gli utenti della rete aziendale dovranno accedere all'applicazione o al servizio federato tramite l'autenticazione integrata di Windows?  
  
-   L'applicazione o il servizio federato verrà usato dagli utenti nella rete perimetrale? In tal caso, sarà necessaria l'autenticazione integrata di Windows?  
  
-   Tutti i server Web che ospitano applicazioni federate eseguono un sistema operativo Windows Server e Internet Information Services \(IIS @ no__t-1?  
  
-   Per chi l'applicazione o il servizio federato fornirà le risorse?  
  
Rispondere a queste domande consentirà di pianificare una progettazione di AD FS solida. Le risposte saranno anche utili per creare una strategia per applicazioni e servizi federati conveniente in termini di costi e di risorse. Per altre informazioni sulla progettazione della strategia più appropriata per applicazioni e servizi federati, vedere gli argomenti seguenti in questa guida:  
  
-   [Fornire agli utenti di Active Directory l'accesso ai servizi e alle applicazioni in grado di riconoscere attestazioni](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
-   [Fornire agli utenti di Active Directory l'accesso ai servizi e alle applicazioni di altre organizzazioni](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)  
  
-   [Fornire agli utenti di un'altra organizzazione l'accesso ai propri servizi e alle applicazioni in grado di riconoscere attestazioni](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
Per ulteriori informazioni su come creare un'applicazione di attestazioni @ no__t-0aware ASP.NET o un servizio WCF, vedere [Windows Identity Foundation SDK](https://go.microsoft.com/fwlink/?LinkId=122266).  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

