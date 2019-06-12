---
ms.assetid: 882abec8-0189-4f73-99c5-792987168080
title: Personalizzazione di AD FS Sign-in Pages avanzata
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 01/16/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ee7bef2afe61500fe75b2d3c61b92b902f9757fa
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444267"
---
# <a name="advanced-customization-of-ad-fs-sign-in-pages"></a>Personalizzazione di AD FS Sign-in Pages avanzata

  
## <a name="advanced-customization-of-ad-fs-sign-in-pages"></a>Personalizzazione di AD FS Sign avanzata\-nelle pagine  
ADFS in Windows Server 2012 R2 fornisce compilato\-in modalità di supporto per la personalizzazione di accesso\-nell'esperienza. Per la maggior parte di questi scenari, incorporati\-in Windows PowerShell cmdlet sono tutto ciò che è obbligatorio.  È consigliabile usare incorporato\-nei comandi di Windows PowerShell per personalizzare gli elementi standard per AD FS firma\-nell'esperienza laddove possibile.  Visualizzare [personalizzazione dell'accesso utente AD FS](AD-FS-user-sign-in-customization.md) per altre informazioni.  
  
In alcuni casi, gli amministratori di AD FS potrebbe essere necessario fornire accesso aggiuntivo\-esperienze che non sono possibili tramite esistente comandi di PowerShell forniti in\-casella con AD FS. In alcuni casi, è possibile \(entro le linee guida riportate di seguito\) agli amministratori di personalizzare il segno\-esperienza ulteriormente tramite l'aggiunta di logica aggiuntiva per **OnLoad** che viene fornito da AD FS e verrà eseguito su tutte le pagine di AD FS.  
  
## <a name="things-to-know-before-you-start"></a>Cose da sapere prima di iniziare  
  
-   Qualsiasi modifica che influisce su flussi di reindirizzamento o modifica i parametri di protocollo utilizzabile con AD FS non è supportato.
  
-   L'OnLoad originale, quella fornita con il tema web predefinito, il codice che gestisce il rendering della pagina per diversi fattori di forma. È consigliabile non modificare il contenuto di OnLoad originale, ma solo aggiungere codice per l'OnLoad esistente che gestisce la logica personalizzata.  
  
-   AD FS viene fornito con incorporata\-tema web denominato predefinito. Non è possibile modificare l'OnLoad del tema web predefinito. Per aggiornare OnLoad, è necessario creare e usare un tema web personalizzato per l'accesso di AD FS\-nelle pagine.  Visualizzare [personalizzazione dell'accesso utente AD FS](AD-FS-user-sign-in-customization.md) per informazioni su come creare un tema web personalizzato.  
  
-   La stessa OnLoad verrà eseguita in tutte le pagine di ad FS \(es. Form\-basati su pagina di accesso, la pagina di individuazione dell'area di autenticazione e così via\). È necessario assicurarsi che il codice nello script viene eseguito solo perché è stato progettato e non venga eseguito in modo imprevisto.  
  
-   Quando si fa riferimento a qualsiasi elemento HTML, assicurarsi che sempre verificare l'esistenza dell'elemento prima che agisce sull'elemento. Questo garantisce affidabilità e assicura che la logica personalizzata potrebbe non essere eseguita nelle pagine che non contengono questo elemento. È possibile visualizzare l'origine HTML semplicemente di accesso di AD FS\-nelle pagine per visualizzare gli elementi esistenti.  
  
-   È consigliabile convalidare le personalizzazioni in un ambiente alternativo e testarli prima di eseguirne il rollback out nell'ambiente di produzione i server AD FS. In questo modo si riduce le possibilità che gli utenti finali viene esposti a queste personalizzazioni prima della convalida.  
  
## <a name="customizing-the-ad-fs-sign-in-experience-by-using-onloadjs"></a>Personalizzazione di accesso di AD FS\-nell'esperienza tramite OnLoad  
Usare i passaggi seguenti quando si personalizza l'OnLoad per il servizio AD FS.  
  
#### <a name="customizing-onloadjs-for-the-ad-fs-service"></a>Personalizzazione di OnLoad per il servizio ADFS  
  
1.  Per aggiungere la logica personalizzata a OnLoad, è necessario innanzitutto creare un tema web personalizzato. Il tema è spedito\-di\-di\-casella è denominata Default. Si può esportare il tema predefinito e usarlo come base per iniziare rapidamente. Il cmdlet seguente crea un tema web personalizzato duplicando il tema web predefinito:  
  
    ```  
    New-AdfsWebTheme –Name custom –SourceName default  
  
    ```  
  
2.  È quindi possibile esportare l'oggetto personalizzato o predefinito di tema web per ottenere il file OnLoad. Per esportare un tema web, usare il cmdlet seguente:  
  
    ```  
    Export-AdfsWebTheme –Name default –DirectoryPath c:\theme  
  
    ```  
  
    Si noterà OnLoad sotto la cartella dello script nella directory specificati nel cmdlet esportazione precedente e aggiungere logica personalizzata per lo script \(vedere casi d'uso nella sezione di esempio riportato di seguito\).  
  
3.  Apportare la modifica necessaria per personalizzare OnLoad in base alle proprie esigenze.  
  
4.  Aggiornare il tema con gli OnLoad modificato. Per applicare l'aggiornamento di OnLoad a tema web personalizzato, usare il cmdlet seguente:  

     Per AD FS in Windows Server 2012 R2:  

    ```  
    Set-AdfsWebTheme -TargetName custom -AdditionalFileResource @{Uri=’/adfs/portal/script/onload.js’;path="c:\theme\script\onload.js"}  
  
    ```  
    Per AD FS in Windows Server 2016:

     ```  
    Set-AdfsWebTheme -TargetName custom -OnLoadScriptPath "c:\ADFStheme\script\onload.js"   
  
    ```  
  
5.  Per applicare il tema web personalizzato per ADFS, usare il cmdlet seguente:  
  
    ```  
    Set-AdfsWebConfig -ActiveThemeName custom  
    ```  
  
## <a name="additional-customization-examples"></a>Esempi di personalizzazione aggiuntivi  
Di seguito sono gli esempi di codice personalizzato aggiunto a OnLoad per diversa granularità\-a scopo di ottimizzazione. Quando si aggiunge codice personalizzato, aggiungere sempre il codice personalizzato in fondo l'OnLoad.  
  
### <a name="example-1-change-sign-in-with-organizational-account-string"></a>Esempio 1: modificare la stringa "Accesso con account aziendale"  
Il valore predefinito il formato di AD FS\-accesso basato su\-nella pagina è disponibile un titolo di "Accedi con l'account aziendale" di sopra di caselle di input utente.  
  
Se si desidera sostituire la stringa con una stringa personalizzata, è possibile aggiungere il codice seguente a OnLoad.  
  
```  
// Sample code to change “Sign in with organizational account” string.  
  
// Check whether the loginMessage element is present on this page.  
var loginMessage = document.getElementById('loginMessage');  
if (loginMessage)  
{  
       // loginMessage element is present, modify its properties.  
       loginMessage.innerHTML = 'Your company description text';  
}  
  
```  
  
### <a name="example-2-accept-sam-account-name-as-a-login-format-on-an-ad-fs-form-based-sign-in-page"></a>Esempio 2: accettare SAM\-nome dell'account come un formato di account di accesso in un form di AD FS\-accesso basato su\-nella pagina  
Form di AD FS predefinito\-accesso basato su\-supporta il formato di account di accesso dei nomi dell'entità utente nella pagina \(UPN\) \(ad esempio <strong>johndoe@contoso.com</strong> \) o di dominio completo di sam\-nomi di account \( **contoso\\mariorossi** oppure **contoso.com\\mariorossi**\). Nel caso in cui tutti gli utenti provengono dallo stesso dominio e informarli solo in merito sam\-i nomi degli account, è possibile supportare lo scenario in cui gli utenti possono accedere usando li sam\-solo i nomi di account. È possibile aggiungere il codice seguente a OnLoad per supportare questo scenario, sostituire semplicemente il dominio "contoso.com" nell'esempio riportato di seguito con il dominio in cui si desidera utilizzare.  
  
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
[AD FS Sign-in personalizzazione dell'utente](AD-FS-user-sign-in-customization.md)  
  

