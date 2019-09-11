---
title: Personalizzazione di autenticazione a più fattori e provider di autenticazione esterni
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 08724d45-9be4-4c56-a5f1-2cf40864e136
ms.technology: identity-adfs
ms.openlocfilehash: 23ec5acbe442527b4eb44c4b857e183b5e0c37ea
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865714"
---
# <a name="multi-factor-authentication-and-external-authentication-providers-customization"></a>Personalizzazione di autenticazione a più fattori e provider di autenticazione esterni 



In ad FS, il supporto per l'autenticazione a più fattori viene fornito\-\-\-come predefinito. È ad esempio possibile configurare ad FS per l'uso dell'\-autenticazione del certificato incorporata come secondo fattore di autenticazione. È anche possibile usare provider di autenticazione esterni. Questo approccio consente di abilitare AD FS per l'integrazione con servizi aggiuntivi, ad esempio Azure a più fattori, oppure è possibile sviluppare un provider personalizzato. Vedere [Guida alla soluzione: Gestire i rischi con\-il controllo](https://technet.microsoft.com/library/dn280937.aspx) degli accessi a più fattori per altre informazioni su come registrare il provider di autenticazione esterno usando ad FS.  
  
È consigliabile che un provider di autenticazione esterno usi le classi definite nel file CSS fornito da AD FS per creare l'interfaccia utente di autenticazione. Per esportare il tema Web predefinito e controllare le classi e gli elementi dell'interfaccia utente definiti nel file CSS, è possibile usare il cmdlet seguente. Il file CSS può essere usato nello sviluppo dell'interfaccia utente di accesso\-di un provider di autenticazione esterno.  
  

    Export-AdfsWebTheme -Name default -DirectoryPath C:\theme  
 
  
Di seguito è riportato un esempio dell'interfaccia\-utente di accesso, evidenziata in rosso, da un provider di autenticazione esterno. L'interfaccia utente usa le classi dell'interfaccia utente nel file AD FS. CSS.  
  
![AD FS e autenticazione a più Fattori](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom8.png)  
  
Prima di scrivere un nuovo metodo di autenticazione personalizzato, è consigliabile studiare il tema AD FS e le definizioni di stile per comprendere i requisiti di creazione del contenuto.  
  
-   Un metodo di autenticazione personalizzato crea solo un segmento HTML nella pagina di\-accesso ad FS e non la pagina intera. Per ottenere un aspetto e un comportamento coerenti, è consigliabile usare la definizione di stile AD FS.  
  
![AD FS e autenticazione a più Fattori](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom9.png)  
  
-   Tenere presente che AD FS amministratori possono personalizzare gli stili di AD FS. . Non è consigliabile impostare come hardcoded gli stili personalizzati, In alternativa, è consigliabile usare gli stili AD FS quando possibile.  
  
-   \(\-\- \(Ad FS stili vengono\-creati con\-uno stiledasinistraadestraedaunrettangoloasinistra\) \-\-\). Gli amministratori possono personalizzare entrambi e fornire stili specifici\-della lingua tramite la definizione del tema Web. Ogni foglio di stile ha tre sezioni con i rispettivi commenti:  
  
    -   **stili del tema** \- Questi stili non devono e non possono essere usati. Questi stili definiscono il tema in tutte le pagine. Vengono definiti appositamente da un ID elemento, in modo che non siano riusati.  
  
    -   **stili comuni** \- Questi sono gli stili che devono essere usati per il contenuto.  
  
    -   **stili del fattore di forma** \- Si tratta di stili per diversi fattori di forma. Questa sezione è importante per garantire che il contenuto funzioni con diversi fattori di forma, ad esempio telefoni e tablet.  
  
Per ulteriori informazioni, vedere [Guida alla soluzione: Gestire i rischi con\-il controllo](https://technet.microsoft.com/library/dn280937.aspx) degli accessi a più fattori e [la guida alla soluzione: Gestire i rischi con l'\-autenticazione a più fattori aggiuntiva per le applicazioni](https://tnstage.redmond.corp.microsoft.com/library/dn280949.aspx)riservate.  

## <a name="additional-references"></a>Altri riferimenti 
[Personalizzazione dell'accesso utente AD FS](AD-FS-user-sign-in-customization.md) 
