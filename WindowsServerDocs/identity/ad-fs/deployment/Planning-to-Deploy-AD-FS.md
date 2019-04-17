---
ms.assetid: c87dc32d-ab33-44d2-a46f-f9f878b4e5b4
title: Pianificazione della distribuzione di ADFS
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: ca9e53d7d98f3ae5e6b7b329e52d4979e8c10215
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="planning-to-deploy-ad-fs"></a>Pianificazione della distribuzione di ADFS

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


Dopo aver raccolto informazioni sull'ambiente e aver scelto una progettazione di Active Directory Federation Services \(AD FS\) seguendo le indicazioni fornite nel [Guida alla progettazione di AD FS in Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx), è possibile iniziare a pianificare la distribuzione della progettazione di AD FS dell'organizzazione. Con il progetto completato e le informazioni contenute in questo argomento, è possibile determinare quali attività eseguire per distribuire ADFS nell'organizzazione.  
  
## <a name="reviewing-your-ad-fs-design"></a>Revisione della progettazione di ADFS  
Se il team di progettazione che ha realizzato ADFS originale di progettazione per l'organizzazione è diverso dal team di distribuzione che verranno effettivamente implementare la distribuzione, assicurarsi che il team rivedano insieme tutto il progetto definitivo con il team di progettazione. Rivedere i punti seguenti del progetto:  
  
-   Strategia del team di progettazione per determinare la topologia fisica ottimale per il posizionamento dei server federativi nella rete aziendale o rete perimetrale. Il team di distribuzione è possibile fare riferimento alla documentazione su questo tema consultando gli argomenti seguenti nella Guida alla progettazione di ADFS:  
  
    -   [Il ruolo del Database di configurazione di ADFS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)  
  
    -   [Pianificazione del posizionamento del Server federativo](https://technet.microsoft.com/library/dd807069.aspx)  
  
    -   [Pianificazione del posizionamento del Proxy Server federativo](https://technet.microsoft.com/library/dd807130.aspx)  
  
    È possibile che il team di progettazione abbia lasciato la definizione del server federativo o del posizionamento del proxy server federativo al team di distribuzione. Il team di distribuzione è quindi responsabile della documentazione e implementazione della topologia fisica dei server.  
  
-   I motivi aziendali per la designazione dell'organizzazione come un provider di attestazioni, relying party o entrambi, nell'ambito del progetto di ADFS documentato. Assicurarsi che i membri del team di distribuzione conoscano i motivi per cui viene distribuito AD FS e altre società o organizzazioni sono coinvolte nella partnership di federazione. Assicurarsi che i membri del team di distribuzione conoscano inoltre i vincoli esistenti per altre società o organizzazioni \ (limitata di hardware, nessun ambiente extranet e in tal caso forth\) che potrebbero limitare l'ambito del progetto in qualche modo. Per ulteriori informazioni sulle organizzazioni partner, vedere [pianificazione della distribuzione](https://technet.microsoft.com/library/dd807083.aspx).  
  
Dopo la progettazione team e il team di distribuzione trovato un accordo su questi problemi, potranno procedere con la distribuzione della progettazione di ADFS. Per ulteriori informazioni, vedere [implementazione del piano di progettazione di AD FS](Implementing-Your-AD-FS-Design-Plan.md).  
