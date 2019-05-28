---
ms.assetid: 1a443181-7ded-4912-8e40-5aa447faf00c
title: Impostazioni di Single Sign-On di AD FS 2016
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 08/17/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f3719277c80eae2bf2a4d923146920d17546601d
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188734"
---
# <a name="ad-fs-single-sign-on-settings"></a>AD ADFS Single Sign-On Settings

Single Sign-On (SSO) consente agli utenti di autenticarsi una sola volta e accedere alle risorse più senza che vengano richieste le credenziali aggiuntive.  Questo articolo descrive l'impostazione predefinita, il comportamento di AD FS per SSO, nonché le impostazioni di configurazione che consentono di personalizzare tale comportamento.  

## <a name="supported-types-of-single-sign-on"></a>Tipi supportati di Single Sign-On

ADFS supporta diversi tipi di esperienze Single Sign-On:  
  
-   **Sessione SSO**  
  
     I cookie di sessione SSO vengono scritti per l'utente autenticato che elimina altre richieste di conferma quando l'utente cambia le applicazioni durante una sessione specifica. Tuttavia, se una determinata sessione viene interrotta, l'utente verrà richiesto di fornire le credenziali nuovamente.  
  
     AD FS imposterà sessione i cookie SSO per impostazione predefinita se non sono registrati i dispositivi degli utenti. Se la sessione del browser è stata completata e viene riavviata, il cookie di sessione viene eliminato e non è valido più.  
  
-   **Accesso SSO persistente**  
  
     Cookie SSO permanenti vengono scritti per l'utente autenticato che elimina un'ulteriore richiesta quando l'utente passa per applicazioni, purché il cookie SSO persistente è valido. La differenza tra permanente sessione SSO e SSO è che è possibile mantenere l'accesso SSO persistente tra sessioni diverse.  
  
     AD FS imposterà cookie SSO permanenti se il dispositivo è registrato. AD FS verrà impostato un cookie SSO persistente anche se un utente seleziona l'opzione "Mantieni l'accesso". Se il cookie SSO persistente non è più valido, verranno rifiutata ed eliminata.  
  
-   **SSO specifico dell'applicazione**  
  
     Nello scenario di OAuth, un token di aggiornamento consente di mantenere lo stato SSO dell'utente all'interno dell'ambito di una determinata applicazione.  
  
     Se un dispositivo viene registrato, AD FS imposterà l'ora di scadenza dei token di aggiornamento in base alla durata di cookie SSO persistente per un dispositivo registrato, ovvero 7 giorni per impostazione predefinita per AD FS 2012 R2 e un massimo di 90 giorni con AD FS 2016 se usano del dispositivo accedere alle risorse di AD FS entro un periodo di 14 giorni. 

Se il dispositivo non è registrato, ma un utente seleziona l'opzione "Mantieni l'accesso", l'ora di scadenza del token di aggiornamento sarà uguale alla durata dei cookie SSO persistente per "Mantieni che l'accesso" è 1 giorno per impostazione predefinita con numero massimo di 7 giorni. In caso contrario, aggiornare la durata dei token è uguale a SSO cookie durata della sessione che è passate 8 ore per impostazione predefinita  
  
 Come indicato in precedenza, gli utenti in dispositivi registrati otterranno sempre un accesso SSO persistente, a meno che l'accesso SSO persistente è disabilitato. Per i dispositivi non registrati, è possibile ottenere l'accesso SSO persistente abilitando la "Mantieni l'accesso in" funzionalità (KMSI). 
 
 Per Windows Server 2012 R2, per abilitare PSSO per lo scenario "Mantieni l'accesso", è necessario eseguire l'installazione [hotfix](https://support.microsoft.com/en-us/kb/2958298/) anch ' esso in parte il di [aggiornamento cumulativo di agosto 2014 per Windows RT 8.1, Windows 8.1 e Windows Server 2012 R2](https://support.microsoft.com/en-us/kb/2975719).   

Attività | PowerShell | Descrizione
------------ | ------------- | -------------
Abilitare o disabilitare l'accesso SSO persistente | ```` Set-AdfsProperties –EnablePersistentSso <Boolean> ````| L'accesso SSO persistente è abilitato per impostazione predefinita. Se è disabilitato, non verrà scritto alcun cookie PSSO.
"Abilitare o disabilitare"Mantieni l'accesso " | ```` Set-AdfsProperties –EnableKmsi <Boolean> ```` | Funzionalità "Mantieni l'accesso" è disabilitata per impostazione predefinita. Se è abilitato, utenti finali potranno vedere un'opzione "Mantieni l'accesso" nella pagina di accesso AD FS



## <a name="ad-fs-2016---single-sign-on-and-authenticated-devices"></a>AD FS 2016 - Single Sign-On e dispositivi autenticati
AD FS 2016 passa il PSSO quando il richiedente esegue l'autenticazione da un dispositivo registrato, aumentando al numero massimo di 90 giorni, ma che richiedono un authenticvation entro un periodo di 14 giorni (finestra di utilizzo di dispositivi).
Dopo aver fornito le credenziali per la prima volta, per impostazione predefinita gli utenti con dispositivi registrati ottenere single Sign-On per un periodo massimo di 90 giorni, condizione che utilizzino il dispositivo per accedere alle risorse di AD FS almeno una volta ogni 14 giorni.  Se rimangono in attesa 15 giorni dopo l'immissione di credenziali, gli utenti verranno richiesto nuovamente le credenziali.  

L'accesso SSO persistente è abilitato per impostazione predefinita. Se è disabilitato, non verrà scritto alcun cookie PSSO. |  

``` powershell
Set-AdfsProperties –EnablePersistentSso <Boolean\>
```     
  
La finestra di utilizzo del dispositivo (14 giorni per impostazione predefinita) è disciplinata dalle proprietà di AD FS **DeviceUsageWindowInDays**.

``` powershell
Set-AdfsProperties -DeviceUsageWindowInDays
```   
Il single Sign-On di periodo massimo (90 giorni per impostazione predefinita) è disciplinato dalle proprietà di AD FS **PersistentSsoLifetimeMins**.

``` powershell
Set-AdfsProperties -PersistentSsoLifetimeMins
```    

## <a name="keep-me-signed-in-for-unauthenticated-devices"></a>Mantenere l'accesso per i dispositivi non autenticati 
Per i dispositivi non registrati, il periodo single sign-on è determinato dal **mantenere Me firmato In (KMSI)** impostazioni delle funzionalità.  KMSI è disabilitato per impostazione predefinita e può essere abilitata impostando la proprietà di AD FS KmsiEnabled su True.

``` powershell
Set-AdfsProperties -EnableKmsi $true  
```    

Con KMSI disabilitato, il single sign-on di periodo predefinito è 8 ore.  Ciò può essere configurato utilizzando la proprietà SsoLifetime.  La proprietà viene misurata in minuti, in modo che il valore predefinito è 480.  

``` powershell
Set-AdfsProperties –SsoLifetime <Int32\> 
```   

Con KMSI abilitata, il periodo predefinito di single sign-on è 24 ore.  Ciò può essere configurato utilizzando la proprietà KmsiLifetimeMins.  La proprietà viene misurata in minuti, in modo che il valore predefinito è 1440.

``` powershell
Set-AdfsProperties –KmsiLifetimeMins <Int32\> 
```   

## <a name="multi-factor-authentication-mfa-behavior"></a>Comportamento di multi-factor authentication (MFA)  
È importante notare che, fornendo relativamente lunghi periodi di single sign-in, AD FS verrà chiesta l'autenticazione aggiuntiva (multi-factor authentication) quando un accesso precedente si basava su credenziali primarie e non autenticazione a più fattori, ma di accesso corrente richiede l'autenticazione a più fattori.  Si tratta indipendentemente dalla configurazione di SSO. AD FS, quando viene ricevuta una richiesta di autenticazione, innanzitutto determina se è presente un contesto SSO (ad esempio un cookie) e quindi, se è richiesta la MFA (ad esempio se la richiesta proviene all'esterno) verrà valutato se il contesto SSO contiene autenticazione a più fattori.  In caso contrario, viene richiesto l'autenticazione a più fattori.  


  
## <a name="psso-revocation"></a>Revoca PSSO  
 Per garantire sicurezza, AD FS rifiuterà qualsiasi cookie SSO persistente emesso in precedenza quando vengono soddisfatte le condizioni seguenti. Questo richiederà all'utente di fornire le proprie credenziali per eseguire l'autenticazione con AD FS. 
  
-   Cambia la password utente  
  
-   Impostazione SSO persistente è disabilitata in AD FS  
  
-   Dispositivo è disabilitato dall'amministratore in caso di perdita o furto  
  
-   AD FS riceve un cookie SSO persistente che viene eseguito per un utente registrato, ma l'utente o il dispositivo non è più registrato  
  
-   AD FS riceve un cookie SSO persistente per un utente registrato ma registrati di nuovo l'utente  
  
-   AD FS riceve un cookie SSO persistente che viene generato come risultato "Mantieni l'accesso" ma "Mantieni l'accesso" impostazione è disabilitata in AD FS  
  
-   AD FS riceve un cookie SSO persistente che viene eseguito per un utente registrato ma certificato del dispositivo è mancante o modificata durante l'autenticazione  
  
-   Amministratore di AD FS è impostato un tempo di cambio data per l'accesso SSO persistente. Quando questo viene configurato, ADFS non verrà accettata qualsiasi cookie SSO persistente rilasciati prima dell'ora  
  
 Per impostare l'ora di cambio data, eseguire il cmdlet di PowerShell seguente:  
  

``` powershell
Set-AdfsProperties -PersistentSsoCutoffTime <DateTime>
```
  
## <a name="enable-psso-for-office-365-users-to-access-sharepoint-online"></a>Abilitare PSSO per gli utenti di Office 365 per accedere a SharePoint Online  
 Una volta PSSO sia abilitato e configurato in AD FS, ADFS scriverà un cookie persistente dopo che un utente è autenticato. La volta successiva che l'utente è disponibile in, se un cookie permanente è ancora valido, un utente non dovrà fornire le credenziali di nuovo l'autenticazione. È anche possibile evitare la richiesta di autenticazione aggiuntivi per Office 365 e SharePoint Online agli utenti configurando i seguenti due attestazioni le regole in AD FS per la persistenza di trigger in SharePoint Online e Microsoft Azure AD.  Per abilitare PSSO per gli utenti di Office 365 per accedere a SharePoint online, è necessario eseguire l'installazione [hotfix](https://support.microsoft.com/en-us/kb/2958298/) anch ' esso in parte il di [aggiornamento cumulativo di agosto 2014 per Windows RT 8.1, Windows 8.1 e Windows Server 2012 R2](https://support.microsoft.com/en-us/kb/2975719).  
  
 Una regola di trasformazione rilascio per pass-through di attestazione InsideCorporateNetwork  
  
```  
@RuleTemplate = "PassThroughClaims"  
@RuleName = "Pass through claim - InsideCorporateNetwork"  
c:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork"]  
=> issue(claim = c);   
A custom Issuance Transform rule to pass through the persistent SSO claim  
@RuleName = "Pass Through Claim - Psso"  
c:[Type == "http://schemas.microsoft.com/2014/03/psso"]  
=> issue(claim = c);  
  
```
  
Per riepilogare:
<table>
  <tr>
    <th colspan="1">Esperienza Single Sign on</th>
    <th colspan="3">ADFS 2012 R2 <br> Dispositivo registrato?</th>
        <th colspan="1"></th>
    <th colspan="3">ADFS 2016 <br> Dispositivo registrato?</th>
  </tr>

  <tr align="center">
    <th></th>
    <th>NO</th>
    <th>NO ma KMSI</th>
    <th>SÌ</th>
    <th></th>
    <th>NO</th>
    <th>NO ma KMSI</th>
    <th>SÌ</th>
  </tr>
 <tr align="center">
    <td>SSO = > impostare Token di aggiornamento = ></td>
    <td>8 ore</td>
    <td>N/D</td>
    <td>N/D</td>
    <th></th>
    <td>8 ore</td>
    <td>N/D</td>
    <td>N/D</td>
  </tr>

 <tr align="center">
    <td>PSSO = > impostare Token di aggiornamento = ></td>
    <td>N/D</td>
    <td>24 ore</td>
    <td>7 giorni</td>
    <th></th>
    <td>N/D</td>
    <td>24 ore</td>
    <td>Max 90 giorni con finestra 14 giorni</td>
  </tr>

 <tr align="center">
    <td>Durata dei token</td>
    <td>1 ore</td>
    <td>1 ore</td>
    <td>1 ore</td>
    <th></th>
    <td>1 ore</td>
    <td>1 ore</td>
    <td>1 ore</td>
  </tr>
</table>

**Dispositivo registrato?** Viene visualizzato un PSSO / l'accesso SSO persistente <br>
**Dispositivo non registrato?** Ottenere un accesso SSO <br>
**Dispositivi non registrati ma KMSI?** Viene visualizzato un PSSO / l'accesso SSO persistente <p>
SE:
 - [x] amministratore ha abilitato la funzionalità KMSI [AND]
 - [x] utente seleziona la casella di controllo KMSI nella pagina di accesso di form
 
**Good conoscere:** <br>
Gli utenti federati non la **LastPasswordChangeTimestamp** attributi sincronizzati vengono emessi cookie di sessione e i token di aggiornamento con un **valore Max Age di 12 ore**.<br>
Ciò si verifica perché Azure AD non è possibile determinare il momento revocare i token che sono correlati a una credenziale precedente (ad esempio una password che è stata modificata). Di conseguenza, Azure AD deve controllare più frequentemente per assicurarsi che l'utente e i token associati siano ancora in buona reputazione.
