---
ms.assetid: bb16e39d-566d-436c-b957-394c06d556db
title: Guida alla progettazione di AD FS in Windows Server 2012
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3f2a6df6a9c9a5cbdfa9c64bc6521e92f4982a15
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191729"
---
# <a name="ad-fs-design-guide-in-windows-server"></a>Guida alla progettazione di AD FS in Windows Server 


  
> [!NOTE]  
> Per informazioni su come distribuire ADFS in Windows Server 2012 R2, vedere [Guida alla distribuzione di Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md).  
  
È possibile usare Active Directory® Federation Services \(ADFS\) con Windows Server® 2012 il sistema operativo di una federazione dei servizi ruolo di provider per autenticare facilmente gli utenti in qualsiasi Web\-servizi basati su o applicazioni che risiedono in un'organizzazione partner risorse, senza la necessità per gli amministratori di creare o mantenere trust esterni o trust di foresta tra le reti di entrambe le organizzazioni e senza la necessità per gli utenti accedere a una seconda volta. Il processo di autenticazione a una rete accedendo alle risorse in un'altra rete, ovvero senza l'onere di ripetute azioni di accesso da parte degli utenti, è nota come l'accesso single sign\-sul \(SSO\).  
  
## <a name="about-this-guide"></a>Informazioni sulla guida  
Questa guida vengono fornite indicazioni che consentono di pianificare una nuova distribuzione di AD FS, in base ai requisiti dell'organizzazione \(detta anche in questa guida gli obiettivi di distribuzione\) e alla progettazione specifica che si desidera creare. Questa guida è destinata a uno specialista di infrastrutture o un progettista del sistema. Evidenzia i punti decisionali principali si pianifica la distribuzione di AD FS. Prima di leggere questa Guida, è necessario avere una buona conoscenza del funzionamento di ADFS a livello funzionale. È anche deve avere una buona conoscenza dei requisiti dell'organizzazione che sarà rispecchiata nella progettazione di ADFS.  
  
Questa guida descrive una serie di obiettivi di distribuzione che si basano su tre progettazioni di AD FS primarie, e consente di decidere la progettazione più adatta per l'ambiente. È possibile usare questi obiettivi di distribuzione per creare uno dei progetti di ADFS seguenti completa o un progetto personalizzato che soddisfi le esigenze dell'ambiente:  
  
-   Federated Web SSO per il supporto commerciale\-al\-business \(B2B\) scenari e supportare la collaborazione tra business unit con foreste indipendenti  
  
-   Web SSO per supportare l'accesso dei clienti alle applicazioni in ambito aziendale\-al\-consumer \(B2C\) scenari  
  
Per ogni progetto, sono disponibili linee guida per raccogliere i dati necessari sull'ambiente. È quindi possibile usare queste linee guida per pianificare e progettare la distribuzione di AD FS. Dopo aver letto questa Guida e terminare la raccolta, documentazione e il mapping ai requisiti dell'organizzazione, sarà necessario le informazioni necessarie per iniziare la distribuzione di ADFS sulla base delle indicazioni di [Windows Server 2012 AD FS Deployment Guide](../../ad-fs/deployment/Windows-Server-2012-AD-FS-Deployment-Guide.md).  
  
## <a name="in-this-guide"></a>Contenuto della guida  
  
-   [Identificazione degli obiettivi di distribuzione di AD FS](Identifying-Your-AD-FS-Deployment-Goals.md)  
  
-   [Mapping degli obiettivi di distribuzione a un progetto di AD FS](Mapping-Your-Deployment-Goals-to-an-AD-FS-Design.md)  
  
-   [Determinare la topologia di distribuzione di AD FS](Determine-Your-AD-FS-Deployment-Topology.md)  
  
-   [Pianificazione della distribuzione](Planning-Your-Deployment.md)  
  
-   [Pianificazione del posizionamento dei server federativi](Planning-Federation-Server-Placement.md)  
  
-   [Pianificazione del posizionamento dei proxy server federativi](Planning-Federation-Server-Proxy-Placement.md)  
  
-   [Pianificazione della capacità per i server AD FS](Planning-for-AD-FS-Server-Capacity.md)  
  
-   [Appendice A: Verifica dei requisiti di AD FS](Appendix-A--Reviewing-AD-FS-Requirements.md)  
  

