---
ms.assetid: 777aab65-c9c7-4dc9-a807-9ab73fac87b8
title: Configurare la protezione di blocco Extranet di AD FS
description: ''
author: billmath
ms.author: billmath
manager: mtilman
ms.date: 02/20/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 904b563da2f1404d873c7352db9eadb7bfe252f2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869762"
---
# <a name="ad-fs-extranet-lockout-and-extranet-smart-lockout"></a>AD FS Extranet Smart Lockout e blocco Extranet

# <a name="overview"></a>Panoramica

>Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

In AD FS in Windows Server 2012 R2, abbiamo introdotto una funzionalità di sicurezza chiamata [blocco Extranet Soft](configure-ad-fs-extranet-soft-lockout-protection.md).  Con questa funzionalità, ADFS viene interrotta l'autenticazione degli utenti dalla rete extranet per un periodo di tempo.  Ciò impedisce che gli account utente di essere bloccati in Active Directory. Oltre a proteggere gli utenti da un blocco degli account di Active Directory, blocco della extranet di AD FS anche protegge contro gli attacchi di individuazione delle password, attacchi di forza bruta.

In giugno 2018, ADFS in Windows Server 2016 ha introdotto **Extranet blocco Smart (ESL)**.  ESL consente AD FS distinguere tra i tentativi di accesso che sono simili da parte dell'utente valido e accessi da ciò che può essere un utente malintenzionato. Di conseguenza, ADFS può bloccare gli utenti malintenzionati, consentendo al contempo valid users di continuare a usare i propri account. Ciò impedisce di tipo denial of service per l'utente e consente di proteggere da attacchi mirati, ad esempio attacchi di "password-spray".  
ESL è disponibile per AD FS in Windows Server 2016 ed è incorporata in ADFS in Windows Server 2019.

> [!NOTE]
> Questa funzionalità funziona solo per i **scenario extranet** in cui le richieste di autenticazione sempre passare attraverso il Proxy applicazione Web e si applica solo ai **autenticazione nome utente e password**.

## <a name="advantages-of-extranet-smart-lockout-in-ad-fs-2016"></a>Vantaggi di blocco Smart Extranet di AD FS 2016
Blocco della Extranet soft in AD FS 2012 R2 fornito i vantaggi chiave seguenti:
- Consente di proteggere gli account utente dalla **attacchi di forza bruta** in cui un utente malintenzionato tenta di indovinare la password dell'utente inviando in modo continuo le richieste di autenticazione e dal **attacchi spray password** dove gli utenti malintenzionati provano a usare le password più comuni con numerosi account diversi
- Consente di proteggere gli account utente dalla **blocco degli account di Active Directory** dalle richieste di autenticazione dannoso con password errata. In questo caso, anche se l'account utente verrà bloccato per l'accesso extranet, l'utente può ancora accedere ad Active Directory dalla rete aziendale. Questo è noto come un **blocco soft**.

Compilazioni di blocco Smart Extranet i vantaggi di blocco della extranet soft aggiungendo il codice seguente:
- Impedisce che gli utenti verificano **blocco account extranet** dalle richieste di autenticazione dannoso.  Il blocco smart impedirà richieste potenzialmente dannose da posizioni non note, consentendo l'utente accedere dalla extranet da posizioni note (posizioni da cui l'utente ha effettuato correttamente nella prima).
- Include una modalità di solo registro in modo che il sistema di altre attività di accesso Sign-on ottimale e potenzialmente dannosi senza disabilitare tutti gli account

## <a name="additional-advantages-of-extranet-smart-lockout-in-ad-fs-2019"></a>Vantaggi aggiuntivi di blocco Smart Extranet di AD FS 2019
Blocco intelligente Extranet di AD FS 2019 aggiunge i vantaggi seguenti rispetto ad AD FS 2016:
- Imposta valori soglia di blocco indipendente per percorsi noti e familiarità in modo che gli utenti in modo ottimale noti possono avere più spazio per l'errore rispetto alle richieste da località sospetta
- Abilitare la modalità di controllo per blocco smart pur continuando a imporre un comportamento di blocco temporanea precedente

## <a name="pre-requisites-for-extranet-smart-lockout-in-ad-fs-2016"></a>Prerequisiti per il blocco Smart Extranet di AD FS 2016
I seguenti prerequisiti sono necessari per ESL con AD FS 2019.

### <a name="install-updates-on-all-nodes-in-the-farm"></a>Installare gli aggiornamenti in tutti i nodi nella farm
In primo luogo, assicurarsi che tutti i server AD FS di Windows Server 2016 vengono aggiornati a partire da giugno 2018 aggiornamenti Windows e che la farm AD FS 2016 sia in esecuzione a livello di comportamento farm 2016.

### <a name="update-artifact-database-permissions"></a>Aggiornare le autorizzazioni di database dell'artefatto
Blocco intelligente Extranet richiede l'account del servizio ADFS di disporre delle autorizzazioni per una nuova tabella nel database dell'artefatto di ad FS.  Concedere questa autorizzazione eseguendo il comando seguente in una finestra di comando di PowerShell:
``` powershell
PS C:\>$cred = Get-Credential
PS C:\>Update-AdfsArtifactDatabasePermission -Credential $cred
```
In cui `$cred` è un account con autorizzazioni di amministratore di AD FS (autorizzazioni di amministratore di AD FS sono necessarie per rendere il database di modifica).

>[!NOTE]
>In una farm con più server che usa il database interno di Windows, il cmdlet precedente richiede che la gestione remota Windows sia abilitato in ogni server AD FS

Se non hai le autorizzazioni di amministratore di AD FS, è possibile configurare le autorizzazioni del database manualmente in SQL o WID eseguendo il comando seguente quando si è connessi al database AdfsArtifactStore.
```
sp_addrolemember 'db_owner', 'db_genevaservice'
```
### <a name="ensure-ad-fs-security-audit-logging-is-enabled"></a>Verificare che sia abilitata la registrazione di controllo di sicurezza AD FS
Questa funzionalità viene utilizzato Security Audit log, pertanto, il controllo deve essere abilitato in AD FS, nonché i criteri locali in tutti i server AD FS.

## <a name="pre-requisites-for-extranet-smart-lockout-in-ad-fs-2019"></a>Prerequisiti per il blocco Smart Extranet in AD FS 2019
I seguenti prerequisiti sono necessari per ESL con AD FS 2016.

### <a name="ensure-ad-fs-security-audit-logging-is-enabled"></a>Verificare che sia abilitata la registrazione di controllo di sicurezza AD FS
Questa funzionalità viene utilizzato Security Audit log, pertanto, il controllo deve essere abilitato in AD FS, nonché i criteri locali in tutti i server AD FS.

## <a name="lockout-settings"></a>Impostazioni di blocco
Blocco intelligente Extranet è costituito da un set di nuove funzionalità disciplinato dalle proprietà di AD FS nuove ed esistenti.

### <a name="extranet-lockout-enabled"></a>Blocco Extranet abilitato
Blocco Extranet smart Usa la stessa proprietà di AD FS che in precedenza è stata usata semplicemente per controllare il blocco della extranet "soft".  La proprietà viene chiamata ExtranetLockoutEnabled e può essere visualizzata tramite Get-AdfsProperties.

### <a name="extranet-smart-lockout-mode"></a>Modalità di blocco Smart Extranet
Una nuova proprietà di AD FS denominata ExtranetLockoutMode aggiunti per controllare il comportamento di blocco "soft" smart Visual Studio.  È possibile impostare tramite Set-AdfsProperties e contiene 3 valori:

    - **ADPasswordCounter** – si tratta di legacy in modalità "blocco extranet soft" di ad FS che non consente di distinguere in base alla posizione.  Rappresenta il valore predefinito.

    - **ADFSSmartLockoutLogOnly** : si tratta di blocco Smart Extranet, ma anziché rifiutare le richieste di autenticazione, AD FS sarà solo scrivere admin e controllo di eventi.

    - **ADFSSmartLockoutEnforce** -si tratta di blocco Smart Extranet con supporto completo per il blocco poco familiare richieste quando vengono raggiunte le soglie.

In AD FS 2019, i valori ADPasswordCounter e ADFSSmartLockoutLogOnly possono essere combinati in modo tale blocco temporanea continua a essere applicato anche se si sta preparando per il blocco smart.

### <a name="lockout-threshold-and-observation-window"></a>Soglia di blocco e finestra di osservazione
Il blocco smart in AD FS 2019 Usa lo stesso due proprietà di AD FS come blocco temporanea usata in precedenza: ExtranetObservationWindow ed ExtranetLockoutThreshold.

- **ExtranetLockoutThreshold &lt;Integer&gt;**  definisce il numero massimo di tentativi con password errate. Quando viene raggiunta la soglia, in ADFSSmartLockoutEnforce modalità ADFS rifiuterà le richieste dalla extranet fino a quando non ha passato la finestra di osservazione.  Nella modalità ADFSSmartLockoutLogOnly, ADFS verrà scritto solo le voci di log.  
- **ExtranetObservationWindow &lt;TimeSpan&gt;**  determina per quanto tempo il nome utente e la password verranno bloccate le richieste provenienti da posizioni non note. AD FS inizierà a eseguire nuovamente l'autenticazione di nome utente e password quando viene passata la finestra.

> [!NOTE]
> Funzioni il blocco della extranet di AD FS in modo indipendente dai criteri di blocco di AD. È consigliabile impostare il **ExtranetLockoutThreshold** valore del parametro su un valore che è inferiore alla soglia di blocco degli account AD. Se non si esegue questa operazione comporta in AD FS sia in grado di proteggere gli account da essere bloccati in Active Directory. 

In AD FS 2019, è stata introdotta una nuova soglia di blocco specifica per percorsi noti buona: ExtranetLockoutThresholdFamiliarLocation.
- **ExtranetLockoutThresholdFamiliarLocation &lt;Integer&gt;**  definisce il numero massimo di tentativi con password errate da posizioni note. In AD FS 2019, il parametro originale ExtranetLockoutThreshold si applica a posizioni non note (indirizzi IP noti non sia valida).

### <a name="primary-domain-controller-requirement"></a>Requisito di Controller di dominio primario
AD FS 2016 offre un parametro che consente il fallback a un altro controller di dominio quando il PDC non è disponibile.

- **ExtranetLockoutRequirePDC &lt;booleana&gt;**  quando abilitato, il blocco della extranet richiede un controller di dominio primario (PDC). Se disabilitato, il blocco della extranet eseguirà il fallback a un altro controller di dominio nel caso in cui il PDC non è disponibile.

   Nell'esempio seguente mostra il cmdlet per abilitare il blocco il requisito del PDC disabilitato:

    ```powershell
    Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30) -ExtranetLockoutRequirePDC $false
    ```

## <a name="configuring-ad-fs-with-smart-lockout-in-log-only-mode"></a>Configurazione di AD FS con il blocco Smart in modalità solo Log

### <a name="ad-fs-2016"></a>AD FS 2016
È consigliabile impostare prima di tutto il comportamento di blocco per registrare solo eseguendo il cmdlet seguente:

 ```powershell
PS C:\>Set-AdfsProperties -ExtranetLockoutMode AdfsSmartlockoutLogOnly
 ```

In questa modalità, ADFS consente di popolare le informazioni di posizione nota agli utenti e scrive gli eventi di controllo di sicurezza, ma non blocca le richieste.  Questa modalità viene utilizzata per convalidare che il blocco smart è in esecuzione e per abilitare AD FS per "informazioni" posizioni note per gli utenti prima di abilitare "Applica" modalità.
Come AD FS apprende, archivia le attività di accesso per utente (se in modalità solo log o applicare la modalità). 

>[!NOTE]
>La configurazione `ExtranetLockoutMode` al `AdfsSmartlockoutLogOnly` comportamento causato da tale legacy AD FS "soft blocco extranet" non è più essere in effetti, anche se il `EnableExtranetLockout` è impostata su True.  Ciò significa che gli utenti che superano le soglie di blocco degli indirizzi IP noti o non conosciuti da non essere bloccati da AD FS Smart Lockout. Tuttavia, in locale AD potrebbe blocco dell'utente in base alla configurazione non esiste.   Vedi [criterio di blocco Account](https://docs.microsoft.com/windows/security/threat-protection/security-policy-settings/account-lockout-policy) per informazioni su come dall'ambiente locale AD possibile blocco degli utenti. "  Si tratta di uno stato temporaneo in modo che il sistema può apprendere il comportamento di account di accesso prima reintroduzione imposizione del blocco con il nuovo comportamento di blocco smart.

Per la nuova modalità rendere effettive, riavviare il servizio AD FS in tutti i nodi nella farm
  
  ``` powershell
PS C:\>Restart-service adfssrv
  ```
Una volta configurata la modalità, è possibile abilitare il blocco smart utilizzando il `EnableExtranetLockout` parametro


``` powershell
PS C:\>Set-AdfsProperties -EnableExtranetLockout $true
```

Si noti che è possibile utilizzare lo stesso cmdlet per disabilitare il blocco

Esempio: Disabilitare il blocco

``` powershell
PS C:\>Set-AdfsProperties -EnableExtranetLockout $false
```
### <a name="ad-fs-2019"></a>AD FS 2019
Se non attualmente si usa AD FS Extranet Lockout temporanea, è consigliabile seguire le stesse linee guida per AD FS 2016 precedente.
Se si usa blocco soft, tuttavia, è consigliabile impostare il comportamento di blocco di AD FS 2019 da registrare per il blocco smart, ma mantiene l'applicazione di blocchi soft, l'uso di powershell seguente:

 ```powershell
PS C:\>Set-AdfsProperties -ExtranetLockoutMode 3
 ```

Quando si esegue questo cmdlet, è possibile utilizzare Get-AdfsProperties quindi eseguire una query il valore della proprietà ExtranetLockoutMode AD FS.  Si noterà che il relativo valore è stato aggiornato a una combinazione bit per bit di ADPasswordCounter e ADFSSmartLockoutLogOnly.

## <a name="observing-audit-events"></a>Osservando gli eventi di controllo
AD FS scriverà gli eventi di blocco extranet per il log di controllo di sicurezza:
-   Quando un utente viene bloccato out (raggiunge la soglia di blocco per i tentativi di accesso non riuscito)
-   Quando AD FS riceve un tentativo di accesso per gli utenti che si trova già in stato di blocco

In modalità solo log, è possibile controllare il log di controllo di sicurezza per gli eventi di blocco.  Per tutti gli eventi disponibili, è possibile controllare lo stato utente usando il cmdlet Get-ADFSAccountActivity per determinare se il blocco si sono verificati da indirizzi IP noti o non conosciuti e per controllare l'elenco di indirizzi IP noti per l'utente.

Evento di esempio:
```
Log Name:      Security
Source:        AD FS Auditing
Date:          5/21/2018 12:55:59 AM
Event ID:      1210
Task Category: (3)
Level:         Information
Keywords:      Classic,Audit Failure
User:          CONTOSO\adfssvc
Computer:      ADFS2016FS1.corp.contoso.com
Description:
An extranet lockout event has occurred. See XML for failure details. 

Activity ID: fa7a8052-0694-48f0-84e2-b51cde40ac3d 

Additional Data 
XML: <?xml version="1.0" encoding="utf-16"?>
<AuditBase xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="ExtranetLockoutAudit">
  <AuditType>ExtranetLockout</AuditType>
  <AuditResult>Failure</AuditResult>
  <FailureType>ExtranetLockoutError</FailureType>
  <ErrorCode>AccountRestrictedAudit</ErrorCode>
  <ContextComponents>
    <Component xsi:type="ResourceAuditComponent">
      <RelyingParty>http://fs.contoso.com/adfs/services/trust</RelyingParty>
      <ClaimsProvider>N/A</ClaimsProvider>
      <UserId>CONTOSO\user</UserId>
    </Component>
    <Component xsi:type="RequestAuditComponent">
      <Server>N/A</Server>
      <AuthProtocol>WSFederation</AuthProtocol>
      <NetworkLocation>Extranet</NetworkLocation>
      <IpAddress>64.187.173.10</IpAddress>
      <ForwardedIpAddress>64.187.173.10</ForwardedIpAddress>
      <ProxyIpAddress>N/A</ProxyIpAddress>
      <NetworkIpAddress>N/A</NetworkIpAddress>
      <ProxyServer>ADFS2016PROXY2</ProxyServer>
      <UserAgentString>Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/66.0.3359.181 Safari/537.36</UserAgentString>
      <Endpoint>/adfs/ls/</Endpoint>
    </Component>
    <Component xsi:type="LockoutConfigAuditComponent">
      <CurrentBadPasswordCount>5</CurrentBadPasswordCount>
      <ConfigBadPasswordCount>5</ConfigBadPasswordCount>
      <LastBadAttempt>05/21/2018 00:55:05</LastBadAttempt>
      <LockoutWindowConfig>00:30:00</LockoutWindowConfig>
    </Component>
  </ContextComponents>
</AuditBase>
Event Xml:
<Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
  <System>
    <Provider Name="AD FS Auditing" />
    <EventID Qualifiers="0">1210</EventID>
    <Level>0</Level>
    <Task>3</Task>
    <Keywords>0x8090000000000000</Keywords>
    <TimeCreated SystemTime="2018-05-21T00:55:59.921880300Z" />
    <EventRecordID>35521235</EventRecordID>
    <Channel>Security</Channel>
    <Computer>ADFS2016FS1.contoso.com</Computer>
    <Security UserID="S-1-5-21-1156273042-1594504307-2076964089-1104" />
  </System>
  <EventData>
    <Data>fa7a8052-0694-48f0-84e2-b51cde40ac3d</Data>
    <Data><?xml version="1.0" encoding="utf-16"?>
<AuditBase xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="ExtranetLockoutAudit">
  <AuditType>ExtranetLockout</AuditType>
  <AuditResult>Failure</AuditResult>
  <FailureType>ExtranetLockoutError</FailureType>
  <ErrorCode>AccountRestrictedAudit</ErrorCode>
  <ContextComponents>
    <Component xsi:type="ResourceAuditComponent">
      <RelyingParty>http://fs.contoso.com/adfs/services/trust</RelyingParty>
      <ClaimsProvider>N/A</ClaimsProvider>
      <UserId>CONTOSO\user</UserId>
    </Component>
    <Component xsi:type="RequestAuditComponent">
      <Server>N/A</Server>
      <AuthProtocol>WSFederation</AuthProtocol>
      <NetworkLocation>Extranet</NetworkLocation>
      <IpAddress>64.187.173.10</IpAddress>
      <ForwardedIpAddress>64.187.173.10</ForwardedIpAddress>
      <ProxyIpAddress>N/A</ProxyIpAddress>
      <NetworkIpAddress>N/A</NetworkIpAddress>
      <ProxyServer>ADFS2016PROXY2</ProxyServer>
      <UserAgentString>Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/66.0.3359.181 Safari/537.36</UserAgentString>
      <Endpoint>/adfs/ls/</Endpoint>
    </Component>
    <Component xsi:type="LockoutConfigAuditComponent">
      <CurrentBadPasswordCount>5</CurrentBadPasswordCount>
      <ConfigBadPasswordCount>5</ConfigBadPasswordCount>
      <LastBadAttempt>05/21/2018 00:55:05</LastBadAttempt>
      <LockoutWindowConfig>00:30:00</LockoutWindowConfig>
    </Component>
  </ContextComponents>
</AuditBase></Data>
  </EventData>
</Event>
```

## <a name="observing-user-activity"></a>Osservando l'attività degli utenti
ADFS offre i cmdlet di powershell per visualizzare e gestire i dati di attività account utente.  Leggere le attività di account attuale per un account utente.  Usare il cmdlet seguente

``` powershell
PS C:\>Get-ADFSAccountActivity user@contoso.com
```

Output di esempio
```
Identifier             : CONTOSO\user
BadPwdCountFamiliar    : 0
BadPwdCountUnknown     : 0
LastFailedAuthFamiliar : 1/1/0001 12:00:00 AM
LastFailedAuthUnknown  : 1/1/0001 12:00:00 AM
FamiliarLockout        : False
UnknownLockout         : False
FamiliarIps            : {}
```

L'output dell'attività corrente contiene i dati seguenti:

**Identificatore**: questo è il nome utente

**BadPwdCountFamiliar**: questo è il numero corrente di tentativi di accesso di password errata provenienti da indirizzi IP presenti nell'elenco di "FamiliarIps" al momento del tentativo

**BadPwdCountUnknown**: questo è il numero corrente di tentativi di accesso di password errata provenienti da indirizzi IP che non erano presenti all'interno delle "FamiliarIps" al momento del tentativo

**LastFailedAuthFamiliar**: questo è l'ora dell'ultimo tentativo di accesso password non corretta da un indirizzo IP presente nell'elenco di "FamiliarIps" al momento del tentativo

**LastFailedAuthUnknown**: questo è l'ora dell'ultimo tentativo di accesso password non corretta da un indirizzo IP non incluso nell'elenco di "FamiliarIps" al momento del tentativo

**FamiliarLockout**: indica se l'utente è attualmente in uno stato di blocco per tentativi con password corrette da indirizzi IP nell'elenco di "FamiliarIps" 

**UnknownLockout**: indica se l'utente è attualmente in uno stato di blocco per tentativi con password corrette da indirizzi IP non inclusi nell'elenco di "FamiliarIps" FamiliarIps: si tratta dell'elenco di indirizzi IP noti per l'utente corrente

## <a name="adjust-threshold-and-window"></a>Regola di soglia e finestra
Dopo che è stato eseguito in modalità solo log per un tempo sufficiente per AD FS per informazioni su percorsi di accesso, è possibile regolare la soglia o osservazione finestra dalle impostazioni predefinite.  Questa operazione viene eseguita usando `Set-AdfsProperties` come illustrato negli esempi seguenti:

L'intervallo di osservazione è impostato tramite `ExtranetObservationWindow`:

Esempio: 

``` powershell
PS C:\>Set-AdfsProperties -ExtranetObservationWindow ( new-timespan -minutes 30 )
```

Dove il valore è un intervallo di tempo

### <a name="setting-threshold-value-in-ad-fs-2016"></a>Impostare valore soglia in AD FS 2016
In AD FS 2016, la soglia viene impostata utilizzando ExtranetLockoutThreshold:

Esempio:

``` powershell
PS C:\>Set-AdfsProperties -ExtranetLockoutThreshold 5
```

### <a name="setting-threshold-values-in-ad-fs-2019"></a>Impostazione dei valori di soglia in AD FS 2019
In AD FS 2019, sono presenti valori di soglia distinti per percorsi noti familiarità e buona

Per impostare il valore di soglia per le posizioni non note, usare la stessa proprietà usata per AD FS 2016 sopra:

Esempio:

``` powershell
PS C:\>Set-AdfsProperties -ExtranetLockoutThreshold 5
```

Per impostare il valore di soglia per percorsi noti ottimale, usare la nuova proprietà ExtranetLockoutThresholdFamiliarLocation, come illustrato nell'esempio seguente:

Esempio:

``` powershell
PS C:\>Set-AdfsProperties -ExtranetLockoutThresholdFamiliarLocation 10
```


## <a name="enable-enforce-mode"></a>Applicare la modalità attiva
Dopo che è stato eseguito in modalità solo log per un tempo sufficiente per AD FS per altre posizioni di accesso e per osservare eventuali attività di blocco, e dopo avere familiarizzato con la finestra di osservazione e soglia di blocco, il blocco smart può essere spostato in "Applica" modalità tramite il PSH cmdlet riportato di seguito:

``` powershell
PS C:\>Set-AdfsProperties -ExtranetLockoutMode AdfsSmartLockoutEnforce
```

Per la nuova modalità rendere effettive, riavviare il servizio AD FS in tutti i nodi nella farm

``` powershell
PS C:\>Restart-service adfssrv
```


## <a name="manage-user-account-activity"></a>Gestire le attività di Account utente
ADFS offre 3 cmdlet per gestire i dati di attività account utente.  Questi cmdlet connettono automaticamente al nodo della farm che detiene il ruolo di master (anche se questo comportamento può essere sottoposto a override, passando il parametro - Server).

> [!NOTE] 
> Per informazioni sulla delega delle autorizzazioni per l'uso di questi cmdlet, vedere [delegato AD FS Powershell cmdlet di accesso agli utenti senza privilegi di amministratore](delegate-ad-fs-pshell-access.md)

Questi cmdlet sono:

`Get-ADFSAccountActivity`

Leggere le attività di account attuale per un account utente.  Il cmdlet si connette sempre automaticamente al master farm usando l'endpoint REST di attività Account, in modo che tutti i dati siano sempre coerenti

``` powershell
Get-ADFSAccountActivity user@contoso.com
```
`
Set-ADFSAccountActivity
`

Aggiornare l'attività di account per un account utente.  Ciò consente di aggiungere nuove posizioni note né di cancellare lo stato per tutti gli account

``` powershell
Set-ADFSAccountActivity user@upnsuffix.com -FamiliarLocation “1.2.3.4”
```
`Reset-ADFSAccountLockout`

Reimposta il contatore del blocco per un account utente

``` powershell
Reset-ADFSAccountLockout user@upnsuffix.com -Familiar
```

## <a name="troubleshooting-esl"></a>Risoluzione dei problemi ESL
Di seguito consente di semplificare la risoluzione dei problemi della funzionalità di blocco smart extranet.

### <a name="updating-database-permissions-for-esl"></a>Aggiornamento delle autorizzazioni di database per ESL
Se tutti gli errori vengono restituiti dai `Update-AdfsArtifactDatabasePermission` cmdlet, verificare quanto segue

1.  L'elenco dei nodi della farm sia corretto.  Se un nodo è presente nell'elenco di farm AD FS ma non è più attivo patch verifica avrà esito negativo.  Ciò può essere risolto eseguendo `remove-adfsnode <node name >`
2.  Verificare che la patch sia distribuita in tutti i nodi nella farm
3.  Verificare le credenziali passate al cmdlet dispongono dell'autorizzazione per modificare il proprietario dello schema del database ad fs dell'artefatto.  

### <a name="logging--auditing"></a>Registrazione o controllo
Quando una richiesta di autenticazione viene rifiutata perché l'account supera la soglia di blocco, AD FS verrà scritto un `ExtranetLockoutEvent` nel flusso di controllo di sicurezza.  

Evento di esempio:

Si è verificato un evento di blocco extranet. Per informazioni dettagliate sull'errore, vedere il XML. 

**ID attività: 172332e1-1301-4e56-0e00-0080000000db**

```
Additional Data 
XML: <?xml version="1.0" encoding="utf-16"?>
<AuditBase xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="ExtranetLockoutAudit">
  <AuditType>ExtranetLockout</AuditType>
  <AuditResult>Failure</AuditResult>
  <FailureType>ExtranetLockoutError</FailureType>
  <ErrorCode>AccountRestrictedAudit</ErrorCode>
  <ContextComponents>
    <Component xsi:type="ResourceAuditComponent">
      <RelyingParty>http://contoso.com/adfs/services/trust</RelyingParty>
      <ClaimsProvider>N/A</ClaimsProvider>
      <UserId>TQDFTD\Administrator</UserId>
    </Component>
    <Component xsi:type="RequestAuditComponent">
      <Server>N/A</Server>
      <AuthProtocol>WSFederation</AuthProtocol>
      <NetworkLocation>Intranet</NetworkLocation>
      <IpAddress>4.4.4.4</IpAddress>
      <ForwardedIpAddress />
      <ProxyIpAddress>1.2.3.4</ProxyIpAddress>
      <NetworkIpAddress>1.2.3.4</NetworkIpAddress>
      <ProxyServer>N/A</ProxyServer>
      <UserAgentString>Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.132 Safari/537.36</UserAgentString>
      <Endpoint>/adfs/ls</Endpoint>
    </Component>
    <Component xsi:type="LockoutConfigAuditComponent">
      <CurrentBadPasswordCount>5</CurrentBadPasswordCount>
      <ConfigBadPasswordCount>5</ConfigBadPasswordCount>
      <LastBadAttempt>02/07/2018 21:47:44</LastBadAttempt>
      <LockoutWindowConfig>00:30:00</LockoutWindowConfig>
    </Component>
  </ContextComponents>
</AuditBase>

```

## <a name="banned-ip-addresses"></a>Indirizzi IP da escludere
Oltre alle funzionalità di blocco smart extranet, l'aggiornamento di giugno 2018 AD FS consente di configurare un set di indirizzi IP a livello globale in AD FS, in modo che le richieste provenienti da tali indirizzi IP, o che dispongono di tali indirizzi IP **x-forwarded-for**  oppure **x-ms-inoltrati-client-ip** intestazioni, verranno bloccate da AD FS.

##### <a name="adding-banned-ips"></a>Aggiunta da escludere gli indirizzi IP
Per aggiungere gli indirizzi IP da escludere per l'elenco globale, usare il cmdlet di Powershell seguente:

``` powershell
PS C:\ >Set-AdfsProperties -AddBannedIps "1.2.3.4", "::3", "1.2.3.4/16"
```

Formati consentiti

1.  IPv4
2.  IPv6
3.  Formato CIDR con IPv4 o v6
4.  Intervallo di indirizzi IP con IPv4 o v6 (vale a dire 1.2.3.4-1.2.3.6)

#### <a name="removing-banned-ips"></a>Rimozione da escludere gli indirizzi IP
Per rimuovere gli indirizzi IP da escludere dall'elenco globale, usare il cmdlet di Powershell seguente:

``` powershell
PS C:\ >Set-AdfsProperties -RemoveBannedIps "1.2.3.4"
```

#### <a name="read-banned-ips"></a>Lettura da escludere gli indirizzi IP
Per leggere il set di indirizzi IP da escludere corrente, usare il cmdlet di Powershell seguente:

``` powershell
PS C:\ >Get-AdfsProperties 
```

Output di esempio:

```
BannedIpList                   : {1.2.3.4, ::3,1.2.3.4/16}
```



## <a name="additional-references"></a>Altri riferimenti  
[Le procedure consigliate per la protezione di Active Directory Federation Services](../../ad-fs/deployment/best-practices-securing-ad-fs.md)

[Set-AdfsProperties](https://technet.microsoft.com/itpro/powershell/windows/adfs/set-adfsproperties)

[Operazioni di AD FS](../../ad-fs/AD-FS-2016-Operations.md)

    
