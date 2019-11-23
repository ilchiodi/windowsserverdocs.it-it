---
ms.assetid: bb16e39d-566d-436c-b957-394c06d556db
title: Guida alla progettazione di AD FS in Windows Server 2012
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: d9d7ec6f4ff575d3aac30b7127e591b78f5ef49b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359215"
---
# <a name="ad-fs-design-guide-in-windows-server"></a>Guida alla progettazione di AD FS in Windows Server 


  
> [!NOTE]  
> Per informazioni su come distribuire AD FS in Windows Server 2012 R2, vedere la [Guida alla distribuzione di Windows server 2012 r2 ad FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md).  
  
È possibile utilizzare Active Directory® Federation Services \(AD FS\) con il sistema operativo Windows Server® 2012 in un ruolo di provider di servizi federativi per autenticare facilmente gli utenti in qualsiasi servizio o applicazione basata su\-Web che risiedono in un'organizzazione partner risorse. senza la necessità per gli amministratori di creare o gestire trust esterni o trust tra foreste tra le reti di entrambe le organizzazioni e senza che gli utenti debbano accedere una seconda volta. Il processo di autenticazione a una rete durante l'accesso alle risorse in un'altra rete, senza il carico di azioni ripetute di accesso da parte degli utenti, è noto come Single Sign\-in \(\)SSO.  
  
## <a name="about-this-guide"></a>Informazioni sulla guida  
In questa guida vengono forniti consigli per pianificare una nuova distribuzione di AD FS, in base ai requisiti dell'organizzazione \(indicati anche in questa guida come obiettivi di distribuzione\) e la progettazione specifica che si desidera creare. Questa guida è destinata a uno specialista di infrastrutture o un progettista del sistema. Vengono evidenziati i punti decisionali principali durante la pianificazione della distribuzione di AD FS. Prima di leggere questa guida, è opportuno conoscere il modo in cui AD FS funziona a livello funzionale. È inoltre necessario avere una conoscenza approfondita dei requisiti dell'organizzazione che verranno riflessi nel progetto AD FS.  
  
In questa guida viene descritto un set di obiettivi di distribuzione basati su tre progetti di AD FS principali che consentono di decidere la progettazione più appropriata per l'ambiente in uso. È possibile usare questi obiettivi di distribuzione per creare uno dei seguenti progetti di AD FS completi o una progettazione personalizzata che soddisfi le esigenze dell'ambiente:  
  
-   Web SSO federativo per supportare il\-aziendale per\-gli scenari di business \(B2B\) e per supportare la collaborazione tra business unit con foreste indipendenti  
  
-   Web SSO per supportare l'accesso dei clienti alle applicazioni nei\-aziendali per\-gli scenari di consumer \(B2C\)  
  
Per ogni progetto, sono disponibili linee guida per raccogliere i dati necessari sull'ambiente. È quindi possibile usare queste linee guida per pianificare e progettare la distribuzione di AD FS. Una volta letta questa guida e completato la raccolta, la documentazione e il mapping dei requisiti dell'organizzazione, saranno disponibili le informazioni necessarie per iniziare la distribuzione di AD FS seguendo le istruzioni riportate nella [Guida alla distribuzione di Windows Server 2012 ad FS](../../ad-fs/deployment/Windows-Server-2012-AD-FS-Deployment-Guide.md).  
  
## <a name="in-this-guide"></a>Contenuto della guida  
  
-   [Identificazione degli obiettivi di distribuzione di AD FS](Identifying-Your-AD-FS-Deployment-Goals.md)  
  
-   [Mapping degli obiettivi di distribuzione a un progetto di AD FS](Mapping-Your-Deployment-Goals-to-an-AD-FS-Design.md)  
  
-   [Determinare la topologia di distribuzione di AD FS](Determine-Your-AD-FS-Deployment-Topology.md)  
  
-   [Pianificazione della distribuzione](Planning-Your-Deployment.md)  
  
-   [Pianificazione del posizionamento dei server federativi](Planning-Federation-Server-Placement.md)  
  
-   [Pianificazione del posizionamento dei proxy server federativi](Planning-Federation-Server-Proxy-Placement.md)  
  
-   [Pianificazione della capacità per i server AD FS](Planning-for-AD-FS-Server-Capacity.md)  
  
-   [Appendice A: revisione dei requisiti di AD FS](Appendix-A--Reviewing-AD-FS-Requirements.md)  
  

