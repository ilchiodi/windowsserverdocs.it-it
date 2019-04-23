---
title: Abilita backup Wbadmin
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c0e57f8a-70fa-4c60-9754-e762e8ad8772
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6fd0bea5da83ca9351d5ea1028c94392bdb40422
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845542"
---
# <a name="wbadmin-enable-backup"></a>Abilita backup Wbadmin



Crea e consente a una pianificazione di backup giornaliera o modifica una pianificazione di backup esistente. Senza parametri specificati, Visualizza le impostazioni di backup attualmente pianificate.

Per configurare o modificare una pianificazione di backup giornaliera, è necessario essere un membro di entrambi i **Administrators** o **al gruppo Backup Operators** gruppo. Inoltre, è necessario eseguire **wbadmin** da un prompt dei comandi con privilegi elevati. (Per aprire un prompt dei comandi con privilegi elevati di rapida **Command prompt** e quindi fare clic su **Esegui come amministratore**.)

Per esempi di come utilizzare questo sottocomando, vedere [esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

Sintassi per Windows Server 2008:
```
wbadmin enable backup
[-addtarget:<BackupTargetDisk>]
[-removetarget:<BackupTargetDisk>]
[-schedule:<TimeToRunBackup>]
[-include:<VolumesToInclude>]
[-allCritical]
[-quiet]
```
Sintassi per Windows Server 2008 R2:
```
wbadmin enable backup
[-addtarget:<BackupTarget>]
[-removetarget:<BackupTarget>]
[-schedule:<TimeToRunBackup>]
[-include:<VolumesToInclude>]
[-nonRecurseInclude:<ItemsToInclude>]
[-exclude:<ItemsToExclude>]
[-nonRecurseExclude:<ItemsToExclude>][-systemState]
[-allCritical]
[-vssFull | -vssCopy]
[-user:<UserName>]
[-password:<Password>]
[-quiet]
```
Sintassi per Windows Server 2012 e Windows Server 2012 R2:
```
wbadmin enable backup
[-addtarget:<BackupTarget>]
[-removetarget:<BackupTarget>]
[-schedule:<TimeToRunBackup>]
[-include:<VolumesToInclude>]
[-nonRecurseInclude:<ItemsToInclude>]
[-exclude:<ItemsToExclude>]
[-nonRecurseExclude:<ItemsToExclude>][-systemState]
[-hyperv:<HyperVComponentsToExclude>]
[-allCritical]
[-systemState] 
[-vssFull | -vssCopy]
[-user:<UserName>]
[-password:<Password>]
[-quiet] 
[-allowDeleteOldBackups]

```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|-addtarget|Per Windows Server 2008, specifica il percorso di archiviazione per i backup. È necessario specificare una destinazione per i backup come identificatore del disco (vedere la sezione Osservazioni). Il disco è formattato prima dell'uso e i dati esistenti su di esso vengono cancellati in modo permanente.</br>Per Windows Server 2008 R2 e versioni successive, specifica il percorso di archiviazione per i backup. È necessario specificare il percorso come un disco, volume o percorso Universal Naming Convention (UNC) in una cartella condivisa remota (\\\\\<servername >\<nomecondivisione >\). Per impostazione predefinita, il backup verrà salvato in: \\ \\ <servername> \<nomecondivisione > \WindowsImageBackup\<ComputerBackedUp >\. Se si specifica un disco, il disco verrà formattato prima dell'uso e i dati esistenti su di esso vengono cancellati in modo permanente. Se si specifica una cartella condivisa, è possibile aggiungere più posizioni. È possibile specificare solo una cartella condivisa come percorso di archiviazione alla volta.</br>Importante: Se si salva una copia di backup in una cartella condivisa remota, tale backup verrà sovrascritto se si usa la stessa cartella di backup anche in questo caso lo stesso computer. Inoltre, se l'operazione di backup non riesce, potrebbe avere alcun backup perché il backup precedente verrà sovrascritto, mentre il backup più recente non sarà utilizzabile. Per evitare questo problema creando sottocartelle nella cartella condivisa remota per organizzare i backup. In questo caso, le sottocartelle saranno necessario il doppio dello spazio della cartella padre.</br>Un solo percorso può essere specificato in un unico comando. È possibile aggiungere più posizioni di archiviazione di backup di volumi e dischi, eseguire nuovamente il comando.|
|-removetarget|Specifica il percorso di archiviazione che si desidera rimuovere dalla pianificazione di backup esistente. È necessario specificare il percorso come un identificatore del disco (vedere la sezione Osservazioni).|
|-schedule|Specifica ore del giorno per creare un backup, nel formato hh: mm e delimitato da virgole.|
|-includere|Per Windows Server 2008, specifica l'elenco delimitato da virgole di lettere di unità, punti di montaggio del volume o i nomi dei volumi basati su GUID da includere nel backup.</br>Per Windows Server 2008 R2and versioni successive, specifica l'elenco delimitato da virgole di elementi da includere nel backup. È possibile includere più file, cartelle o volumi. È possibile specificare i percorsi dei volumi utilizzando lettere di unità dei volumi, punti di montaggio dei volumi o nomi dei volumi basati su GUID. Se si usa un nome di volume basato su GUID, deve terminare con una barra rovesciata (\). È possibile usare il carattere jolly (*) nel nome del file quando si specifica un percorso di un file.|
|-nonRecurseInclude|Per Windows Server 2008 R2 e versioni successive, specifica di non ricorsiva, un elenco delimitato da virgole di elementi da includere nel backup. È possibile includere più file, cartelle o volumi. È possibile specificare i percorsi dei volumi utilizzando lettere di unità dei volumi, punti di montaggio dei volumi o nomi dei volumi basati su GUID. Se si usa un nome di volume basato su GUID, deve terminare con una barra rovesciata (\). È possibile usare il carattere jolly (*) nel nome del file quando si specifica un percorso di un file. Deve essere utilizzato solo quando viene usato il parametro - backupTarget.|
|-escludere|Per Windows Server 2008 R2 e versioni successive, specifica l'elenco delimitato da virgole di elementi da escludere dal backup. È possibile escludere i file, cartelle o volumi. È possibile specificare i percorsi dei volumi utilizzando lettere di unità dei volumi, punti di montaggio dei volumi o nomi dei volumi basati su GUID. Se si usa un nome di volume basato su GUID, deve terminare con una barra rovesciata (\). È possibile usare il carattere jolly (*) nel nome del file quando si specifica un percorso di un file.|
|-nonRecurseExclude|Per Windows Server 2008 R2 e versioni successive, specifica di non ricorsiva, un elenco delimitato da virgole di elementi da escludere dal backup. È possibile escludere i file, cartelle o volumi. È possibile specificare i percorsi dei volumi utilizzando lettere di unità dei volumi, punti di montaggio dei volumi o nomi dei volumi basati su GUID. Se si usa un nome di volume basato su GUID, deve terminare con una barra rovesciata (\). È possibile usare il carattere jolly (*) nel nome del file quando si specifica un percorso di un file.|
|-hyperv|Specifica l'elenco delimitato da virgole dei componenti da includere nel backup. L'identificatore potrebbe essere un nome del componente o un componente GUID (con o senza parentesi graffe).|
|-systemState|Per Windows 7 e Windows Server 2008 R2 e versioni successive, viene creato un backup che include lo stato del sistema oltre a qualsiasi altro elemento specificato con il **-include** parametro. Lo stato del sistema contiene i file di avvio (Boot. ini, NDTLDR, NTDetect.com), il Registro di sistema di Windows comprese le impostazioni di COM, la cartella SYSVOL (criteri di gruppo e script di accesso), Active Directory e NTDS. DIT sui controller di dominio e, se è installato il servizio certificati, il certificato Store. Se il server è installato il ruolo di server Web, IIS Metadirectory sarà inclusa. Se il server fa parte di un cluster, informazioni sul servizio Cluster è anche inclusi.|
|-allCritical|Specifica che tutti i volumi critici (volumi che contengono lo stato del sistema operativo) vengono inclusi nei backup. Questo parametro è utile se si sta creando un backup completo del sistema o di ripristino dello stato del sistema. Deve essere utilizzato solo quando viene specificato - backupTarget, in caso contrario, il comando avrà esito negativo. Può essere utilizzato con il **-includono** (opzione).</br>Suggerimento: Il volume di destinazione per il backup del volume critico può essere un'unità locale, ma non può essere uno dei volumi inclusi nel backup.|
|-vssFull|Per Windows Server 2008 R2 e versioni successive, esegue una procedura completa di eseguire il backup usando il servizio Copia Shadow del Volume (VSS). Tutti i file di backup, la cronologia di ogni file viene aggiornata per indicare che è stato eseguito il backup e i registri di backup precedente possono essere troncati. Se questo parametro non viene utilizzato wbadmin start backup esegue una copia backup, ma la cronologia dei file in corso il backup non viene aggiornata.</br>Attenzione: Non utilizzare questo parametro se si usa un prodotto diverso da Windows Server Backup per eseguire il backup di applicazioni che si trovano i volumi inclusi nel backup corrente. Tale operazione così può provocare problemi di incrementali, differenziali o altro tipo di backup che crea l'altro prodotto di backup perché la cronologia che sono affidarsi a determinare la quantità di dati di backup potrebbe essere mancante ed è possibile eseguire una procedura completa di backup inutilmente.|
|-vssCopy|Per Windows Server 2008 R2 e versioni successive, esegue un copia backup tramite VSS. Tutti i file vengono sottoposti a backup, ma la cronologia dei file da eseguire il backup non viene aggiornata in modo da conservare tutte le informazioni su quali file modificati, eliminati e così via, nonché qualsiasi file di log dell'applicazione. Con questo tipo di backup non influenza la sequenza di backup incrementale e backup differenziali che possono verificarsi indipendente da questo backup di copia. Rappresenta il valore predefinito.</br>Avviso: Un copia backup non è utilizzabile per il backup incrementale o differenziale o Ripristina.|
|-utente|Per Windows Server 2008 R2 e versioni successive, specifica l'utente con autorizzazione di scrittura nella destinazione di archiviazione di backup (se si tratta di una cartella condivisa remota). L'utente deve essere un membro del gruppo Administrators o Backup Operators nel computer in cui è stato eseguito il backup.|
|-password|Per Windows Server 2008 R2 e versioni successive, specifica la password per il nome utente fornito dal parametro-utente.|
|-quiet|Esegue il sottocomando senza alcuna richiesta visualizzata all'utente.|
|-allowDeleteOldBackups|Sovrascrive qualsiasi backup eseguiti prima che il computer è stato aggiornato.|

## <a name="remarks"></a>Note

Per visualizzare il valore dell'identificatore del disco per i dischi, digitare **wbadmin ottenere dischi**.

## <a name="BKMK_examples"></a>Esempi

Gli esempi seguenti illustrano come il **abilitare il backup wbadmin** comando possa essere usato in diversi scenari di backup:

Scenario di #1
-   Pianificare i backup dei dischi rigidi e:, d:\mountpoint, e \\ \\? \Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
-   Salvare i file sul disco DiskID
-   Esegui backup ogni giorno alle ore 9:00 e le 18.00
```
wbadmin enable backup -addtarget:DiskID -schedule:09:00,18:00 -include:e:,d:\mountpoint,\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
```
Scenario 2 #
-   Pianificare i backup di d:\documents la cartella nel percorso di rete \\ \\backupshare\backup1
-   Usare le credenziali di rete per l'amministratore di backup Aaren Ekelund (aekel), che è un membro del dominio CONTOSOEAST per autenticare l'accesso alla condivisione di rete. Password del Aaren *hM 3 dollari 9 ^ 5lp*.
-   Esegui backup ogni giorno alle 12:00. e le 21.00
```
wbadmin enable backup –addtarget:\\backupshare\backup1 –include: d:\documents –user:CONTOSOEAST\aekel –password:$3hM9^5lp –schedule:00:00,19:00
```
Scenario 3 #
-   Pianificare i backup del volume d:\documents t: e la cartella per l'unità h, ma ne esclude le d:\documents cartella\~tmp
-   Eseguire un backup completo usando il servizio Copia Shadow del Volume.
-   Esegui backup ogni giorno all'1:00 AM.
```
wbadmin enable backup –addtarget:H: –include T:,D:\documents –exclude D:\documents\~tmp –vssfull –schedule:01:00
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)