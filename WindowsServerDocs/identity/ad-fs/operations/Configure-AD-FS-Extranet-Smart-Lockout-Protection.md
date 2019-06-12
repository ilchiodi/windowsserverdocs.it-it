---
ms.assetid: 777aab65-c9c7-4dc9-a807-9ab73fac87b8
title: Configurare la protezione di blocco Extranet di AD FS
description: ''
author: billmath
ms.author: billmath
manager: mtilman
ms.date: 05/20/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 7a4ad8c0199f0f62d7cd69a43897cb4608ddb365
ms.sourcegitcommit: ccc802338b163abdad2e53b55f39addcfea04603
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/05/2019
ms.locfileid: "66687367"
---
# <a name="ad-fs-extranet-lockout-and-extranet-smart-lockout"></a>AD FS Extranet Smart Lockout e blocco Extranet

## <a name="overview"></a>Panoramica

Blocco intelligente Extranet (ESL) protegge gli utenti si verifichi il blocco degli account extranet da attività dannose.  

ESL consente AD FS distinguere tra i tentativi di accesso da una posizione nota per un utente e tentativi di accesso da ciò che può essere un utente malintenzionato. ADFS può bloccare gli utenti malintenzionati, consentendo al contempo valid users di continuare a usare i propri account. Ciò impedisce e protegge da determinate classi di attacchi spray password per l'utente e di tipo denial of service. ESL è disponibile per AD FS in Windows Server 2016 ed è incorporata in ADFS in Windows Server 2019.

ESL è disponibile solo per il nome utente e le richieste di autenticazione password disponibili tramite rete extranet con il Proxy applicazione Web o un 3rd party proxy. Qualsiasi proxy di terze parti 3rd deve supportare il protocollo da usare al posto di Proxy applicazione Web, ad esempio MS-ADFSPIP [F5 BIG-IP Access Policy Manager](https://devcentral.f5.com/s/articles/ad-fs-proxy-replacement-on-f5-big-ip-30191). Consultare la documentazione del proxy di terze parti 3rd per determinare se il proxy supporta il protocollo MS-ADFSPIP.   

## <a name="additional-features-in-ad-fs-2019"></a>Funzionalità aggiuntive in AD FS 2019
Blocco intelligente Extranet di AD FS 2019 aggiunge i vantaggi seguenti rispetto ad AD FS 2016:
- Imposta valori soglia di blocco indipendente per percorsi noti e familiarità in modo che gli utenti in modo ottimale noti possono avere più spazio per l'errore rispetto alle richieste da località sospetta
- Abilitare la modalità di controllo per blocco smart pur continuando a imporre un comportamento di blocco temporanea precedente. Ciò consente di ottenere informazioni sulle posizioni note utente e comunque protetta tramite la funzionalità di blocco extranet che è disponibile da AD FS 2012 R2.  

## <a name="how-it-works"></a>Come funziona
### <a name="configuration-information"></a>informazioni di configurazione
Quando ESL è abilitata, viene creata una nuova tabella nel database dell'elemento, AdfsArtifactStore.AccountActivity, e si seleziona un nodo della farm AD FS come server master di "Attività dell'utente". In una configurazione database interno di Windows, questo nodo è sempre il nodo primario. In una configurazione di SQL, un nodo viene selezionato per essere il master di attività dell'utente.  

Per visualizzare il nodo selezionato come server master di attività dell'utente. Get-AdfsFarmInformation.FarmRoles

Tutti i nodi secondari saranno contattare il nodo master su ogni nuovo account di accesso tramite la porta 80 per scoprire il valore più recente dei conteggi di password errata e nuovi valori di posizione nota e aggiornare tale nodo dopo che l'account di accesso viene elaborato.

![configurazione](media/configure-ad-fs-extranet-smart-lockout-protection/esl1.png)

 Se il nodo secondario non riesce a contattare il master, scriverà gli eventi di errore nel Registro di amministrazione di AD FS. Le autenticazioni continueranno a essere elaborati, ma AD FS verrà scritto solo lo stato aggiornato in locale. AD FS ripeterà il tentativo di contattare il master ogni 10 minuti e passerà al master dopo che il master è disponibile.

### <a name="terminology"></a>Terminologia
- **FamiliarLocation**: Durante una richiesta di autenticazione, ESL controlla che tutti presentati gli indirizzi IP. Questi indirizzi IP sarà una combinazione di indirizzo IP di rete, inoltrare l'indirizzo IP e l'IP facoltativo x-forwarded-for. Se la richiesta ha esito positivo, tutti gli indirizzi IP vengono aggiunti alla tabella di attività dell'Account come "gli indirizzi IP noti". Se la richiesta possiede tutti gli indirizzi IP presenti negli "IP familiari", la richiesta viene considerata come un percorso "Note".
- **UnknownLocation**: Se una richiesta in arrivo ha almeno un indirizzo IP non è presente nell'elenco "FamiliarLocation" esistente, quindi la richiesta viene considerata come un percorso "Sconosciuto". Questo serve a gestire gli scenari di inoltro dei dati, ad esempio l'autenticazione legacy Exchange Online in cui gli indirizzi di Exchange Online gestiscono le richieste riuscite e non riuscite.  
- **badPwdCount**: Un valore che rappresenta il numero di volte in cui che è stata inviata una password errata e l'autenticazione non è riuscito. Per ogni utente, vengono conservati contatori separati per posizioni note e posizioni sconosciute.
- **UnknownLockout**: Valore booleano per ogni utente. Se l'utente viene bloccato l'accesso da posizioni sconosciute. Questo valore viene calcolato in base alle badPwdCountUnfamiliar e ai valori ExtranetLockoutThreshold.
- **ExtranetLockoutThreshold**: Questo valore determina il numero massimo di tentativi con password errate. Quando viene raggiunta la soglia, ad FS rifiuterà le richieste dalla extranet fino a quando non ha passato la finestra di osservazione.
- **ExtranetObservationWindow**: Questo valore determina la durata del nome utente e password richieste da posizioni sconosciute sono bloccate. Quando è trascorsa la finestra, ad FS inizierà a eseguire nuovamente l'autenticazione di nome utente e password da posizioni sconosciute.
- **ExtranetLockoutRequirePDC**: Quando abilitata, il blocco della extranet richiede un controller di dominio primario (PDC). Se disabilitato, il blocco della extranet eseguirà il fallback a un altro controller di dominio nel caso in cui il PDC non è disponibile.  
- **ExtranetLockoutMode**: Controlli di accesso solo in modalità di Visual Studio applicata della Extranet di blocco Smart
    - **ADFSSmartLockoutLogOnly**: Extranet di blocco Smart è abilitato, ma AD FS verrà solo scrivere admin e gli eventi di controllo, ma verrà Rifiuta le richieste di autenticazione. Questa modalità deve essere abilitata inizialmente per FamiliarLocation vengano inserite prima 'ADFSSmartLockoutEnforce' è abilitata.
    - **ADFSSmartLockoutEnforce**: Supporto completo per le richieste di autenticazione familiarità di blocco quando vengono raggiunte le soglie.

Gli indirizzi IPv4 e IPv6 sono supportati.

### <a name="anatomy-of-a-transaction"></a>Anatomia di una transazione
- **Controllo di pre-Auth**: Durante una richiesta di autenticazione, ESL controlla che tutti presentati gli indirizzi IP. Questi indirizzi IP sarà una combinazione di indirizzo IP di rete, inoltrare l'indirizzo IP e l'IP facoltativo x-forwarded-for. I log di controllo, questi indirizzi IP sono elencati nel <IpAddress> campo in base all'ordine x-ms-inoltrati-client-ip, x-forwarded-for, x-ms-proxy--indirizzo ip del client.

  Basato su questi indirizzi IP, ad FS determina se la richiesta proviene da una posizione di familiare o non conosciuta e quindi controlla se i rispettivi badPwdCount è minore rispetto al limite di soglia impostato o se l'ultima **non è stato possibile** tentativo è accaduto più il intervallo di tempo di osservazione finestra. Se una di queste condizioni è vera, ad FS consente questa transazione per un'ulteriore elaborazione e convalida delle credenziali. Se entrambe le condizioni sono false, l'account è già in uno stato bloccato fino a quando non passa la finestra di osservazione. Dopo che la finestra di osservazione ha esito positivo, l'utente è consentito un tentativo di eseguire l'autenticazione. Si noti che in 2019, ad FS verrà verificata rispetto al limite di soglia appropriata in base se l'indirizzo IP corrisponde a una posizione nota o non.
- **Accesso con esito positivo**: Se il log ha esito positivo, gli indirizzi IP della richiesta vengono aggiunti all'elenco di IP posizione nota dell'utente.  
- **Accesso non riuscito**: Se il log ha esito negativo per il badPwdCount viene aumentato. L'utente entra in uno stato di blocco se un utente malintenzionato invia altre password errate al sistema rispetto alla soglia. (badPwdCount > ExtranetLockoutThreshold)  

![configurazione](media/configure-ad-fs-extranet-smart-lockout-protection/esl2.png)

Il valore "UnknownLockout" sarà uguale a true quando l'account è bloccato. Ciò significa che badPwdCount dell'utente venga usato rispetto alla soglia, ad esempio che un utente ha tentato altre password che sono stati consentita dal sistema. In questo stato, esistono 2 modi in cui può accedere un utente valido.
- L'utente deve attendere il tempo ObservationWindow a trascorrere o
- Per reimpostare lo stato di blocco, reimpostare il badPwdCount su zero con 'Reset-ADFSAccountLockout'.

Se si verifica non Reimposta, l'account sarà possibile un tentativo di unica password valida in Active Directory per ogni intervallo di osservazione. L'account tornerà allo stato bloccato in seguito verrà riavviato tentativo e nella finestra di osservazione. Il valore di badPwdCount solo verrà reimpostato automaticamente dopo un accesso tramite password ha esito positivo.

### <a name="log-only-mode-versus-enforce-mode"></a>Modalità "solo log" rispetto a modalità 'Applica'
La tabella AccountActivity viene popolata sia durante la modalità 'Solo Log' e 'Applica'. Se ' Log' modalità "solo" viene ignorata ed ESL viene spostata direttamente in modalità 'Applica' senza il periodo di attesa consigliato, gli indirizzi IP di familiarità degli utenti non è nota ad ad FS. In questo caso, ESL si comporteranno come 'ADBadPasswordCounter', potenzialmente bloccano il traffico utente legittimo se l'account utente è sotto attacco di forza bruta attivo. Se la modalità 'Solo Log' viene ignorata e l'utente immette un blocco con "UnknownLockout" stato = TRUE e tenta di accedere con una password valida da un indirizzo IP che non si trova l'elenco IP "note", quindi non saranno in grado di accedere. Modalità "solo log" è consigliata per 3 a 7 giorni per evitare questo scenario. Se gli account sono attivamente in corso un attacco, un minimo di 24 ore di ' Log' modalità "solo" è necessario evitare blocchi per gli utenti legittimi.  

## <a name="extranet-smart-lockout-configuration"></a>Configurazione di blocco Smart Extranet  

### <a name="prerequisites-for-ad-fs-2016"></a>Prerequisiti per AD FS 2016

1. **Installare gli aggiornamenti in tutti i nodi nella farm**

   In primo luogo, assicurarsi che tutti i server AD FS di Windows Server 2016 vengono aggiornati a partire da giugno 2018 aggiornamenti Windows e che la farm AD FS 2016 sia in esecuzione a livello di comportamento farm 2016.
1. **Verificare le autorizzazioni**

   Blocco Smart Extranet richiede che la gestione remota Windows sia abilitata in tutti i server AD FS.
3. **Aggiornare le autorizzazioni di database dell'artefatto**

   Blocco intelligente Extranet richiede l'account del servizio ADFS disponga delle autorizzazioni per creare una nuova tabella nel database dell'artefatto di AD FS. Accedere a qualsiasi server AD FS come amministratore di AD FS e quindi concedere questa autorizzazione eseguendo i comandi seguenti in una finestra del prompt dei comandi di PowerShell:

   ``` powershell
   PS C:\>$cred = Get-Credential
   PS C:\>Update-AdfsArtifactDatabasePermission -Credential $cred
   ```
   >[!NOTE]
   >Il segnaposto $cred è un account che disponga delle autorizzazioni di amministratore di AD FS. Ciò deve fornire le autorizzazioni di scrittura per creare la tabella.

   I comandi precedenti potrebbero non riuscire a causa di mancanza di autorizzazioni sufficienti perché utilizza SQL Server della farm AD FS, e le credenziali fornite in precedenza non dispone delle autorizzazioni di amministratore nel computer SQL server. In questo caso, è possibile configurare le autorizzazioni del database manualmente nel Database di SQL Server eseguendo il comando seguente quando si è connessi al database AdfsArtifactStore.
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

### <a name="ensure-ad-fs-security-audit-logging-is-enabled"></a>Verificare che sia abilitata la registrazione di controllo di sicurezza AD FS
Questa funzionalità viene utilizzato Security Audit log, pertanto, il controllo deve essere abilitato in AD FS, nonché i criteri locali in tutti i server AD FS.

### <a name="configuration-instructions"></a>Istruzioni di configurazione
Blocco Extranet intelligente viene utilizzata la proprietà di ad FS **ExtranetLockoutEnabled**. Questa proprietà è stato precedentemente utilizzato per controllare "Blocco Extranet Soft" in Server 2012 R2. Se è stato abilitato il blocco della Extranet Soft, per visualizzare la configurazione della proprietà corrente, eseguire ` Get-AdfsProperties` .

### <a name="configuration-recommendations"></a>Consigli sulla configurazione
Quando si configura il blocco Smart Extranet, seguire le procedure consigliate per l'impostazione di soglie:  

`ExtranetObservationWindow (new-timespan -Minutes 30)`

`ExtranetLockoutThreshold: – 2x AD Threshold Value`

Valore di AD: 20, ExtranetLockoutThreshold: 10

Blocco di Active Directory funziona in modo indipendente dal blocco Extranet Smart. Tuttavia, se è abilitato il blocco di Active Directory, ExtranetLockoutThreshold in AD FS < soglia di blocco Account in Active Directory

`ExtranetLockoutRequirePDC - $false`

Quando abilitata, il blocco della extranet richiede un controller di dominio primario (PDC). Quando disabilitata e configurato come false, il blocco della extranet eseguirà il fallback a un altro controller di dominio nel caso in cui il PDC non è disponibile.

Per impostare questa proprietà di esecuzione:

``` powershell
Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30) -ExtranetLockoutRequirePDC $false
```
### <a name="enable-log-only-mode"></a>Abilitare la modalità "solo Log"

In modalità solo log, ADFS consente di popolare le informazioni di posizione nota agli utenti e scrive gli eventi di controllo di sicurezza, ma non blocca le richieste. Questa modalità viene utilizzata per convalidare che il blocco smart è in esecuzione e per abilitare AD FS per "informazioni" posizioni note per gli utenti prima di abilitare "Applica" modalità. Come AD FS apprende, archivia le attività di accesso per utente (se in modalità solo log o applicare la modalità).
Impostare il comportamento di blocco per registrare solo eseguendo il cmdlet seguente.  

`Set-AdfsProperties -ExtranetLockoutMode AdfsSmartlockoutLogOnly`

Modalità solo log deve essere un stato temporaneo in modo che il sistema può apprendere il comportamento di account di accesso prima dell'introduzione di imposizione di blocco con il comportamento di blocco smart. La durata consigliata per la modalità "solo log" è 3 a 7 giorni. Se gli account sono attivamente in corso un attacco, modalità "solo log" deve essere eseguita per un minimo di 24 ore.

In AD FS 2016, se il comportamento di 'Blocco Extranet Soft' 2012R2 è abilitato prima di abilitare il blocco Smart Extranet, modalità "solo Log" disabiliterà il comportamento di 'Blocco Extranet Soft'. Blocco intelligente di AD FS non consente di bloccare gli utenti in modalità "solo Log". Tuttavia, in locale AD potrebbero bloccare l'utente in base alla configurazione di Active Directory. Verificare i criteri di blocco di AD per informazioni su come dall'ambiente locale AD possibile blocco degli utenti.

In AD FS 2019, deve essere in grado di abilitare la modalità "solo log" per il blocco smart pur continuando a imporre il comportamento soft blocco precedente usando un ulteriore vantaggio di Powershell seguente.

`Set-AdfsProperties -ExtranetLockoutMode 3`

Per la nuova modalità rendere effettive, riavviare il servizio AD FS in tutti i nodi nella farm

`Restart-service adfssrv`

Una volta configurata la modalità, è possibile abilitare il blocco smart Usa il parametro EnableExtranetLockout

`Set-AdfsProperties -EnableExtranetLockout $true`

### <a name="enable-enforce-mode"></a>Applicare la modalità attiva

Dopo avere acquisito familiarità con la finestra di osservazione e soglia di blocco, ESL può essere spostato in "Applica" modalità usando il cmdlet PSH seguente:

`Set-AdfsProperties -ExtranetLockoutMode AdfsSmartLockoutEnforce`

Per la nuova modalità rendere effettive, riavviare il servizio AD FS in tutti i nodi nella farm utilizzando il comando seguente.

`Restart-service adfssrv`

## <a name="manage-user-account-activity"></a>Gestire le attività di account utente
ADFS offre tre cmdlet per gestire i dati di attività account. Questi cmdlet connettersi automaticamente al nodo della farm che detiene il ruolo master.
>[!NOTE]
>Just Enough Administration (JEA) è utilizzabile per delegare i commandlet di AD FS per reimpostare i blocchi degli account. Ad esempio, il personale Help Desk possono essere autorizzazioni delegate per utilizzare i commandlet ESL. Per informazioni sulla delega delle autorizzazioni per l'uso di questi cmdlet, vedere [delegato AD FS Powershell cmdlet di accesso agli utenti senza privilegi di amministratore](delegate-ad-fs-pshell-access.md)

Questo comportamento può essere sottoposto a override, passando il parametro - Server.

- Get-ADFSAccountActivity -UserPrincipalName

  Leggere le attività di account attuale per un account utente. Il cmdlet si connette sempre automaticamente al master farm tramite l'endpoint REST dell'Account attività. Pertanto, tutti i dati devono essere sempre coerente.

`Get-ADFSAccountActivity user@contoso.com`

  Proprietà:
    - BadPwdCountFamiliar: Incrementato quando l'autenticazione ha esito positivo da una posizione nota.
    - BadPwdCountUnknown: Incrementato quando l'autenticazione ha esito negativo da una posizione sconosciuta
    - LastFailedAuthFamiliar: Se l'autenticazione ha esito negativo da una posizione nota, LastFailedAuthUnknown è impostato al momento dell'autenticazione non riusciti
    - LastFailedAuthUnknown: Se l'autenticazione ha esito negativo da una posizione sconosciuta, LastFailedAuthUnknown è impostato al momento dell'autenticazione non riusciti
    - FamiliarLockout: Valore booleano che è "True" se "BadPwdCountFamiliar" > ExtranetLockoutThreshold
    - UnknownLockout: Valore booleano che è "True" se "BadPwdCountUnknown" > ExtranetLockoutThreshold  
    - FamiliarIPs: numero massimo di 20 indirizzi IP che sono familiari per l'utente. Quando viene superato questo valore verrà rimosso l'indirizzo IP meno recente nell'elenco.
-    Set-ADFSAccountActivity

     Aggiunge nuove posizioni note. L'elenco di IP familiarità con un massimo di 20 voci, se tale limite viene superato, l'indirizzo IP meno recente nell'elenco verrà rimossi.

`Set-ADFSAccountActivity user@contoso.com -AdditionalFamiliarIps “1.2.3.4”`

- Reset-ADFSAccountLockout

  Reimposta il contatore del blocco per un account utente per ogni posizione nota (badPwdCountFamiliar) o i contatori di posizione insolita (badPwdCountUnfamiliar). Ripristinando un contatore, il valore "FamiliarLockout" o "UnfamiliarLockout" aggiornerà, come il Reimposta contatore sarà inferiore alla soglia.  

`Reset-ADFSAccountLockout user@contoso.com -Location Familiar`
`Reset-ADFSAccountLockout user@contoso.com -Location Unknown`

## <a name="event-logging--user-activity-information-for-ad-fs-extranet-lockout"></a>La registrazione degli eventi e informazioni sull'attività per AD FS Extranet Lockout

### <a name="connect-health"></a>Connect Health
È consigliabile monitorare l'attività di account utente tramite Connect Health. Connect Health genera la creazione di report scaricabili per gli indirizzi IP rischiosi e tentativi con password errata. Ogni elemento nel report sugli indirizzi IP rischiosi Mostra informazioni aggregate su AD FS sign-nelle attività non riuscite che superano la soglia designata. Non appena questo errore si verifica con le impostazioni di posta elettronica personalizzabili, è possono impostare le notifiche di posta elettronica per avvisare gli amministratori. Per altre informazioni e istruzioni di installazione, visitare il [documentazione di Connect Health](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-health-adfs).

### <a name="ad-fs-extranet-smart-lockout-events"></a>Eventi di AD FS Extranet Lockout intelligente.
Per gli eventi di blocco Smart Extranet da scrivere, ESL deve essere abilitata in modalità 'solo log' o 'applica' e il controllo di sicurezza di ad FS è abilitato.
AD FS scriverà gli eventi di blocco extranet per il log di controllo di sicurezza:
- Quando un utente viene bloccato out (raggiunge la soglia di blocco per i tentativi di accesso non riuscito)
- Quando AD FS riceve un tentativo di accesso per gli utenti che si trova già in stato di blocco

In modalità solo log, è possibile controllare il log di controllo di sicurezza per gli eventi di blocco. Per tutti gli eventi disponibili, è possibile controllare lo stato utente usando il cmdlet Get-ADFSAccountActivity per determinare se il blocco si sono verificati da indirizzi IP noti o non conosciuti e per controllare l'elenco di indirizzi IP noti per l'utente.


|ID evento|Descrizione|
|-----|-----|
|1203|Questo evento viene scritto per ogni tentativo di password errata. Non appena il badPwdCount raggiunge il valore specificato in ExtranetLockoutThreshold, l'account verrà bloccato in ad FS per la durata specificata in ExtranetObservationWindow.</br>ID attività: %1</br>XML: %2|
|1201|Questo evento viene scritto ogni volta che un utente viene bloccato. </br>ID attività: %1</br>XML: %2|
|557 (ADFS 2019)| Si è verificato un errore durante il tentativo di comunicare con il servizio rest di archivio di account nel nodo %1. Se si tratta di una farm database interno di Windows nel nodo primario potrebbe essere offline. Se si tratta di una farm SQL ad FS seleziona automaticamente un nuovo nodo per ospitare il ruolo di master archivio utente.|
|562 (ADFS 2019)|Si è verificato un errore quando si iniziare la comunicazione con l'account di archivio dell'endpoint nel server %1.</br>Messaggio eccezione: %2|
|563 (ADFS 2019)|Si è verificato un errore durante il calcolo dello stato di blocco extranet. A causa del valore %%1 l'impostazione dell'autenticazione sarà consentita per questo utente e continuerà l'emissione di token. Se si tratta di una farm database interno di Windows nel nodo primario potrebbe essere offline. Se si tratta di una farm SQL ad FS seleziona automaticamente un nuovo nodo per ospitare il ruolo di master archivio utente.</br>Nome del server archivio account: %2</br>Id utente: %3</br>Messaggio eccezione: %4|
|512|L'account per l'utente seguente è bloccato. Un tentativo di accesso sia consentito a causa della configurazione di sistema.</br>ID attività: %1 </br>Utente: %2 </br>Client IP: %3 </br>Numero di Password errata: %4  </br>Ultimo tentativo di Password errata: %5|
|515|Il seguente account utente era in uno stato bloccato e la password corretta semplicemente è stata fornita. Questo account può risultare compromesso.</br>Dati aggiuntivi </br>ID attività: %1 </br>Utente: %2 </br>Client IP: %3 |
|516|Il seguente account utente è stato bloccato a causa di troppi tentativi con password errate.</br>ID attività: %1  </br>Utente: %2  </br>Client IP: %3  </br>Numero di Password errata: %4  </br>Ultimo tentativo di Password errata: %5|

## <a name="esl-frequently-asked-questions"></a>Domande frequenti su ESL

**Una farm di ad FS con il blocco Smart Extranet in applicherà la modalità sempre vedere i blocchi degli utenti malintenzionati?** 

R: Se ad FS il blocco Smart è impostato su 'applica' modalità quindi non si vedranno mai account dell'utente legittimo bloccato da attacchi di forza bruta o denial of service. L'unico modo che un blocco degli account dannosi può impedire l'accesso un utente è se l'attore non valida è la password dell'utente o possono inviare richieste da un indirizzo IP valido (noti) noto per quell'utente. 

**Cosa accade ESL è abilitata e l'attore non valido dispone di una password utente?** 

R: Lo scopo tipico di scenario di attacco di forza bruta è indovinare una password e effettuare l'accesso.  Se un utente è phishing delle o se è stata ipotizzata una password quindi la funzionalità ESL non bloccherà l'accesso perché l'accesso soddisferà "riusciti" criteri di password corretta più nuovo indirizzo IP. L'indirizzo IP cattivi attori verrebbe quindi visualizzata come una "note". La soluzione migliore in questo scenario consiste nel deselezionare l'attività dell'utente in ad FS e per richiedere Multi-Factor Authentication per gli utenti. È fortemente consigliabile installare la protezione con Password AAD che garantisce le password da indovinare non si ottengono nel sistema.

**Se l'utente non ha mai eseguito l'accesso da un indirizzo IP e quindi prova con password errata più volte verrà potranno eseguire l'accesso una volta vengono infine digitare la password in modo corretto?** 

R: Se un utente invia più password non valida (ad esempio legittimamente erroneamente la digitazione) e sul tentativo seguente ottiene la password corretta, quindi l'utente avrà immediatamente esito positivo per l'accesso.  Verrà deselezionare il conteggio delle password errate e verrà aggiunto all'elenco FamiliarIPs tale IP.  Tuttavia, se si superano la soglia di accessi non riusciti dal percorso sconosciuto, dovrà immettere in stato di blocco e richiedono attendere oltre la finestra di osservazione e Accedi con una password valida o richiedono l'intervento dell'amministratore per reimpostare il proprio account.  

**ESL funziona intranet troppo?**   
R: Se il client si connettono direttamente ai server ad FS e non tramite Server Proxy applicazione Web quindi il comportamento ESL non verrà applicata. 

**Vengono visualizzati gli indirizzi IP di Microsoft nel campo indirizzo IP del Client. Attacchi di forza ESL blocco EXO trasmessa tramite proxy bruta?**  

R: ESL funzionerà correttamente per evitare di Exchange Online o in altri scenari di attacco di forza bruta autenticazione legacy. "ID attività" 00000000-0000-0000-0000-000000000000 dispone di un'autenticazione legacy. In questi attacchi, l'attore malintenzionato potrebbe sta sfruttando l'autenticazione di base Exchange Online (noto anche come autenticazione legacy) in modo che l'indirizzo IP client visualizzato come Microsoft Outlook. Server a Exchange online nel proxy di cloud, la verifica di autenticazione per conto del client di Outlook. In questi scenari, l'indirizzo IP del richiedente malintenzionato sarà in x-ms-inoltrati--ip client e il server Microsoft Exchange Online che sarà IP nel valore di x-ms-client-ip.
Blocco Extranet Smart controlla rete indirizzi IP, inoltrati gli indirizzi IP, x-inoltrati--IP client e il valore di x-ms-client-ip. Se la richiesta ha esito positivo, tutti gli indirizzi IP vengono aggiunti all'elenco familiare. Se una richiesta in entrata e in uno qualsiasi degli IP presentato non sono presenti nell'elenco familiare quindi la richiesta verrà contrassegnata come non note. L'utente ha familiarità sarà in grado di accedere correttamente anche se le richieste provenienti da posizioni non note verranno bloccate.  


## <a name="additional-references"></a>Altri riferimenti  
[Le procedure consigliate per la protezione di Active Directory Federation Services](../../ad-fs/deployment/best-practices-securing-ad-fs.md)

[Set-AdfsProperties](https://technet.microsoft.com/itpro/powershell/windows/adfs/set-adfsproperties)

[Operazioni di AD FS](../../ad-fs/AD-FS-2016-Operations.md)
