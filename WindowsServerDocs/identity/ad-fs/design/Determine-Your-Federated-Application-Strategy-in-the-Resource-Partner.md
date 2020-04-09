---
ms.assetid: 9eab8c43-a0f2-4d19-a5a4-e1399f0d5f25
title: Determinare la strategia per le applicazioni federate nel partner risorse
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b7b50d93ef09259cd6d1893eda4fd5c651b8e688
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853154"
---
# <a name="determine-your-federated-application-strategy-in-the-resource-partner"></a>Determinare la strategia per le applicazioni federate nel partner risorse

Una parte importante della progettazione di una nuova Active Directory Federation Services \(AD FS infrastruttura\) nell'organizzazione partner risorse consiste nel determinare il set completo di applicazioni e servizi che verranno usati per partecipare alla Federazione e quali partner account saranno i destinatari di tali risorse. Prima di progettare una strategia per applicazioni e servizi federati, prendere in considerazione le domande seguenti:  
  
-   Verrà abilitata e distribuita un'applicazione ASP.NET o un Windows Communication Foundation \(servizio WCF\) per la Federazione?  
  
-   Gli utenti della rete aziendale dovranno accedere all'applicazione o al servizio federato tramite l'autenticazione integrata di Windows?  
  
-   L'applicazione o il servizio federato verrà usato dagli utenti nella rete perimetrale? In tal caso, sarà necessaria l'autenticazione integrata di Windows?  
  
-   Tutti i server Web che ospitano applicazioni federate eseguono un sistema operativo Windows Server e Internet Information Services \(\)IIS?  
  
-   Per chi l'applicazione o il servizio federato fornirà le risorse?  
  
Rispondere a queste domande consentirà di pianificare una progettazione di AD FS solida. Le risposte saranno anche utili per creare una strategia per applicazioni e servizi federati conveniente in termini di costi e di risorse. Per altre informazioni sulla progettazione della strategia più appropriata per applicazioni e servizi federati, vedere gli argomenti seguenti in questa guida:  
  
-   [Fornire agli utenti di Active Directory l'accesso ai servizi e alle applicazioni in grado di riconoscere attestazioni](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
-   [Fornire agli utenti di Active Directory l'accesso ai servizi e alle applicazioni di altre organizzazioni](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)  
  
-   [Fornire agli utenti di un'altra organizzazione l'accesso ai propri servizi e alle applicazioni in grado di riconoscere attestazioni](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
Per ulteriori informazioni sulla creazione di attestazioni\-l'applicazione ASP.NET o il servizio WCF in grado di riconoscere le attestazioni, vedere [Windows Identity Foundation SDK](https://go.microsoft.com/fwlink/?LinkId=122266).  
  
## <a name="see-also"></a>Vedi anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

