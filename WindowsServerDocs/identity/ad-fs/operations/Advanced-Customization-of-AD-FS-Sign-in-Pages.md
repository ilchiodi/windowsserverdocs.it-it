---
ms.assetid: 882abec8-0189-4f73-99c5-792987168080
title: Personalizzazione avanzata delle pagine di accesso AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 01/16/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: a4a70632ea4c1db39c020327bbe135f4798e6970
ms.sourcegitcommit: 2cc251eb5bc3069bf09bc08e06c3478fcbe1f321
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/03/2020
ms.locfileid: "84333892"
---
# <a name="advanced-customization-of-ad-fs-sign-in-pages"></a>Personalizzazione avanzata delle pagine di accesso AD FS

  
## <a name="advanced-customization-of-ad-fs-sign-in-pages"></a>Personalizzazione avanzata delle pagine di accesso AD FS \-  
AD FS in Windows Server 2012 R2 offre \- supporto incorporato per la personalizzazione dell'esperienza di accesso \- . Per la maggior parte di questi scenari, i \- cmdlet incorporati di Windows PowerShell sono tutti necessari.  È consigliabile usare i comandi incorporati di \- Windows PowerShell per personalizzare gli elementi standard per l'esperienza di accesso ad FS \- quando possibile.  Per ulteriori informazioni, vedere [personalizzazione dell'accesso utente ad FS](AD-FS-user-sign-in-customization.md) .  
  
In alcuni casi, gli amministratori AD FS possono fornire esperienze di accesso aggiuntive \- non possibili tramite i comandi di PowerShell esistenti inclusi in \- box con ad FS. In alcuni casi, è fattibile \( all'interno delle linee guida fornite di seguito per consentire agli \) amministratori di personalizzare \- ulteriormente l'esperienza di accesso aggiungendo logica aggiuntiva a **OnLoad. js** fornita da ad FS e verrà eseguita in tutte le pagine ad FS.  
  
## <a name="things-to-know-before-you-start"></a>Informazioni importanti prima di iniziare  
  
-   Le modifiche che influiscono sui flussi di reindirizzamento o modificano i parametri del protocollo che AD FS funziona con non sono supportate.
  
-   L'oggetto OnLoad. js originale, quello che include il tema Web predefinito, contiene il codice che gestisce il rendering della pagina per diversi fattori di forma. Si consiglia di non modificare il contenuto originale di OnLoad. js, ma solo di aggiungere il codice al metodo OnLoad. js esistente che gestisce la logica personalizzata.  
  
-   AD FS viene fornito con un \- tema Web incorporato, denominato predefinito. Non è possibile modificare OnLoad. js del tema Web predefinito. Per aggiornare OnLoad. js, è necessario creare e usare un tema Web personalizzato per AD FS le \- pagine di accesso.  Per informazioni su come creare un tema Web personalizzato, vedere [personalizzazione dell'accesso utente ad FS](AD-FS-user-sign-in-customization.md) .  
  
-   Lo stesso OnLoad. js verrà eseguito in tutte le pagine ad FS \( , ad esempio. \-pagina di accesso basata su form, pagina di individuazione dell'area di autenticazione principale e così via \) . È necessario assicurarsi che il codice nello script venga eseguito solo quando è stato progettato e non viene eseguito in modo imprevisto.  
  
-   Quando si fa riferimento a qualsiasi elemento HTML, verificare che venga sempre verificata l'esistenza dell'elemento prima di agire sull'elemento. In questo modo viene garantita l'affidabilità e si garantisce che la logica personalizzata non venga eseguita sulle pagine che non contengono questo elemento. \-Per visualizzare gli elementi esistenti, è possibile visualizzare semplicemente l'origine HTML nelle pagine di accesso ad FS.  
  
-   Si consiglia vivamente di convalidare le personalizzazioni in un ambiente alternativo e di testarle prima di distribuirle in server AD FS di produzione. In questo modo si riducono le probabilità che gli utenti finali vengano esposti a queste personalizzazioni prima della convalida.  
  
## <a name="customizing-the-ad-fs-sign-in-experience-by-using-onloadjs"></a>Personalizzazione dell' \- esperienza di accesso ad FS usando OnLoad. js  
Usare la procedura seguente per personalizzare OnLoad. js per il servizio AD FS.  
  
#### <a name="customizing-onloadjs-for-the-ad-fs-service"></a>Personalizzazione di OnLoad. js per il servizio AD FS  
  
1.  Per aggiungere la logica personalizzata a OnLoad. js, è necessario creare prima un tema Web personalizzato. Il tema è spedito\-di\-di\-casella è denominata Default. Si può esportare il tema predefinito e usarlo come base per iniziare rapidamente. Il cmdlet seguente crea un tema Web personalizzato che duplica il tema Web predefinito:  
  
    ```  
    New-AdfsWebTheme –Name custom –SourceName default  
  
    ```  
  
2.  È quindi possibile esportare il tema Web personalizzato o predefinito per ottenere il file OnLoad. js. Per esportare un tema Web, usare il cmdlet seguente:  
  
    ```  
    Export-AdfsWebTheme –Name default –DirectoryPath c:\theme  
  
    ```  
  
    È possibile trovare OnLoad. js sotto la cartella script nella directory specificata nel cmdlet Export precedente e aggiungere la logica personalizzata allo script \( vedere i casi d'uso nella sezione di esempio riportata di seguito \) .  
  
3.  Apportare le modifiche necessarie per personalizzare OnLoad. js in base alle esigenze.  
  
4.  Aggiornare il tema con l'oggetto OnLoad. js modificato. Usare il cmdlet seguente per applicare l'aggiornamento OnLoad. js al tema Web personalizzato:  

     Per AD FS in Windows Server 2012 R2:  

    ```  
    Set-AdfsWebTheme -TargetName custom -AdditionalFileResource @{Uri='/adfs/portal/script/onload.js';path="c:\theme\script\onload.js"}  
  
    ```  
    Per AD FS in Windows Server 2016:

     ```  
    Set-AdfsWebTheme -TargetName custom -OnLoadScriptPath "c:\theme\script\onload.js"   
  
    ```  
  
5.  Per applicare il tema Web personalizzato a AD FS, usare il cmdlet seguente:  
  
    ```  
    Set-AdfsWebConfig -ActiveThemeName custom  
    ```  
  
## <a name="additional-customization-examples"></a>Esempi di personalizzazione aggiuntivi  
Di seguito sono riportati gli esempi di codice personalizzato aggiunto a OnLoad. js per \- scopi di ottimizzazione diversi. Quando si aggiunge il codice personalizzato, aggiungere sempre il codice personalizzato alla fine di OnLoad. js.  
  
### <a name="example-1-change-sign-in-with-organizational-account-string"></a>Esempio 1: modificare la stringa "Accedi con account aziendale"  
La pagina di accesso predefinita basata sul modulo AD FS form \- \- ha il titolo "Accedi con l'account aziendale" sopra le caselle di input dell'utente.  
  
Se si vuole sostituire questa stringa con una stringa personalizzata, è possibile aggiungere il codice seguente a OnLoad. js.  
  
```  
// Sample code to change "Sign in with organizational account" string.  
  
// Check whether the loginMessage element is present on this page.  
var loginMessage = document.getElementById('loginMessage');  
if (loginMessage)  
{  
       // loginMessage element is present, modify its properties.  
       loginMessage.innerHTML = 'Your company description text';  
}  
  
```  
  
### <a name="example-2-accept-sam-account-name-as-a-login-format-on-an-ad-fs-form-based-sign-in-page"></a>Esempio 2: accettare il \- nome dell'account SAM come formato di accesso in una \- pagina di accesso basata su ad FS form \-  
La \- pagina di accesso ad FS basata su form predefinita supporta il \- formato di accesso dei nomi dell'entità utente \( UPN \) \( , ad esempio, <strong>johndoe@contoso.com</strong> \) o i nomi di account sam con dominio completo \- \( **Contoso \\ johndoe** o **contoso.com \\ johndoe** \) . Se tutti gli utenti provengono dallo stesso dominio e conoscono solo i \- nomi degli account SAM, è possibile che si voglia supportare lo scenario in cui gli utenti possono accedere usando \- solo nomi di account SAM. È possibile aggiungere il codice seguente a OnLoad. js per supportare questo scenario, basta sostituire il dominio "contoso.com" nell'esempio seguente con il dominio che si vuole usare.  
  
```  
if (typeof Login != 'undefined'){  
    Login.submitLoginRequest = function () {   
    var u = new InputUtil();  
    var e = new LoginErrors();  
    var userName = document.getElementById(Login.userNameInput);  
    var password = document.getElementById(Login.passwordInput);  
    if (userName.value && !userName.value.match('[@\\\\]'))   
    {  
        var userNameValue = 'contoso.com\\' + userName.value;  
        document.forms['loginForm'].UserName.value = userNameValue;  
    }  
  
    if (!userName.value) {  
       u.setError(userName, e.userNameFormatError);  
       return false;  
    }  
  
    if (!password.value)   
    {  
        u.setError(password, e.passwordEmpty);  
        return false;  
    }  
    document.forms['loginForm'].submit();  
    return false;  
};  
}  
  
```  
  
## <a name="additional-references"></a>Altri riferimenti 
[Personalizzazione dell'accesso utente AD FS](AD-FS-user-sign-in-customization.md)  
  

