---
ms.assetid: bb16e39d-566d-436c-b957-394c06d556db
title: Guida alla progettazione di ADFS in Windows Server 2012
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e660c1dabcc5a683fa74068ea148fd4efbeee569
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="ad-fs-design-guide-in-windows-server-2012"></a>Guida alla progettazione di ADFS in Windows Server 2012

>Si applica a: Windows Server 2012
  
> [!NOTE]  
> Per informazioni su come distribuire ADFS in Windows Server 2012 R2, vedere [Guida alla distribuzione di Windows Server 2012 R2 AD ADFS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md).  
  
È possibile utilizzare Active Directory® Federation Services \(AD FS\) con il sistema operativo Windows Server® 2012 in un ruolo di provider di servizi federativi per autenticare facilmente gli utenti a qualsiasi basata sul Web di servizi o applicazioni che risiedono in un'organizzazione partner risorse, senza la necessità per gli amministratori di creare o gestire i trust esterni o trust tra foreste tra le reti di entrambe le organizzazioni e senza la necessità per gli utenti di accedere a una seconda volta. Il processo di autenticazione a una rete durante l'accesso alle risorse in un'altra rete, senza l'onere di ripetute azioni di accesso da parte degli utenti, è noto come singolo \(SSO\) accesso-on.  
  
## <a name="about-this-guide"></a>Informazioni sulla Guida  
Questa guida vengono fornite indicazioni che consentono di pianificare una nuova distribuzione di ADFS, in base ai requisiti dell'organizzazione \ (denominato anche in questa guida come distribuzione goals\) e la progettazione specifica che si desidera creare. Questa guida è destinata per l'uso da un architetto di sistema o uno specialista dell'infrastruttura. Evidenzia i punti principali decisioni da prendere quando si pianifica la distribuzione di ADFS. Prima di leggere questa Guida, è una buona comprensione del funzionamento di AD FS in un livello di funzionalità. Hai anche una buona conoscenza dei requisiti dell'organizzazione che si rifletteranno nella progettazione di ADFS.  
  
Questa guida descrive una serie di obiettivi di distribuzione basati su tre progetti ADFS primari e consente di decidere la progettazione più adatta per l'ambiente. È possibile utilizzare questi obiettivi di distribuzione per creare uno dei progetti di ADFS seguenti completi o un progetto personalizzato che soddisfi le esigenze dell'ambiente:  
  
-   SSO Web federativo per supportare scenari \(B2B\) business\-da destra-business e per supportare la collaborazione tra business unit con foreste indipendenti  
  
-   Web SSO per supportare l'accesso dei clienti alle applicazioni in scenari \(B2C\) business\-da destra-consumer  
  
Per ogni progetto, sono disponibili linee guida per raccogliere i dati necessari sull'ambiente. È quindi possibile utilizzare queste linee guida per pianificare e progettare la distribuzione di ADFS. Dopo aver leggere questa Guida e completare la raccolta, la documentazione e il mapping dei requisiti dell'organizzazione, hai le informazioni necessarie per iniziare la distribuzione di ADFS sulla base delle indicazioni di [Guida alla distribuzione di Windows Server 2012 AD ADFS](../../ad-fs/deployment/Windows-Server-2012-AD-FS-Deployment-Guide.md).  
  
## <a name="in-this-guide"></a>In questa Guida  
  
-   [Identificazione degli obiettivi di distribuzione di AD FS](Identifying-Your-AD-FS-Deployment-Goals.md)  
  
-   [Mapping degli obiettivi di distribuzione per un progetto di ADFS](Mapping-Your-Deployment-Goals-to-an-AD-FS-Design.md)  
  
-   [Determinare la topologia di distribuzione AD FS](Determine-Your-AD-FS-Deployment-Topology.md)  
  
-   [Pianificazione della distribuzione](Planning-Your-Deployment.md)  
  
-   [Pianificazione del posizionamento del Server federativo](Planning-Federation-Server-Placement.md)  
  
-   [Pianificazione del posizionamento del Proxy Server federativo](Planning-Federation-Server-Proxy-Placement.md)  
  
-   [Pianificazione della capacità per i Server ADFS](Planning-for-AD-FS-Server-Capacity.md)  
  
-   [Appendice a: revisione AD requisiti per ADFS](Appendix-A--Reviewing-AD-FS-Requirements.md)  
  

