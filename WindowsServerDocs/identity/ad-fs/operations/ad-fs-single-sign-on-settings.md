---
ms.assetid: 1a443181-7ded-4912-8e40-5aa447faf00c
title: Impostazioni di Single Sign-On di AD FS 2016
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 08/17/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 311789fdec160faeeeba0ecf26491d1e0cd6105d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407393"
---
# <a name="ad-fs-single-sign-on-settings"></a>Impostazioni di AD FS Single Sign-on

Single Sign-on (SSO) consente agli utenti di eseguire l'autenticazione una sola volta e di accedere a più risorse senza che venga richiesta l'immissione di credenziali aggiuntive.  Questo articolo descrive il comportamento predefinito del AD FS per SSO, nonché le impostazioni di configurazione che consentono di personalizzare questo comportamento.  

## <a name="supported-types-of-single-sign-on"></a>Tipi supportati di Single Sign-on

AD FS supporta diversi tipi di esperienze di Single Sign-on:  
  
-   **SSO sessione**  
  
     I cookie SSO della sessione vengono scritti per l'utente autenticato, che Elimina ulteriori richieste quando l'utente passa le applicazioni durante una determinata sessione. Tuttavia, se una determinata sessione termina, all'utente verranno richieste nuovamente le credenziali.  
  
     AD FS imposta i cookie SSO della sessione per impostazione predefinita se i dispositivi degli utenti non sono registrati. Se la sessione del browser è terminata e viene riavviata, il cookie di sessione viene eliminato e non è più valido.  
  
-   **SSO permanente**  
  
     I cookie SSO permanenti vengono scritti per l'utente autenticato, che Elimina ulteriori richieste quando l'utente passa le applicazioni fino a quando il cookie SSO persistente è valido. La differenza tra SSO permanente e SSO della sessione è che l'accesso SSO permanente può essere mantenuto tra sessioni diverse.  
  
     AD FS imposta i cookie SSO permanenti se il dispositivo è registrato. AD FS imposta anche un cookie SSO permanente se un utente seleziona l'opzione "Mantieni l'accesso". Se il cookie SSO persistente non è più valido, verrà rifiutato ed eliminato.  
  
-   **SSO specifico dell'applicazione**  
  
     Nello scenario OAuth, viene usato un token di aggiornamento per mantenere lo stato SSO dell'utente nell'ambito di una particolare applicazione.  
  
     Se un dispositivo è registrato, AD FS imposta l'ora di scadenza di un token di aggiornamento in base alla durata dei cookie SSO permanenti per un dispositivo registrato che è 7 giorni per impostazione predefinita per AD FS 2012R2 e fino a un massimo di 90 giorni con AD FS 2016 se usano il dispositivo per accedere alle risorse AD FS entro un intervallo di 14 giorni. 

Se il dispositivo non è registrato, ma un utente seleziona l'opzione "Mantieni l'accesso", l'ora di scadenza del token di aggiornamento sarà uguale alla durata dei cookie SSO permanenti per "Mantieni l'accesso", ovvero 1 giorno per impostazione predefinita con un massimo di 7 giorni. In caso contrario, la durata del token di aggiornamento equivale alla durata dei cookie SSO della sessione, che è 8 ore per impostazione predefinita  
  
 Come indicato in precedenza, gli utenti di dispositivi registrati riceveranno sempre un SSO permanente a meno che l'SSO permanente non sia disabilitato. Per i dispositivi non registrati, è possibile ottenere un SSO permanente abilitando la funzionalità "Mantieni l'accesso" (KMSI). 
 
 Per Windows Server 2012 R2, per abilitare ACCAVALLARE per lo scenario "Mantieni l'accesso", è necessario installare questo [hotfix](https://support.microsoft.com/en-us/kb/2958298/) che fa anche parte dell' [aggiornamento cumulativo di agosto 2014 per Windows RT 8,1, Windows 8.1 e Windows Server 2012 R2](https://support.microsoft.com/en-us/kb/2975719).   

Attività | PowerShell | Descrizione
------------ | ------------- | -------------
Abilita/Disabilita SSO permanente | ```` Set-AdfsProperties –EnablePersistentSso <Boolean> ````| L'accesso SSO permanente è abilitato per impostazione predefinita. Se è disabilitato, non verrà scritto alcun cookie ACCAVALLARE.
"Abilita/Disabilita" Mantieni l'accesso " | ```` Set-AdfsProperties –EnableKmsi <Boolean> ```` | Per impostazione predefinita, la funzionalità "Mantieni l'accesso" è disabilitata. Se è abilitata, l'utente finale visualizzerà una scelta "Mantieni l'accesso" nella pagina di accesso AD FS



## <a name="ad-fs-2016---single-sign-on-and-authenticated-devices"></a>AD FS 2016-Single Sign-on e dispositivi autenticati
AD FS 2016 modifica ACCAVALLARE quando il richiedente esegue l'autenticazione da un dispositivo registrato che aumenta fino a un massimo di 90 giorni, ma richiede un'autenticazione entro 14 giorni (finestra Utilizzo dispositivo).
Dopo aver fornito le credenziali per la prima volta, per impostazione predefinita gli utenti con dispositivi registrati ottengono l'accesso Single Sign-on per un periodo massimo di 90 giorni, purché usino il dispositivo per accedere alle risorse AD FS almeno una volta ogni 14 giorni.  Se attendono 15 giorni dopo aver fornito le credenziali, agli utenti verranno richieste nuovamente le credenziali.  

L'accesso SSO permanente è abilitato per impostazione predefinita. Se è disabilitato, non verrà scritto alcun cookie ACCAVALLARE. |  

``` powershell
Set-AdfsProperties –EnablePersistentSso <Boolean\>
```     
  
La finestra Utilizzo dispositivo (14 giorni per impostazione predefinita) è regolata dalla proprietà AD FS **DeviceUsageWindowInDays**.

``` powershell
Set-AdfsProperties -DeviceUsageWindowInDays
```   
Il periodo di Single Sign-on massimo (90 giorni per impostazione predefinita) è regolato dalla proprietà AD FS **PersistentSsoLifetimeMins**.

``` powershell
Set-AdfsProperties -PersistentSsoLifetimeMins
```    

## <a name="keep-me-signed-in-for-unauthenticated-devices"></a>Mantieni l'accesso per i dispositivi non autenticati 
Per i dispositivi non registrati, il periodo di Single Sign-On è determinato dalle impostazioni della funzionalità **Mantieni l'accesso (KMSI)** .  KMSI è disabilitato per impostazione predefinita e può essere abilitato impostando la proprietà AD FS KmsiEnabled su true.

``` powershell
Set-AdfsProperties -EnableKmsi $true  
```    

Con KMSI disabilitato, il periodo di Single Sign-On predefinito è 8 ore.  Questa configurazione può essere eseguita usando la proprietà SsoLifetime.  La proprietà viene misurata in minuti, quindi il valore predefinito è 480.  

``` powershell
Set-AdfsProperties –SsoLifetime <Int32\> 
```   

Con KMSI abilitato, il periodo di Single Sign-On predefinito è 24 ore.  Questa configurazione può essere eseguita usando la proprietà KmsiLifetimeMins.  La proprietà viene misurata in minuti, quindi il valore predefinito è 1440.

``` powershell
Set-AdfsProperties –KmsiLifetimeMins <Int32\> 
```   

## <a name="multi-factor-authentication-mfa-behavior"></a>Comportamento di multi-factor authentication  
È importante tenere presente che, durante la fornitura di periodi relativamente lunghi di Single Sign-on, AD FS richiederà l'autenticazione aggiuntiva (multi-factor authentication) quando un accesso precedente si basa su credenziali primarie e non su autenticazione a più fattori, ma l'accesso corrente richiede l'autenticazione a più fattori.  Indipendentemente dalla configurazione di SSO. AD FS, quando riceve una richiesta di autenticazione, determina innanzitutto se è presente o meno un contesto SSO (ad esempio un cookie) e quindi, se è richiesta l'autenticazione a più fattori (ad esempio se la richiesta è in arrivo dall'esterno), valuterà se il contesto SSO contiene o meno l'autenticazione a più fattori.  In caso contrario, viene richiesta l'autenticazione a più fattori.  


  
## <a name="psso-revocation"></a>Revoca ACCAVALLARE  
 Per proteggere la sicurezza, AD FS rifiuterà qualsiasi cookie SSO permanente emesso in precedenza quando vengono soddisfatte le condizioni seguenti. In questo modo l'utente deve fornire le proprie credenziali per eseguire nuovamente l'autenticazione con AD FS. 
  
- Password modifiche utente  
  
- L'impostazione SSO permanente è disabilitata in AD FS  
  
- Il dispositivo è disabilitato dall'amministratore in caso di smarrimento o furto  
  
- AD FS riceve un cookie SSO permanente che viene emesso per un utente registrato, ma l'utente o il dispositivo non è più registrato  
  
- AD FS riceve un cookie SSO permanente per un utente registrato, ma l'utente ha nuovamente registrato  
  
- AD FS riceve un cookie SSO permanente che viene emesso come risultato dell'impostazione "Mantieni l'accesso" ma "Mantieni l'accesso" è disabilitato in AD FS  
  
- AD FS riceve un cookie SSO permanente emesso per un utente registrato, ma il certificato del dispositivo è mancante o modificato durante l'autenticazione  
  
- AD FS amministratore ha impostato un limite di tempo per l'accesso SSO permanente. Quando questa configurazione è configurata, AD FS rifiuterà qualsiasi cookie SSO permanente emesso prima di questa ora  
  
  Per impostare l'ora di interruzione, eseguire il cmdlet PowerShell seguente:  
  

``` powershell
Set-AdfsProperties -PersistentSsoCutoffTime <DateTime>
```
  
## <a name="enable-psso-for-office-365-users-to-access-sharepoint-online"></a>Abilitare ACCAVALLARE per gli utenti di Office 365 per accedere a SharePoint Online  
 Quando ACCAVALLARE viene abilitato e configurato in AD FS, AD FS scriverà un cookie persistente dopo l'autenticazione dell'utente. Al successivo arrivo dell'utente, se un cookie persistente è ancora valido, un utente non deve fornire le credenziali per l'autenticazione. È anche possibile evitare la richiesta di autenticazione aggiuntiva per gli utenti di Office 365 e SharePoint Online configurando le seguenti due regole attestazioni in AD FS per attivare la persistenza in Microsoft Azure AD e SharePoint Online.  Per consentire agli utenti di ACCAVALLARE per Office 365 di accedere a SharePoint Online, è necessario installare questo [hotfix](https://support.microsoft.com/en-us/kb/2958298/) che fa anche parte dell' [aggiornamento cumulativo di agosto 2014 per Windows RT 8,1, Windows 8.1 e Windows Server 2012 R2](https://support.microsoft.com/en-us/kb/2975719).  
  
 Una regola di trasformazione rilascio per passare attraverso l'attestazione InsideCorporateNetwork  
  
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
    <th colspan="1">Esperienza Sign-on singola</th>
    <th colspan="3">ADFS 2012 R2 <br> Il dispositivo è registrato?</th>
        <th colspan="1"></th>
    <th colspan="3">ADFS 2016 <br> Il dispositivo è registrato?</th>
  </tr>

  <tr align="center">
    <th></th>
    <th>NO</th>
    <th>NO, ma KMSI</th>
    <th>SÌ</th>
    <th></th>
    <th>NO</th>
    <th>NO, ma KMSI</th>
    <th>SÌ</th>
  </tr>
 <tr align="center">
    <td>SSO =&gt;imposta token di aggiornamento =&gt;</td>
    <td>8 ore</td>
    <td>N/D</td>
    <td>N/D</td>
    <th></th>
    <td>8 ore</td>
    <td>N/D</td>
    <td>N/D</td>
  </tr>

 <tr align="center">
    <td>Accavallare =&gt;imposta token di aggiornamento =&gt;</td>
    <td>N/D</td>
    <td>24 ore</td>
    <td>7 giorni</td>
    <th></th>
    <td>N/D</td>
    <td>24 ore</td>
    <td>Massimo 90 giorni con la finestra 14 giorni</td>
  </tr>

 <tr align="center">
    <td>Durata del token</td>
    <td>1 ore</td>
    <td>1 ore</td>
    <td>1 ore</td>
    <th></th>
    <td>1 ore</td>
    <td>1 ore</td>
    <td>1 ore</td>
  </tr>
</table>

**Dispositivo registrato?** Si ottiene un SSO ACCAVALLARE/persistente <br>
**Dispositivo non registrato?** Si ottiene un SSO <br>
**Dispositivo non registrato ma KMSI?** Si ottiene un SSO ACCAVALLARE/persistente <p>
SE
 - [x] l'amministratore ha abilitato la funzionalità KMSI [e]
 - [x] l'utente fa clic sulla casella di controllo KMSI nella pagina di accesso ai moduli
 
**Bene conoscere:** <br>
Gli utenti federati che non dispongono dell'attributo **LastPasswordChangeTimestamp** sincronizzato vengono rilasciati i cookie di sessione e i token di aggiornamento con un **valore di validità massimo di 12 ore**.<br>
Questo problema si verifica perché Azure AD non è in grado di determinare quando revocare i token correlati a una credenziale precedente, ad esempio una password modificata. Pertanto, Azure AD necessario controllare con maggiore frequenza per assicurarsi che l'utente e i token associati siano ancora in esecuzione.
