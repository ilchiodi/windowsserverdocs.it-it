---
ms.assetid: 626e33fc-4ac2-4d38-9ac9-addaa4c8d9bb
title: Home Realm Discovery personalizzazione
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 328c313b2d5bf174f6e6af20cd91ac9620edac82
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="home-realm-discovery-customization"></a>Home Realm Discovery personalizzazione

>Si applica a: Windows Server 2016, Windows Server 2012 R2

Quando il client di ADFS richiede innanzitutto una risorsa, il server federativo di risorsa è disponibile alcuna informazione sull'area di autenticazione del client. Il server federativo di risorsa risponde al client ADFS con un **individuazione dell'area di autenticazione Client** pagina in cui l'utente seleziona l'area di autenticazione principale da un elenco. I valori di elenco sono popolati dalla proprietà del nome visualizzato nel Provider di attestazioni. Utilizzare i cmdlet di Windows PowerShell seguenti per modificare e personalizzare l'esperienza di AD FS Home Realm Discovery.  
  
![area di autenticazione principale](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom4.png)  
  
> [!WARNING]  
> Tenere presente che il nome del Provider di attestazioni visualizzato per Active Directory locale è il nome visualizzato del servizio federativo.  
  
## <a name="configure-identity-provider-to-use-certain-email-suffixes"></a>Configurare il Provider di identità per l'utilizzo di suffissi di posta elettronica  
Un'organizzazione può creare una federazione con più provider di attestazioni. ADFS offre ora la funzionalità attive per gli amministratori elencare i suffissi, ad esempio, @us.contoso.com, @eu.contoso.com, che viene supportata da un provider di attestazioni e abilitarlo per l'individuazione basata su suffix\. Con questa configurazione, gli utenti finali possono digitare nel proprio account aziendale e AD FS seleziona automaticamente il provider di attestazioni corrispondente.  
  
Per configurare un provider di identità \(IDP\), ad esempio `fabrikam`, per utilizzare determinati suffissi di posta elettronica, usare la sintassi e i cmdlet di Windows PowerShell seguente.  
  

`Set-AdfsClaimsProviderTrust -TargetName fabrikam -OrganizationalAccountSuffix @("fabrikam.com";"fabrikam2.com") ` 
 
  
## <a name="configure-an-identity-provider-list-per-relying-party"></a>Configurare un elenco di provider di identità per ogni relying party  
Per alcuni scenari, un'organizzazione potrebbero essere agli utenti di visualizzare solo i provider di attestazioni che sono specifici di un'applicazione in modo che solo un sottoinsieme di provider di attestazioni vengono visualizzati nella pagina di individuazione dell'area di autenticazione.  
  
Per configurare un elenco di IDP per ogni relying party \(RP\), utilizzare la sintassi e i cmdlet Windows PowerShell seguente.  
  
 
`Set-AdfsRelyingPartyTrust -TargetName claimapp -ClaimsProviderName @("Fabrikam","Active Directory") ` 

  
## <a name="bypass-home-realm-discovery-for-the-intranet"></a>Escludere l'individuazione dell'area di autenticazione principale per intranet  
La maggior parte delle organizzazioni supporta solo i controller di dominio Active Directory locale per tutti gli utenti che accedono dall'interno del firewall. In questi casi, gli amministratori possono configurare ADFS per escludere l'individuazione dell'area di autenticazione per la rete intranet.  
  
Per ignorare HRD per la rete intranet, utilizzare la sintassi e i cmdlet Windows PowerShell seguente.  
  

`Set-AdfsProperties -IntranetUseLocalClaimsProvider $true ` 
 
  
> [!IMPORTANT]  
> Si noti che se un elenco di provider di identità per una relying party è stata configurata, anche se è stata abilitata l'impostazione precedente e accede l'utente dalla rete intranet, ADFS viene comunque mostrata la pagina \(HRD\) di individuazione dell'area di autenticazione. Per ignorare HRD in questo caso, è necessario assicurarsi che "Active Directory" viene anche aggiunto all'elenco IDP per questa relying party.  

## <a name="additional-references"></a>Riferimenti aggiuntivi 
[AD FS Sign-personalizzazione utente](AD-FS-user-sign-in-customization.md)  
