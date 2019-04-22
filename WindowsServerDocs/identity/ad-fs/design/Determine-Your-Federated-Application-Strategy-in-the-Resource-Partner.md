---
ms.assetid: 9eab8c43-a0f2-4d19-a5a4-e1399f0d5f25
title: Determinare la strategia per le applicazioni federate nel partner risorse
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: aca47658cc5a20f63dbd59a26ebe135dd04def92
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59811932"
---
# <a name="determine-your-federated-application-strategy-in-the-resource-partner"></a>Determinare la strategia per le applicazioni federate nel partner risorse

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Una parte importante della progettazione di un nuovo Active Directory Federation Services \(ADFS\) infrastruttura nell'organizzazione partner risorse consiste nel determinare il set completo di applicazioni e servizi che verranno usati per partecipare il federazione e quali partner account saranno i destinatari di tali risorse. Prima di progettare una strategia per applicazioni e servizi federati, prendere in considerazione le domande seguenti:  
  
-   Verrà è l'abilitazione e la distribuzione di un'applicazione ASP.NET o un Windows Communication Foundation \(WCF\) servizio per la federazione?  
  
-   Gli utenti della rete aziendale dovranno accedere all'applicazione o al servizio federato tramite l'autenticazione integrata di Windows?  
  
-   L'applicazione o il servizio federato verrà usato dagli utenti nella rete perimetrale? In tal caso, sarà necessaria l'autenticazione integrata di Windows?  
  
-   Tutti i server Web che ospitano applicazioni federate eseguono un sistema operativo Windows Server e Internet Information Services \(IIS\)?  
  
-   Per chi l'applicazione o il servizio federato fornirà le risorse?  
  
Rispondendo a queste domande consentirà di pianificare una progettazione di AD FS a tinta unita. Le risposte saranno anche utili per creare una strategia per applicazioni e servizi federati conveniente in termini di costi e di risorse. Per altre informazioni sulla progettazione della strategia più appropriata per applicazioni e servizi federati, vedere gli argomenti seguenti in questa guida:  
  
-   [Fornire agli utenti di Active Directory accesso ai servizi e alle applicazioni in grado di riconoscere attestazioni](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
-   [Fornire l'accesso agli utenti di Active Directory per le applicazioni e servizi di altre organizzazioni](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)  
  
-   [Fornire agli utenti in un'altra organizzazione l'accesso ai servizi e alle applicazioni in grado di riconoscere attestazioni](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
Per altre informazioni su come creare un attestazioni\-applicazione compatibile con ASP.NET o un servizio WCF, vedere [Windows Identity Foundation SDK](https://go.microsoft.com/fwlink/?LinkId=122266).  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

