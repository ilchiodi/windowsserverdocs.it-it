---
title: La gestione del servizio sorveglianza Host
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: eecb002e-6ae5-4075-9a83-2bbcee2a891c
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.openlocfilehash: dab27e71e42970507f321271edda90f6d161c691
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447396"
---
# <a name="managing-the-host-guardian-service"></a>La gestione del servizio sorveglianza Host

> Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

Il servizio sorveglianza Host (HGS) è il cuore della soluzione di infrastruttura sorvegliata.
È responsabile per garantire che gli host Hyper-V nell'infrastruttura sono noti all'host o dell'organizzazione e l'esecuzione di software considerato attendibile e per gestire le chiavi usate per avviare le macchine virtuali schermate.
Quando un tenant decide di attendibilità per ospitare le proprie macchine virtuali schermate, si inseriscono i trust nella configurazione e gestione del servizio sorveglianza Host.
Pertanto, è molto importante seguire le procedure consigliate quando si gestiscono il servizio sorveglianza Host per garantire la sicurezza, disponibilità e affidabilità dell'infrastruttura sorvegliata.
Le indicazioni fornite nelle sezioni seguenti risolve i problemi operativi più comuni per gli amministratori del servizio HGS.

## <a name="limiting-admin-access-to-hgs"></a>Limitando l'accesso amministratore HGS
A causa della natura sensibile di sicurezza del servizio HGS, è importante assicurarsi che gli amministratori siano estremamente attendibili i membri dell'organizzazione e, in teoria, separano dagli amministratori delle risorse di infrastruttura.
Inoltre, è consigliabile gestire solo HGS da workstation protetta usando protocolli di comunicazione protetta, ad esempio gestione remota Windows su HTTPS.

### <a name="separation-of-duties"></a>Separazione dei compiti
Quando si configura HGS, si ha la possibilità di creare una foresta isolata di Active Directory solo per il servizio HGS oppure per unire HGS a un dominio esistente e attendibile.
Questa decisione, nonché i ruoli assegnare gli amministratori dell'organizzazione, è possibile determinare il limite di attendibilità per HGS.
Chiunque può accedere a HGS, se direttamente come un amministratore o indirettamente l'amministratore di un altro elemento (ad esempio, Active Directory) che possono influenzare il servizio HGS, ha il controllo tramite l'infrastruttura sorvegliata.
Gli amministratori HGS scegliere quali host Hyper-V sono autorizzati a eseguire VM schermate e gestire i certificati necessari per avviare i backup di macchine virtuali schermate.
Un amministratore malintenzionato chi può accedere a HGS o un utente malintenzionato può utilizzare questo power per autorizzare host compromessi a eseguire VM schermate, per avviare un attacco denial of service rimuovendo il materiale della chiave e altro ancora.

Per evitare questo rischio, si tratta *fortemente* consigliabile limitare la sovrapposizione tra gli amministratori di HGS (incluso il dominio a cui viene aggiunto HGS) e ambienti Hyper-V.
Verificando che nessuna solo amministratore può accedere a entrambi i sistemi, un utente malintenzionato compromettere account diversi 2 di 2 singoli utenti per completare il suo obiettivo per modificare i criteri HGS.
Questo significa anche che gli amministratori dell'organizzazione e dominio per i due ambienti di Active Directory non devono essere la stessa persona né HGS utilizzino la stessa foresta di Active Directory come gli host Hyper-V.
Chiunque può concedere a se stessi l'accesso alle altre risorse rappresenta un rischio di sicurezza.

### <a name="using-just-enough-administration"></a>Utilizzo di Just Enough Administration
È dotato di HGS [Just Enough Administration](https://aka.ms/JEAdocs) ruoli (JEA) predefiniti che consentono di gestire in modo più sicuro.
JEA consente, consentendo di delegare attività amministrative per utenti non amministratori, vale a dire le persone che gestiscono i criteri di HGS non devono essere effettivamente gli amministratori dell'intera macchina o del dominio.
Il funzionamento di JEA, limitare i comandi che un utente può eseguire in una sessione di PowerShell e usare un account temporaneo locale dietro le quinte (univoche per ogni sessione utente) per eseguire i comandi che in genere non richiedono l'elevazione dei privilegi.

HGS dotato di 2 ruoli JEA preconfigurati:
- **Gli amministratori di HGS** che consente agli utenti di gestire tutti i criteri HGS, inclusa l'autorizzazione nuovi host per l'esecuzione di macchine virtuali schermate.
- **I revisori di HGS** consentendo solo agli utenti il diritto di controllare i criteri esistenti. Non è possibile rendono tutte le modifiche alla configurazione del servizio HGS.

Per usare JEA, è necessario innanzitutto creare un nuovo utente standard e renderli un membro del gruppo di revisori HGS o gli amministratori HGS.
Se è stata usata `Install-HgsServer` per configurare una nuova foresta per HGS, questi gruppi verranno denominati "*servicename*gli amministratori" e "*servicename*revisori", rispettivamente, in cui *servicename*  è il nome di rete del cluster servizio HGS.
Se HGS è stato aggiunto a un dominio esistente, è consigliabile vedere i nomi dei gruppi specificati nella `Initialize-HgsServer`.

**Creare gli utenti standard per i ruoli di amministratore e revisore HGS**

```powershell
$hgsServiceName = (Get-ClusterResource HgsClusterResource | Get-ClusterParameter DnsName).Value
$adminGroup = $hgsServiceName + "Administrators"
$reviewerGroup = $hgsServiceName + "Reviewers"

New-ADUser -Name 'hgsadmin01' -AccountPassword (Read-Host -AsSecureString -Prompt 'HGS Admin Password') -ChangePasswordAtLogon $false -Enabled $true
Add-ADGroupMember -Identity $adminGroup -Members 'hgsadmin01'

New-ADUser -Name 'hgsreviewer01' -AccountPassword (Read-Host -AsSecureString -Prompt 'HGS Reviewer Password') -ChangePasswordAtLogon $false -Enabled $true
Add-ADGroupMember -Identity $reviewerGroup -Members 'hgsreviewer01'
```

**Criteri di controllo con il ruolo del revisore**

In un computer remoto che disponga della connettività di rete al servizio HGS, eseguire i comandi seguenti in PowerShell per immettere la sessione JEA con le credenziali del revisore.
È importante notare che poiché l'account del revisore è solo un utente standard, non può essere usato per regolare comunicazione remota di Windows PowerShell, accesso Desktop remoto a HGS e così via.

```powershell
Enter-PSSession -ComputerName <hgsnode> -Credential '<hgsdomain>\hgsreviewer01' -ConfigurationName 'microsoft.windows.hgs'
```

È quindi possibile controllare quali comandi sono consentiti nella sessione con `Get-Command` ed eseguire i comandi consentiti per controllare la configurazione.
Nell'esempio seguente è in corso un controllo quali criteri sono abilitati nel servizio HGS.

```powershell
Get-Command

Get-HgsAttestationPolicy
```

Digitare il comando `Exit-PSSession` o il relativo alias `exit`, al termine funziona con la sessione JEA. 

**Aggiungere un nuovo criterio usando il ruolo di amministratore HGS**

Per modificare un criterio, è necessario connettersi all'endpoint JEA con un'identità appartenente al gruppo 'hgsAdministrators'.
Nell'esempio seguente viene illustrato come copiare un nuovo criterio di integrità codice HGS e registrarlo con JEA.
La sintassi può essere diversa da quella usata per.
Si tratta di supportare alcune delle limitazioni previste JEA come non avere accesso al sistema completo del file.

```powershell
$cipolicy = Get-Item "C:\temp\cipolicy.p7b"
$session = New-PSSession -ComputerName <hgsnode> -Credential '<hgsdomain>\hgsadmin01' -ConfigurationName 'microsoft.windows.hgs'
Copy-Item -Path $cipolicy -Destination 'User:' -ToSession $session

# Now that the file is copied, we enter the interactive session to register it with HGS
Enter-PSSession -Session $session
Add-HgsAttestationCiPolicy -Name 'New CI Policy via JEA' -Path 'User:\cipolicy.p7b'

# Confirm it was added successfully
Get-HgsAttestationPolicy -PolicyType CiPolicy

# Finally, remove the PSSession since it is no longer needed
Exit-PSSession
Remove-PSSession -Session $session
```

## <a name="monitoring-hgs"></a>Monitoraggio servizio HGS
### <a name="event-sources-and-forwarding"></a>Origini evento e l'inoltro
Gli eventi dal servizio HGS verranno visualizzato nel registro eventi di Windows in 2 origini:
- **HostGuardianService-Attestation**
- **HostGuardianService-KeyProtection**

È possibile visualizzare questi eventi, aprire Visualizzatore eventi e passando al Microsoft-Windows-HostGuardianService-attestazione e Microsoft-Windows-HostGuardianService-KeyProtection.

In un ambiente di grandi dimensioni, è spesso preferibile per inoltrare eventi a una raccolta di eventi Windows centrale per semplificare l'analisi degli eventi.
Per altre informazioni, consultare il [documentazione di Windows Event Forwarding](https://msdn.microsoft.com/library/windows/desktop/bb427443.aspx).

### <a name="using-system-center-operations-manager"></a>Utilizzo di System Center Operations Manager
È anche possibile usare System Center 2016 - Operations Manager per monitorare gli host sorvegliati e HGS.
Il management pack dell'infrastruttura sorvegliata dispone i monitoraggi di eventi per verificare la presenza di errori di configurazione comuni che possono portare a tempi di inattività Data Center, inclusi gli host non passando l'attestazione e i server HGS la segnalazione degli errori.

Per iniziare, [installare e configurare 2016 SCOM](https://technet.microsoft.com/system-center-docs/om/welcome-to-operations-manager) e [scaricare il management pack dell'infrastruttura sorvegliata](https://www.microsoft.com/download/details.aspx?id=52764).
La Guida inclusi management pack viene illustrato come configurare il management pack e comprendere l'ambito dei relativi monitoraggi.

## <a name="backing-up-and-restoring-hgs"></a>Backup e ripristino HGS
### <a name="disaster-recovery-planning"></a>Pianificazione del ripristino di emergenza
Quando disegnate piani di ripristino di emergenza, è importante prendere in considerazione i requisiti univoci del servizio sorveglianza Host nell'infrastruttura sorvegliata.
Se l'utente smarrisce alcuni o tutti i nodi del servizio HGS, si potrebbero riscontrare problemi di disponibilità immediata che impediscono agli utenti di avviare le proprie macchine virtuali schermate.
In uno scenario in cui si perde l'intero cluster HGS, è necessario disporre di un backup completo della configurazione del servizio HGS disponibili per il cluster del servizio HGS di ripristinare e riprendere le normali operazioni.
In questa sezione illustra i passaggi necessari per preparare per tale scenario.

In primo luogo, è importante comprendere che cosa HGS è importante eseguire il backup.
HGS consente di mantenere alcuni tipi di informazioni che consentono di determinano quali host sono autorizzati a eseguire VM schermate.
È possibile creare, ad esempio:
1. Gli identificatori di protezione di Active Directory per i gruppi contenenti host (quando si usa l'attestazione di Active Directory); attendibili
2. Identificatori TPM univoci per ogni host nell'ambiente in uso;
3. Criteri TPM per ogni configurazione univoci di host. e
4. Criteri di integrità di codice che determinano quale software è consentito l'esecuzione negli host.

Questi elementi attestazione richiedono il coordinamento con gli amministratori dell'infrastruttura di hosting per ottenere, potenzialmente è difficile ottenere questa informazione nuovamente dopo un'emergenza.

Inoltre, HGS richiede l'accesso a 2 o più certificati usati per crittografare e firmare le informazioni necessarie per avviare una macchina virtuale schermata (la protezione con chiave).
Questi certificati sono ben noti (usate dai proprietari delle macchine virtuali schermate per autorizzare l'infrastruttura a eseguire le proprie macchine virtuali) e devono essere ripristinati dopo un'emergenza per un'esperienza di ripristino sicuro.
Dovrebbe aver non ripristinato HGS con gli stessi certificati dopo un'emergenza, ogni macchina virtuale dovrà essere aggiornata per autorizzare le nuove chiavi per decrittografare le informazioni.
Per motivi di sicurezza, solo il proprietario della macchina virtuale può aggiornare la configurazione della macchina virtuale per autorizzare le nuove chiavi, errore di significato per ripristinare le chiavi dopo un'emergenza comporterà ogni proprietario della macchina virtuale che sia necessario intervenire per ottenere le proprie macchine virtuali in esecuzione anche in questo caso.

#### <a name="preparing-for-the-worst"></a>Preparazione per il peggiore
Per preparare una perdita completa di HGS, esistono 2 passaggi che è necessario eseguire:
1. Eseguire il backup dei criteri di attestazione HGS
2. Il backup delle chiavi HGS

Vengono fornite istruzioni su come eseguire entrambi i passaggi nel [backup HGS](#backing-up-hgs) sezione.

È inoltre consigliabile, ma non obbligatorio, esecuzione di un elenco di utenti autorizzati a gestire HGS nel proprio dominio di Active Directory o Active Directory stessa.

Deve essere backup regolarmente per verificare che le informazioni siano aggiornati e archiviati in modo sicuro per evitare la manomissione o furto.

Si tratta **sconsigliato** eseguire il backup o tentare di ripristinare un'immagine del sistema intera di un nodo di HGS.
Nel caso in cui si è perso l'intero cluster, la procedura consigliata è configurare un nuovo nodo HGS e ripristinare solo lo stato del servizio HGS, non l'intero server del sistema operativo.

#### <a name="recovering-from-the-loss-of-one-node"></a>Il recupero dalla perdita di un nodo
Se si perde una o più nodi (ma non tutti i nodi) nel cluster HGS, è possibile semplicemente [aggiungere nodi al cluster](guarded-fabric-configure-additional-hgs-nodes.md) seguendo le istruzioni nella Guida alla distribuzione.
I criteri di attestazione verranno sincronizzati automaticamente, così come tutti i certificati che sono stati forniti al servizio HGS come file PFX con le relative password.
Per i certificati aggiunti al servizio HGS usando un'identificazione personale (non esportabile e basato su hardware certificati, comunemente), è necessario assicurarsi che ciascun nuovo nodo abbia accesso alla chiave privata di ogni certificato.

#### <a name="recovering-from-the-loss-of-the-entire-cluster"></a>Ripristino dopo la perdita dell'intero cluster
Se l'intero cluster HGS diventa inattiva e non si riesce a riportarlo in linea, dovrai ripristinare HGS da un backup.
Il ripristino di HGS da un backup prevede prima impostazione di un nuovo cluster HGS per il [linee guida nella Guida alla distribuzione](guarded-fabric-setting-up-the-host-guardian-service-hgs.md).
È consigliabile, ma non obbligatorio, usare lo stesso nome del cluster quando si configura l'ambiente del servizio HGS di ripristino per agevolare la risoluzione dei nomi dagli host.
Usando lo stesso nome evita di dover riconfigurare gli host con nuova attestazione e protezione con chiave URL.
Se è stato ripristinato gli oggetti per il dominio di Active Directory eseguire il backup HGS, si consiglia di rimuovere gli oggetti che rappresentano cluster HGS, computer, account del servizio e i gruppi di JEA prima di inizializzare il server HGS.

Dopo avere configurato il primo nodo del servizio HGS (ad esempio, è stato installato e inizializzato), si seguiranno le procedure descritte in [HGS il ripristino da un backup](#restoring-hgs-from-a-backup) per ripristinare i criteri di attestazione e metà pubblica la protezione delle chiavi certificati.
È necessario ripristinare le chiavi private per i certificati manualmente in base alle indicazioni del provider di certificati (ad esempio, importare il certificato in Windows, o configurare l'accesso al modulo di protezione hardware certificati).
Dopo aver configurato il primo nodo, è possibile continuare a [installare ulteriori nodi al cluster](guarded-fabric-configure-additional-hgs-nodes.md) fino a quando non è stata raggiunta la capacità e resilienza desiderato.

### <a name="backing-up-hgs"></a>Backup di HGS
L'amministratore HGS dovrebbe essere responsabile per il backup HGS a intervalli regolari.
Un backup completo contiene materiale riservato che deve essere protetta in modo appropriato.
Un'entità non attendibile dovrà ad accedere a queste chiavi, è possibile usare che materiale per configurare un ambiente di HGS dannoso allo scopo di compromettere macchine virtuali schermate.

**Il backup dei criteri di attestazione** per eseguire il backup dei criteri di attestazione HGS, eseguire il comando seguente in qualsiasi nodo del server HGS funzionante.
Verrà richiesto di fornire una password.
Questa password viene utilizzata per crittografare i certificati aggiunti al servizio HGS usando un file PFX (anziché un'identificazione personale del certificato).

```powershell
Export-HgsServerState -Path C:\temp\HGSBackup.xml
```

> [!NOTE]
> Se si usa l'attestazione di attendibilità dell'amministratore, è necessario eseguire separatamente backup l'appartenenza a gruppi di sicurezza usati dal servizio HGS per autorizzare gli host sorvegliati.
> HGS eseguirà solo il backup il SID dei gruppi di sicurezza, non l'appartenenza al loro interno.
> Nel caso in cui questi gruppi vengono persi durante un'emergenza, dovrai ricreare i gruppi e aggiungere di nuovo ogni host sorvegliato ad essi.

**Backup dei certificati**

Il `Export-HgsServerState` comando verrà eseguito il backup di tali certificati in base al file PFX aggiunti al servizio HGS al momento dell'esecuzione del comando.
Se i certificati è stato aggiunto al servizio HGS usando un'identificazione personale (tipica per i certificati non esportabile e basato su hardware), dovrai eseguire manualmente il backup di chiavi private per i certificati.
Per identificare quali certificati sono registrati con HGS e necessario eseguire il backup manualmente, eseguire il comando PowerShell seguente in qualsiasi nodo del server HGS funzionante.

```powershell
Get-HgsKeyProtectionCertificate | Where-Object { $_.CertificateData.GetType().Name -eq 'CertificateReference' } | Format-Table Thumbprint, @{ Label = 'Subject'; Expression = { $_.CertificateData.Certificate.Subject } }
```

Per ognuno dei certificati elencati, è necessario eseguire manualmente il backup della chiave privata.
Se si usa certificato basata su software che non può essere esportata, contattare l'autorità di certificazione per assicurarsi che dispone di un backup del certificato e/o può ripetere su richiesta.
Per i certificati creati e archiviati in moduli di protezione hardware, è consigliabile consultare la documentazione per il dispositivo, per indicazioni sulla pianificazione del ripristino di emergenza.

È consigliabile archiviare i backup del certificato con i backup dei criteri di attestazione in un luogo sicuro in modo che entrambe le parti possono essere ripristinati insieme.

**Configurazione aggiuntiva per eseguire il backup**

Backup di HGS lo stato del server non includerà il nome del cluster del servizio HGS, tutte le informazioni da Active Directory, o qualsiasi certificato SSL usato per proteggere le comunicazioni con le API del servizio HGS.
Queste impostazioni sono importanti per garantire l'uniformità ma non è fondamentale per ottenere nuovamente online il cluster del servizio HGS dopo un'emergenza.

Per acquisire il nome del servizio HGS, eseguire `Get-HgsServer` e prendere nota del nome flat nell'attestazione e l'URL di protezione con chiave.
Ad esempio, se è l'URL di attestazione "<http://hgs.contoso.com/Attestation>", "hgs" è il nome del servizio HGS.

Il dominio di Active Directory utilizzato dal servizio HGS deve essere gestito come qualsiasi altro dominio di Active Directory.
Quando si ripristina HGS dopo un'emergenza, non necessariamente dovrai ricreare esattamente gli oggetti presenti nel dominio corrente.
Tuttavia, verranno effettuate ripristino più semplice se eseguire il backup di Active Directory e di mantenere un elenco degli utenti JEA autorizzato a gestire il sistema, nonché l'appartenenza ai gruppi di sicurezza utilizzato dall'attestazione amministratore per autorizzare gli host sorvegliati.

Per identificare l'identificazione personale dei certificati SSL configurato per HGS, eseguire il comando seguente in PowerShell.
È quindi possibile eseguire il backup i certificati SSL in base alle istruzioni del provider del certificato.

```powershell
Get-WebBinding -Protocol https | Select-Object certificateHash
```

### <a name="restoring-hgs-from-a-backup"></a>Il ripristino di HGS da un backup
I passaggi seguenti descrivono come ripristinare le impostazioni di HGS da un backup.
I passaggi sono rilevanti per entrambe le situazioni in cui si sta tentando di annullare le modifiche apportate alle istanze del servizio HGS di già in esecuzione e quando crea un nuovo cluster HGS dopo una perdita completa di quello precedente.

#### <a name="set-up-a-replacement-hgs-cluster"></a>Configurare un cluster del servizio HGS sostituzione
Prima di poter ripristinare HGS, è necessario avere un cluster del servizio HGS inizializzato a cui è possibile ripristinare la configurazione.
Se si importano le impostazioni che sono state eliminate accidentalmente semplicemente a un cluster esistente (in esecuzione), è possibile ignorare questo passaggio.
Se si ripristina da una perdita completa di HGS, è necessario installare e inizializzare almeno HGS nodo seguendo la [linee guida nella Guida alla distribuzione](guarded-fabric-setting-up-the-host-guardian-service-hgs.md).

In particolare, è necessario:
1. [Configurare il dominio del servizio HGS](guarded-fabric-choose-where-to-install-hgs.md) o join HGS a un dominio esistente
2. [Inizializzare il server HGS](guarded-fabric-initialize-hgs.md) usando le chiavi esistenti *o* un set di chiavi temporanee. È possibile [rimuovere le chiavi temporanee](#renewing-or-replacing-keys) dopo l'importazione di chiavi effettive da HGS il backup dei file.
3. [Importare le impostazioni di HGS](#import-settings-from-a-backup) dal backup per ripristinare i gruppi di host attendibili, i criteri di integrità di codice, le linee di base TPM e gli identificatori TPM

> [!TIP]
> Il nuovo cluster HGS non devi usare la stessa dei certificati, nome del servizio o nel dominio dell'istanza del servizio HGS da cui è stato esportato il file di backup.

#### <a name="import-settings-from-a-backup"></a>Importare le impostazioni da un backup

Per ripristinare l'attestazione criteri, i certificati basati sul file PFX e le chiavi pubbliche dei certificati PFX non al nodo del servizio HGS da un file di backup, eseguire il comando seguente in un nodo server HGS inizializzato.
Verrà richiesto di immettere la password specificata durante la creazione del backup.

```powershell
Import-HgsServerState -Path C:\Temp\HGSBackup.xml
```

Se si desidera solo l'importazione di criteri di attestazione amministratore o i criteri di attestazione TPM, è possibile farlo, specificando il `-ImportActiveDirectoryModeState` o `-ImportTpmModeState` flag [importazione HgsServerState](https://technet.microsoft.com/library/mt652168.aspx).

Verificare che l'aggiornamento cumulativo più recente per Windows Server 2016 è installato prima dell'esecuzione `Import-HgsServerState`.
In caso contrario, può comportare un errore di importazione.

> [!NOTE]
> Se si ripristinano i criteri in un nodo HGS esistente che dispone già di uno o più di tali criteri installati, il comando di importazione verrà visualizzato un errore per ogni criterio duplicazione.
> Questo è un comportamento previsto e può essere tranquillamente ignorato nella maggior parte dei casi.

#### <a name="reinstall-private-keys-for-certificates"></a>Reinstallare le chiavi private dei certificati
Se uno qualsiasi dei certificati utilizzati sul servizio HGS da cui è stato creato il backup sono stati aggiunti tramite le identificazioni personali, solo la chiave pubblica dei certificati verrà inclusi nel file di backup.
Ciò significa che devi manualmente installare e/o concedere l'accesso alle chiavi private per ognuno di tali certificati prima di HGS può servire le richieste dagli host Hyper-V.
Le azioni necessarie per completare tale passaggio varia a seconda del modo in cui il certificato è stato inizialmente emesso.
Per certificati basate su software rilasciati da un'autorità di certificazione, devi contattare l'autorità di certificazione per ottenere la chiave privata e installarlo nel **ogni** nodo HGS per le istruzioni.
Analogamente, se i certificati sono basato su hardware, è necessario consultare la documentazione del produttore dell'hardware security module per installare i driver necessari in ogni nodo HGS per connettere il modulo di protezione hardware e concedere l'accesso ogni macchina per la chiave privata.

Come promemoria, aggiunti al servizio HGS usando le identificazioni personali dei certificati richiedono la replica manuale delle chiavi private per ogni nodo.
È necessario ripetere questo passaggio in ogni nodo aggiuntivo che aggiunti al cluster servizio HGS ripristinato.

#### <a name="review-imported-attestation-policies"></a>Esaminare i criteri di attestazione importati
Dopo aver importato le impostazioni da un backup, è consigliabile controllare strettamente tutti i criteri importati tramite `Get-HgsAttestationPolicy` per assicurarsi che solo gli host considerato attendibile per eseguire VM schermate saranno in grado di attestare correttamente.
Se si trova tutti i criteri che non corrispondono più al comportamento di sicurezza, è possibile [disabilitare o rimuoverli](#review-attestation-policies).

#### <a name="run-diagnostics-to-check-system-state"></a>Eseguire la diagnostica per verificare lo stato del sistema
Terminata la configurazione e ripristino dello stato del nodo del servizio HGS, è necessario eseguire lo strumento di diagnostica del servizio HGS per controllare lo stato del sistema.
A tale scopo, eseguire il comando seguente nel nodo del servizio HGS, in cui è stato ripristinato la configurazione:

```powershell
Get-HgsTrace -RunDiagnostics
```

Se il risultato"generale" non "passare", sono necessari passaggi aggiuntivi per completare la configurazione del sistema.
Controllare i messaggi inviati in subtest(s) che non è riuscita per ulteriori informazioni.

## <a name="patching-hgs"></a>L'applicazione di patch HGS
È importante mantenere aggiornati i nodi del servizio sorveglianza Host installando l'aggiornamento cumulativo più recente quando si tratta di. Se si configura un nuovo nodo HGS, è consigliabile installare eventuali aggiornamenti disponibili prima di installare il ruolo del servizio HGS o configurazione.
In tal modo, eventuali nuove o modificate funzionalità avrà effetto immediato.

Quando l'applicazione di patch l'infrastruttura sorvegliata, è consigliabile aggiornare prima di tutto *tutte* host Hyper-V **prima dell'aggiornamento HGS**.
Questo modo si garantisce che vengono apportate modifiche per i criteri di attestazione HGS *dopo* gli host Hyper-V sono stati aggiornati per fornire le informazioni necessarie per loro.
Se un aggiornamento venga modificato il comportamento dei criteri, questi saranno non abilitati automaticamente per evitare di danneggiare l'infrastruttura.
Questi aggiornamenti richiedono di seguire le indicazioni fornite nella sezione seguente per attivare i criteri di attestazione nuove o modificate.
Si consiglia di leggere le note sulla versione per Windows Server e di eventuali aggiornamenti cumulativi installati in modo per verificare se sono necessari gli aggiornamenti dei criteri.

### <a name="updates-requiring-policy-activation"></a>Aggiornamenti che richiedono attivazione criteri
Se un aggiornamento per il servizio HGS introduce o la modifica in modo significativo il comportamento dei criteri di attestazione in un passaggio aggiuntivo necessario per attivare il criterio modificato.
Modifiche ai criteri vengono eseguiti solo dopo l'esportazione e importazione lo stato del servizio HGS.
È consigliabile attivare solo i criteri nuovi o modificati dopo aver applicato l'aggiornamento cumulativo di tutti gli host e tutti i nodi del servizio HGS nell'ambiente in uso.
Dopo aver aggiornato tutti i computer, eseguire i comandi seguenti in qualsiasi nodo HGS per attivare il processo di aggiornamento:

```powershell
$password = Read-Host -AsSecureString -Prompt "Enter a temporary password"
Export-HgsServerState -Path .\temporaryExport.xml -Password $password
Import-HgsServerState -Path .\temporaryExport.xml -Password $password
```

Se è stato introdotto un nuovo criterio, verrà disabilitata per impostazione predefinita.
Per abilitare i nuovi criteri, trovare prima nell'elenco dei criteri di Microsoft (preceduti da 'HGS_') e quindi abilitarlo usando i comandi seguenti:

```powershell
Get-HgsAttestationPolicy

Enable-HgsAttestationPolicy -Name <Hgs_NewPolicyName>
```

## <a name="managing-attestation-policies"></a>Gestione dei criteri di attestazione
HGS gestisce diversi criteri di attestazione che definiscono il set minimo di requisiti di che un host deve soddisfare per essere considerato "Integro" e a eseguire VM schermate.
Alcuni di questi criteri sono definiti da Microsoft, altri utenti vengono aggiunti dall'utente per definire i criteri di integrità di codice consentiti, le linee di base TPM e host nell'ambiente in uso.
È necessaria assicurarsi che gli host possono continuare attestazione correttamente come si aggiorna e sostituirli, e per assicurarsi che tutti gli host non trusted o configurazioni vengono bloccate correttamente attestazione regolare manutenzione dei criteri.

Per l'attestazione amministratore, è disponibile un solo criterio che determina se un host è integro: l'appartenenza a un gruppo di sicurezza note e attendibili.
L'attestazione TPM è più complicato e comporta diversi criteri per misurare il codice e la configurazione di un sistema prima di determinare se è integro.

Un servizio HGS singolo può essere configurato con i criteri di Active Directory e TPM in una sola volta, ma il servizio viene controllato solo i criteri per la modalità corrente che è configurato per quando un host prova che attesti.
Per controllare la modalità del server HGS, eseguire `Get-HgsServer`.

### <a name="default-policies"></a>Criteri predefiniti
Per l'attestazione TPM, esistono alcuni criteri predefiniti configurati nel servizio HGS.
Alcuni di questi criteri sono "bloccati", vale a dire che non può essere disabilitate per motivi di sicurezza.
La tabella seguente illustra lo scopo di ogni criterio predefinito.

Nome criterio                    | Scopo
-------------------------------|-----------------------------------------------------
Hgs_SecureBootEnabled          | Richiede che gli host per l'avvio protetto è abilitato. Ciò è necessario misurare i file binari di avvio e altre impostazioni di blocco UEFI.
Hgs_UefiDebugDisabled          | Assicura l'host non è abilitato un debugger del kernel. Debugger in modalità utente vengono bloccate con i criteri di integrità di codice.
Hgs_SecureBootSettings         | Criterio negativo per verificare che gli host corrisponde almeno una base TPM (definito dall'amministratore).
Hgs_CiPolicy                   | Criterio negativo per verificare che gli host utilizza uno dei criteri degli elementi di configurazione definito dall'amministratore.
Hgs_HypervisorEnforcedCiPolicy | Richiede criteri di integrità del codice possano essere applicate dall'hypervisor. Disattivando questo criterio indebolisce i meccanismi contro gli attacchi dei criteri di integrità codice in modalità kernel.
Hgs_FullBoot                   | Assicura che l'host non è stato ripristinato dalla modalità sospensione o ibernazione. Gli host devono essere riavviati o arrestati per soddisfare i criteri in modo corretto.
Hgs_VsmIdkPresent              | Richiede la sicurezza di virtualizzazione basata sia in esecuzione nell'host. Il IDK rappresenta la chiave necessaria per crittografare le informazioni inviate al spazio di memoria protetta dell'host.
Hgs_PageFileEncryptionEnabled  | Richiede che i file di paging siano crittografati nell'host. Disattivando questo criterio può comportare l'esposizione di informazioni se un file di paging non crittografata viene controllato per i segreti di tenant.
Hgs_BitLockerEnabled           | Richiede che BitLocker deve essere abilitata nell'host Hyper-V. Questo criterio è disabilitato per impostazione predefinita per motivi di prestazioni e consiglia di non essere abilitato. Questo criterio non è rilevante per la crittografia delle macchine virtuali schermate autonomamente.
Hgs_IommuEnabled               | Richiede che l'host disponga di un dispositivo IOMMU in uso ed evitare gli attacchi di accesso diretto a memoria. Disattivando questo criterio e host senza un IOMMU abilitata possono esporre i segreti della macchina virtuale tenant in modo da indirizzare gli attacchi di memoria.
Hgs_NoHibernation              | Richiede sospensione deve essere disabilitato nell'host Hyper-V. Disattivando questo criterio può consentire gli host risparmiare memoria della macchina virtuale schermata in un file di ibernazione non crittografati.
Hgs_NoDumps                    | Richiede che i dump di memoria deve essere disabilitato nell'host Hyper-V. Se si disabilita questo criterio, è consigliabile configurare crittografia del dump per evitare di memoria della macchina virtuale schermata vengano salvati in file di dump di arresto anomalo del sistema non crittografati.
Hgs_DumpEncryption             | Richiede che i dump di memoria, se abilitata nell'host Hyper-V deve essere crittografato con una chiave di crittografia considerato attendibile dal servizio HGS. Questo criterio non è applicabile se i dump non sono abilitati nell'host. Se questo criterio e *Hgs\_NoDumps* sono entrambe disabilitate, schermata memoria della macchina virtuale è stato possibile salvare in un file di dump non crittografati.
Hgs_DumpEncryptionKey          | Negativo criteri per assicurarsi che gli host configurati per consentire memoria dump Usa una chiave di crittografia di file di dump definita dall'amministratore nota al servizio HGS. Non si applica quando questo criterio *Hgs\_DumpEncryption* è disabilitato.

### <a name="authorizing-new-guarded-hosts"></a>Autorizzazione dei nuovi host sorvegliati
Per autorizzare un nuovo host per diventare un host sorvegliato (ad esempio, di attestare correttamente), HGS deve considerare attendibile l'host e (se configurato per usare l'attestazione TPM) il software in esecuzione su di esso.
I passaggi per autorizzare un nuovo host variano a seconda della modalità di attestazione per cui è attualmente configurato HGS.
Per controllare la modalità di attestazione per l'infrastruttura sorvegliata, eseguire `Get-HgsServer` in qualsiasi nodo del servizio HGS.

#### <a name="software-configuration"></a>Configurazione del software
Nel nuovo host Hyper-V, assicurarsi che Windows Server 2016 Datacenter edition è installato.
Windows Server 2016 Standard non è possibile eseguire le macchine virtuali schermate in un'infrastruttura sorvegliata.
L'host può essere installato esperienza Desktop o Server Core.

Nel server con esperienza desktop e Server Core, è necessario installare i ruoli del server Hyper-V e supporto Hyper-V per sorveglianza Host:

```powershell
Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
```

#### <a name="admin-trusted-attestation"></a>Attestazione amministratore
Per registrare un nuovo host nel servizio HGS quando si usa l'attestazione amministratore, è innanzitutto necessario aggiungere l'host a un gruppo di sicurezza nel dominio a cui viene aggiunto.
In genere, ogni dominio avrà un gruppo di sicurezza per gli host sorvegliati.
Se tale gruppo è già registrata con HGS, l'unica azione che occorre intraprendere è riavviare l'host per aggiornare le appartenenze a gruppi.

È possibile controllare quali gruppi di sicurezza sono considerati attendibili dal servizio HGS eseguendo il comando seguente:

```powershell
Get-HgsAttestationHostGroup
```

Per registrare un nuovo gruppo di sicurezza con HGS, prima di tutto acquisire l'ID di sicurezza (SID) del gruppo nel dominio dell'host e registrare il SID con HGS.

```powershell
Add-HgsAttestationHostGroup -Name "Contoso Guarded Hosts" -Identifier "S-1-5-21-3623811015-3361044348-30300820-1013"
```

Le istruzioni su come impostare la relazione di trust tra il dominio host e il servizio HGS sono disponibili nella Guida alla distribuzione.

#### <a name="tpm-trusted-attestation"></a>Attestazione TPM
Quando HGS è configurato in modalità TPM, gli host devono superare tutti i criteri bloccati e i criteri "abilitato" preceduti "Hgs_", nonché almeno una linea di base di TPM, identificatore TPM e criteri di integrità del codice.
Ogni volta che si aggiunge un nuovo host, si dovrà registrare il nuovo identificatore TPM con HGS.
Fino a quando l'host è in esecuzione lo stesso software (e ha applicato criteri di integrità del codice stesso) e TPM della linea di base come un altro host nel proprio ambiente, non dovrai aggiungere nuovi criteri di integrazione continua o linee di base.

**Aggiunge l'identificatore TPM per un nuovo host** nel nuovo host, eseguire il comando seguente per acquisire l'identificatore TPM.
Assicurarsi di specificare un nome univoco per l'host che consentirà di cercarla nel servizio HGS.
Queste informazioni è necessario rimuovere l'host o impedire che le macchine virtuali schermate in esecuzione nel servizio HGS.

```powershell
(Get-PlatformIdentifier -Name "Host01").InnerXml | Out-File C:\temp\host01.xml -Encoding UTF8
```

Copiare questo file per il server HGS, quindi eseguire il comando seguente per registrare l'host con HGS.

```powershell
Add-HgsAttestationTpmHost -Name 'Host01' -Path C:\temp\host01.xml
```

**Aggiunta di una nuova linea di base TPM** se il nuovo host viene eseguito un nuovo hardware o la configurazione del firmware per il proprio ambiente, potrebbe essere necessario richiedere una nuova linea di base TPM.
A tale scopo, eseguire il comando seguente nell'host.

```powershell
Get-HgsAttestationBaselinePolicy -Path 'C:\temp\hardwareConfig01.tcglog'
```

> [!NOTE]
> Se si riceve un errore indicante che l'host non è riuscita la convalida e non verrà attestare correttamente, non ti preoccupare.
> Questo è un controllo dei prerequisiti per assicurarsi che l'host possa eseguire macchine virtuali schermate e probabilmente significa che non è stato ancora applicato un criterio di integrità del codice o altra impostazione obbligatoria.
> Leggere il messaggio di errore, apportare le modifiche suggerite da esso, quindi riprovare.
> In alternativa, è possibile ignorare la convalida al momento aggiungendo il `-SkipValidation` flag al comando.

Copiare la linea di base TPM per il server HGS, quindi registrarlo con il comando seguente.
Si consiglia di usare una convenzione di denominazione che consente di comprendere la configurazione hardware e firmware di questa classe dell'host Hyper-V.

```powershell
Add-HgsAttestationTpmPolicy -Name 'HardwareConfig01' -Path 'C:\temp\hardwareConfig01.tcglog'
```

**Aggiunta di un nuovo criterio di integrità codice** se sono state modificate di criteri di integrità del codice in esecuzione negli host Hyper-V, si dovrà registrare il nuovo criterio con HGS prima che questi host consente di attestare correttamente.
In un host di riferimento, che funge da un'immagine master per le macchine Hyper-V attendibili nell'ambiente in uso, acquisire un nuovo criterio di integrazione continua tramite la `New-CIPolicy` comando.
Si consiglia di usare la **FilePublisher** livello e **Hash** fallback per i criteri degli elementi di configurazione di host Hyper-V.
È innanzitutto necessario creare un criterio di integrazione in modalità di controllo per assicurarsi che tutto funzioni come previsto.
Dopo la convalida di un carico di lavoro di esempio nel sistema, è possibile applicare i criteri e copiare la versione imposta HGS.
Per un elenco completo di opzioni di configurazione dei criteri di integrità codice, consultare il [documentazione di Device Guard](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-deploy-code-integrity-policies).

```powershell
# Capture a new CI policy with the FilePublisher primary level and Hash fallback and enable user mode code integrity protections
New-CIPolicy -FilePath 'C:\temp\ws2016-hardware01-ci.xml' -Level FilePublisher -Fallback Hash -UserPEs

# Apply the CI policy to the system
ConvertFrom-CIPolicy -XmlFilePath 'C:\temp\ws2016-hardware01-ci.xml' -BinaryFilePath 'C:\temp\ws2016-hardware01-ci.p7b'
Copy-Item 'C:\temp\ws2016-hardware01-ci.p7b' 'C:\Windows\System32\CodeIntegrity\SIPolicy.p7b'
Restart-Computer

# Check the event log for any untrusted binaries and update the policy if necessary
# Consult the Device Guard documentation for more details

# Change the policy to be in enforced mode
Set-RuleOption -FilePath 'C:\temp\ws2016-hardare01-ci.xml' -Option 3 -Delete

# Apply the enforced CI policy on the system
ConvertFrom-CIPolicy -XmlFilePath 'C:\temp\ws2016-hardware01-ci.xml' -BinaryFilePath 'C:\temp\ws2016-hardware01-ci.p7b'
Copy-Item 'C:\temp\ws2016-hardware01-ci.p7b' 'C:\Windows\System32\CodeIntegrity\SIPolicy.p7b'
Restart-Computer
```

Dopo aver creato il criterio creato, testati e applicati, copiare il file binario (con estensione p7b) nel server HGS e registrare i criteri.

```powershell
Add-HgsAttestationCiPolicy -Name 'WS2016-Hardware01' -Path 'C:\temp\ws2016-hardware01-ci.p7b'
```

**Aggiunta di una chiave di crittografia del dump della memoria**

Quando la *Hgs\_NoDumps* criterio è disabilitato e *Hgs\_DumpEncryption* criteri sono abilitato, gli host sorvegliati consentiti per i dump della memoria (inclusi i dump di arresto anomalo del sistema) per essere abilitato, purché i dump vengono crittografati. Gli host sorvegliati passerà l'attestazione solo se si hanno i dump di memoria disabilitati o sono crittografandole con una chiave nota al servizio HGS. Per impostazione predefinita, le chiavi di crittografia non dump sono configurate in HGS.

Per aggiungere una chiave di crittografia del dump HGS, utilizzare il `Add-HgsAttestationDumpPolicy` cmdlet per fornire HGS con l'hash della chiave di crittografia del dump.
Se si acquisisce una linea di base TPM in un host Hyper-V configurato con la crittografia del dump, il valore hash è incluso nel tcglog e può essere fornito al `Add-HgsAttestationDumpPolicy` cmdlet.

```powershell
Add-HgsAttestationDumpPolicy -Name 'DumpEncryptionKey01' -Path 'C:\temp\TpmBaselineWithDumpEncryptionKey.tcglog'
```

In alternativa, è possibile fornire direttamente la rappresentazione di stringa dell'algoritmo hash al cmdlet.

```powershell
Add-HgsAttestationDumpPolicy -Name 'DumpEncryptionKey02' -PublicKeyHash '<paste your hash here>'
```

Assicurarsi di aggiungere ogni chiave di crittografia del dump univoco al servizio HGS, se si sceglie di usare tasti diversi tra l'infrastruttura sorvegliata.
Gli host che esegue la crittografia i dump di memoria con una chiave non nota al servizio HGS non passerà l'attestazione.

Consultare la documentazione di Hyper-V per altre informazioni sulle [configurazione dump crittografia negli host](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption).

#### <a name="check-if-the-system-passed-attestation"></a>Controllare se il sistema passata l'attestazione
Dopo aver registrato le informazioni necessarie con HGS, è necessario controllare se l'host superi l'attestazione.
Nell'host Hyper-V appena aggiunto, eseguire `Set-HgsClientConfiguration` e fornire gli URL corretti per il cluster del servizio HGS.
Questi URL possono essere ottenuti eseguendo `Get-HgsServer` in qualsiasi nodo del servizio HGS.

```powershell
Set-HgsClientConfiguration -KeyProtectionServerUrl 'http://hgs.bastion.local/KeyProtection' -AttestationServerUrl 'http://hgs.bastion.local/Attestation'
```

Se lo stato risulta non indica "IsHostGuarded: True"è necessario risolvere i problemi di configurazione.
Nell'host che non hanno superato l'attestazione, eseguire il comando seguente per ottenere un report dettagliato sui problemi che consentono di risolvere l'attestazione non riuscita.

```powershell
Get-HgsTrace -RunDiagnostics -Detailed
```

> [!IMPORTANT]
> Se si usa Windows Server 2019 o Windows 10, versione 1809 e usano i criteri di integrità di codice, `Get-HgsTrace` può restituire un errore per il **codice l'integrità dei criteri Active** diagnostica.
> Quando è il solo errori diagnostica, è possibile ignorare questo risultato.

### <a name="review-attestation-policies"></a>Esaminare i criteri di attestazione
Per esaminare lo stato corrente dei criteri configurati nel servizio HGS, eseguire i comandi seguenti in qualsiasi nodo del servizio HGS:

```powershell
# List all trusted security groups for admin-trusted attestation
Get-HgsAttestationHostGroup

# List all policies configured for TPM-trusted attestation
Get-HgsAttestationPolicy
```

Se si trova un criterio abilitato che non soddisfa più i requisiti di sicurezza (ad esempio, un vecchio codice criteri di integrità che a questo punto viene considerato non sicuro), è possibile disabilitarlo sostituendo il nome del criterio nel comando seguente:

```powershell
Disable-HgsAttestationPolicy -Name 'PolicyName'
```

Analogamente, è possibile usare `Enable-HgsAttestationPolicy` per riabilitare un criterio.

Se non è più necessario un criterio e si desidera rimuoverlo da tutti i nodi del servizio HGS, eseguire `Remove-HgsAttestationPolicy -Name 'PolicyName'` eliminare definitivamente i criteri.

## <a name="changing-attestation-modes"></a>Modifica delle modalità di attestazione
Se è stata avviata l'infrastruttura sorvegliata con attestazione amministratore, è probabile che si vuole aggiornare per la modalità di attestazione TPM più avanzato molto, non appena si dispone di sufficienti host 2.0 compatibile TPM nell'ambiente in uso.
Quando si è pronti per passare, è possibile precaricare tutti gli elementi di attestazione (criteri di integrazione continua, le linee di base TPM e TPM identificatori) nel servizio HGS pur continuando a eseguire HGS con attestazione amministratore.
A tale scopo, è sufficiente seguire le istruzioni disponibili nel [autorizzare un nuovo host sorvegliato](#authorizing-new-guarded-hosts) sezione.

Dopo aver aggiunto tutti i criteri di HGS, il passaggio successivo è eseguire un tentativo di attestazione sintetiche negli host per verificare se l'attestazione passa in modalità TPM.
Ma non lo stato operativo corrente del servizio HGS.
In un computer dotato di accesso a tutti gli host nell'ambiente e almeno un nodo HGS, è necessario eseguire i comandi seguenti.
Se il firewall o altri criteri di sicurezza evitare questo problema, è possibile ignorare questo passaggio.
Quando possibile, è consigliabile eseguire l'attestazione sintetico per offrire una buona indicazione del fatto che "capovolgimento" per la modalità TPM determinerà tempi di inattività per le macchine virtuali. 

```powershell
# Get information for each host in your environment
$hostNames = 'host01.contoso.com', 'host02.contoso.com', 'host03.contoso.com'
$credential = Get-Credential -Message 'Enter a credential with admin privileges on each host'
$targets = @()
$hostNames | ForEach-Object { $targets += New-HgsTraceTarget -Credential $credential -Role GuardedHost -HostName $_ }

$hgsCredential = Get-Credential -Message 'Enter an admin credential for HGS'
$targets += New-HgsTraceTarget -Credential $hgsCredential -Role HostGuardianService -HostName 'HGS01.bastion.local'

# Initiate the synthetic attestation attempt
Get-HgsTrace -RunDiagnostics -Target $targets -Diagnostic GuardedFabricTpmMode 
```

Dopo la diagnostica completa, esaminare le informazioni di output per determinare se tutti gli host sarebbe riuscito l'attestazione in modalità TPM.
Eseguire di nuovo i dati di diagnostica fino a quando non è ottenere un "passare" da ogni host, quindi procedere HGS alla modalità per il TPM.

**Passaggio alla modalità TPM** richiede qualche secondo per il completamento.
Eseguire il comando seguente su qualsiasi nodo HGS per aggiornare la modalità di attestazione.

```powershell
Set-HgsServer -TrustTpm
```

Se si verificano problemi ed è necessario tornare alla modalità di Active Directory, è possibile farlo eseguendo `Set-HgsServer -TrustActiveDirectory`.

Dopo aver verificato che tutto funzioni come previsto, è necessario rimuovere tutti i gruppi di host di Active Directory trusted dal servizio HGS e rimuovere la relazione di trust tra i domini HGS e fabric.
Se si lascia il trust di Active Directory posto, si rischia di qualcuno commutazione HGS alla modalità di Active Directory, permettendo potenzialmente non attendibili per eseguire codice non controllato nel host sorvegliati e riabilitare la relazione di trust.

## <a name="key-management"></a>Gestione delle chiavi
La soluzione di infrastruttura sorvegliata Usa diverse coppie di chiavi pubblica/privata per convalidare l'integrità dei vari componenti della soluzione e crittografare i segreti di tenant.
Il servizio sorveglianza Host è configurato con almeno due certificati (con chiavi pubbliche e private), che vengono usati per firmare e crittografare le chiavi usate per avviare le macchine virtuali schermate.
Tali chiavi devono essere gestite con attenzione.
Se la chiave privata viene acquisita da un antagonista, saranno in grado di tutte le macchine virtuali in esecuzione nell'infrastruttura di unshield o configurare un cluster del servizio HGS impostore che usa i criteri di attestazione più vulnerabili per ignorare le protezioni che è mettere in atto.
Si deve perdere le chiavi private durante un'emergenza e non li si trova in un backup, è necessario configurare una nuova coppia di chiavi e dispone di ogni macchina virtuale nuovamente con chiave fornita per autorizzare i nuovi certificati.

In questa sezione vengono trattati argomenti di gestione delle chiavi generali che consentono di configurare le chiavi in modo da risultare efficace e protetta.

### <a name="adding-new-keys"></a>Aggiunta di nuove chiavi
Anche se HGS deve essere inizializzata con un set di chiavi, è possibile aggiungere più di una crittografia e firma chiave HGS.
I due motivi più comuni per cui è necessario aggiungere nuove chiavi HGS sono:
1. Per supportare gli scenari "bring your own key" in cui i tenant copiare le chiavi private per il modulo di protezione hardware e si limitano ad autorizzare le relative chiavi per le proprie macchine virtuali schermate di avvio.
2. Per sostituire le chiavi esistenti per HGS innanzitutto aggiungendo le nuove chiavi e mantenere entrambi i set di chiavi fino a ogni macchina virtuale di configurazione è stata aggiornata per usare le nuove chiavi.

Il processo per aggiungere le nuove chiavi è diversa a seconda del tipo di certificato in che uso.

**Opzione 1: Aggiunta di un certificato archiviato in un modulo di protezione hardware**

L'approccio consigliato per la protezione delle chiavi di HGS è usare i certificati creati in un modulo di protezione hardware (HSM).
Moduli di protezione hardware verificare che l'uso delle chiavi è associato all'accesso fisico a un dispositivo sensibili alla sicurezza nel tuo Data Center.
Ogni modulo di protezione hardware è diverso e dispone di un processo univoco per creare i certificati e questi vengono registrati con HGS.
I passaggi seguenti sono utili per fornire indicazioni generali per l'uso di moduli di protezione hardware supportato certificati.
Per le funzionalità e i passaggi esatti, consultare la documentazione del fornitore HSM.

1. Installare il software di modulo di protezione hardware in ogni nodo HGS nel cluster. A seconda che si disponga di una rete o un dispositivo HSM locale, si potrebbe essere necessario configurare il modulo di protezione per concedere l'accesso al computer nel proprio archivio chiave.
2. Creare 2 certificati con il modulo di protezione hardware **chiavi RSA a 2048 bit** per la crittografia e firma
    1. Creare un certificato di crittografia con la **crittografia dati** chiave proprietà usage nel modulo di protezione hardware
    2. Creare un certificato di firma con il **firma digitale** chiave proprietà usage nel modulo di protezione hardware
3. Installare i certificati nell'archivio certificati locale di ogni nodo HGS base alle indicazioni del fornitore HSM.
4. Se il modulo di protezione hardware Usa autorizzazioni granulari per concedere l'autorizzazione per usare la chiave privata di applicazioni o utenti specifici, è necessario concedere l'accesso di account del servizio gestito del gruppo HGS al certificato. È possibile trovare il nome dell'account gMSA HGS eseguendo `(Get-IISAppPool -Name KeyProtection).ProcessModel.UserName`
5. Aggiungere i certificati di firma e crittografia a HGS sostituendo le identificazioni personali con quelli dei certificati della nei comandi seguenti:

    ```powershell
    Add-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint "AABBCCDDEEFF00112233445566778899"
    Add-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint "99887766554433221100FFEEDDCCBBAA"
    ```

**Opzione 2: Aggiunta di certificati software non esportabile**

Se si dispone di un certificato basato su software rilasciato dalla propria società o un'autorità di certificazione pubblica che ha una chiave privata non esportabile, sarà necessario aggiungere il certificato per servizio HGS usando l'identificazione personale.
1. Installare il certificato nel computer in base alle istruzioni dell'autorità di certificazione.
2. Concedere il servizio HGS gestito del gruppo servizio account accesso in lettura alla chiave privata del certificato. È possibile trovare il nome dell'account gMSA HGS eseguendo `(Get-IISAppPool -Name KeyProtection).ProcessModel.UserName`
3. Registrare il certificato con HGS usando il comando seguente e sostituendo i valori identificazione personale del certificato (cambiare *crittografia* al *firma* per i certificati di firma):

    ```powershell
    Add-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint "AABBCCDDEEFF00112233445566778899"
    ```

> [!IMPORTANT]
> È necessario installare la chiave privata e concedere l'accesso in lettura all'account gMSA in ogni nodo del servizio HGS manualmente.
> HGS non è possibile replicare automaticamente le chiavi private per *qualsiasi* certificato registrato dalla relativa identificazione personale.

**Opzione 3: Aggiunta di certificati archiviati nel file PFX**

Se si dispone di un certificato basato su software con una chiave privata esportabile che può essere archiviato nel formato di file PFX e protetto da password, HGS può gestire automaticamente i certificati per l'utente.
I certificati aggiunti con i file PFX vengono replicati automaticamente in ogni nodo del cluster di HGS e HGS protegge l'accesso alle chiavi private.
Per aggiungere un nuovo certificato usando un file PFX, eseguire i comandi seguenti in qualsiasi nodo del servizio HGS (cambiare *crittografia* al *firma* per i certificati di firma):

```powershell
$certPassword = Read-Host -AsSecureString -Prompt "Provide the PFX file password"
Add-HgsKeyProtectionCertificate -CertificateType Encryption -CertificatePath "C:\temp\encryptionCert.pfx" -CertificatePassword $certPassword
```

**Identificazione e la modifica i certificati primari** mentre HGS può supportare più certificati di firma e crittografia, Usa una coppia come i propri certificati "primary".
Questi sono i certificati che verranno usati se un utente scarica i metadati di sorveglianza per tale cluster HGS.
Per verificare quali certificati sono attualmente contrassegnati come i certificati primari, eseguire il comando seguente:

```powershell
Get-HgsKeyProtectionCertificate -IsPrimary $true
```

Per impostare una nuova crittografia primario o un certificato di firma, trovare l'identificazione digitale del certificato desiderato e contrassegnarlo come primaria usando i comandi seguenti:

```powershell
Get-HgsKeyProtectionCertificate
Set-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint "AABBCCDDEEFF00112233445566778899" -IsPrimary
Set-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint "99887766554433221100FFEEDDCCBBAA" -IsPrimary
```

### <a name="renewing-or-replacing-keys"></a>Rinnovo o la sostituzione delle chiavi
Quando si creano i certificati usati dal servizio HGS, i certificati verranno assegnati una data di scadenza in base al criterio dell'autorità di certificazione e le informazioni di richiesta.
In genere, in scenari in cui la validità del certificato è importante come proteggere le comunicazioni HTTP, rinnovare i certificati devono essere prima della scadenza per evitare un'interruzione del servizio o un messaggio di errore preoccupante.
HGS non usa i certificati in tal senso.
HGS semplicemente Usa i certificati come un modo pratico per creare e archiviare una coppia di chiavi asimmetrica.
Una crittografia scaduta o certificato di firma in HGS non indica un punto debole o perdita di protezione per le macchine virtuali schermate.
Inoltre, i controlli di revoca dei certificati non vengono eseguiti dal servizio HGS.
Se viene revocato un certificato di servizio HGS o dell'autorità di certificazione emittente, non influirà sull'uso HGS del certificato.

L'unica volta che è necessario preoccuparsi di un certificato di servizio HGS è se hai motivo di ritenere che la relativa chiave privata è stata rubata.
In tal caso, l'integrità delle macchine virtuali schermate è a rischio perché il possesso della metà privata della crittografia del servizio HGS e coppia di chiavi di firma è sufficiente rimuovere le protezioni di schermatura in una macchina virtuale o realizza un server HGS fittizio che usa criteri di attestazione più deboli.

Se si ritrova in questo caso, o sono richiesti da standard di conformità per aggiornare regolarmente le chiavi di certificato, i passaggi seguenti illustrano il processo per modificare le chiavi in un server HGS.
Si noti che il materiale sussidiario seguente rappresenti un'impresa molto ardua che genererà un'interruzione del servizio a ogni macchina virtuale gestite da parte del cluster del servizio HGS.
Una pianificazione appropriata per la modifica delle chiavi di HGS è necessario ridurre al minimo l'interruzione del servizio e garantire la sicurezza delle macchine virtuali tenant.

In un nodo del servizio HGS, eseguire la procedura seguente per registrare una nuova coppia di crittografia e firma dei certificati.
Vedere la sezione sulla [aggiunta di nuove chiavi](#adding-new-keys) per altre informazioni su vari modi per aggiungere nuove chiavi di HGS.
1. Creare una nuova coppia di crittografia e certificati di firma per il server HGS. In teoria, questi verrà creati in un modulo di protezione hardware.
2. Registrare la nuova crittografia e firma dei certificati con **Add-HgsKeyProtectionCertificate**

    ```powershell
    Add-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint>
    Add-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint>
    ```
3. Se si usa le identificazioni personali, è necessario passare a ogni nodo del cluster per installare la chiave privata e concedere l'accesso di gMSA HGS alla chiave.
4. Rendere i nuovi certificati, i certificati predefiniti in HGS

    ```powershell
    Set-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint> -IsPrimary
    Set-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint> -IsPrimary
    ```

A questo punto, i dati creati con metadati ottenuti dal nodo del servizio HGS di schermatura utilizzerà i nuovi certificati, ma le macchine virtuali esistenti continueranno a funzionare perché i certificati precedenti sono ancora presenti.
Per garantire che tutte le macchine virtuali esistente funzionerà con le nuove chiavi, è necessario aggiornare la protezione con chiave in ogni macchina virtuale.
Si tratta di un'azione che richiede il proprietario della macchina virtuale (persona o entità in possesso di sorveglianza il "proprietario") per essere coinvolto.
Per ogni macchina virtuale schermata, seguire i passaggi seguenti:
5. Arrestare la macchina virtuale. La macchina virtuale non è possibile riattivare fino a quando i passaggi rimanenti sono stati completati, altrimenti sarà necessario ricominciare dall'inizio del processo.
6. Salvare la protezione con chiave corrente in un file: `Get-VMKeyProtector -VMName 'VM001' | Out-File '.\VM001.kp'`
7. Trasferire il KP al proprietario della macchina virtuale
8. Hai effettuato il download di proprietario, le informazioni di sorveglianza aggiornato dal servizio HGS e importarlo nel sistema locale
9. Leggere il KP correnti in memoria, concedere al nuovo accesso di sorveglianza per il KP e salvarla in un nuovo file eseguendo i comandi seguenti:

    ```powershell
    $kpraw = Get-Content -Path .\VM001.kp
    $kp = ConvertTo-HgsKeyProtector -Bytes $kpraw
    $newGuardian = Get-HgsGuardian -Name 'UpdatedHgsGuardian'
    $updatedKP = Grant-HgsKeyProtectorAccess -KeyProtector $kp -Guardian $newGuardian
    $updatedKP.RawData | Out-File .\updatedVM001.kp
    ```
10. Copiare il KP aggiornato nuovamente l'infrastruttura di hosting
11. Applicare il KP alla macchina virtuale originale:

   ```powershell
   $updatedKP = Get-Content -Path .\updatedVM001.kp
   Set-VMKeyProtector -VMName VM001 -KeyProtector $updatedKP
   ```
12. Infine, avviare la macchina virtuale e verificare che venga eseguito correttamente.

> [!NOTE]
> Se il proprietario della macchina virtuale viene impostata una protezione con chiave non corretta nella macchina virtuale e non autorizza l'infrastruttura per l'esecuzione della macchina virtuale, non sarà possibile avviare la macchina virtuale schermata.
> Per ripristinare l'ultimo note buona protezione con chiave, eseguire `Set-VMKeyProtector -RestoreLastKnownGoodKeyProtector`

Dopo che tutte le macchine virtuali sono state aggiornate per autorizzare le nuove chiavi di sorveglianza, è possibile disabilitare e rimuovere le chiavi precedenti.

13. Ottenere le identificazioni personali dei certificati precedenti da `Get-HgsKeyProtectionCertificate -IsPrimary $false`

14. Disabilitare ogni certificato eseguendo i comandi seguenti:  

   ```powershell
   Set-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint> -IsEnabled $false
   Set-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint> -IsEnabled $false
   ```

15. Dopo aver verificato le macchine virtuali siano ancora in grado di iniziare con i certificati disabilitati, rimuovere i certificati da HGS eseguendo i comandi seguenti:

   ```powershell
   Remove-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint>`
   Remove-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint>`
   ```

> [!IMPORTANT]
> I backup delle VM conterrà le informazioni di protezione con chiave meno recenti che consentono i vecchi certificati da utilizzare per l'avvio della macchina virtuale.
> Se si è consapevoli che la chiave privata è stata compromessa, si deve presupporre che il backup di macchine Virtuali possono essere compromessi, e intraprendere l'azione appropriata.
> Eliminazione definitiva di configurazione della macchina virtuale dalle copie di backup (con estensione vmcx) rimuoverà le protezioni con chiave, aumentando tuttavia la necessità di usare la password di ripristino BitLocker per la macchina virtuale di avvio la volta successiva.

### <a name="key-replication-between-nodes"></a>Replica delle chiavi tra i nodi
Ogni nodo del cluster del servizio HGS deve essere configurato con la stessa crittografia, firma e (se configurato) i certificati SSL.
Ciò è necessario assicurarsi che gli host Hyper-V raggiungere tutti i nodi del cluster possono avere le richieste elaborate correttamente.

**Se è stato inizializzato server HGS con i certificati basati sul file PFX** quindi HGS replicherà automaticamente sia la chiave pubblica e privata di tali certificati in ogni nodo del cluster.
È sufficiente aggiungere le chiavi in un nodo.

**Se è stato inizializzato server HGS con riferimenti ai certificati** o identificazioni personali, quindi HGS saranno replicate solo le *pubblico* chiavi nel certificato di ogni nodo.
Inoltre, non può concedere stesso HGS accedere alla chiave privata in qualsiasi nodo in questo scenario.
Pertanto, è responsabilità dell'utente:
1. Installare la chiave privata in ogni nodo del servizio HGS
2. Concedere l'accesso di account (gMSA) HGS servizio gestito del gruppo per la chiave privata in ogni nodo che queste attività aggiungere oneri aggiuntivi operativa, tuttavia sono necessari per i certificati con chiavi private non esportabile e chiavi basate su moduli di protezione hardware.

**I certificati SSL** non vengono mai replicate in qualsiasi forma.
È responsabilità dell'utente per inizializzare ogni server HGS con lo stesso certificato SSL e aggiornare ogni server ogni volta che si desidera rinnovare o di sostituire il certificato SSL.
Quando si sostituisce il certificato SSL, si consiglia di operazione che viene eseguita utilizzando il [Set-HgsServer](https://technet.microsoft.com/library/mt652180.aspx) cmdlet.

## <a name="unconfiguring-hgs"></a>Annullamento della configurazione di HGS

Se è necessario rimuovere o riconfigurare in modo significativo in un server HGS, è possibile farlo usando il [Clear-HgsServer](https://technet.microsoft.com/library/mt652176.aspx) oppure [Disinstalla HgsServer](https://technet.microsoft.com/library/mt652182.aspx) cmdlet.

### <a name="clearing-the-hgs-configuration"></a>Cancellare la configurazione del servizio HGS

Per rimuovere un nodo dal cluster HGS, usare il [Clear-HgsServer](https://technet.microsoft.com/library/mt652176.aspx) cmdlet.
Questo cmdlet verrà apportare le modifiche seguenti nel server in cui viene eseguito:

- Annulla la registrazione di servizi di protezione di attestazione e chiave
- Rimuove l'endpoint di gestione "microsoft.windows.hgs" JEA
- Rimuove il computer locale dal cluster di failover HGS

Se il server è l'ultimo nodo HGS nel cluster, il cluster e la relativa risorsa nome rete distribuita corrispondente verrà anche eliminate.

```powershell
# Removes the local computer from the HGS cluster
Clear-HgsServer
```

Al termine dell'operazione di cancellazione, può essere reinizializzati con i server HGS [Initialize-HgsServer](https://technet.microsoft.com/library/mt652185.aspx).
Se è stata usata [Install-HgsServer](https://technet.microsoft.com/library/mt652169.aspx) per configurare un dominio di Active Directory Domain Services, tale dominio rimarranno configurato e operativo dopo l'operazione di cancellazione.

### <a name="uninstalling-hgs"></a>Disinstallazione di HGS

Se si vuole rimuovere un nodo dal cluster servizio HGS **e** abbassare di livello il Controller di dominio Active Directory in esecuzione su di esso, utilizzare il [Disinstalla HgsServer](https://technet.microsoft.com/library/mt652182.aspx) cmdlet.
Questo cmdlet verrà apportare le modifiche seguenti nel server in cui viene eseguito:

- Annulla la registrazione di servizi di protezione di attestazione e chiave
- Rimuove l'endpoint di gestione "microsoft.windows.hgs" JEA
- Rimuove il computer locale dal cluster di failover HGS
- Abbassa di livello il Controller di dominio Active Directory, se configurato

Se il server è l'ultimo nodo HGS in cluster, il dominio, il cluster di failover, e verranno eliminata anche risorse nome rete distribuita del cluster.

```powershell
# Removes the local computer from the HGS cluster and demotes the ADDC (restart required)
$newLocalAdminPassword = Read-Host -AsSecureString -Prompt "Enter a new password for the local administrator account"
Uninstall-HgsServer -LocalAdministratorPassword $newLocalAdminPassword -Restart
```

Al termine dell'operazione di disinstallazione e il computer è stato riavviato, è possibile reinstallare ADDC e con HGS [Install-HgsServer](https://technet.microsoft.com/library/mt652169.aspx) o aggiungere il computer a un dominio e inizializzare il server HGS in tale dominio con [ Initialize-HgsServer](https://technet.microsoft.com/library/mt652185.aspx).

Se non è più si intende utilizzare il computer come un nodo HGS, è possibile rimuovere il ruolo da Windows.

```powershell
Uninstall-WindowsFeature HostGuardianServiceRole
```
