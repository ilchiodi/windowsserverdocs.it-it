---
title: Gestione del servizio sorveglianza host
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: eecb002e-6ae5-4075-9a83-2bbcee2a891c
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.openlocfilehash: 41912c90beacbb0c0c285ea4da8305c1afdf2a51
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386539"
---
# <a name="managing-the-host-guardian-service"></a>Gestione del servizio sorveglianza host

> Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

Il servizio sorveglianza host (HGS) è il fulcro della soluzione di infrastruttura sorvegliata.
È responsabile della garanzia che gli host Hyper-V nell'infrastruttura siano noti per il provider di hosting o l'azienda e per l'esecuzione di software attendibile e per la gestione delle chiavi usate per avviare le macchine virtuali schermate.
Quando un tenant decide di considerarsi attendibile per ospitare le macchine virtuali schermate, il suo trust è la configurazione e la gestione del servizio sorveglianza host.
Pertanto, è molto importante seguire le procedure consigliate per la gestione del servizio sorveglianza host per garantire la sicurezza, la disponibilità e l'affidabilità dell'infrastruttura sorvegliata.
Le linee guida riportate nelle sezioni seguenti illustrano i problemi operativi più comuni che riguardano gli amministratori di HGS.

## <a name="limiting-admin-access-to-hgs"></a>Limitazione dell'accesso amministrativo a HGS
A causa della natura sensibile alla sicurezza di HGS, è importante assicurarsi che gli amministratori siano membri altamente attendibili della propria organizzazione e, idealmente, separatamente dagli amministratori delle risorse dell'infrastruttura.
Inoltre, è consigliabile gestire solo HGS da workstation sicure usando protocolli di comunicazione protetti, ad esempio WinRM su HTTPS.

### <a name="separation-of-duties"></a>Separazione dei compiti
Quando si configura HGS, è possibile scegliere di creare una foresta Active Directory isolata solo per HGS o di aggiungere HGS a un dominio trusted esistente.
Questa decisione, oltre ai ruoli assegnati agli amministratori nell'organizzazione, determina il confine di trust per HGS.
Chiunque abbia accesso a HGS, direttamente come amministratore o indirettamente come amministratore di qualcos'altro, ad esempio Active Directory, che può influenzare HGS, ha il controllo sull'infrastruttura sorvegliata.
Gli amministratori di HGS scelgono gli host Hyper-V autorizzati a eseguire macchine virtuali schermate e gestire i certificati necessari per avviare le VM schermate.
Un utente malintenzionato o un amministratore malintenzionato che ha accesso a HGS può usare questa potenza per autorizzare gli host compromessi a eseguire macchine virtuali schermate, avviare un attacco Denial of Service rimuovendo il materiale della chiave e molto altro ancora.

Per evitare questo rischio, *è consigliabile limitare* la sovrapposizione tra gli amministratori di HGS (incluso il dominio al quale è stato aggiunto HGS) e gli ambienti Hyper-V.
Garantendo che nessun amministratore possa accedere a entrambi i sistemi, un utente malintenzionato dovrà compromettere 2 account diversi da 2 individui per completare la sua missione per modificare i criteri di HGS.
Ciò significa anche che il dominio e gli amministratori dell'organizzazione per i due ambienti Active Directory non devono essere la stessa persona, né che HGS usi la stessa foresta Active Directory degli host Hyper-V.
Chiunque possa concedere l'accesso a un maggior numero di risorse costituisce un rischio per la sicurezza.

### <a name="using-just-enough-administration"></a>Utilizzo di just enough Administration
HGS viene fornita con i ruoli di amministrazione (Jea) [just enough](https://aka.ms/JEAdocs) per semplificare la gestione in modo più sicuro.
JEA consente di delegare le attività amministrative agli utenti non amministratori, vale a dire che le persone che gestiscono i criteri di HGS non devono essere effettivamente amministratori dell'intero computer o dominio.
JEA funziona limitando i comandi che un utente può eseguire in una sessione di PowerShell e usando un account locale temporaneo in background (univoco per ogni sessione utente) per eseguire i comandi che in genere richiedono l'elevazione.

HGS viene fornito con 2 ruoli JEA preconfigurati:
- **Amministratori di HGS** che consentono agli utenti di gestire tutti i criteri di HGS, inclusa l'autorizzazione di nuovi host a eseguire VM schermate.
- **HGS revisori** che consente solo agli utenti di controllare i criteri esistenti. Non possono apportare modifiche alla configurazione HGS.

Per usare JEA, è prima di tutto necessario creare un nuovo utente standard e impostarlo come membro del gruppo HGS Admins o HGS revisori.
Se è stato usato `Install-HgsServer` per configurare una nuova foresta per HGS, questi gruppi verranno denominati rispettivamente "*ServiceName*Administrators" e "*ServiceName*revisori", dove *ServiceName* è il nome di rete del cluster HGS.
Se HGS è stato aggiunto a un dominio esistente, è necessario fare riferimento ai nomi dei gruppi specificati in `Initialize-HgsServer`.

**Creare utenti standard per i ruoli di amministratore e revisore HGS**

```powershell
$hgsServiceName = (Get-ClusterResource HgsClusterResource | Get-ClusterParameter DnsName).Value
$adminGroup = $hgsServiceName + "Administrators"
$reviewerGroup = $hgsServiceName + "Reviewers"

New-ADUser -Name 'hgsadmin01' -AccountPassword (Read-Host -AsSecureString -Prompt 'HGS Admin Password') -ChangePasswordAtLogon $false -Enabled $true
Add-ADGroupMember -Identity $adminGroup -Members 'hgsadmin01'

New-ADUser -Name 'hgsreviewer01' -AccountPassword (Read-Host -AsSecureString -Prompt 'HGS Reviewer Password') -ChangePasswordAtLogon $false -Enabled $true
Add-ADGroupMember -Identity $reviewerGroup -Members 'hgsreviewer01'
```

**Criteri di controllo con il ruolo revisore**

In un computer remoto con connettività di rete a HGS, eseguire i comandi seguenti in PowerShell per immettere la sessione JEA con le credenziali del revisore.
È importante notare che, poiché l'account revisore è semplicemente un utente standard, non può essere usato per la comunicazione remota di Windows PowerShell normale, Desktop remoto l'accesso a HGS e così via.

```powershell
Enter-PSSession -ComputerName <hgsnode> -Credential '<hgsdomain>\hgsreviewer01' -ConfigurationName 'microsoft.windows.hgs'
```

È quindi possibile verificare quali comandi sono consentiti nella sessione con `Get-Command` ed eseguire tutti i comandi consentiti per controllare la configurazione.
Nell'esempio seguente vengono verificati i criteri abilitati in HGS.

```powershell
Get-Command

Get-HgsAttestationPolicy
```

Digitare il comando `Exit-PSSession` o il relativo alias, `exit`, al termine dell'utilizzo della sessione JEA. 

**Aggiungere un nuovo criterio a HGS usando il ruolo amministratore**

Per modificare effettivamente un criterio, è necessario connettersi all'endpoint JEA con un'identità appartenente al gruppo ' hgsAdministrators '.
Nell'esempio seguente viene illustrato come è possibile copiare un nuovo criterio di integrità del codice in HGS e registrarlo tramite JEA.
La sintassi può essere diversa da quella usata.
Questo consente di soddisfare alcune delle restrizioni in JEA, ad esempio l'accesso al file system completo.

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

## <a name="monitoring-hgs"></a>Monitoraggio di HGS
### <a name="event-sources-and-forwarding"></a>Origini eventi e inoltri
Gli eventi da HGS vengono visualizzati nel registro eventi di Windows in due origini:
- **HostGuardianService-attestazione**
- **HostGuardianService-protezione dati**

È possibile visualizzare questi eventi aprendo Visualizzatore eventi e passando a Microsoft-Windows-HostGuardianService-Attestation e Microsoft-Windows-HostGuardianService-reprotection.

In un ambiente di grandi dimensioni, spesso è preferibile eseguire l'invio di eventi a un agente di raccolta eventi di Windows centrale per semplificare l'analisi degli eventi.
Per ulteriori informazioni, consultare la [documentazione sull'invio di eventi di Windows](https://msdn.microsoft.com/library/windows/desktop/bb427443.aspx).

### <a name="using-system-center-operations-manager"></a>Utilizzo di System Center Operations Manager
È anche possibile usare System Center 2016-Operations Manager per monitorare HGS e gli host sorvegliati.
L'infrastruttura sorvegliata Management Pack ha monitoraggi eventi per verificare la presenza di errori di configurazione comuni che possono causare tempi di inattività del Data Center, inclusi gli host che non passano l'attestazione e i server HGS che segnalano errori.

Per iniziare, [installare e configurare SCOM 2016](https://technet.microsoft.com/system-center-docs/om/welcome-to-operations-manager) e [scaricare l'infrastruttura sorvegliata Management Pack](https://www.microsoft.com/download/details.aspx?id=52764).
Nella guida Management Pack inclusa viene illustrato come configurare l'Management Pack e comprendere l'ambito dei monitoraggi.

## <a name="backing-up-and-restoring-hgs"></a>Backup e ripristino di HGS
### <a name="disaster-recovery-planning"></a>Pianificazione del ripristino di emergenza
Quando si elaborano i piani di ripristino di emergenza, è importante considerare i requisiti univoci del servizio sorveglianza host nell'infrastruttura sorvegliata.
Se si perdono alcuni o tutti i nodi HGS, è possibile che si verifichino problemi di disponibilità immediata che impediscono agli utenti di avviare le VM schermate.
In uno scenario in cui si perde l'intero cluster HGS, sarà necessario avere a disposizione backup completi della configurazione di HGS per ripristinare il cluster HGS e riprendere le normali operazioni.
In questa sezione vengono illustrati i passaggi necessari per la preparazione di tale scenario.

Prima di tutto, è importante comprendere il modo in cui è importante eseguire il backup di HGS.
HGS conserva diverse informazioni che consentono di determinare quali host sono autorizzati a eseguire macchine virtuali schermate.
È possibile creare, ad esempio:
1. Active Directory identificatori di sicurezza per i gruppi contenenti host attendibili (quando si usa l'attestazione Active Directory);
2. Identificatori TPM univoci per ogni host nell'ambiente in uso;
3. Criteri TPM per ogni configurazione univoca dell'host; e
4. Criteri di integrità del codice che determinano quale software è consentita l'esecuzione negli host.

Questi elementi di attestazione richiedono il coordinamento con gli amministratori dell'infrastruttura di hosting da ottenere, rendendo potenzialmente difficoltoso il recupero di queste informazioni dopo un'emergenza.

Inoltre, HGS richiede l'accesso a due o più certificati usati per crittografare e firmare le informazioni necessarie per avviare una macchina virtuale schermata (la protezione con chiave).
Questi certificati sono noti (usati dai proprietari delle macchine virtuali schermate per autorizzare l'infrastruttura a eseguire le VM) e devono essere ripristinati dopo un'emergenza per un'esperienza di ripristino senza problemi.
Se non si esegue il ripristino di HGS con gli stessi certificati dopo un'emergenza, è necessario aggiornare ogni macchina virtuale per autorizzare le nuove chiavi a decrittografare le informazioni.
Per motivi di sicurezza, solo il proprietario della macchina virtuale può aggiornare la configurazione della macchina virtuale per autorizzare queste nuove chiavi. Ciò significa che il mancato ripristino delle chiavi dopo un'emergenza comporterà la necessità da parte di ogni proprietario della macchina virtuale di eseguire un'azione per riportare le macchine virtuali.

#### <a name="preparing-for-the-worst"></a>Preparazione alla peggiore
Per prepararsi a una perdita completa di HGS, è necessario eseguire due passaggi:
1. Eseguire il backup dei criteri di attestazione HGS
2. Eseguire il backup delle chiavi di HGS

Le linee guida su come eseguire entrambi questi passaggi sono disponibili nella sezione [backup di HGS](#backing-up-hgs) .

È inoltre consigliabile, ma non necessario, eseguire il backup dell'elenco di utenti autorizzati a gestire HGS nel dominio Active Directory o Active Directory stesso.

I backup devono essere eseguiti regolarmente per assicurarsi che le informazioni siano aggiornate e archiviate in modo sicuro per evitare manomissioni o furti.

Non è **consigliabile** eseguire il backup o tentare di ripristinare un'intera immagine del sistema di un nodo HGS.
In caso di perdita dell'intero cluster, la procedura consigliata consiste nel configurare un nuovo nodo HGS e ripristinare solo lo stato HGS, non l'intero sistema operativo server.

#### <a name="recovering-from-the-loss-of-one-node"></a>Ripristino dalla perdita di un nodo
In caso di perdita di uno o più nodi (ma non di tutti i nodi) nel cluster HGS, è possibile [aggiungere semplicemente nodi al cluster](guarded-fabric-configure-additional-hgs-nodes.md) seguendo le istruzioni riportate nella Guida alla distribuzione.
I criteri di attestazione verranno sincronizzati automaticamente, così come tutti i certificati forniti a HGS come file PFX con password associate.
Per i certificati aggiunti a HGS usando un'identificazione personale (certificati supportati da hardware e non esportabili, in genere), è necessario assicurarsi che ogni nuovo nodo possa accedere alla chiave privata di ogni certificato.

#### <a name="recovering-from-the-loss-of-the-entire-cluster"></a>Ripristino dalla perdita dell'intero cluster
Se l'intero cluster HGS diventa inattivo e non è possibile riportarlo online, sarà necessario ripristinare HGS da un backup.
Il ripristino di HGS da un backup prevede innanzitutto la configurazione di un nuovo cluster HGS in base alle [indicazioni disponibili nella Guida alla distribuzione](guarded-fabric-setting-up-the-host-guardian-service-hgs.md).
È consigliabile, ma non obbligatorio, usare lo stesso nome del cluster quando si configura l'ambiente HGS di ripristino per facilitare la risoluzione dei nomi dagli host.
L'uso dello stesso nome evita di dover riconfigurare gli host con nuovi URL di attestazione e di protezione delle chiavi.
Se gli oggetti sono stati ripristinati nel dominio Active Directory HGS, è consigliabile rimuovere gli oggetti che rappresentano il cluster HGS, i computer, l'account del servizio e i gruppi JEA prima di inizializzare il server HGS.

Dopo aver configurato il primo nodo HGS (ad esempio, è stato installato e inizializzato), si seguiranno le procedure descritte in [ripristino di HGS da un backup](#restoring-hgs-from-a-backup) per ripristinare i criteri di attestazione e le metà pubbliche dei certificati di protezione delle chiavi.
Sarà necessario ripristinare manualmente le chiavi private per i certificati in base alle linee guida del provider di certificati, ad esempio importare il certificato in Windows o configurare l'accesso ai certificati supportati da HSM.
Dopo aver configurato il primo nodo, è possibile continuare a [installare nodi aggiuntivi nel cluster](guarded-fabric-configure-additional-hgs-nodes.md) fino a raggiungere la capacità e la resilienza desiderate.

### <a name="backing-up-hgs"></a>Backup di HGS
L'amministratore di HGS deve essere responsabile del backup di HGS a intervalli regolari.
Un backup completo conterrà materiale della chiave sensibile che deve essere protetto in modo appropriato.
Se un'entità non attendibile Ottiene l'accesso a queste chiavi, può usare tale materiale per configurare un ambiente HGS dannoso allo scopo di compromettere le macchine virtuali schermate.

**Backup dei criteri di attestazione** Per eseguire il backup dei criteri di attestazione di HGS, eseguire il comando seguente in qualsiasi nodo del server HGS funzionante.
Verrà richiesto di specificare una password.
Questa password viene usata per crittografare tutti i certificati aggiunti a HGS usando un file PFX, anziché un'identificazione personale del certificato.

```powershell
Export-HgsServerState -Path C:\temp\HGSBackup.xml
```

> [!NOTE]
> Se si usa un'attestazione attendibile per l'amministratore, è necessario eseguire separatamente il backup dell'appartenenza ai gruppi di sicurezza usati da HGS per autorizzare gli host sorvegliati.
> HGS eseguirà il backup solo del SID dei gruppi di sicurezza, non dell'appartenenza al suo interno.
> Nel caso in cui questi gruppi vadano perduti durante un'emergenza, sarà necessario ricreare i gruppi e aggiungere di nuovo ogni host sorvegliato.

**Backup dei certificati**

Il comando `Export-HgsServerState` eseguirà il backup dei certificati basati su PFX aggiunti a HGS al momento dell'esecuzione del comando.
Se sono stati aggiunti certificati a HGS usando un'identificazione personale (tipica per i certificati non esportabili e supportati dall'hardware), sarà necessario eseguire manualmente il backup delle chiavi private per i certificati.
Per identificare i certificati registrati con HGS ed è necessario eseguirne manualmente il backup, eseguire il comando di PowerShell seguente in qualsiasi nodo del server HGS funzionante.

```powershell
Get-HgsKeyProtectionCertificate | Where-Object { $_.CertificateData.GetType().Name -eq 'CertificateReference' } | Format-Table Thumbprint, @{ Label = 'Subject'; Expression = { $_.CertificateData.Certificate.Subject } }
```

Per ogni certificato elencato, sarà necessario eseguire manualmente il backup della chiave privata.
Se si utilizza un certificato basato su software non esportabile, è necessario contattare l'autorità di certificazione per assicurarsi che disponga di un backup del certificato e/o che possa essere riprodotto su richiesta.
Per i certificati creati e archiviati nei moduli di protezione hardware, è necessario consultare la documentazione del dispositivo per informazioni aggiuntive sulla pianificazione del ripristino di emergenza.

È necessario archiviare i backup dei certificati insieme ai backup del criterio di attestazione in una posizione sicura in modo che sia possibile ripristinare entrambe le parti.

**Configurazione aggiuntiva per il backup**

Lo stato del server HGS di cui è stato eseguito il backup non includerà il nome del cluster HGS, le informazioni provenienti da Active Directory o i certificati SSL usati per proteggere le comunicazioni con le API HGS.
Queste impostazioni sono importanti per coerenza, ma non cruciali, per riportare online il cluster HGS dopo un'emergenza.

Per acquisire il nome del servizio HGS, eseguire `Get-HgsServer` e annotare il nome Flat negli URL di attestazione e di protezione delle chiavi.
Ad esempio, se l'URL di attestazione è "<http://hgs.contoso.com/Attestation>", "HGS" è il nome del servizio HGS.

Il dominio Active Directory usato da HGS deve essere gestito come qualsiasi altro dominio di Active Directory.
Quando si ripristina HGS dopo un'emergenza, non sarà necessario ricreare gli oggetti esatti presenti nel dominio corrente.
Tuttavia, il ripristino sarà più semplice se si esegue il backup Active Directory e si mantiene un elenco degli utenti JEA autorizzati a gestire il sistema, nonché l'appartenenza di qualsiasi gruppo di sicurezza usato dall'attestazione attendibile dell'amministratore per autorizzare gli host sorvegliati.

Per identificare l'identificazione personale dei certificati SSL configurati per HGS, eseguire il comando seguente in PowerShell.
È quindi possibile eseguire il backup di tali certificati SSL in base alle istruzioni del provider di certificati.

```powershell
Get-WebBinding -Protocol https | Select-Object certificateHash
```

### <a name="restoring-hgs-from-a-backup"></a>Ripristino di HGS da un backup
Nei passaggi seguenti viene descritto come ripristinare le impostazioni HGS da un backup.
La procedura è pertinente per entrambe le situazioni in cui si sta provando a annullare le modifiche apportate alle istanze di HGS già in esecuzione e quando si sta eseguendo un nuovo cluster HGS dopo una perdita completa di quello precedente.

#### <a name="set-up-a-replacement-hgs-cluster"></a>Configurare un cluster HGS sostitutivo
Prima di poter ripristinare HGS, è necessario avere un cluster HGS inizializzato in cui è possibile ripristinare la configurazione.
Se si importano semplicemente impostazioni che sono state eliminate accidentalmente in un cluster esistente (in esecuzione), è possibile ignorare questo passaggio.
Se si esegue il ripristino da una perdita completa di HGS, sarà necessario installare e inizializzare almeno un nodo HGS seguendo le istruzioni riportate [nella Guida alla distribuzione](guarded-fabric-setting-up-the-host-guardian-service-hgs.md).

In particolare, sarà necessario:
1. [Configurare il dominio HGS](guarded-fabric-choose-where-to-install-hgs.md) o aggiungere HGS a un dominio esistente
2. [Inizializzare il server HGS](guarded-fabric-initialize-hgs.md) usando le chiavi esistenti *o* un set di chiavi temporanee. È possibile [rimuovere le chiavi temporanee](#renewing-or-replacing-keys) dopo aver importato le chiavi effettive dai file di backup di HGS.
3. [Importare le impostazioni di HGS](#import-settings-from-a-backup) dal backup per ripristinare i gruppi host attendibili, i criteri di integrità del codice, le linee di base del TPM e gli IDENTIFICAtori TPM

> [!TIP]
> Il nuovo cluster HGS non deve usare gli stessi certificati, il nome del servizio o il dominio dell'istanza di HGS da cui è stato esportato il file di backup.

#### <a name="import-settings-from-a-backup"></a>Importa le impostazioni da un backup

Per ripristinare i criteri di attestazione, i certificati basati su PFX e le chiavi pubbliche dei certificati non PFX al nodo HGS da un file di backup, eseguire il comando seguente in un nodo del server HGS inizializzato.
Verrà richiesto di immettere la password specificata durante la creazione del backup.

```powershell
Import-HgsServerState -Path C:\Temp\HGSBackup.xml
```

Se si desidera importare solo i criteri di attestazione attendibile per l'amministratore o i criteri di attestazione TPM, è possibile specificare i flag `-ImportActiveDirectoryModeState` o `-ImportTpmModeState` per [Import-HgsServerState](https://technet.microsoft.com/library/mt652168.aspx).

Verificare che sia installato l'aggiornamento cumulativo più recente per Windows Server 2016 prima di eseguire `Import-HgsServerState`.
In caso contrario, potrebbe verificarsi un errore di importazione.

> [!NOTE]
> Se si ripristinano i criteri in un nodo HGS esistente in cui sono già installati uno o più di questi criteri, il comando importa visualizzerà un errore per ogni criterio duplicato.
> Si tratta di un comportamento previsto che può essere tranquillamente ignorato nella maggior parte dei casi.

#### <a name="reinstall-private-keys-for-certificates"></a>Reinstallare le chiavi private per i certificati
Se uno dei certificati utilizzati nella HGS da cui è stato creato il backup è stato aggiunto utilizzando le identificazioni personali, solo la chiave pubblica di tali certificati verrà inclusa nel file di backup.
Ciò significa che sarà necessario installare manualmente e/o concedere l'accesso alle chiavi private per ognuno di questi certificati prima che HGS possa soddisfare le richieste da host Hyper-V.
Le azioni necessarie per completare questo passaggio variano a seconda della modalità di emissione originale del certificato.
Per i certificati supportati da software emessi da un'autorità di certificazione, è necessario contattare la CA per ottenere la chiave privata e installarla in **ogni** nodo HGS in base alle istruzioni.
Analogamente, se i certificati sono basati su hardware, sarà necessario consultare la documentazione del fornitore del modulo di protezione hardware per installare i driver necessari in ogni nodo HGS per connettersi al modulo HSM e concedere a ogni computer l'accesso alla chiave privata.

Come promemoria, i certificati aggiunti a HGS usando le identificazioni personali richiedono la replica manuale delle chiavi private in ogni nodo.
Sarà necessario ripetere questo passaggio in ogni nodo aggiuntivo aggiunto al cluster HGS ripristinato.

#### <a name="review-imported-attestation-policies"></a>Esaminare i criteri di attestazione importati
Dopo aver importato le impostazioni da un backup, è consigliabile esaminare attentamente tutti i criteri importati usando `Get-HgsAttestationPolicy` per assicurarsi che solo gli host considerati attendibili per l'esecuzione di VM schermate possano essere attestati correttamente.
Se si individuano criteri che non corrispondono più alla situazione di sicurezza, è possibile [disabilitarli o rimuoverli](#review-attestation-policies).

#### <a name="run-diagnostics-to-check-system-state"></a>Eseguire la diagnostica per controllare lo stato del sistema
Al termine dell'installazione e del ripristino dello stato del nodo HGS, è necessario eseguire lo strumento di diagnostica HGS per verificare lo stato del sistema.
A tale scopo, eseguire il comando seguente nel nodo HGS in cui è stata ripristinata la configurazione:

```powershell
Get-HgsTrace -RunDiagnostics
```

Se il "risultato complessivo" non è "Pass", per completare la configurazione del sistema sono necessari passaggi aggiuntivi.
Per ulteriori informazioni, controllare i messaggi segnalati nei sottotest non riusciti.

## <a name="patching-hgs"></a>Applicazione di patch HGS
È importante fare in modo che i nodi del servizio sorveglianza host siano aggiornati installando l'aggiornamento cumulativo più recente quando viene visualizzato. Se si sta configurando un nuovo nodo HGS, si consiglia di installare gli aggiornamenti disponibili prima di installare il ruolo HGS o di configurarlo.
In questo modo le funzionalità nuove o modificate diverranno effettive immediatamente.

Quando si applica l'applicazione di patch all'infrastruttura sorvegliata, è consigliabile aggiornare prima *tutti gli* host Hyper-V **prima di aggiornare HGS**.
In questo modo si garantisce che tutte le modifiche apportate ai criteri di attestazione in HGS vengano apportate *dopo* l'aggiornamento degli host Hyper-V per fornire le informazioni necessarie.
Se un aggiornamento modifica il comportamento dei criteri, non verrà abilitato automaticamente per evitare di compromettere l'infrastruttura.
Tali aggiornamenti richiedono che vengano seguite le istruzioni riportate nella sezione seguente per attivare i criteri di attestazione nuovi o modificati.
Si consiglia di leggere le note sulla versione di Windows Server e gli eventuali aggiornamenti cumulativi installati per verificare se sono necessari gli aggiornamenti dei criteri.

### <a name="updates-requiring-policy-activation"></a>Aggiornamenti che richiedono l'attivazione dei criteri
Se un aggiornamento per HGS introduce o modifica significativamente il comportamento di un criterio di attestazione, è necessario un passaggio aggiuntivo per attivare i criteri modificati.
Le modifiche ai criteri vengono eseguite solo dopo l'esportazione e l'importazione dello stato HGS.
È necessario attivare i criteri nuovi o modificati solo dopo aver applicato l'aggiornamento cumulativo a tutti gli host e a tutti i nodi HGS nell'ambiente in uso.
Dopo aver aggiornato ogni computer, eseguire i comandi seguenti in qualsiasi nodo HGS per attivare il processo di aggiornamento:

```powershell
$password = Read-Host -AsSecureString -Prompt "Enter a temporary password"
Export-HgsServerState -Path .\temporaryExport.xml -Password $password
Import-HgsServerState -Path .\temporaryExport.xml -Password $password
```

Se è stato introdotto un nuovo criterio, questo verrà disabilitato per impostazione predefinita.
Per abilitare il nuovo criterio, individuarlo nell'elenco dei criteri Microsoft (con prefisso "HGS_") e quindi abilitarlo usando i comandi seguenti:

```powershell
Get-HgsAttestationPolicy

Enable-HgsAttestationPolicy -Name <Hgs_NewPolicyName>
```

## <a name="managing-attestation-policies"></a>Gestione dei criteri di attestazione
HGS gestisce diversi criteri di attestazione che definiscono il set minimo di requisiti che un host deve soddisfare per essere ritenuti "integri" ed è autorizzato a eseguire macchine virtuali schermate.
Alcuni di questi criteri sono definiti da Microsoft, altri vengono aggiunti dall'utente per definire i criteri di integrità del codice consentiti, le linee di base del TPM e gli host nell'ambiente in uso.
La manutenzione regolare di questi criteri è necessaria per garantire che gli host possano continuare l'attestazione correttamente quando vengono aggiornati e sostituiti e per assicurarsi che gli host o le configurazioni non attendibili siano bloccati dall'attestazione.

Per l'attestazione attendibile per l'amministratore, esiste un solo criterio che determina se un host è integro: l'appartenenza a un gruppo di sicurezza noto e attendibile.
L'attestazione TPM è più complessa e prevede diversi criteri per misurare il codice e la configurazione di un sistema prima di determinare se è integro.

Una singola HGS può essere configurata contemporaneamente con criteri Active Directory e TPM, ma il servizio controllerà solo i criteri per la modalità corrente per cui è configurato quando un host prova a attestare.
Per controllare la modalità del server HGS, eseguire `Get-HgsServer`.

### <a name="default-policies"></a>Criteri predefiniti
Per l'attestazione TPM attendibile, in HGS sono configurati diversi criteri predefiniti.
Alcuni di questi criteri sono "bloccati", vale a dire che non possono essere disabilitati per motivi di sicurezza.
Nella tabella seguente viene illustrato lo scopo di ogni criterio predefinito.

Nome criterio                    | Scopo
-------------------------------|-----------------------------------------------------
Hgs_SecureBootEnabled          | Richiede l'abilitazione dell'avvio protetto per gli host. Questa operazione è necessaria per misurare i binari di avvio e altre impostazioni con blocco UEFI.
Hgs_UefiDebugDisabled          | Verifica che per gli host non sia abilitato un debugger del kernel. I debugger in modalità utente sono bloccati con criteri di integrità del codice.
Hgs_SecureBootSettings         | Criteri negativi per assicurarsi che gli host corrispondano almeno a una baseline TPM (definita dall'amministratore).
Hgs_CiPolicy                   | Criteri negativi per assicurarsi che gli host utilizzino uno dei criteri CI definiti dall'amministratore.
Hgs_HypervisorEnforcedCiPolicy | Richiede che il criterio di integrità del codice venga applicato dall'hypervisor. La disabilitazione di questo criterio indebolisce le protezioni dagli attacchi ai criteri di integrità del codice in modalità kernel.
Hgs_FullBoot                   | Garantisce che l'host non sia stato ripreso dalla modalità di sospensione o di ibernazione. Per passare questo criterio, gli host devono essere riavviati o arrestati correttamente.
Hgs_VsmIdkPresent              | Richiede che la sicurezza basata sulla virtualizzazione sia in esecuzione nell'host. Il IDK rappresenta la chiave necessaria per crittografare le informazioni restituite allo spazio di memoria protetto dell'host.
Hgs_PageFileEncryptionEnabled  | Richiede che paging sia crittografato nell'host. La disabilitazione di questo criterio può comportare l'esposizione delle informazioni se un file di paging non crittografato viene controllato per i segreti dei tenant.
Hgs_BitLockerEnabled           | Richiede l'abilitazione di BitLocker nell'host Hyper-V. Questo criterio è disabilitato per impostazione predefinita per motivi di prestazioni e non è consigliabile abilitarlo. Questo criterio non ha alcun impatto sulla crittografia delle macchine virtuali schermate.
Hgs_IommuEnabled               | Richiede che l'host disponga di un dispositivo IOMMU in uso per impedire attacchi di accesso diretto alla memoria. La disabilitazione di questo criterio e l'uso di host senza IOMMU abilitato possono esporre i segreti delle VM tenant per indirizzare gli attacchi alla memoria.
Hgs_NoHibernation              | Richiede la disabilitazione della modalità di ibernazione nell'host Hyper-V. La disabilitazione di questo criterio potrebbe consentire agli host di salvare la memoria della macchina virtuale schermata in un file di ibernazione non crittografato.
Hgs_NoDumps                    | Richiede che i dump della memoria siano disabilitati nell'host Hyper-V. Se si disabilita questo criterio, è consigliabile configurare la crittografia del dump per impedire che la memoria della macchina virtuale schermata venga salvata nei file di dump di arresto anomalo del sistema non crittografati.
Hgs_DumpEncryption             | Richiede i dump della memoria, se abilitati nell'host Hyper-V, da crittografare con una chiave di crittografia considerata attendibile da HGS. Questo criterio non si applica se i dump non sono abilitati nell'host. Se questi criteri e *HGS @ no__t-1NoDumps* sono entrambi disabilitati, la memoria della macchina virtuale schermata potrebbe essere salvata in un file di dump non crittografato.
Hgs_DumpEncryptionKey          | Criteri negativi per assicurarsi che gli host configurati per consentire i dump della memoria utilizzino una chiave di crittografia del file di dump definita dall'amministratore nota a HGS. Questo criterio non si applica quando *HGS @ no__t-1DumpEncryption* è disabilitato.

### <a name="authorizing-new-guarded-hosts"></a>Autorizzazione di nuovi host sorvegliati
Per autorizzare un nuovo host a diventare un host sorvegliato, ad esempio l'attestazione, HGS deve considerare attendibile l'host e, quando configurato per l'uso dell'attestazione TPM, il software in esecuzione su di esso.
I passaggi per autorizzare un nuovo host variano in base alla modalità di attestazione per cui è attualmente configurato HGS.
Per controllare la modalità di attestazione per l'infrastruttura sorvegliata, eseguire `Get-HgsServer` in qualsiasi nodo HGS.

#### <a name="software-configuration"></a>Configurazione software
Nel nuovo host Hyper-V verificare che sia installato Windows Server 2016 Datacenter Edition.
Windows Server 2016 standard non è in grado di eseguire VM schermate in un'infrastruttura sorvegliata.
È possibile che l'host sia installato nell'esperienza desktop o Server Core.

Sul server con esperienza desktop e Server Core, è necessario installare i ruoli del server supporto Hyper-V e sorveglianza host per Hyper-V:

```powershell
Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
```

#### <a name="admin-trusted-attestation"></a>Attestazione amministratore
Per registrare un nuovo host in HGS quando si usa l'attestazione attendibile amministratore, è necessario innanzitutto aggiungere l'host a un gruppo di sicurezza nel dominio a cui è stato aggiunto.
In genere, ogni dominio avrà un gruppo di sicurezza per gli host sorvegliati.
Se il gruppo è già stato registrato con HGS, l'unica azione che è necessario eseguire è riavviare l'host per aggiornare l'appartenenza al gruppo.

È possibile verificare quali gruppi di sicurezza sono considerati attendibili da HGS eseguendo il comando seguente:

```powershell
Get-HgsAttestationHostGroup
```

Per registrare un nuovo gruppo di sicurezza con HGS, acquisire prima di tutto l'ID di sicurezza (SID) del gruppo nel dominio dell'host e registrare il SID con HGS.

```powershell
Add-HgsAttestationHostGroup -Name "Contoso Guarded Hosts" -Identifier "S-1-5-21-3623811015-3361044348-30300820-1013"
```

Le istruzioni su come configurare la relazione di trust tra il dominio host e HGS sono disponibili nella Guida alla distribuzione.

#### <a name="tpm-trusted-attestation"></a>Attestazione TPM
Quando HGS è configurato in modalità TPM, gli host devono passare tutti i criteri bloccati e i criteri "Enabled" con prefisso "Hgs_", oltre ad almeno una baseline TPM, un identificatore TPM e un criterio di integrità del codice.
Ogni volta che si aggiunge un nuovo host, sarà necessario registrare il nuovo identificatore TPM con HGS.
Fino a quando l'host esegue lo stesso software (con lo stesso criterio di integrità del codice applicato) e la baseline del TPM come un altro host nell'ambiente in uso, non sarà necessario aggiungere nuovi criteri CI o linee di base.

**Aggiunta dell'identificatore TPM per un nuovo host** Nel nuovo host eseguire il comando seguente per acquisire l'identificatore del TPM.
Assicurarsi di specificare un nome univoco per l'host che consentirà di cercarlo in HGS.
Queste informazioni sono necessarie se si rimuove le autorizzazioni dell'host o si vuole impedire che esegua macchine virtuali schermate in HGS.

```powershell
(Get-PlatformIdentifier -Name "Host01").InnerXml | Out-File C:\temp\host01.xml -Encoding UTF8
```

Copiare questo file nel server HGS, quindi eseguire il comando seguente per registrare l'host con HGS.

```powershell
Add-HgsAttestationTpmHost -Name 'Host01' -Path C:\temp\host01.xml
```

**Aggiunta di una nuova baseline TPM** Se il nuovo host esegue una nuova configurazione hardware o firmware per l'ambiente in uso, potrebbe essere necessario adottare una nuova baseline TPM.
A tale scopo, eseguire il comando seguente nell'host.

```powershell
Get-HgsAttestationBaselinePolicy -Path 'C:\temp\hardwareConfig01.tcglog'
```

> [!NOTE]
> Se viene visualizzato un errore che indica che l'host non ha superato la convalida e non è stato in grado di attestare, non preoccuparsi.
> Si tratta di un controllo dei prerequisiti per assicurarsi che l'host possa eseguire macchine virtuali schermate e probabilmente significa che non è stato ancora applicato un criterio di integrità del codice o un'altra impostazione necessaria.
> Leggere il messaggio di errore, apportare le modifiche suggerite, quindi riprovare.
> In alternativa, è possibile ignorare la convalida in questo momento aggiungendo il flag `-SkipValidation` al comando.

Copiare la baseline TPM nel server HGS, quindi registrarla con il comando seguente.
Si consiglia di utilizzare una convenzione di denominazione che consenta di comprendere la configurazione hardware e firmware di questa classe di host Hyper-V.

```powershell
Add-HgsAttestationTpmPolicy -Name 'HardwareConfig01' -Path 'C:\temp\hardwareConfig01.tcglog'
```

**Aggiunta di un nuovo criterio di integrità del codice** Se sono stati modificati i criteri di integrità del codice in esecuzione negli host Hyper-V, sarà necessario registrare i nuovi criteri con HGS prima che tali host possano essere attestati correttamente.
In un host di riferimento, che funge da immagine master per i computer Hyper-V attendibili nell'ambiente, acquisire un nuovo criterio CI usando il comando `New-CIPolicy`.
Si consiglia di usare il livello **filepublisher** e il fallback **hash** per i criteri ci dell'host Hyper-V.
Prima di tutto, è necessario creare un criterio CI in modalità di controllo per assicurarsi che tutto funzioni come previsto.
Dopo aver convalidato un carico di lavoro di esempio nel sistema, è possibile applicare i criteri e copiare la versione applicata a HGS.
Per un elenco completo delle opzioni di configurazione dei criteri di integrità del codice, consultare la [documentazione di Device Guard](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-deploy-code-integrity-policies).

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

Una volta creati, testati e applicati i criteri, copiare il file binario (con estensione p7b) nel server HGS e registrare il criterio.

```powershell
Add-HgsAttestationCiPolicy -Name 'WS2016-Hardware01' -Path 'C:\temp\ws2016-hardware01-ci.p7b'
```

**Aggiunta di una chiave di crittografia del dump della memoria**

Quando il criterio *HGS @ no__t-1NoDumps* è disabilitato e il criterio *HGS @ no__t-3DumpEncryption* è abilitato, gli host sorvegliati possono disporre di dump di memoria (inclusi i dump di arresto anomalo del sistema) per essere abilitati finché tali dump sono crittografati. Gli host sorvegliati passeranno l'attestazione solo se i dump della memoria sono disabilitati o li crittografano con una chiave nota a HGS. Per impostazione predefinita, nessuna chiave di crittografia del dump viene configurata in HGS.

Per aggiungere una chiave di crittografia del dump a HGS, usare il cmdlet `Add-HgsAttestationDumpPolicy` per fornire HGS con l'hash della chiave di crittografia del dump.
Se si acquisisce una baseline TPM in un host Hyper-V configurato con la crittografia del dump, l'hash è incluso in tcglog e può essere fornito al cmdlet `Add-HgsAttestationDumpPolicy`.

```powershell
Add-HgsAttestationDumpPolicy -Name 'DumpEncryptionKey01' -Path 'C:\temp\TpmBaselineWithDumpEncryptionKey.tcglog'
```

In alternativa, è possibile fornire direttamente la rappresentazione di stringa dell'hash al cmdlet.

```powershell
Add-HgsAttestationDumpPolicy -Name 'DumpEncryptionKey02' -PublicKeyHash '<paste your hash here>'
```

Assicurarsi di aggiungere ogni chiave di crittografia di dump univoca a HGS se si sceglie di usare chiavi diverse nell'infrastruttura sorvegliata.
Gli host che eseguono la crittografia dei dump della memoria con una chiave non nota a HGS non passeranno l'attestazione.

Per ulteriori informazioni sulla [configurazione della crittografia dump negli host](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption), consultare la documentazione di Hyper-V.

#### <a name="check-if-the-system-passed-attestation"></a>Controllare se il sistema ha superato l'attestazione
Dopo aver registrato le informazioni necessarie con HGS, è necessario controllare se l'host supera l'attestazione.
Nell'host Hyper-V appena aggiunto eseguire `Set-HgsClientConfiguration` e specificare gli URL corretti per il cluster HGS.
Questi URL possono essere ottenuti eseguendo `Get-HgsServer` in qualsiasi nodo HGS.

```powershell
Set-HgsClientConfiguration -KeyProtectionServerUrl 'http://hgs.bastion.local/KeyProtection' -AttestationServerUrl 'http://hgs.bastion.local/Attestation'
```

Se lo stato risultante non indica "IsHostGuarded: True "sarà necessario risolvere i problemi relativi alla configurazione.
Nell'host che non ha superato l'attestazione, eseguire il comando seguente per ottenere un report dettagliato sui problemi che possono aiutare a risolvere l'attestazione non riuscita.

```powershell
Get-HgsTrace -RunDiagnostics -Detailed
```

> [!IMPORTANT]
> Se si usa Windows Server 2019 o Windows 10, versione 1809 e si usano criteri di integrità del codice, `Get-HgsTrace` può restituire un errore per la diagnostica **attiva del criterio di integrità del codice** .
> Questo risultato può essere ignorato in modo sicuro quando è l'unica diagnostica non riuscita.

### <a name="review-attestation-policies"></a>Esaminare i criteri di attestazione
Per esaminare lo stato corrente dei criteri configurati in HGS, eseguire i comandi seguenti in qualsiasi nodo HGS:

```powershell
# List all trusted security groups for admin-trusted attestation
Get-HgsAttestationHostGroup

# List all policies configured for TPM-trusted attestation
Get-HgsAttestationPolicy
```

Se si individua un criterio abilitato che non soddisfa più i requisiti di sicurezza, ad esempio un criterio di integrità del codice obsoleto che ora viene considerato non sicuro, è possibile disabilitarlo sostituendo il nome del criterio nel comando seguente:

```powershell
Disable-HgsAttestationPolicy -Name 'PolicyName'
```

Analogamente, è possibile usare `Enable-HgsAttestationPolicy` per riabilitare i criteri.

Se non è più necessario un criterio e si desidera rimuoverlo da tutti i nodi HGS, eseguire `Remove-HgsAttestationPolicy -Name 'PolicyName'` per eliminare definitivamente i criteri.

## <a name="changing-attestation-modes"></a>Modifica delle modalità di attestazione
Se l'infrastruttura sorvegliata è stata avviata usando un'attestazione attendibile dell'amministratore, è probabile che si voglia eseguire l'aggiornamento alla modalità di attestazione TPM molto più avanzata non appena si dispone di un numero sufficiente di host compatibili con TPM 2,0 nell'ambiente.
Quando si è pronti per passare, è possibile precaricare tutti gli elementi di attestazione (criteri CI, linee di base TPM e identificatori TPM) in HGS continuando a eseguire HGS con attestazione attendibile per l'amministratore.
A tale scopo, è sufficiente seguire le istruzioni nella sezione [autorizzazione di un nuovo host sorvegliato](#authorizing-new-guarded-hosts) .

Una volta aggiunti tutti i criteri a HGS, il passaggio successivo consiste nell'eseguire un tentativo di attestazione sintetica sugli host per verificare se passano l'attestazione in modalità TPM.
Questo non influisce sullo stato operativo corrente di HGS.
I comandi seguenti devono essere eseguiti in un computer che ha accesso a tutti gli host nell'ambiente e almeno un nodo HGS.
Se il firewall o altri criteri di sicurezza ne impediscono questa operazione, è possibile ignorare questo passaggio.
Quando possibile, è consigliabile eseguire l'attestazione sintetica per indicare se l'"capovolgimento" in modalità TPM provocherà tempi di inattività per le macchine virtuali. 

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

Al termine della diagnostica, rivedere le informazioni restituite per determinare se gli host non avrebbero superato l'attestazione in modalità TPM.
Eseguire di nuovo la diagnostica fino a quando non si ottiene un "passaggio" da ogni host, quindi procedere con la modifica di HGS in modalità TPM.

Il completamento della **modifica della modalità TPM** richiede solo un secondo.
Eseguire il comando seguente in qualsiasi nodo HGS per aggiornare la modalità di attestazione.

```powershell
Set-HgsServer -TrustTpm
```

Se si verificano problemi ed è necessario tornare alla modalità di Active Directory, è possibile eseguire questa operazione eseguendo `Set-HgsServer -TrustActiveDirectory`.

Dopo aver verificato che tutto funzioni come previsto, è necessario rimuovere tutti i gruppi host Active Directory attendibili da HGS e rimuovere la relazione di trust tra i domini HGS e fabric.
Se si lascia il trust di Active Directory, si rischia di riabilitare il trust e di passare HGS alla modalità Active Directory, che potrebbe consentire l'esecuzione di codice non attendibile sugli host sorvegliati.

## <a name="key-management"></a>Gestione delle chiavi
La soluzione infrastruttura sorvegliata usa diverse coppie di chiavi pubbliche/private per convalidare l'integrità dei vari componenti della soluzione e crittografare i segreti del tenant.
Il servizio sorveglianza host è configurato con almeno due certificati (con chiavi pubbliche e private), che vengono usati per firmare e crittografare le chiavi usate per avviare le macchine virtuali schermate.
Tali chiavi devono essere gestite con cautela.
Se la chiave privata viene acquisita da un antagonista, sarà in grado di rimuovere schermate tutte le macchine virtuali in esecuzione nell'infrastruttura o di configurare un cluster HGS impostore che usa criteri di attestazione più vulnerabili per ignorare le protezioni inserite.
Se si perdono le chiavi private durante un'emergenza e non le si trova in un backup, è necessario configurare una nuova coppia di chiavi e fare in modo che ogni macchina virtuale venga nuovamente riordinata per autorizzare i nuovi certificati.

In questa sezione vengono illustrati gli argomenti generali di gestione delle chiavi che consentono di configurare le chiavi in modo che siano funzionali e sicure.

### <a name="adding-new-keys"></a>Aggiunta di nuove chiavi
Mentre HGS deve essere inizializzato con un set di chiavi, è possibile aggiungere più di una chiave di crittografia e di firma a HGS.
I due motivi più comuni per cui si aggiungono nuove chiavi a HGS sono i seguenti:
1. Per supportare scenari "Bring your own key" in cui i tenant copiano le proprie chiavi private nel modulo di protezione hardware e autorizzano solo le chiavi per avviare le VM schermate.
2. Per sostituire le chiavi esistenti per HGS aggiungendo prima le nuove chiavi e mantenendo entrambi i set di chiavi finché ogni configurazione della macchina virtuale non è stata aggiornata per l'uso delle nuove chiavi.

Il processo per aggiungere le nuove chiavi è diverso in base al tipo di certificato usato.

**Opzione 1: Aggiunta di un certificato archiviato in un modulo di protezione hardware @ no__t-0

L'approccio consigliato per la protezione delle chiavi di HGS consiste nell'usare i certificati creati in un modulo di protezione hardware (HSM).
HSM assicurarsi che l'uso delle chiavi sia associato all'accesso fisico a un dispositivo sensibile alla sicurezza nel Data Center.
Ogni modulo di protezione hardware è diverso ed è dotato di un processo univoco per creare certificati e registrarli con HGS.
I passaggi seguenti hanno lo scopo di fornire indicazioni di base per l'uso dei certificati supportati da HSM.
Consultare la documentazione del fornitore del modulo di protezione hardware per informazioni sui passaggi e le funzionalità esatte.

1. Installare il software HSM in ogni nodo HGS del cluster. A seconda che si disponga di una rete o di un dispositivo HSM locale, potrebbe essere necessario configurare il modulo di protezione hardware per concedere al computer l'accesso all'archivio chiavi.
2. Creare 2 certificati nel modulo di protezione hardware con **chiavi RSA a 2048 bit** per la crittografia e la firma
    1. Creare un certificato di crittografia con la proprietà utilizzo chiave di crittografia **dei dati** nel modulo di protezione hardware
    2. Creare un certificato di firma con la proprietà utilizzo chiave **firma digitale** nel modulo di protezione hardware
3. Installare i certificati nell'archivio certificati locale di ogni nodo HGS in base alle indicazioni del fornitore del modulo di protezione hardware.
4. Se il modulo di protezione hardware usa autorizzazioni granulari per concedere a applicazioni o utenti specifici l'autorizzazione per usare la chiave privata, sarà necessario concedere all'account del servizio gestito del gruppo HGS l'accesso al certificato. È possibile trovare il nome dell'account HGS gMSA eseguendo `(Get-IISAppPool -Name KeyProtection).ProcessModel.UserName`
5. Aggiungere i certificati di firma e crittografia a HGS sostituendo le identificazioni personali con quelli dei certificati nei comandi seguenti:

    ```powershell
    Add-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint "AABBCCDDEEFF00112233445566778899"
    Add-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint "99887766554433221100FFEEDDCCBBAA"
    ```

**Opzione 2: Aggiunta di certificati software non esportabili @ no__t-0

Se si dispone di un certificato supportato da software emesso dall'azienda o da un'autorità di certificazione pubblica con una chiave privata non esportabile, sarà necessario aggiungere il certificato a HGS usando la relativa identificazione personale.
1. Installare il certificato nel computer in base alle istruzioni dell'autorità di certificazione.
2. Concedere all'account del servizio gestito del gruppo HGS l'accesso in lettura alla chiave privata del certificato. È possibile trovare il nome dell'account HGS gMSA eseguendo `(Get-IISAppPool -Name KeyProtection).ProcessModel.UserName`
3. Registrare il certificato con HGS usando il comando seguente e sostituendo nell'identificazione personale del certificato (modificare la *crittografia* per la *firma* per i certificati di firma):

    ```powershell
    Add-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint "AABBCCDDEEFF00112233445566778899"
    ```

> [!IMPORTANT]
> Sarà necessario installare manualmente la chiave privata e concedere l'accesso in lettura all'account gMSA in ogni nodo HGS.
> HGS non è in grado di replicare automaticamente le chiavi private per *i* certificati registrati dalla relativa identificazione personale.

**Option 3: Aggiunta di certificati archiviati nei file PFX @ no__t-0

Se si dispone di un certificato supportato da software con una chiave privata esportabile che può essere archiviata nel formato di file PFX e protetto con una password, HGS può gestire automaticamente i certificati.
I certificati aggiunti con i file PFX vengono replicati automaticamente in ogni nodo del cluster HGS e HGS protegge l'accesso alle chiavi private.
Per aggiungere un nuovo certificato usando un file PFX, eseguire i comandi seguenti in qualsiasi nodo HGS (modificare la *crittografia* per la *firma* per i certificati di firma):

```powershell
$certPassword = Read-Host -AsSecureString -Prompt "Provide the PFX file password"
Add-HgsKeyProtectionCertificate -CertificateType Encryption -CertificatePath "C:\temp\encryptionCert.pfx" -CertificatePassword $certPassword
```

**Identificazione e modifica dei certificati primari** Sebbene HGS possa supportare più certificati di firma e crittografia, usa una coppia come certificati "primari".
Questi sono i certificati che verranno usati se un utente Scarica i metadati del tutore per il cluster HGS.
Per verificare quali certificati sono attualmente contrassegnati come certificati primari, eseguire il comando seguente:

```powershell
Get-HgsKeyProtectionCertificate -IsPrimary $true
```

Per impostare un nuovo certificato di firma o di crittografia primaria, trovare l'identificazione personale del certificato desiderato e contrassegnarla come primaria usando i comandi seguenti:

```powershell
Get-HgsKeyProtectionCertificate
Set-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint "AABBCCDDEEFF00112233445566778899" -IsPrimary
Set-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint "99887766554433221100FFEEDDCCBBAA" -IsPrimary
```

### <a name="renewing-or-replacing-keys"></a>Rinnovo o sostituzione di chiavi
Quando si creano i certificati usati da HGS, ai certificati verrà assegnata una data di scadenza in base ai criteri dell'autorità di certificazione e alle informazioni sulla richiesta.
In genere, negli scenari in cui la validità del certificato è importante, ad esempio la protezione delle comunicazioni HTTP, i certificati devono essere rinnovati prima della scadenza per evitare un'interferenza del servizio o un messaggio di errore preoccupante.
HGS non usa i certificati in questo senso.
HGS usa semplicemente i certificati come modo pratico per creare e archiviare una coppia di chiavi asimmetriche.
Un certificato di firma o di crittografia scaduto in HGS non indica una debolezza o una perdita di protezione per le macchine virtuali schermate.
Inoltre, i controlli di revoca dei certificati non vengono eseguiti da HGS.
Se un certificato HGS o un certificato dell'autorità emittente viene revocato, non influirà sull'utilizzo del certificato da parte di HGS.

L'unico momento in cui è necessario preoccuparsi di un certificato HGS è se si ha motivo di ritenere che la chiave privata sia stata rubata.
In tal caso, l'integrità delle VM schermate è a rischio perché il possesso della parte privata della coppia di chiavi di firma e di crittografia HGS è sufficiente per rimuovere le protezioni di schermatura in una macchina virtuale o per configurare un server HGS fasullo con criteri di attestazione più vulnerabili.

Se ci si trova in questa situazione o sono richiesti dagli standard di conformità per aggiornare regolarmente le chiavi del certificato, i passaggi seguenti illustrano il processo di modifica delle chiavi in un server HGS.
Si noti che le linee guida seguenti rappresentano un'impresa significativa che comporterà un'interferenza del servizio per ogni macchina virtuale servita dal cluster HGS.
Per ridurre al minimo l'interferenza dei servizi e garantire la sicurezza delle macchine virtuali tenant, è necessaria una pianificazione corretta per modificare le chiavi di HGS.

In un nodo HGS eseguire la procedura seguente per registrare una nuova coppia di certificati di crittografia e firma.
Vedere la sezione relativa all' [aggiunta di nuove chiavi](#adding-new-keys) per informazioni dettagliate sui vari modi per aggiungere nuove chiavi a HGS.
1. Creare una nuova coppia di certificati di crittografia e firma per il server HGS. Idealmente, questi verranno creati in un modulo di protezione hardware.
2. Registrare i nuovi certificati di crittografia e firma con **Add-HgsKeyProtectionCertificate**

    ```powershell
    Add-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint>
    Add-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint>
    ```
3. Se sono state usate le identificazioni personali, è necessario passare a ogni nodo del cluster per installare la chiave privata e concedere a HGS gMSA l'accesso alla chiave.
4. Rendere i nuovi certificati i certificati predefiniti in HGS

    ```powershell
    Set-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint> -IsPrimary
    Set-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint> -IsPrimary
    ```

A questo punto, la schermatura dei dati creati con i metadati ottenuti dal nodo HGS utilizzerà i nuovi certificati, ma le macchine virtuali esistenti continueranno a funzionare perché i certificati precedenti sono ancora presenti.
Per assicurarsi che tutte le macchine virtuali esistenti funzionino con le nuove chiavi, sarà necessario aggiornare la protezione con chiave in ogni macchina virtuale.
Si tratta di un'azione che richiede che il proprietario della macchina virtuale (persona o entità in possesso del tutore "proprietario") sia interessato.
Per ogni macchina virtuale schermata, seguire questa procedura:
5. Arrestare la macchina virtuale. Non è possibile riattivare la macchina virtuale fino a quando non sono stati completati i passaggi rimanenti, altrimenti sarà necessario riavviare il processo.
6. Salva la protezione con chiave corrente in un file: `Get-VMKeyProtector -VMName 'VM001' | Out-File '.\VM001.kp'`
7. Trasferire il protocollo KP al proprietario della macchina virtuale
8. Chiedere al proprietario di scaricare le informazioni aggiornate sul tutore da HGS e importarle nel sistema locale
9. Leggere il KP corrente in memoria, concedere al nuovo Guardian l'accesso a KP e salvarlo in un nuovo file eseguendo i comandi seguenti:

    ```powershell
    $kpraw = Get-Content -Path .\VM001.kp
    $kp = ConvertTo-HgsKeyProtector -Bytes $kpraw
    $newGuardian = Get-HgsGuardian -Name 'UpdatedHgsGuardian'
    $updatedKP = Grant-HgsKeyProtectorAccess -KeyProtector $kp -Guardian $newGuardian
    $updatedKP.RawData | Out-File .\updatedVM001.kp
    ```
10. Copiare di nuovo il protocollo KP aggiornato nell'infrastruttura host
11. Applicare il protocollo KP alla VM originale:

   ```powershell
   $updatedKP = Get-Content -Path .\updatedVM001.kp
   Set-VMKeyProtector -VMName VM001 -KeyProtector $updatedKP
   ```
12. Infine, avviare la macchina virtuale e assicurarsi che venga eseguita correttamente.

> [!NOTE]
> Se il proprietario della macchina virtuale imposta una protezione con chiave non corretta nella macchina virtuale e non autorizza l'infrastruttura a eseguire la macchina virtuale, non sarà possibile avviare la macchina virtuale schermata.
> Per tornare all'ultima protezione con chiave corretta nota, eseguire `Set-VMKeyProtector -RestoreLastKnownGoodKeyProtector`

Dopo che tutte le macchine virtuali sono state aggiornate per autorizzare le nuove chiavi di sorveglianza, è possibile disabilitare e rimuovere le chiavi obsolete.

13. Ottenere le identificazioni personali dei certificati obsoleti da `Get-HgsKeyProtectionCertificate -IsPrimary $false`

14. Disabilitare ogni certificato eseguendo i comandi seguenti:  

   ```powershell
   Set-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint> -IsEnabled $false
   Set-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint> -IsEnabled $false
   ```

15. Dopo aver verificato che le macchine virtuali siano ancora in grado di iniziare con i certificati disabilitati, rimuovere i certificati da HGS eseguendo i comandi seguenti:

   ```powershell
   Remove-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint>`
   Remove-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint>`
   ```

> [!IMPORTANT]
> I backup delle VM contengono informazioni obsolete sulla protezione con chiave che consentono di usare i certificati obsoleti per avviare la macchina virtuale.
> Se si è consapevoli del fatto che la chiave privata è stata compromessa, è necessario presupporre che i backup della macchina virtuale possano essere compromessi e intraprendere l'azione appropriata.
> Eliminando la configurazione della macchina virtuale dai backup (con estensione vmcx) si rimuoveranno le protezioni con chiave, al costo della necessità di usare la password di ripristino di BitLocker per avviare la VM la volta successiva.

### <a name="key-replication-between-nodes"></a>Replica chiave tra nodi
Ogni nodo nel cluster HGS deve essere configurato con gli stessi certificati SSL di crittografia, firma e (se configurato).
Questa operazione è necessaria per garantire che le richieste vengano gestite correttamente dagli host Hyper-V che raggiungono tutti i nodi del cluster.

**Se il server HGS è stato inizializzato con certificati basati su PFX,** HGS replica automaticamente sia la chiave pubblica che quella privata di tali certificati in ogni nodo del cluster.
È sufficiente aggiungere le chiavi in un solo nodo.

**Se il server HGS è stato inizializzato con i riferimenti al certificato** o le identificazioni personali, HGS replica solo la chiave *pubblica* nel certificato in ogni nodo.
Inoltre, HGS non è in grado di concedere l'accesso alla chiave privata in tutti i nodi in questo scenario.
Pertanto, è responsabilità dell'utente:
1. Installare la chiave privata in ogni nodo HGS
2. Concedere all'account del servizio gestito del gruppo HGS (gMSA) la chiave privata in ogni nodo. queste attività aggiungono un carico di lavoro aggiuntivo, ma sono necessarie per le chiavi e i certificati supportati da HSM con chiavi private non esportabili.

I **certificati SSL** non vengono mai replicati in alcun formato.
È responsabilità dell'utente inizializzare ogni server HGS con lo stesso certificato SSL e aggiornare ogni server ogni volta che si sceglie di rinnovare o sostituire il certificato SSL.
Quando si sostituisce il certificato SSL, è consigliabile usare il cmdlet [set-HgsServer](https://technet.microsoft.com/library/mt652180.aspx) .

## <a name="unconfiguring-hgs"></a>Annullamento della configurazione di HGS

Se è necessario rimuovere le autorizzazioni o riconfigurare in modo significativo un server HGS, è possibile usare i cmdlet [Clear-HgsServer](https://technet.microsoft.com/library/mt652176.aspx) o [Uninstall-HgsServer](https://technet.microsoft.com/library/mt652182.aspx) .

### <a name="clearing-the-hgs-configuration"></a>Cancellazione della configurazione di HGS

Per rimuovere un nodo dal cluster HGS, usare il cmdlet [Clear-HgsServer](https://technet.microsoft.com/library/mt652176.aspx) .
Questo cmdlet effettuerà le seguenti modifiche nel server in cui viene eseguito:

- Annulla la registrazione dei servizi di attestazione e protezione delle chiavi
- Rimuove l'endpoint di gestione di JEA "Microsoft. Windows. HGS"
- Rimuove il computer locale dal cluster di failover di HGS

Se il server è l'ultimo nodo HGS nel cluster, verranno eliminati anche il cluster e la corrispondente risorsa del nome di rete distribuita.

```powershell
# Removes the local computer from the HGS cluster
Clear-HgsServer
```

Al termine dell'operazione di cancellazione, il server HGS può essere reinizializzato con [Initialize-HgsServer](https://technet.microsoft.com/library/mt652185.aspx).
Se è stato usato [Install-HgsServer](https://technet.microsoft.com/library/mt652169.aspx) per configurare un dominio Active Directory Domain Services, tale dominio rimarrà configurato e operativo dopo l'operazione di cancellazione.

### <a name="uninstalling-hgs"></a>Disinstallazione di HGS

Se si vuole rimuovere un nodo dal cluster HGS **e** abbassare di valore il dominio di Active Directory controller in esecuzione, usare il cmdlet [Uninstall-HgsServer](https://technet.microsoft.com/library/mt652182.aspx) .
Questo cmdlet effettuerà le seguenti modifiche nel server in cui viene eseguito:

- Annulla la registrazione dei servizi di attestazione e protezione delle chiavi
- Rimuove l'endpoint di gestione di JEA "Microsoft. Windows. HGS"
- Rimuove il computer locale dal cluster di failover di HGS
- Abbassa di più il controller di Dominio di Active Directory, se configurato

Se il server è l'ultimo nodo HGS nel cluster, verranno eliminati anche il dominio, il cluster di failover e la risorsa del nome di rete distribuita del cluster.

```powershell
# Removes the local computer from the HGS cluster and demotes the ADDC (restart required)
$newLocalAdminPassword = Read-Host -AsSecureString -Prompt "Enter a new password for the local administrator account"
Uninstall-HgsServer -LocalAdministratorPassword $newLocalAdminPassword -Restart
```

Al termine dell'operazione di disinstallazione e dopo il riavvio del computer, è possibile reinstallare ADDC e HGS usando [Install-HgsServer](https://technet.microsoft.com/library/mt652169.aspx) o aggiungere il computer a un dominio e inizializzare il server HGS in tale dominio con [Initialize-HgsServer](https://technet.microsoft.com/library/mt652185.aspx).

Se non si intende più usare il computer come nodo HGS, è possibile rimuovere il ruolo da Windows.

```powershell
Uninstall-WindowsFeature HostGuardianServiceRole
```
