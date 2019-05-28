---
title: L'autenticazione a più fattori e personalizzazione di provider di autenticazione esterno
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 08724d45-9be4-4c56-a5f1-2cf40864e136
ms.technology: identity-adfs
ms.openlocfilehash: 347b4783e82a6561334f8757029b1fddec6a85a3
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189076"
---
# <a name="multi-factor-authentication-and-external-authentication-providers-customization"></a>L'autenticazione a più fattori e personalizzazione di provider di autenticazione esterno 



In AD FS, viene fornito il supporto per multi-factor authentication out\-dei\-il\-casella. Ad esempio, è possibile configurare ADFS per utilizzare compilato\-nell'autenticazione del certificato come secondo fattore di autenticazione. È anche possibile usare provider di autenticazione esterni. Questo approccio può abilitare ADFS per l'integrazione con altri servizi, ad esempio Azure multi-factor Authentication, oppure è possibile sviluppare un provider personalizzato. Vedere [Guida alla soluzione: Gestire i rischi con Multi\-fattore di controllo di accesso](https://technet.microsoft.com/library/dn280937.aspx) per altre informazioni su come registrare i provider di autenticazione esterno tramite AD FS.  
  
È consigliabile che un provider di autenticazione esterno usi le classi definite nel file CSS che AD FS offre per la creazione dell'interfaccia utente di autenticazione. Per esportare il tema Web predefinito e controllare le classi e gli elementi dell'interfaccia utente definiti nel file CSS, è possibile usare il cmdlet seguente. Il file con estensione CSS può essere utilizzato nello sviluppo del segno di\-nell'interfaccia utente del provider di autenticazione esterno.  
  

    Export-AdfsWebTheme -Name default -DirectoryPath C:\theme  
 
  
Di seguito è riportato un esempio di accesso\-nell'interfaccia utente di cui è evidenziato in rosso, da un provider di autenticazione esterni. L'interfaccia utente Usa le classi di interfaccia utente nel file CSS AD FS.  
  
![AD FS e autenticazione a più Fattori](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom8.png)  
  
Prima di scrivere un nuovo metodo di autenticazione personalizzata, è consigliabile esaminare le definizioni di tema e uno stile di AD FS per conoscere i requisiti di creazione del contenuto.  
  
-   Un metodo di autenticazione personalizzato crea solo un segmento di codice HTML di accesso di AD FS\-nella pagina e non l'intera pagina. Utilizzare la definizione di stile di ADFS per ottenere l'aspetto uniforme e il comportamento.  
  
![AD FS e autenticazione a più Fattori](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom9.png)  
  
-   Tenere presente che gli amministratori di AD FS possono personalizzare gli stili di AD FS. . Non è consigliabile impostare come hardcoded gli stili personalizzati, È invece consigliabile usare gli stili di AD FS laddove possibile.  
  
-   Out\-di\-casella degli stili di AD FS vengono creati con uno da sinistra\-al\-a destra \(conservazione a lungo termine\) stile e una destra\-a\-a sinistra \(RTL\). Gli amministratori possono personalizzarli entrambi e possono fornire language\-stili specifici tramite la definizione del tema web. Ogni foglio di stile ha tre sezioni con i rispettivi commenti:  
  
    -   **gli stili dei temi** \- non si dovrebbero e non può essere utilizzato. Questi stili definiscono il tema in tutte le pagine. Vengono definiti appositamente da un ID elemento, in modo che non siano riusati.  
  
    -   **stili comuni** \- questi sono gli stili che devono essere utilizzati per il contenuto.  
  
    -   **stili del fattore di forma** \- sono destinati ai diversi fattori di forma. Questa sezione è importante per garantire che il contenuto funzioni con diversi fattori di forma, ad esempio telefoni e tablet.  
  
Per altre informazioni, vedere [Guida alla soluzione: Gestire i rischi con Multi\-fattore di controllo degli accessi](https://technet.microsoft.com/library/dn280937.aspx) e [Guida alla soluzione: Gestire i rischi con aggiuntive Multi\-fattore di autenticazione per le applicazioni sensibili](https://tnstage.redmond.corp.microsoft.com/library/dn280949.aspx).  

## <a name="additional-references"></a>Altri riferimenti 
[AD FS Sign-in personalizzazione dell'utente](AD-FS-user-sign-in-customization.md) 
