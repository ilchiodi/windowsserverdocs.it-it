---
title: Personalizzazione di autenticazione a più fattori e provider di autenticazione esterni
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.assetid: 08724d45-9be4-4c56-a5f1-2cf40864e136
ms.technology: identity-adfs
ms.openlocfilehash: 8252244738d59f11a07c3bebadbbf2a5f4818845
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80816234"
---
# <a name="multi-factor-authentication-and-external-authentication-providers-customization"></a>Personalizzazione di autenticazione a più fattori e provider di autenticazione esterni 



In AD FS, viene fornito il supporto per l'autenticazione a più fattori\-di\-casella\-. È ad esempio possibile configurare AD FS per l'uso di\-compilate nell'autenticazione del certificato come secondo fattore di autenticazione. È anche possibile usare provider di autenticazione esterni. Questo approccio consente di abilitare AD FS per l'integrazione con servizi aggiuntivi, ad esempio Azure a più fattori, oppure è possibile sviluppare un provider personalizzato. Vedere [Guida alla soluzione: gestire i rischi con il controllo degli accessi a più\-Factor](https://technet.microsoft.com/library/dn280937.aspx) per ulteriori informazioni su come registrare il provider di autenticazione esterno usando ad FS.  
  
È consigliabile che un provider di autenticazione esterno usi le classi definite nel file CSS fornito da AD FS per creare l'interfaccia utente di autenticazione. Per esportare il tema Web predefinito e controllare le classi e gli elementi dell'interfaccia utente definiti nel file CSS, è possibile usare il cmdlet seguente. Il file CSS può essere usato nello sviluppo del\-di firma nell'interfaccia utente di un provider di autenticazione esterno.  
  

    Export-AdfsWebTheme -Name default -DirectoryPath C:\theme  
 
  
Di seguito è riportato un esempio di sign\-nell'interfaccia utente, evidenziato in rosso, da un provider di autenticazione esterno. L'interfaccia utente usa le classi dell'interfaccia utente nel file AD FS. CSS.  
  
![AD FS e autenticazione a più Fattori](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom8.png)  
  
Prima di scrivere un nuovo metodo di autenticazione personalizzato, è consigliabile studiare il tema AD FS e le definizioni di stile per comprendere i requisiti di creazione del contenuto.  
  
-   Un metodo di autenticazione personalizzato crea solo un segmento HTML nella pagina AD FS\-di accesso e non nella pagina intera. Per ottenere un aspetto e un comportamento coerenti, è consigliabile usare la definizione di stile AD FS.  
  
![AD FS e autenticazione a più Fattori](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom9.png)  
  
-   Tenere presente che AD FS amministratori possono personalizzare gli stili di AD FS. . Non è consigliabile impostare come hardcoded gli stili personalizzati, In alternativa, è consigliabile usare gli stili AD FS quando possibile.  
  
-   \-di\-box, gli stili AD FS vengono creati con un\-a sinistra\-\(destra\) LTR e un\-destro\-\(sinistra\)RTL. Gli amministratori possono personalizzare entrambi e possono fornire lingue\-stili specifici tramite la definizione del tema Web. Ogni foglio di stile ha tre sezioni con i rispettivi commenti:  
  
    -   gli **stili del tema** \- questi stili non devono e non possono essere usati. Questi stili definiscono il tema in tutte le pagine. Vengono definiti appositamente da un ID elemento, in modo che non siano riusati.  
  
    -   gli **stili comuni** \- questi sono gli stili che devono essere usati per il contenuto.  
  
    -   gli **stili del fattore di forma** \- questi sono stili per diversi fattori di forma. Questa sezione è importante per garantire che il contenuto funzioni con diversi fattori di forma, ad esempio telefoni e tablet.  
  
Per altre informazioni, vedere [Guida alla soluzione: gestire i rischi con il controllo degli accessi a più\-Factor](https://technet.microsoft.com/library/dn280937.aspx) e la [Guida alla soluzione: gestire i rischi con l'autenticazione a più fattori\-per le applicazioni riservate](https://tnstage.redmond.corp.microsoft.com/library/dn280949.aspx).  

## <a name="additional-references"></a>Altri riferimenti 
[Personalizzazione dell'accesso utente AD FS](AD-FS-user-sign-in-customization.md) 
