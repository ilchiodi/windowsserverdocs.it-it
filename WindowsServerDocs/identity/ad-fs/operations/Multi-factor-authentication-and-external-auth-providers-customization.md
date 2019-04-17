---
title: "Autenticazione a più fattori e personalizzazione di provider di autenticazione esterno"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 08724d45-9be4-4c56-a5f1-2cf40864e136
ms.technology: identity-adfs
ms.openlocfilehash: 6d06c017601003e3b93df32f5fa50190ce54541d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="multi-factor-authentication-and-external-authentication-providers-customization"></a>Autenticazione a più fattori e personalizzazione di provider di autenticazione esterno 

>Si applica a: Windows Server 2016, Windows Server 2012 R2

In ADFS, il supporto per l'autenticazione a più fattori è fornito out-of\-the\-box. Ad esempio, è possibile configurare ADFS per utilizzare l'autenticazione del certificato predefinita come secondo fattore di autenticazione. È inoltre possibile utilizzare i provider di autenticazione esterni. Questo approccio può abilitare ADFS per l'integrazione con altri servizi, ad esempio Azure multi-factor Authentication, oppure è possibile sviluppare un provider personalizzato. Vedere [Guida alla soluzione: gestire i rischi con il controllo di accesso più fattori](https://technet.microsoft.com/library/dn280937.aspx) per ulteriori informazioni sulla registrazione del provider di autenticazione esterno con AD FS.  
  
È consigliabile che i provider di autenticazione esterno usi le classi definite nel file CSS da ADFS per creare l'interfaccia utente di autenticazione. È possibile utilizzare il cmdlet seguente per esportare il tema web predefinito e controllare le classi di interfaccia utente e gli elementi che sono definiti nel file CSS. Il file CSS può essere utilizzato per lo sviluppo dell'interfaccia utente di accesso del provider di autenticazione esterno.  
  

    Export-AdfsWebTheme -Name default -DirectoryPath C:\theme  
 
  
Di seguito è riportato un esempio dell'interfaccia utente di accesso, che viene evidenziata in rosso, da un provider di autenticazione esterno. L'interfaccia utente Usa le classi di interfaccia utente nel file CSS AD FS.  
  
![AD FS e autenticazione a più fattori](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom8.png)  
  
Prima di scrivere un nuovo metodo di autenticazione personalizzato, è consigliabile esaminare le definizioni del tema e lo stile di ADFS per conoscere i requisiti di creazione del contenuto.  
  
-   Un metodo di autenticazione personalizzato crea solo un segmento HTML nella pagina di accesso di ADFS e non l'intera pagina. Utilizzare la definizione di stile di ADFS per ottenere un aspetto coerenza e comportamento.  
  
![AD FS e autenticazione a più fattori](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom9.png)  
  
-   Tenere presente che gli amministratori di AD FS possono personalizzare gli stili di ADFS. . Non è consigliabile impostare come hardcoded gli stili personalizzati. Al contrario, si consiglia di utilizzare gli stili di ADFS quando possibile.  
  
-   Out-of\-della casella, gli stili di AD FS vengono creati come standard con uno stile \(LTR\) da sinistra a destra e uno \(RTL\) con-da destra a sinistra. Gli amministratori possono personalizzarli entrambi e possono fornire stili specifici della lingua tramite la definizione del tema web. Ogni foglio di stile ha tre sezioni con i rispettivi commenti:  
  
    -   **stili del tema** \-questi stili non si dovrebbero e non può essere utilizzati. Questi stili sono concepiti per definiscono il tema in tutte le pagine. Vengono definiti da un ID elemento intenzionalmente in modo che non siano riusati.  
  
    -   **stili comuni** \-questi sono gli stili che devono essere utilizzati per il contenuto.  
  
    -   **stili del fattore di forma** \-sono destinati ai diversi fattori di forma. È necessario comprendere in questa sezione per assicurarsi che il contenuto funzioni con diversi fattori di forma, ad esempio, telefoni e Tablet.  
  
Per ulteriori informazioni, vedere [Guida alla soluzione: gestire i rischi con il controllo di accesso più fattori](https://technet.microsoft.com/library/dn280937.aspx) e [Guida alla soluzione: gestire i rischi con l'autenticazione più fattori aggiuntiva per le applicazioni sensibili](https://tnstage.redmond.corp.microsoft.com/library/dn280949.aspx).  

## <a name="additional-references"></a>Riferimenti aggiuntivi 
[AD FS Sign-personalizzazione utente](AD-FS-user-sign-in-customization.md) 
