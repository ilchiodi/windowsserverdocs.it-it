---
ms.assetid: 777aab65-c9c7-4dc9-a807-9ab73fac87b8
title: Configurare la protezione del blocco Extranet AD FS
author: billmath
ms.author: billmath
manager: mtilman
ms.date: 05/20/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 28f7fc4a8c7129d9f88cc030b1b150db44321bf9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859944"
---
# <a name="ad-fs-extranet-lockout-and-extranet-smart-lockout"></a>AD FS blocco Extranet e blocco intelligente Extranet

## <a name="overview"></a>Panoramica

Il blocco Smart Extranet (ESL) consente agli utenti di riscontrare il blocco degli account Extranet da attività dannose.  

ESL consente AD FS di distinguere tra i tentativi di accesso da una posizione nota a un utente e i tentativi di accesso da quello che potrebbe essere un utente malintenzionato. AD FS possibile bloccare gli utenti malintenzionati e consentire agli utenti validi di continuare a usare i propri account. In questo modo si evitano e si proteggono da attacchi Denial of Service e da determinate classi di attacchi di tipo spray per le password all'utente. ESL è disponibile per AD FS in Windows Server 2016 ed è integrato in AD FS in Windows Server 2019.

ESL è disponibile solo per le richieste di autenticazione con nome utente e password che passano attraverso la rete Extranet con il proxy dell'applicazione Web o un proxy di terze parti. Qualsiasi proxy di terze parti deve supportare il protocollo MS-ADFSPIP da usare al posto del proxy dell'applicazione Web, ad esempio [F5 Big-IP Access Policy Manager](https://devcentral.f5.com/s/articles/ad-fs-proxy-replacement-on-f5-big-ip-30191). Consultare la documentazione del proxy di terze parti per determinare se il proxy supporta il protocollo MS-ADFSPIP.   

## <a name="additional-features-in-ad-fs-2019"></a>Funzionalità aggiuntive di AD FS 2019
Il blocco intelligente Extranet in AD FS 2019 aggiunge i vantaggi seguenti rispetto a AD FS 2016:
- Impostare soglie di blocco indipendenti per posizioni note e non note, in modo che gli utenti in posizioni valide note possano avere più spazio per l'errore rispetto alle richieste provenienti da posizioni sospette
- Abilitare la modalità di controllo per il blocco intelligente continuando a applicare il comportamento di blocco soft precedente. In questo modo è possibile acquisire familiarità con le posizioni note agli utenti ed essere ancora protetti dalla funzionalità di blocco Extranet disponibile da AD FS 2012R2.  

## <a name="how-it-works"></a>Funzionamento
### <a name="configuration-information"></a>Informazioni di configurazione
Quando è abilitata la funzionalità ESL, viene creata una nuova tabella nel database di elementi, AdfsArtifactStore. AccountActivity, e viene selezionato un nodo nella farm AD FS come master "attività utente". In una configurazione WID, questo nodo è sempre il nodo primario. In una configurazione SQL, un nodo viene selezionato come master attività utente.  

Per visualizzare il nodo selezionato come master attività utente. Get-AdfsFarmInformation. FarmRoles

Tutti i nodi secondari contatteranno il nodo master in ogni accesso aggiornato tramite la porta 80 per conoscere il valore più recente dei conteggi delle password errate e i nuovi valori di posizione noti e aggiornare il nodo dopo l'elaborazione dell'account di accesso.

![configurazione](media/configure-ad-fs-extranet-smart-lockout-protection/esl1.png)

 Se il nodo secondario non è in grado di contattare il database master, verrà scritto un evento di errore nel registro AD FS admin. Le autenticazioni continueranno a essere elaborate, ma AD FS scriverà solo lo stato aggiornato localmente. AD FS tenterà di contattare il master ogni 10 minuti e tornerà al master dopo che il master sarà disponibile.

### <a name="terminology"></a>Terminologia
- **FamiliarLocation**: durante una richiesta di autenticazione, ESL controlla tutti gli indirizzi IP presentati. Questi IP saranno costituiti da una combinazione di IP di rete, IP con inoltri e dell'IP facoltativo x-inoltred-for. Se la richiesta ha esito positivo, tutti gli indirizzi IP vengono aggiunti alla tabella attività account come "indirizzi IP familiari". Se la richiesta include tutti gli indirizzi IP presenti in "indirizzi IP familiari", la richiesta verrà considerata come una località "familiare".
- **UnknownLocation**: se una richiesta ricevuta ha almeno un IP non presente nell'elenco "FamiliarLocation" esistente, la richiesta verrà considerata come una posizione "sconosciuta". Questo consente di gestire gli scenari di proxy, ad esempio l'autenticazione legacy di Exchange Online, in cui gli indirizzi Exchange Online gestiscono le richieste riuscite e non riuscite.  
- **badPwdCount**: valore che rappresenta il numero di volte in cui è stata inviata una password non corretta e l'autenticazione non è riuscita. Per ogni utente, i contatori separati vengono conservati per percorsi noti e posizioni sconosciute.
- **UnknownLockout**: valore booleano per utente se l'accesso all'utente da posizioni sconosciute non è stato bloccato. Questo valore viene calcolato in base ai valori badPwdCountUnfamiliar e ExtranetLockoutThreshold.
- **ExtranetLockoutThreshold**: questo valore determina il numero massimo di tentativi di accesso con password errata. Quando viene raggiunta la soglia, ADFS rifiuterà le richieste dalla rete Extranet fino a quando non sarà stata superata la finestra di osservazione.
- **ExtranetObservationWindow**: questo valore determina la durata di blocco delle richieste di nome utente e password da posizioni sconosciute. Una volta superata la finestra, ADFS inizierà a eseguire di nuovo l'autenticazione con nome utente e password da percorsi sconosciuti.
- **ExtranetLockoutRequirePDC**: se abilitata, il blocco Extranet richiede un controller di dominio primario (PDC). Se disabilitato, il blocco Extranet verrà fallback a un altro controller di dominio nel caso in cui il PDC non sia disponibile.  
- **ExtranetLockoutMode**: controlla solo la modalità di blocco intelligente della Extranet
    - **ADFSSmartLockoutLogOnly**: il blocco Smart Extranet è abilitato, ma ad FS scriverà solo eventi di amministrazione e di controllo, ma non rifiuterà le richieste di autenticazione. Questa modalità deve essere inizialmente abilitata per il popolamento di FamiliarLocation prima di abilitare ' ADFSSmartLockoutEnforce '.
    - **ADFSSmartLockoutEnforce**: supporto completo per bloccare le richieste di autenticazione non note quando vengono raggiunte le soglie.

Sono supportati gli indirizzi IPv4 e IPv6.

### <a name="anatomy-of-a-transaction"></a>Anatomia di una transazione
- **Controllo pre-autenticazione**: durante una richiesta di autenticazione, ESL controlla tutti gli indirizzi IP presentati. Questi IP saranno costituiti da una combinazione di IP di rete, IP con inoltri e dell'IP facoltativo x-inoltred-for. Nei log di controllo questi indirizzi IP sono elencati nel campo <IpAddress> nell'ordine di x-ms-inoltred-client-IP, x-inoltred-for, x-MS-proxy-client-IP.

  In base a questi indirizzi IP, ADFS determina se la richiesta provenga da un percorso familiare o non noto, quindi controlla se il rispettivo badPwdCount è inferiore al limite di soglia impostato o se l'ultimo tentativo **non riuscito** è stato eseguito più a lungo dell'intervallo di tempo della finestra di osservazione. Se una di queste condizioni è true, ADFS consente questa transazione per ulteriori operazioni di elaborazione e convalida delle credenziali. Se entrambe le condizioni sono false, l'account si trova già in uno stato bloccato fino a quando non viene superata la finestra di osservazione. Al termine della finestra di osservazione, all'utente viene consentito un tentativo di autenticazione. Si noti che, in 2019, ADFS verificherà il limite di soglia appropriato in base a se l'indirizzo IP corrisponde a una posizione familiare.
- **Accesso riuscito**: se l'accesso ha esito positivo, gli IP della richiesta vengono aggiunti all'elenco indirizzi IP del percorso familiare dell'utente.  
- **Accesso non riuscito**: se l'accesso ha esito negativo, il badPwdCount viene aumentato. L'utente entra in uno stato di blocco se l'utente malintenzionato invia al sistema password più valide rispetto alla soglia consentita. (badPwdCount > ExtranetLockoutThreshold)  

![configurazione](media/configure-ad-fs-extranet-smart-lockout-protection/esl2.png)

Il valore "UnknownLockout" sarà uguale a true quando l'account è bloccato. Ciò significa che il valore di badPwdCount dell'utente è superiore alla soglia, ovvero che un utente ha tentato più password del consentito dal sistema. In questo stato, è possibile accedere a un utente valido in due modi.
- L'utente deve attendere la scadenza del tempo di ObservationWindow o
- Per reimpostare lo stato di blocco, reimpostare il badPwdCount su zero con ' Reset-ADFSAccountLockout '.

Se non si verificano reimpostazioni, all'account verrà consentito un solo tentativo di accesso con password per ogni finestra di osservazione. L'account tornerà allo stato bloccato dopo il tentativo e la finestra di osservazione verrà riavviata. Il valore di badPwdCount verrà reimpostato automaticamente solo dopo un accesso con password riuscito.

### <a name="log-only-mode-versus-enforce-mode"></a>Modalità di sola registrazione rispetto alla modalità' Applica '
La tabella AccountActivity viene popolata sia durante la modalità' log-only ' che in modalità' enforce '. Se la modalità "solo log" viene ignorata e l'istruzione ESL viene spostata direttamente in modalità "Imponi" senza il periodo di attesa consigliato, gli indirizzi IP familiari degli utenti non saranno noti ad ADFS. In questo caso, l'ESL si comporterebbe come ' ADBadPasswordCounter ', bloccando potenzialmente il traffico utente legittimo se l'account utente è in un attacco di forza bruta attivo. Se la modalità "solo log" viene ignorata e l'utente immette uno stato bloccato con "UnknownLockout" = TRUE e tenta di accedere con una password valida da un indirizzo IP non presente nell'elenco IP "familiare", non sarà in grado di eseguire l'accesso. Per evitare questo scenario, è consigliabile usare la modalità di sola registrazione per 3-7 giorni. Se gli account sono attivamente sotto attacco, è necessario un minimo di 24 ore per la modalità "solo log" per impedire il blocco agli utenti legittimi.  

## <a name="extranet-smart-lockout-configuration"></a>Configurazione del blocco intelligente Extranet  

### <a name="prerequisites-for-ad-fs-2016"></a>Prerequisiti per AD FS 2016

1. **Installare gli aggiornamenti in tutti i nodi della farm**

   Assicurarsi prima di tutto che tutti i server di AD FS di Windows Server 2016 siano aggiornati a partire dagli aggiornamenti di Windows di giugno 2018 e che la farm AD FS 2016 sia in esecuzione a livello di comportamento della farm 2016.
1. **Verificare le autorizzazioni**

   Per il blocco intelligente Extranet è necessario che gestione remota Windows sia abilitato in ogni server AD FS.
3. **Aggiornare le autorizzazioni del database degli artefatti**

   Per il blocco intelligente Extranet è necessario che l'account del servizio AD FS disponga delle autorizzazioni per creare una nuova tabella nel database degli artefatti AD FS. Accedere a un server di AD FS come amministratore AD FS, quindi concedere questa autorizzazione eseguendo i comandi seguenti in una finestra del prompt dei comandi di PowerShell:

   ``` powershell
   PS C:\>$cred = Get-Credential
   PS C:\>Update-AdfsArtifactDatabasePermission -Credential $cred
   ```
   >[!NOTE]
   >Il segnaposto $cred è un account che dispone di autorizzazioni di amministratore AD FS. Questa operazione deve fornire le autorizzazioni di scrittura per creare la tabella.

   I comandi precedenti potrebbero non riuscire a causa della mancanza di autorizzazioni sufficienti perché la farm AD FS utilizza SQL Server e le credenziali fornite in precedenza non dispongono dell'autorizzazione di amministratore per SQL Server. In questo caso, è possibile configurare manualmente le autorizzazioni del database in SQL Server database eseguendo il comando seguente quando si è connessi al database AdfsArtifactStore.
    ```  
    # when prompted with “Are you sure you want to perform this action?”, enter Y.

    [CmdletBinding(SupportsShouldProcess=$true,ConfirmImpact = 'High')]
    Param()

    $fileLocation = "$env:windir\ADFS\Microsoft.IdentityServer.Servicehost.exe.config"

    if (-not [System.IO.File]::Exists($fileLocation))
    {
    write-error "Unable to open ADFS configuration file."
    return
    }

    $doc = new-object Xml
    $doc.Load($fileLocation)
    $connString = $doc.configuration.'microsoft.identityServer.service'.policystore.connectionString
    $connString = $connString -replace "Initial Catalog=AdfsConfigurationV[0-9]*", "Initial Catalog=AdfsArtifactStore"

    if ($PSCmdlet.ShouldProcess($connString, "Executing SQL command sp_addrolemember 'db_owner', 'db_genevaservice' "))
    {
    $cli = new-object System.Data.SqlClient.SqlConnection
    $cli.ConnectionString = $connString
    $cli.Open()

    try
    {     

    $cmd = new-object System.Data.SqlClient.SqlCommand
    $cmd.CommandText = "sp_addrolemember 'db_owner', 'db_genevaservice'"
    $cmd.Connection = $cli
    $rowsAffected = $cmd.ExecuteNonQuery()  
    if ( -1 -eq $rowsAffected )
    {
    write-host "Success"
    }
    }
    finally
    {
    $cli.CLose()
    }
    }
    ```

### <a name="ensure-ad-fs-security-audit-logging-is-enabled"></a>Assicurarsi che la registrazione del controllo di sicurezza AD FS sia abilitata
Questa funzionalità consente di utilizzare i log di controllo della sicurezza, pertanto è necessario abilitare il controllo in AD FS e i criteri locali in tutti i server AD FS.

### <a name="configuration-instructions"></a>Istruzioni di configurazione
Il blocco Smart Extranet usa la proprietà ADFS **ExtranetLockoutEnabled**. Questa proprietà è stata utilizzata in precedenza per controllare "blocco flessibile Extranet" nel server 2012R2. Se è stato abilitato il blocco flessibile Extranet, per visualizzare la configurazione della proprietà corrente eseguire ` Get-AdfsProperties`.

### <a name="configuration-recommendations"></a>Suggerimenti per la configurazione
Quando si configura il blocco intelligente Extranet, attenersi alle procedure consigliate per l'impostazione delle soglie:  

`ExtranetObservationWindow (new-timespan -Minutes 30)`

`ExtranetLockoutThreshold: – 2x AD Threshold Value`

Valore di Active Directory: 20, ExtranetLockoutThreshold: 10

Il blocco Active Directory funziona indipendentemente dal blocco Smart Extranet. Tuttavia, se Active Directory blocco è abilitato, ExtranetLockoutThreshold in AD FS < soglia di blocco dell'account in Active Directory

`ExtranetLockoutRequirePDC - $false`

Se abilitata, il blocco Extranet richiede un controller di dominio primario (PDC). Se disabilitata e configurata su false, il blocco Extranet verrà fallback a un altro controller di dominio nel caso in cui il PDC non sia disponibile.

Per impostare questa proprietà eseguire:

``` powershell
Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30) -ExtranetLockoutRequirePDC $false
```
### <a name="enable-log-only-mode"></a>Abilita modalità solo log

In modalità solo log, AD FS popola le informazioni sul percorso note degli utenti e scrive gli eventi di controllo di sicurezza, ma non blocca le richieste. Questa modalità viene usata per convalidare l'esecuzione del blocco intelligente e per consentire a AD FS di "apprendere" i percorsi noti per gli utenti prima di abilitare la modalità "applica". Come AD FS apprende, archivia l'attività di accesso per utente (in modalità solo registrazione o in modalità di imposizione).
Impostare il comportamento di blocco su log solo eseguendo il cmdlet seguente.  

`Set-AdfsProperties -ExtranetLockoutMode AdfsSmartlockoutLogOnly`

La modalità solo log è uno stato temporaneo, in modo che il sistema possa apprendere il comportamento di accesso prima di introdurre l'applicazione di blocco con il comportamento di blocco Smart. La durata consigliata per la modalità solo log è 3-7 giorni. Se gli account sono attivamente sotto attacco, la modalità di solo log deve essere eseguita per almeno 24 ore.

In AD FS 2016, se è abilitato il comportamento di blocco flessibile Extranet di 2012R2 prima di abilitare il blocco intelligente Extranet, la modalità di sola registrazione Disabilita il comportamento "blocco flessibile Extranet". AD FS blocco intelligente non bloccherà gli utenti in modalità solo log. Tuttavia, AD locale può bloccare l'utente in base alla configurazione di AD. Esaminare i criteri di blocco di Active Directory per informazioni su come gli utenti locali possono bloccare gli utenti.

In AD FS 2019, un ulteriore vantaggio consiste nell'abilitare la modalità di sola registrazione per il blocco intelligente continuando a applicare il comportamento di blocco soft precedente usando il seguente PowerShell.

`Set-AdfsProperties -ExtranetLockoutMode 3`

Per rendere effettive le nuove modalità, riavviare il servizio AD FS in tutti i nodi della farm

`Restart-service adfssrv`

Una volta configurata la modalità, è possibile abilitare il blocco intelligente utilizzando il parametro EnableExtranetLockout

`Set-AdfsProperties -EnableExtranetLockout $true`

### <a name="enable-enforce-mode"></a>Abilita modalità Imponi

Quando si ha familiarità con la soglia di blocco e la finestra di osservazione, è possibile spostare ESL in modalità "Imponi" usando il cmdlet PSH seguente:

`Set-AdfsProperties -ExtranetLockoutMode AdfsSmartLockoutEnforce`

Per rendere effettive le nuove modalità, riavviare il servizio AD FS in tutti i nodi della farm usando il comando seguente.

`Restart-service adfssrv`

## <a name="manage-user-account-activity"></a>Attività Gestisci account utente
AD FS fornisce tre cmdlet per gestire i dati dell'attività dell'account. Questi cmdlet si connettono automaticamente al nodo della farm che include il ruolo di master.
>[!NOTE]
>È possibile usare just enough Administration (JEA) per delegare AD FS cmdlet per reimpostare i blocchi degli account. Ad esempio, è possibile delegare le autorizzazioni per l'uso di cmdlet ESL per il personale del supporto tecnico. Per informazioni sulla delega delle autorizzazioni per l'uso di questi cmdlet, vedere [Delegare ad FS PowerShell cmdlet accesso a utenti non amministratori](delegate-ad-fs-pshell-access.md)

È possibile eseguire l'override di questo comportamento passando il parametro-Server.

- Get-ADFSAccountActivity-UserPrincipalName

  Leggere l'attività corrente dell'account per un account utente. Il cmdlet si connette sempre automaticamente al Master della farm utilizzando l'endpoint REST dell'attività account. Pertanto, tutti i dati devono essere sempre coerenti.

`Get-ADFSAccountActivity user@contoso.com`

  Proprietà:
    - BadPwdCountFamiliar: incrementato quando un'autenticazione ha esito positivo da un percorso noto.
    - BadPwdCountUnknown: incrementato quando un'autenticazione ha esito negativo da una posizione sconosciuta
    - LastFailedAuthFamiliar: se l'autenticazione non ha avuto esito positivo da una posizione nota, LastFailedAuthUnknown è impostato su ora di autenticazione non riuscita
    - LastFailedAuthUnknown: se l'autenticazione non ha avuto esito positivo da una posizione sconosciuta, LastFailedAuthUnknown è impostato su ora di autenticazione non riuscita
    - FamiliarLockout: valore booleano che sarà "true" se "BadPwdCountFamiliar" > ExtranetLockoutThreshold
    - UnknownLockout: valore booleano che sarà "true" se "BadPwdCountUnknown" > ExtranetLockoutThreshold  
    - FamiliarIPs: massimo 20 IP che hanno familiarità con l'utente. Quando viene superato, l'indirizzo IP meno recente nell'elenco verrà rimosso.
-    Set-ADFSAccountActivity

     Aggiunge nuove posizioni note. L'elenco di indirizzi IP familiare ha un massimo di 20 voci, se questa viene superata, l'IP meno recente nell'elenco verrà rimosso.

`Set-ADFSAccountActivity user@contoso.com -AdditionalFamiliarIps “1.2.3.4”`

- Reimposta-ADFSAccountLockout

  Reimposta il contatore di blocco per un account utente per ogni percorso noto (badPwdCountFamiliar) o per i contatori di posizione non noti (badPwdCountUnfamiliar). Reimpostando un contatore, il valore "FamiliarLockout" o "UnfamiliarLockout" verrà aggiornato, in quanto il contatore di reimpostazione sarà inferiore alla soglia.  

`Reset-ADFSAccountLockout user@contoso.com -Location Familiar`
`Reset-ADFSAccountLockout user@contoso.com -Location Unknown`

## <a name="event-logging--user-activity-information-for-ad-fs-extranet-lockout"></a>Registrazione eventi & informazioni sull'attività dell'utente per il blocco AD FS Extranet

### <a name="connect-health"></a>Connetti stato
Il modo consigliato per monitorare l'attività dell'account utente è tramite Connect Health. Connect Health genera report scaricabili sugli indirizzi IP rischiosi e i tentativi di accesso con password errata. Ogni elemento nel report degli indirizzi IP rischiosi Mostra informazioni aggregate sulle attività di accesso non riuscite AD FS che superano la soglia designata. Le notifiche tramite posta elettronica possono essere impostate per gli amministratori degli avvisi non appena si verificano con le impostazioni di posta elettronica personalizzabili. Per ulteriori informazioni e istruzioni per la configurazione, vedere la [documentazione di Connect Health](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-health-adfs).

### <a name="ad-fs-extranet-smart-lockout-events"></a>Eventi di blocco Smart Extranet AD FS.
Per gli eventi di blocco Smart Extranet da scrivere, è necessario abilitare la funzionalità ESL in modalità "solo log" o "applica" ed è abilitato il controllo di sicurezza di ADFS.
AD FS scriverà gli eventi di blocco Extranet nel registro di controllo di sicurezza:
- Quando un utente è bloccato (raggiunge la soglia di blocco per i tentativi di accesso non riusciti)
- Quando AD FS riceve un tentativo di accesso per un utente che è già in stato di blocco

In modalità solo log è possibile controllare il registro di controllo di sicurezza per gli eventi di blocco. Per tutti gli eventi rilevati, è possibile controllare lo stato utente usando il cmdlet Get-ADFSAccountActivity per determinare se il blocco è stato eseguito da indirizzi IP familiari o non noti e per controllare l'elenco di indirizzi IP noti per tale utente.


|ID evento|Descrizione|
|-----|-----|
|1203|Questo evento viene scritto per ogni tentativo di accesso con password non valida. Non appena il badPwdCount raggiunge il valore specificato in ExtranetLockoutThreshold, l'account verrà bloccato in ADFS per la durata specificata in ExtranetObservationWindow.</br>ID attività: %1</br>XML: %2|
|1201|Questo evento viene scritto ogni volta che un utente viene bloccato. </br>ID attività: %1</br>XML: %2|
|557 (ADFS 2019)| Si è verificato un errore durante il tentativo di comunicare con il servizio REST dell'archivio account nel nodo %1. Se si tratta di una farm WID, il nodo primario potrebbe essere offline. Se si tratta di una farm SQL, ADFS selezionerà automaticamente un nuovo nodo per ospitare il ruolo di master dell'archivio utenti.|
|562 (ADFS 2019)|Si è verificato un errore durante l'communcating con l'endpoint dell'archivio account nel server %1.</br>Messaggio eccezione: %2|
|563 (ADFS 2019)|Si è verificato un errore durante il calcolo dello stato di blocco Extranet. A causa del valore dell'impostazione %1, l'autenticazione sarà consentita per questo utente e il rilascio dei token continuerà. Se si tratta di una farm WID, il nodo primario potrebbe essere offline. Se si tratta di una farm SQL, ADFS selezionerà automaticamente un nuovo nodo per ospitare il ruolo di master dell'archivio utenti.</br>Nome server archivio account: %2</br>ID utente: %3</br>Messaggio eccezione: %4|
|512|L'account per l'utente seguente è bloccato. Tentativo di accesso consentito a causa della configurazione di sistema.</br>ID attività: %1 </br>Utente: %2 </br>IP client: %3 </br>Conteggio password errate: %4  </br>Ultimo tentativo di accesso con password non valida: %5|
|515|Il seguente account utente si trova in uno stato bloccato e la password corretta è stata appena fornita. Questo account potrebbe essere compromesso.</br>Ulteriori dati </br>ID attività: %1 </br>Utente: %2 </br>IP client: %3 |
|516|Il seguente account utente è stato bloccato a causa di un numero eccessivo di tentativi di accesso con password errata.</br>ID attività: %1  </br>Utente: %2  </br>IP client: %3  </br>Conteggio password errate: %4  </br>Ultimo tentativo di accesso con password non valida: %5|

## <a name="esl-frequently-asked-questions"></a>Domande frequenti su ESL

**Una farm ADFS che utilizza il blocco intelligente Extranet in modalità di imposizione vedrà mai blocchi di utenti malintenzionati?** 

R: se ADFS è impostato sulla modalità' Applica ', non sarà mai possibile visualizzare l'account utente legittimo bloccato da forza bruta o Denial of Service. L'unico modo in cui un blocco di account dannoso può impedire l'accesso di un utente è se l'attore malintenzionato ha la password utente o può inviare richieste da un indirizzo IP noto (noto) per tale utente. 

**Che cosa accade per la funzionalità ESL e l'attore malintenzionato ha una password utente?** 

R: l'obiettivo tipico dello scenario di attacco di forza bruta è indovinare una password e accedere correttamente.  Se un utente è phishing o se viene indovinata una password, la funzionalità ESL non bloccherà l'accesso perché l'accesso soddisferà i criteri "riusciti" della password corretta e del nuovo IP. L'IP di Bad Actors viene quindi visualizzato come uno "familiare". La migliore attenuazione in questo scenario consiste nel cancellare le attività dell'utente in ADFS e richiedere l'autenticazione a più fattori per gli utenti. Si consiglia vivamente di installare AAD Password Protection che garantisce che le password indovinate non vengano riportate nel sistema.

**Se l'utente non ha mai eseguito l'accesso correttamente da un indirizzo IP e quindi prova a usare una password errata alcune volte, sarà in grado di eseguire l'accesso una volta che la password è stata digitata correttamente?** 

R: se un utente invia più password non valide, ad esempio se si digita in modo legittimo, e il tentativo seguente consente di ottenere la password corretta, l'utente riuscirà immediatamente ad eseguire l'accesso.  In questo modo verrà cancellato il numero di password non valide e l'IP verrà aggiunto all'elenco FamiliarIPs.  Tuttavia, se superano la soglia degli accessi non riusciti dalla località sconosciuta, entreranno nello stato di blocco e dovranno attendere oltre la finestra di osservazione e accedere con una password valida o richiedere l'intervento dell'amministratore per reimpostare l'account.  
 
**ESL funziona anche su Intranet?**

R: se i client si connettono direttamente ai server ADFS e non tramite server proxy applicazione Web, il comportamento di ESL non verrà applicato.  

**Vengono visualizzati gli indirizzi IP Microsoft nel campo IP del client. ESL blocca gli attacchi di forza bruta con proxy di EXO?**  

R: la soluzione ESL funziona bene per impedire lo scenario di attacco di forza bruta di Exchange Online o di altri scenari di autenticazione legacy. Un'autenticazione legacy ha un "ID attività" di 00000000-0000-0000-0000-000000000000. In questi attacchi, l'attore malintenzionato sta sfruttando l'autenticazione di base di Exchange Online, nota anche come autenticazione legacy, in modo che l'indirizzo IP del client venga visualizzato come Microsoft One. Il server Exchange Online nel cloud delega la verifica dell'autenticazione per conto del client Outlook. In questi scenari, l'indirizzo IP del mittente dannoso si troverà in x-ms-inoltred-client-IP e l'indirizzo IP di Microsoft Exchange Online server sarà nel valore x-MS-client-IP.
Il blocco intelligente Extranet controlla gli indirizzi IP di rete, gli IP con inoltri, l'x-inoltred-client-IP e il valore x-MS-client-IP. Se la richiesta ha esito positivo, tutti gli indirizzi IP vengono aggiunti all'elenco familiare. Se viene ricevuta una richiesta e uno degli indirizzi IP presentati non è incluso nell'elenco familiare, la richiesta verrà contrassegnata come non nota. Il familiare utente sarà in grado di accedere con successo mentre le richieste provenienti da posizioni non note verranno bloccate.  

\* * D: è possibile stimare le dimensioni di ADFSArtifactStore prima di abilitare la funzionalità ESL?

R: con la funzionalità ESL abilitata, AD FS tiene traccia dell'attività dell'account e dei percorsi noti per gli utenti nel database ADFSArtifactStore. Le dimensioni di questo database variano in base al numero di utenti e di percorsi noti di cui viene tenuta traccia. Quando pianifichi l'abilitazione di ESL, puoi stimare le dimensioni del database ADFSArtifactStore considerando un aumento fino a un massimo di 1 GB ogni 100.000 utenti. Se la AD FS farm utilizza il database interno di Windows, il percorso predefinito per i file di database è C:\Windows\WID\Data\. Per evitare che questa unità si riempia, verifica di avere almeno 5 GB di spazio di archiviazione disponibile prima di abilitare ESL. Oltre che dello spazio di archiviazione su disco, esegui la pianificazione tenendo conto che la memoria di elaborazione totale dopo l'abilitazione di ESL si espanderà fino a un massimo di 1 ulteriore GB di RAM per popolazioni di un massimo di 500.000 utenti.


## <a name="additional-references"></a>Altri riferimenti  
[Procedure consigliate per la protezione di Active Directory Federation Services](../../ad-fs/deployment/best-practices-securing-ad-fs.md)

[Set-AdfsProperties](https://technet.microsoft.com/itpro/powershell/windows/adfs/set-adfsproperties)

[Operazioni di AD FS](../../ad-fs/AD-FS-2016-Operations.md)
