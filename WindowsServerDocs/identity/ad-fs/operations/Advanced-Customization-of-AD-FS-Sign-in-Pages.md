---
ms.assetid: 882abec8-0189-4f73-99c5-792987168080
title: Personalizzazione di AD FS Sign-in Pages avanzata
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/13/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ea01d0ff2a38c4fef2f68091608d777d8412e91b
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="advanced-customization-of-ad-fs-sign-in-pages"></a>Personalizzazione di AD FS Sign-in Pages avanzata

>Si applica a: Windows Server 2016, Windows Server 2012 R2
  
## <a name="advanced-customization-of-ad-fs-sign-in-pages"></a>Personalizzazione avanzata di pagine di accesso di ADFS AD  
ADFS in Windows Server 2012 R2 supporta predefinita per personalizzare l'esperienza di accesso. Per la maggior parte di questi scenari, i cmdlet di Windows PowerShell predefinita sono sono necessari.  È consigliabile utilizzare i comandi di Windows PowerShell predefinita per personalizzare elementi standard per AD FS accesso esperienza quando possibile.  Vedere [personalizzazione dell'accesso utente AD FS](AD-FS-user-sign-in-customization.md) per ulteriori informazioni.  
  
In alcuni casi, gli amministratori di ADFS potrebbero voler offrire esperienze di accesso aggiuntive che non sono disponibili tramite i comandi di PowerShell esistenti forniti attive-box con AD FS. In alcuni casi, è possibile \ (all'interno di below\ le indicazioni fornite) per gli amministratori di personalizzare l'accesso esperienza ulteriormente aggiungendo logica aggiuntiva per **onload.js** che viene fornito da ADFS e verrà eseguito in tutte le pagine di ADFS.  
  
## <a name="things-to-know-before-you-start"></a>Cose da sapere prima di iniziare  
  
-   Qualsiasi modifica che influisce sui flussi di reindirizzamento o modifica i parametri di protocollo utilizzabile con ADFS non è supportato.
  
-   Il onload.js originale, quello che viene fornita con il tema web predefinito, il codice che gestisce il rendering della pagina per diversi fattori di forma. È consigliabile non modificare il contenuto originale onload.js ma solo di aggiungere il codice di onload.js esistente che gestisce la logica personalizzata.  
  
-   ADFS viene fornito con un tema web predefinita che viene chiamato predefinito. È possibile modificare il onload.js del tema web predefinito. Per aggiornare onload.js, è necessario creare e usare un tema web personalizzato per pagine di accesso AD ADFS.  Vedere [personalizzazione dell'accesso utente AD FS](AD-FS-user-sign-in-customization.md) per informazioni su come creare un tema web personalizzato.  
  
-   Consente di eseguire la stessa onload.js in tutti i \(ex. pagine ADFS pagina di accesso basata su moduli, pagina di individuazione dell'area di autenticazione e così via. \). È necessario assicurarsi che venga eseguito solo il codice nello script come viene utilizzata e non vengono eseguita in modo imprevisto.  
  
-   Quando si fa riferimento qualsiasi elemento HTML, assicurarsi che sempre verificare l'esistenza dell'elemento prima di agire per l'elemento. Questo offre affidabilità e garantisce che la logica personalizzata non verrà eseguita nelle pagine che non contengono questo elemento. È possibile visualizzare semplicemente il codice HTML nelle pagine di accesso di ADFS per visualizzare gli elementi esistenti.  
  
-   Si consiglia di convalidare le personalizzazioni in un ambiente alternativo e testarli prima di distribuirlo out in produzione server AD FS. Ciò riduce le probabilità che agli utenti finali esposti a queste personalizzazioni prima di convalida.  
  
## <a name="customizing-the-ad-fs-sign-in-experience-by-using-onloadjs"></a>Personalizzazione dell'esperienza di accesso AD ADFS tramite onload.js  
Quando si personalizza l'onload.js per il servizio ADFS, utilizzare la procedura seguente.  
  
#### <a name="customizing-onloadjs-for-the-ad-fs-service"></a>Personalizzazione onload.js per il servizio ADFS  
  
1.  Per aggiungere la logica personalizzata per onload.js, devi prima creare un tema web personalizzato. Il tema è spedito out-of\-the\-box è denominato Default. È possibile esportare il tema predefinito e usarlo in modo che è possibile avviare rapidamente. Il cmdlet seguente crea un tema web personalizzato duplicando il tema web predefinito:  
  
    ```  
    New-AdfsWebTheme –Name custom –SourceName default  
  
    ```  
  
2.  È quindi possibile esportare personalizzato o predefinito tema web per ottenere file onload.js. Per esportare un tema web, usare il cmdlet seguente:  
  
    ```  
    Export-AdfsWebTheme –Name default –DirectoryPath c:\theme  
  
    ```  
  
    Troverai onload.js sotto la cartella di script nella directory specificate nel cmdlet export precedente e aggiungere la logica personalizzata per lo script \ (vedere casi di utilizzo di below\ sezione esempio).  
  
3.  Apportare le modifiche necessarie per personalizzare onload.js in base alle esigenze.  
  
4.  Aggiornare il tema con il onload.js modificato. Per applicare l'aggiornamento onload.js tema web personalizzato, usare il cmdlet seguente:  
  
    ```  
    Set-AdfsWebTheme -TargetName custom -AdditionalFileResource @{Uri=’/adfs/portal/script/onload.js’;path="c:\theme\script\onload.js"}  
  
    ```  
  
5.  Per applicare il tema web personalizzato per ADFS, usare il cmdlet seguente:  
  
    ```  
    Set-AdfsWebConfig -ActiveThemeName custom  
    ```  
  
## <a name="additional-customization-examples"></a>Esempi di personalizzazione aggiuntive  
Di seguito sono esempi di codice personalizzato aggiunto a onload.js per scopi diversi fine\-tune. Quando si aggiunge il codice personalizzato, per aggiungere sempre il codice personalizzato a fondo il onload.js.  
  
### <a name="example-1-change-sign-in-with-organizational-account-string"></a>Esempio 1: modificare la stringa "Accedi con l'account aziendale"  
L'impostazione predefinita, AD FS basata su moduli della pagina di accesso dispone di un titolo di "Accedi con l'account dell'organizzazione" sopra le caselle di input dell'utente.  
  
Se si desidera sostituire questa stringa con la stringa personalizzata, è possibile aggiungere il codice seguente per onload.js.  
  
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
  
### <a name="example-2-accept-sam-account-name-as-a-login-format-on-an-ad-fs-form-based-sign-in-page"></a>Esempio 2: accettare il nome di account SAM\ come formato di accesso in un ADFS basata su moduli della pagina di accesso  
Il valore predefinito ADFS basata su moduli della pagina di accesso supporta accesso formato dei nomi dell'entità utente \(UPNs\) \(for example, ** johndoe@contoso.com **\) o dominio qualificato i nomi degli account di sam\ \ (**contoso\\johndoe** o **contoso.com\\johndoe**\). Nel caso in cui tutti gli utenti provenienti dallo stesso dominio e sappiano solo sui nomi degli account di sam\, si desidera supportare lo scenario in cui gli utenti possono eseguire l'accesso a inviare solo i nomi degli account di sam\. È possibile aggiungere il codice seguente per onload.js per supportare questo scenario, è necessario sostituire solo il dominio "contoso.com" nell'esempio riportato di seguito con il dominio che si desidera utilizzare.  
  
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
  
## <a name="additional-references"></a>Riferimenti aggiuntivi 
[AD FS Sign-personalizzazione utente](AD-FS-user-sign-in-customization.md)  
  

