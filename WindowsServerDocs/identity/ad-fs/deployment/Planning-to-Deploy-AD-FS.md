---
ms.assetid: c87dc32d-ab33-44d2-a46f-f9f878b4e5b4
title: Pianificazione della distribuzione di AD FS
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: ed2082706975d58a1535aaeb61e6c5283d23306a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359497"
---
# <a name="planning-to-deploy-ad-fs"></a>Pianificazione della distribuzione di AD FS


Dopo aver raccolto informazioni sull'ambiente in uso e aver scelto una progettazione Active Directory Federation Services \(AD FS @ no__t-1 seguendo le istruzioni riportate nella [Guida alla progettazione ad FS di Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx), è possibile iniziare a pianificare la distribuzione di il progetto di AD FS dell'organizzazione. Con la progettazione completata e le informazioni contenute in questo argomento, è possibile determinare quali attività eseguire per distribuire AD FS nell'organizzazione.  
  
## <a name="reviewing-your-ad-fs-design"></a>Revisione del progetto di ADFS  
Se il team di progettazione che ha costruito il progetto di AD FS originale per l'organizzazione è diverso dal team di distribuzione che implementerà effettivamente la distribuzione, assicurarsi che il team di distribuzione esamini la progettazione finale con il team di progettazione. Rivedere i punti seguenti del progetto:  
  
-   La strategia del team di progettazione per determinare la topologia fisica ottimale per il posizionamento dei server federativi nella rete aziendale o in quella perimetrale. Il team di distribuzione può fare riferimento alla documentazione relativa a questo argomento riguardando gli argomenti seguenti nella Guida alla progettazione di AD FS:  
  
    -   [Ruolo del database di configurazione di AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)  
  
    -   [Pianificazione del posizionamento dei server federativi](https://technet.microsoft.com/library/dd807069.aspx)  
  
    -   [Pianificazione del posizionamento dei proxy server federativi](https://technet.microsoft.com/library/dd807130.aspx)  
  
    È possibile che il team di progettazione abbia lasciato la definizione del posizionamento del server federativo o del proxy server federativo al team di distribuzione. Il team di distribuzione è quindi responsabile della documentazione e dell'implementazione della topologia fisica dei server.  
  
-   I motivi aziendali per la designazione dell'organizzazione come provider di attestazioni, relying party o entrambi, nell'ambito del progetto di ADFS documentato. Assicurarsi che i membri del team di distribuzione conoscano i motivi per cui viene distribuita AD FS e le altre società o organizzazioni interessate dalla relazione di Federazione. Assicurarsi che i membri del team di distribuzione conoscano anche i vincoli esistenti per le altre società o organizzazioni \(limited hardware, nessun ambiente Extranet e così via @ no__t-1 che potrebbe limitare l'ambito della progettazione in qualche modo. Per altre informazioni sulle organizzazioni partner, vedere [Pianificazione della distribuzione](https://technet.microsoft.com/library/dd807083.aspx).  
  
Dopo che i team di progettazione e i team di distribuzione hanno accettato questi problemi, possono procedere con la distribuzione del AD FS progettazione. Per altre informazioni, vedere [Implementazione del piano di progetto di ADFS](Implementing-Your-AD-FS-Design-Plan.md).  
