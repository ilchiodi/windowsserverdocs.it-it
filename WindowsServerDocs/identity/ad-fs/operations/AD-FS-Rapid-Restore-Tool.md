---
ms.assetid: 4deff06a-d0ef-4e5a-9701-5911ba667201
title: Strumento di ripristino rapido di AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/02/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 506734812689a42ec17768652ac715f5c7e24401
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858104"
---
# <a name="ad-fs-rapid-restore-tool"></a>Strumento di ripristino rapido di AD FS

## <a name="overview"></a>Panoramica
Oggi ADFS viene reso disponibile mediante l'impostazione di una farm ADFS. Alcune organizzazioni desiderano un modo per avere un singolo server di distribuzione di ADFS, eliminando la necessità di più server ADFS e bilanciamento di infrastruttura, mantenendo al contempo alcune carico di rete garantisce che service può essere ripristinato rapidamente se si è verificato un problema.
Il nuovo strumento di ripristino rapido di AD FS fornisce un modo per ripristinare i dati di ADFS senza un backup completo e il ripristino del sistema operativo o lo stato del sistema. È possibile utilizzare il nuovo strumento per esportare la configurazione di ADFS in Azure o in un percorso locale.  È quindi possibile applicare i dati esportati per una nuova installazione di ADFS, ricreare o duplicare l'ambiente di ADFS. 

## <a name="scenarios"></a>Scenari
Lo strumento di ripristino rapido di AD FS può essere utilizzato negli scenari seguenti:

1. Ripristinare rapidamente la funzionalità di ADFS dopo un problema
    - Utilizzare lo strumento per creare un'installazione di standby a freddo di ADFS che possono essere distribuiti rapidamente al posto del server ADFS online
2. Distribuire gli ambienti di test e produzione identici
    - Utilizzare lo strumento per creare rapidamente una copia della produzione ADFS in un ambiente di test o per distribuire rapidamente una configurazione di test di convalida nell'ambiente di produzione
3. Eseguire la migrazione da una configurazione basata su SQL a WID e viceversa
    - Utilizzare lo strumento per passare da una configurazione della farm basata su SQL a WID o viceversa. 


>[!NOTE] 
>Se si usa la replica di tipo merge SQL o i gruppi di disponibilità always on, lo strumento di ripristino rapido non è supportato. È consigliabile usare i backup basati su SQL e un backup del certificato SSL come alternativa.

## <a name="what-is-backed-up"></a>Ciò che viene eseguito il backup
Lo strumento esegue il backup della configurazione di ADFS seguente
    
- Database di configurazione AD FS (SQL o WID)
- File di configurazione (che si trova nella cartella AD FS)
- Generato automaticamente i token di firma e la decrittografia di certificati e chiavi private (dal contenitore di Active Directory distribuite)
- Certificato SSL ed eventuali esternamente registrati certificati (la firma di token, la decrittografia di token e la comunicazione dei servizi) e le corrispondenti chiavi private (Nota: le chiavi private devono essere esportabile e l'utente che esegue lo script deve disporre delle autorizzazioni per accedervi)
- Un elenco di provider di autenticazione personalizzato, gli archivi di attributi, provider di attestazioni locale considera attendibile che sono installati.

## <a name="how-to-use-the-tool"></a>Procedura: utilizzare lo strumento
Primo, [scaricare](https://go.microsoft.com/fwlink/?LinkId=825646) e installare il file MSI per il server ADFS.  

Eseguire il comando seguente da un prompt di PowerShell:

```powershell
import-module 'C:\Program Files (x86)\ADFS Rapid Recreation Tool\ADFSRapidRecreationTool.dll'
```

>[!NOTE] 
>Se si utilizza il Database integrata di Windows (WID), questo strumento deve essere eseguito sul server ADFS primario.  È possibile utilizzare il `Get-AdfsSyncProperties` cmdlet PowerShell per determinare se il server si utilizza è il server primario.

### <a name="system-requirements"></a>Requisiti di sistema

- Questo strumento funziona per AD FS in Windows Server 2012 R2 e versioni successive. 
- Richiesto .NET framework è almeno 4.0. 
- Il ripristino deve essere eseguito in un server ADFS della stessa versione per il backup e che utilizza lo stesso account di Active Directory come account del servizio ADFS.

## <a name="create-a-backup"></a>Creare un backup
Per creare un backup, utilizzare il cmdlet Backup-ADFS. Questo cmdlet esegue il backup di configurazione di ADFS, database, i certificati SSL, e così via. 

L'utente deve essere almeno un amministratore locale per eseguire questo cmdlet. Per eseguire il backup del Active Directory contenitore DKM (necessario nella configurazione AD FS predefinita), l'utente deve essere amministratore di dominio, deve passare le credenziali dell'account del servizio AD FS o avere accesso al contenitore DKM.  Se si usa un account gMSA, l'utente deve essere amministratore di dominio o avere le autorizzazioni per il contenitore. non è possibile specificare le credenziali di gMSA. 

Il backup verrà denominato in base al modello "adfsBackup_ID_Date-Time". Contiene il numero di versione, data e ora in cui è stato eseguito il backup.
Il cmdlet accetta i parametri seguenti:
    
Set di parametri

![Strumento di ripristino rapido di AD FS](media/AD-FS-Rapid-Restore-Tool/parameter1.png)

### <a name="detailed-description"></a>Descrizione dettagliata

- **BackupDKM** -esegue il backup il contenitore di Active Directory distribuite che contiene le chiavi di ADFS nella configurazione predefinita (generato automaticamente token di firma e la decrittografia di certificati). Viene usato uno strumento AD "LDIFDE" per esportare il contenitore di Active Directory e tutti i relativi sottoalberi.

- -**StorageType &lt;stringa&gt;** : il tipo di archiviazione che l'utente vuole usare. "FileSystem" indica che l'utente vuole archiviarlo in una cartella in locale o nella rete "Azure" indica che l'utente vuole archiviarlo nel contenitore di archiviazione di Azure quando l'utente esegue il backup, seleziona il percorso di backup, ovvero il file System o nel cloud. Per Azure da utilizzare, le credenziali di archiviazione di Azure deve essere passate al cmdlet. Le credenziali di archiviazione contiene il nome dell'account e la chiave. Inoltre, un nome di contenitore deve anche essere passato. Se il contenitore non esiste, viene creato durante il backup. Per il file system da utilizzare, è necessario fornire un percorso di archiviazione. In questa directory verrà creata una nuova directory per ogni backup. Ogni directory creata conterrà i file di backup. 

- **EncryptionPassword &lt;stringa&gt;** -la password che verrà utilizzato per crittografare tutti i file di backup prima dell'archiviazione

- **AzureConnectionCredentials &lt;pscredential&gt;** -il nome dell'account e la chiave per l'account di archiviazione di Azure

- **AzureStorageContainer &lt;stringa&gt;** -contenitore di archiviazione in cui verrà archiviato il backup in Azure

- **StoragePath &lt;stringa&gt;** -il percorso di backup verranno archiviati

- **ServiceAccountCredential &lt;pscredential&gt;** : specifica l'account del servizio utilizzato per il servizio ad FS attualmente in esecuzione. Questo parametro è necessario solo se l'utente vuole eseguire il backup di DKM e non è amministratore di dominio o non ha accesso al contenuto del contenitore. 

- **BackupComment &lt;string []&gt;** -una stringa informativa sul backup verrà visualizzato durante il ripristino, simile al concetto di denominazione checkpoint Hyper-V. Il valore predefinito è una stringa vuota

 
## <a name="backup-examples"></a>Esempi di backup
Di seguito sono riportati esempi di backup per l'utilizzo dello strumento di ripristino rapido AD FS.

### <a name="backup-the-ad-fs-configuration-with-the-dkm-to-the-file-system-and-has-access-to-the-dkm-container-contents-either-domain-admin-or-delegated"></a>Eseguire il backup della configurazione di AD FS, con DKM, nel file System e avere accesso al contenuto del contenitore DKM (amministratore di dominio o delegato)

```powershell
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)" -BackupDKM
```
 
### <a name="backup-the-ad-fs-configuration-with-the-dkm-to-the-file-system-with-the-service-account-credential-running-as-local-admin"></a>Eseguire il backup della configurazione di AD FS, con DKM, nella file system con le credenziali dell'account del servizio, in esecuzione come amministratore locale

```powershell
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)" -BackupDKM -ServiceAccountCredential $cred
```

### <a name="backup-the-ad-fs-configuration-without-the-dkm-to-the-azure-storage-container"></a>Eseguire il backup della configurazione AD FS senza DKM nel contenitore di archiviazione di Azure.

```powershell
Backup-ADFS -StorageType "Azure" -AzureConnectionCredentials $cred -AzureStorageContainer "adfsbackups"  -EncryptionPassword "password" -BackupComment "Clean Install of ADFS"
```

### <a name="backup-the-ad-fs-configuration-without-the-dkm-to-the-file-system"></a>Eseguire il backup della configurazione AD FS senza DKM nel file System

```powershell     
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)"
```

## <a name="restore-from-backup"></a>Ripristino da backup
Per applicare una configurazione creata utilizzando Backup ADFS per una nuova installazione di ADFS, utilizzare il cmdlet Restore-ADFS.

Questo cmdlet crea una nuova farm AD FS usando il cmdlet `Install-AdfsFarm` e ripristina la configurazione AD FS, il database, i certificati e così via.  Se il ruolo AD FS non è stato installato nel server, verrà installato dal cmdlet.  Il cmdlet verifica il percorso di ripristino per i backup esistenti e richiede all'utente di scegliere un backup appropriato in base alla data e ora che è stato eseguito e qualsiasi commento backup che l'utente potrebbe avere collegato per il backup. Se sono presenti più configurazioni di ADFS con i nomi dei servizi di federazione, l'utente viene richiesto di scegliere la configurazione di ADFS appropriata.
L'utente deve essere un amministratore locali e di dominio per eseguire questo cmdlet.


>[!NOTE] 
>Prima di utilizzare lo strumento di ripristino rapido AD ADFS, assicurarsi che il server appartiene al dominio prima di ripristinare il backup. 

Il cmdlet accetta i parametri seguenti: 

![Strumento di ripristino rapido di AD FS](media/AD-FS-Rapid-Restore-Tool/parameter2.png)

### <a name="detailed-description"></a>Descrizione dettagliata

- **StorageType &lt;stringa&gt;** -il tipo di archiviazione, l'utente desidera utilizzare.
 "FileSystem" indica che l'utente vuole archiviarlo in una cartella in locale o nella rete "Azure" indica che l'utente vuole archiviarlo nel contenitore di archiviazione di Azure

- **DecryptionPassword &lt;stringa&gt;** -la password utilizzata per crittografare tutti i file di backup 

- **AzureConnectionCredentials &lt;pscredential&gt;** -il nome dell'account e la chiave per l'account di archiviazione di Azure

- **AzureStorageContainer &lt;stringa&gt;** -contenitore di archiviazione in cui verrà archiviato il backup in Azure

- **StoragePath &lt;stringa&gt;** -il percorso di backup verranno archiviati

- **ADFSName &lt; stringa &gt;** -il nome della federazione che è stato eseguito il backup e si intende ripristinare. Se non viene fornito ed è nome del servizio un solo federativo allora che verrà utilizzato. Se è presente più di un servizio federativo backup nel percorso, quindi l'utente viene chiesto di scegliere uno dei backup Federation Services.

- **ServiceAccountCredential &lt; pscredential &gt;** -specifica l'account del servizio che verrà usato per il nuovo servizio di ad FS da ripristinare 

- **GroupServiceAccountIdentifier &lt;stringa&gt;** : gMSA che l'utente vuole usare per il nuovo servizio di ad FS da ripristinare. Per impostazione predefinita, se non viene fornito quindi il backup di nome dell'account viene utilizzato se è stato GESTITO, else all'utente viene richiesto di inserire in un account di servizio

- **DBConnectionString &lt;stringa&gt;** : se l'utente desidera utilizzare un database diverso per il ripristino, quindi si deve passare la stringa di connessione SQL o un tipo di database interno di Windows per sarà WID

- **Forza &lt;bool&gt;** -ignorare le istruzioni che lo strumento potrebbe avere una volta scelto il backup

- **RestoreDKM &lt;bool&gt;** : ripristinare il contenitore di gestione delle chiavi distribuite per Active Directory, deve essere impostata se un nuovo annuncio e la gestione delle chiavi distribuite backup è stato inizialmente.

## <a name="restore-examples"></a>Esempi di ripristino

### <a name="restore-the-ad-fs-configuration-without-the-dkm-from-the-azure-storage-container"></a>Ripristinare la configurazione del AD FS senza DKM dal contenitore di archiviazione di Azure

```powershell
Restore-ADFS -StorageType "Azure" -AzureConnectionCredential $cred -DecryptionPassword "password" -AzureStorageContainer "adfsbackups"
```

### <a name="restore-the-ad-fs-configuration-without-the-dkm-from-the-file-system"></a>Ripristinare la configurazione del AD FS senza DKM dal file System
 
```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -DecryptionPassword "password"
```

### <a name="restore-the-ad-fs-configuration-with-the-dkm-to-the-file-system"></a>Ripristinare la configurazione di AD FS con DKM nel file System 
 
```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -DecryptionPassword "password" -RestoreDKM
```

### <a name="restore-the-ad-fs-configuration-to-wid"></a>Ripristinare la configurazione di AD FS in WID

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -DecryptionPassword "password" -DBConnectionString "WID"
``` 

### <a name="restore-the-ad-fs-configuration-to-sql"></a>Ripristinare la configurazione di AD FS in SQL

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -DecryptionPassword "password" -DBConnectionString "Data Source=TESTMACHINE\SQLEXPRESS; Integrated Security=True"
```

### <a name="restores-the-ad-fs-configuration-with-the-specified-gmsa"></a>Ripristina la configurazione di AD FS con il GMSA specificato

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -DecryptionPassword "password" -GroupServiceAccountIdentifier "mangupd1\adfsgmsa$"
```
### <a name="restore-the-ad-fs-configuration-with-the-specified-service-account-creds"></a>Ripristinare la configurazione di AD FS con l'account del servizio specificato credenziali

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -DecryptionPassword "password" -ServiceAccountCredential $cred
```

## <a name="encryption-information"></a>Informazioni sulla crittografia
Tutti i dati di backup viene crittografato prima di pubblicarlo nel cloud o archiviare i dati nel file system.  

Ogni documento che viene creato come parte del backup viene crittografato con AES-256. La password passate nello strumento viene utilizzata come passphrase per generare una nuova password utilizzando la classe Rfc2898DeriveBytes. 

RngCryptoServiceProvider viene utilizzato per generare il valore salt utilizzato da AES e la classe Rfc2898DeriveBytes. 

## <a name="log-files"></a>File di log
Ogni volta che viene eseguito un backup o un ripristino, viene creato un file di log. Si trovano nel percorso seguente:

- **%localappdata%\ADFSRapidRecreationTool**

>[!NOTE]
> Quando si esegue un ripristino, è possibile creare un file di PostRestore_Instructions contenente una panoramica dei provider di autenticazione aggiuntivi, gli archivi di attributi e i trust del provider di attestazioni locali da installare manualmente prima di avviare il servizio di AD FS.

## <a name="version-release-history"></a>Cronologia delle versioni

### <a name="version-10820"></a>Versione 1.0.82.0
Versione: 2019 luglio

**Problemi risolti:**
- Correzione di bug per i nomi di account di servizio AD FS che contengono caratteri di escape LDAP


### <a name="version-10810"></a>Versione: 1.0.81.0
Versione: aprile 2019

**Problemi risolti:**


- Correzioni di bug per il backup e il ripristino dei certificati
- Ulteriori informazioni di traccia per il file di log


### <a name="version-10750"></a>Versione: 1.0.75.0
Versione: agosto 2018

**Problemi risolti:**
* Aggiornare backup-ADFS quando si usa l'opzione-BackupDKM.  Lo strumento determinerà se il contesto corrente ha accesso al contenitore DKM.  In tal caso, non sarà necessario specificare i privilegi di amministratore di dominio o le credenziali dell'account del servizio.  In questo modo, i backup automatici vengono eseguiti senza fornire le credenziali in modo esplicito o come account di amministratore di dominio.

### <a name="version-10730"></a>Versione: 1.0.73.0
Versione: agosto 2018

**Problemi risolti:**
* Aggiornare gli algoritmi di crittografia in modo che l'applicazione sia conforme a FIPS
    
    >[!NOTE]
    > I backup precedenti non funzioneranno con la nuova versione a causa di modifiche apportate agli algoritmi di crittografia in base alla conformità FIPS
    
* Aggiunta del supporto per i cluster SQL che utilizzano la replica di tipo merge

### <a name="version-10720"></a>Versione: 1.0.72.0
Versione: 2018 luglio

**Problemi risolti:**

   - Correzione di bug: è stata corretta. Programma di installazione MSI per supportare gli aggiornamenti sul posto 

### <a name="10180"></a>1.0.18.0
Versione: 2018 luglio

**Problemi risolti:**

   - Correzione di bug: gestire le password degli account del servizio che contengono caratteri speciali (ad esempio,' &')
   - Correzione di bug: il ripristino non riesce perché Microsoft. IdentityServer. ServiceHost. exe. config è in uso da un altro processo


### <a name="1000"></a>1.0.0.0
Rilasciata: ottobre 2016

Versione iniziale di AD FS strumento di ripristino rapido
