---
ms.assetid: 9eab8c43-a0f2-4d19-a5a4-e1399f0d5f25
title: Determinare la strategia di applicazioni federate nel Partner risorse
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: aca47658cc5a20f63dbd59a26ebe135dd04def92
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="determine-your-federated-application-strategy-in-the-resource-partner"></a>Determinare la strategia di applicazioni federate nel Partner risorse

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Una parte importante della progettazione di una nuova infrastruttura \(AD FS\) Active Directory Federation Services nell'organizzazione partner risorse consiste nel determinare il set completo di applicazioni e servizi che verrà usata per partecipare alla federazione e quali partner account saranno i destinatari di tali risorse. Prima di progettare una strategia di servizi e applicazioni federati, prendere in considerazione le domande seguenti:  
  
-   Verrà si abilitazione e la distribuzione di un'applicazione ASP.NET o un servizio Windows Communication Foundation \(WCF\) per la federazione?  
  
-   Gli utenti della rete azienda richiederanno l'accesso per l'applicazione federata o il servizio tramite autenticazione integrata di Windows?  
  
-   L'applicazione federata o il servizio da utilizzare per gli utenti nella rete perimetrale? Se in questo modo, sarà necessaria autenticazione integrata di Windows?  
  
-   Sono tutti i server Web che ospitano applicazioni federate eseguono un sistema operativo Windows Server e Internet Information Services \(IIS\)?  
  
-   Che l'applicazione federata o il servizio fornirà le risorse per?  
  
Rispondendo a queste domande consentirà di pianificare una progettazione di AD FS tinta unita. Sono molto utili per la creazione di un'applicazione federata e strategia servizi costi e risorse efficiente. Per ulteriori informazioni sulla progettazione la strategia di applicazioni e servizi federativa più appropriata per l'organizzazione, vedere gli argomenti seguenti in questa guida:  
  
-   [Fornire agli utenti di Active Directory accesso ai propri servizi e applicazioni in grado di riconoscere attestazioni](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
-   [Fornire agli utenti di Active Directory accesso alle applicazioni e servizi di altre organizzazioni](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)  
  
-   [Fornire agli utenti in un'altra organizzazione l'accesso ai propri servizi e applicazioni in grado di riconoscere attestazioni](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
Per ulteriori informazioni su come creare un'applicazione in grado di riconoscere claims\ ASP.NET o un servizio WCF, vedere [Windows Identity Foundation SDK](https://go.microsoft.com/fwlink/?LinkId=122266).  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di ADFS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

