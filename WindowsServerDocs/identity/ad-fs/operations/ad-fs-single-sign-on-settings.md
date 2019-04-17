---
ms.assetid: 1a443181-7ded-4912-8e40-5aa447faf00c
title: AD FS 2016 Single Sign-On impostazioni
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 08/17/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e8c24399949efc1b8d0b1782e299593c02374c62
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="ad-fs-single-sign-on-settings"></a>AD ADFS Single Sign-On impostazioni

>Si applica a: Windows Server 2016, Windows Server 2012 R2

Single Sign-On (SSO) consente agli utenti di eseguire l'autenticazione una sola volta e accedere alle risorse più senza dover specificare credenziali aggiuntive.  Questo articolo descrive l'impostazione predefinita, il comportamento di ADFS per SSO, nonché le impostazioni di configurazione che consentono di personalizzare questo comportamento.  

## <a name="supported-types-of-single-sign-on"></a>Tipi supportati di Single Sign-On

ADFS supporta vari tipi di esperienze Single Sign-On:  
  
-   **Sessione SSO**  
  
     I cookie SSO di sessione vengono scritti per l'utente autenticato che elimina ulteriori richieste quando l'utente passa applicazioni durante una sessione specifica. Tuttavia, se termina una sessione specifica, l'utente verrà richiesto di fornire le credenziali nuovamente.  
  
     AD FS verrà sessione i cookie SSO per impostazione predefinita se non sono registrati i dispositivi degli utenti. Se la sessione del browser è scaduta e viene riavviata, il cookie di sessione viene eliminato e non è valido più.  
  
-   **Accesso SSO persistente**  
  
     I cookie SSO permanenti vengono scritti per l'utente autenticato che elimina ulteriori richieste quando l'utente per le applicazioni, purché il cookie SSO persistente è valido. La differenza tra permanente SSO e sessione SSO è che è possibile mantenere l'accesso SSO persistente tra diverse sessioni.  
  
     AD FS imposterà il cookie SSO permanenti se il dispositivo è registrato. AD FS verrà impostato un cookie SSO persistente anche se un utente seleziona l'opzione "Mantieni l'accesso". Se non è più valido il cookie SSO persistente, verranno rifiutato ed eliminato.  
  
-   **SSO specifico dell'applicazione**  
  
     Nello scenario OAuth, un token di aggiornamento viene utilizzato per gestire lo stato SSO dell'utente all'interno dell'ambito di una particolare applicazione.  
  
     Se un dispositivo viene registrato, AD FS verrà impostato l'ora di scadenza di un token di aggiornamento in base alla durata di cookie SSO persistente per un dispositivo registrato, ovvero 7 giorni per impostazione predefinita. Se un utente seleziona l'opzione "Mantieni l'accesso", la durata di cookie SSO persistente per "mi registrato nel mantenere" sarà uguale a ora di scadenza del token di aggiornamento che è 1 giorno per impostazione predefinita con massimo di sette giorni. In caso contrario, aggiornare durata dei token è uguale a SSO cookie durata della sessione che è passate 8 ore per impostazione predefinita  
  
 Come indicato in precedenza, gli utenti di dispositivi registrati riceveranno sempre un accesso SSO persistente, a meno che l'accesso SSO persistente è disabilitato. Per i dispositivi di annullamento della registrazione, può essere ottenuto l'accesso SSO persistente abilitando la "Mantieni me connessi" funzionalità (KMSI). 
 
 Per Windows Server 2012 R2, per abilitare PSSO per lo scenario "Mantieni l'accesso", è necessario installare questo [hotfix](https://support.microsoft.com/en-us/kb/2958298/) anch ' esso in parte il di [agosto 2014 aggiornamento cumulativo per Windows RT 8.1, Windows 8.1 e Windows Server 2012 R2](https://support.microsoft.com/en-us/kb/2975719).   
 
  
## <a name="single-sign-on-and-authenticated-devices"></a>Single Sign-On e dispositivi autenticati  
Dopo avere fornito le credenziali per la prima volta, per impostazione predefinita gli utenti con dispositivi registrati ottengono single Sign-On per un periodo massimo di 90 giorni, condizione che usano il dispositivo di accedere alle risorse di AD FS almeno una volta ogni 14 giorni.  Se attendono 15 giorni dopo avere fornito le credenziali, gli utenti verranno richiesto nuovamente le credenziali.  

L'accesso SSO persistente è abilitata per impostazione predefinita. Se è disabilitato, non verrà scritto alcun cookie PSSO. |  

``` powershell
Set-AdfsProperties –EnablePersistentSso <Boolean\>
```     
  
Finestra di utilizzo del dispositivo (14 giorni per impostazione predefinita) è disciplinata dalla proprietà ADFS **DeviceUsageWindowInDays**.

``` powershell
Set-AdfsProperties -DeviceUsageWindowInDays
```   
Il single Sign-On periodo massimo (90 giorni per impostazione predefinita) è disciplinato dalla proprietà ADFS **PersistentSsoLifetimeMins**.

``` powershell
Set-AdfsProperties -PersistentSsoLifetimeMins
```    

## <a name="keep-me-signed-in-for-unauthenticated-devices"></a>Mantieni l'accesso per i dispositivi non autenticati 
Per i dispositivi non registrati, il periodo di single sign-on varia a seconda di **mantenere Me registrato In (KMSI)** le impostazioni delle funzionalità.  KMSI è disabilitato per impostazione predefinita e può essere abilitata impostando la proprietà di ADFS KmsiEnabled su True.

``` powershell
Set-AdfsProperties -EnableKmsi $true  
```    

Con KMSI disabilitato, il single sign-on periodo predefinito è 8 ore.  Questo può essere configurato con la proprietà SsoLifetime.  La proprietà viene misurata in minuti, in modo che il valore predefinito è 480.  

``` powershell
Set-AdfsProperties –SsoLifetime <Int32\> 
```   

Con KMSI abilitato, il single sign-on periodo predefinito è 24 ore.  Questo può essere configurato con la proprietà KmsiLifetimeMins.  La proprietà viene misurata in minuti, in modo che il valore predefinito è 1440.

``` powershell
Set-AdfsProperties –KmsiLifetimeMins <Int32\> 
```   

## <a name="multi-factor-authentication-mfa-behavior"></a>Comportamento di autenticazione a più fattori (MFA)  
È importante notare che, anche se fornire relativamente lunghi periodi di single sign-on, ADFS richiederà aggiuntiva authentication (autenticazione a più fattori) quando un precedente accesso era basata su credenziali primarie e non a più fattori, ma accedere corrente richiede autenticazione a più fattori.  Si tratta indipendentemente dalla configurazione SSO. Quando riceve una richiesta di autenticazione, AD FS, determina innanzitutto se esiste un contesto SSO (ad esempio, un cookie) e quindi, se è necessaria autenticazione a più fattori (ad esempio se la richiesta proviene all'esterno) valuterà o meno il contesto SSO contiene l'autenticazione a più fattori.  In caso contrario, viene richiesto l'autenticazione a più fattori.  


  
## <a name="psso-revocation"></a>Revoca PSSO  
 Per garantire sicurezza, AD FS rifiuteranno qualsiasi cookie SSO persistente emesso in precedenza quando vengono soddisfatte le condizioni seguenti. Ciò richiederà all'utente di fornire le proprie credenziali per eseguire l'autenticazione con ADFS. 
  
-   Modifica password dell'utente  
  
-   Impostazione SSO persistente è disabilitato in AD FS  
  
-   Dispositivo è stato disabilitato dall'amministratore in caso di perdita o furto  
  
-   AD FS riceve un cookie SSO persistente che viene eseguito per un utente registrato, ma l'utente o il dispositivo non è più registrato  
  
-   AD FS riceve un cookie SSO persistente per un utente registrato, ma l'utente registrato nuovamente  
  
-   AD FS riceve un cookie SSO persistente rilasciato in seguito a "Mantieni l'accesso" ma "Mantieni l'accesso" è disabilitata in ADFS  
  
-   AD FS riceve un cookie SSO persistente che viene eseguito per un utente registrato, ma il certificato di dispositivo è mancante o modificato durante l'autenticazione  
  
-   Amministratore di AD FS è impostato un tempo limito per l'accesso SSO persistente. Quando ciò è configurato, ADFS rifiuteranno qualsiasi cookie SSO persistente rilasciati prima di tale orario  
  
 Per impostare il tempo massimo, eseguire il cmdlet PowerShell seguente:  
  

``` powershell
Set-AdfsProperties -PersistentSsoCutoffTime <DateTime>
```
  
## <a name="enable-psso-for-office-365-users-to-access-sharepoint-online"></a>Abilitare PSSO per gli utenti di Office 365 accedere a SharePoint Online  
 Una volta PSSO è abilitato e configurato in ADFS, AD FS verrà scritto un cookie permanente dopo che un utente ha eseguito l'autenticazione. La volta successiva che l'utente entra in gioco, se un cookie permanente è ancora valido, un utente non è necessario eseguire nuovamente l'autenticazione delle credenziali. È anche possibile evitare la richiesta di autenticazione aggiuntivi per Office 365 e SharePoint Online degli utenti configurando i seguenti due attestazioni regole in ADFS per la persistenza di trigger in Microsoft Azure Active Directory e SharePoint Online.  Per abilitare PSSO per gli utenti di Office 365 accedere a SharePoint online, è necessario installare questo [degli aggiornamenti rapidi](https://support.microsoft.com/en-us/kb/2958298/) anch ' esso in parte il di [agosto 2014 aggiornamento cumulativo per Windows RT 8.1, Windows 8.1 e Windows Server 2012 R2](https://support.microsoft.com/en-us/kb/2975719).  
  
 Una regola di trasformazione rilascio per il passaggio attraverso l'attestazione InsideCorporateNetwork  
  
```  
@RuleTemplate = "PassThroughClaims"  
@RuleName = "Pass through claim - InsideCorporateNetwork"  
c:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork"]  
=> issue(claim = c);   
A custom Issuance Transform rule to pass through the persistent SSO claim  
@RuleName = "Pass Through Claim - Psso"  
c:[Type == "https://schemas.microsoft.com/2014/03/psso"]  
=> issue(claim = c);  
  
```
  
  
    


