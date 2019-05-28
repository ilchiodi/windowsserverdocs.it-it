---
ms.assetid: c87dc32d-ab33-44d2-a46f-f9f878b4e5b4
title: Pianificazione della distribuzione di AD FS
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 1459cade5071374ca39d453b9915a68e4bcfe539
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192045"
---
# <a name="planning-to-deploy-ad-fs"></a>Pianificazione della distribuzione di AD FS


Dopo aver raccolto informazioni sull'ambiente e aver scelto un Active Directory Federation Services \(ADFS\) seguendo le indicazioni fornite nel [Guida alla progettazione di AD FS in Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx), è possibile iniziare a pianificare la distribuzione della progettazione di AD FS dell'organizzazione. Con il progetto completato e le informazioni contenute in questo argomento, è possibile determinare quali attività eseguire per distribuire AD FS nell'organizzazione.  
  
## <a name="reviewing-your-ad-fs-design"></a>Revisione del progetto di ADFS  
Se il team di progettazione ADFS originale nella costruzione di progettazione per l'organizzazione è diverso dal team di distribuzione che verrà effettivamente implementerà la distribuzione, assicurarsi che il team di distribuzione esamina il progetto finale con il team di progettazione. Rivedere i punti seguenti del progetto:  
  
-   La strategia del team di progettazione per determinare la topologia fisica ottimale per il posizionamento dei server federativi nella rete aziendale o in quella perimetrale. Il team di distribuzione è possibile fare riferimento alla documentazione su questo tema consultando gli argomenti seguenti nella Guida alla progettazione di AD FS:  
  
    -   [Ruolo del database di configurazione di AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)  
  
    -   [Pianificazione del posizionamento dei server federativi](https://technet.microsoft.com/library/dd807069.aspx)  
  
    -   [Pianificazione del posizionamento dei proxy server federativi](https://technet.microsoft.com/library/dd807130.aspx)  
  
    È possibile che il team di progettazione abbia lasciato la definizione del posizionamento del server federativo o del proxy server federativo al team di distribuzione. Il team di distribuzione è quindi responsabile della documentazione e dell'implementazione della topologia fisica dei server.  
  
-   I motivi aziendali per la designazione dell'organizzazione come provider di attestazioni, relying party o entrambi, nell'ambito del progetto di ADFS documentato. Assicurarsi che i membri del team di distribuzione conoscano i motivi per cui viene distribuito AD FS e altre società o organizzazioni sono coinvolte nella partnership di federazione. Assicurarsi che i membri del team di distribuzione conoscano anche i vincoli esistenti per le altre società o organizzazioni \(limitata di hardware, nessun ambiente extranet e così via\) che potrebbe limitare l'ambito della progettazione in qualche modo. Per altre informazioni sulle organizzazioni partner, vedere [Pianificazione della distribuzione](https://technet.microsoft.com/library/dd807083.aspx).  
  
Dopo il progetto team e team di distribuzione concordare questi problemi, potranno procedere con la distribuzione della progettazione di AD FS. Per altre informazioni, vedere [Implementazione del piano di progetto di ADFS](Implementing-Your-AD-FS-Design-Plan.md).  
