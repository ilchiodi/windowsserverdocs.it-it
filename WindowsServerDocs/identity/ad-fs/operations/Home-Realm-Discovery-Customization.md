---
ms.assetid: 626e33fc-4ac2-4d38-9ac9-addaa4c8d9bb
title: Personalizzazione di individuazione dell'area di autenticazione
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e91306ecd8ef08dd6af9173ead314a39dd5d2eff
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189135"
---
# <a name="home-realm-discovery-customization"></a>Personalizzazione di individuazione dell'area di autenticazione


Quando il client di ADFS richiede innanzitutto una risorsa, il server federativo di risorsa non contiene informazioni sull'area di autenticazione del client. Il server federativo di risorsa risponde al client ADFS con un **individuazione dell'area di autenticazione Client** pagina, in cui l'utente seleziona l'area di autenticazione principale da un elenco. Il valori dell'elenco sono popolati dalla proprietà del nome visualizzato in Attendibilità provider di attestazioni. Utilizzare i cmdlet di Windows PowerShell seguenti per modificare e personalizzare l'esperienza di AD FS Home Realm Discovery.  
  
![area di autenticazione principale](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom4.png)  
  
> [!WARNING]  
> Si noti che il nome del provider di attestazioni visualizzato per Active Directory locale è il nome visualizzato del Server federativo.  
  



## <a name="configure-identity-provider-to-use-certain-email-suffixes"></a>Configurare il provider di identità per l'uso di determinati suffissi di posta elettronica  
Un'organizzazione può creare una federazione con più provider di attestazioni. ADFS offre ora la in\-casella funzionalità per gli amministratori elencare i suffissi, ad esempio, @us.contoso.com, @eu.contoso.com, che viene supportato dal provider di attestazioni e abilitarla per suffisso\-individuazione basata su. Con questa configurazione, gli utenti finali possono digitare nel proprio account aziendale e AD FS seleziona automaticamente il provider di attestazioni corrispondente.  
  
Per configurare un provider di identità \(IDP\), ad esempio `fabrikam`, per utilizzare determinati suffissi di posta elettronica, utilizzare il cmdlet di Windows PowerShell e la sintassi seguente.  
  

`Set-AdfsClaimsProviderTrust -TargetName fabrikam -OrganizationalAccountSuffix @("fabrikam.com";"fabrikam2.com") ` 
 
>[!NOTE]
> La federazione tra due server AD FS, è possibile impostare proprietà PromptLoginFederation nell'attendibilità del provider di attestazioni a ForwardPromptAndHintsOverWsFederation.  Si tratta in modo che AD FS inoltrerà il login_hint e parametro dei messaggi di richiesta al provider di identità.  Questa operazione può essere eseguita tramite il cmdlet di PowerShell seguente:
>
>`Set-AdfsclaimsProviderTrust -PromptLoginFederation ForwardPromptAndHintsOverWsFederation`

## <a name="configure-an-identity-provider-list-per-relying-party"></a>Configurare un elenco dei provider di identità per ogni relying party  
Per alcuni scenari è possibile che un'organizzazione desideri che gli utenti finali vedano solo i provider di attestazioni specifici per un'applicazione, in modo che nella pagina di individuazione dell'area di autenticazione principale sia visualizzato solo un subset di provider di attestazioni.  
  
Per configurare un elenco di provider di identità per ogni relying party \(RP\), utilizzare la sintassi e i cmdlet Windows PowerShell seguente.  
  
 
`Set-AdfsRelyingPartyTrust -TargetName claimapp -ClaimsProviderName @("Fabrikam","Active Directory") ` 

  
## <a name="bypass-home-realm-discovery-for-the-intranet"></a>Escludere l'individuazione dell'area di autenticazione principale per la rete Intranet  
La maggior parte delle organizzazioni supporta solo l'istanza di Active Directory locale per tutti gli utenti che accedono dall'interno del firewall. In questi casi, gli amministratori possono configurare ADFS per escludere l'individuazione dell'area di autenticazione per la rete intranet.  
  
Per ignorare HRD per la rete intranet, utilizzare la sintassi e i cmdlet Windows PowerShell seguente.  
  

`Set-AdfsProperties -IntranetUseLocalClaimsProvider $true ` 
 
  
> [!IMPORTANT]  
> Si noti che se è stato configurato un elenco di provider di identità per una relying party, anche se l'impostazione precedente sia stato abilitato e l'utente accede dalla rete intranet, ADFS viene comunque mostrata l'individuazione dell'area di autenticazione \(HRD\) pagina. Per escludere l'individuazione dell'area di autenticazione principale in questo caso, è necessario verificare che all'elenco di provider di identità per questa relying pary sia aggiunto anche "Active Directory".  

## <a name="additional-references"></a>Altri riferimenti 
[AD FS Sign-in personalizzazione dell'utente](AD-FS-user-sign-in-customization.md)  
