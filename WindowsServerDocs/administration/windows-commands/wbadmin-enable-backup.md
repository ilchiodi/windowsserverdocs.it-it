---
title: wbadmin Abilita backup
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: d30136f7eb3ea48b8d9b0a740eb5a77e981ae3f0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362385"
---
# <a name="wbadmin-enable-backup"></a>wbadmin Abilita backup



Crea e Abilita una pianificazione di backup giornaliera o modifica una pianificazione di backup esistente. Se non è specificato alcun parametro, vengono visualizzate le impostazioni di backup attualmente pianificate.

Per configurare o modificare una pianificazione di backup giornaliera, è necessario essere un membro del gruppo **Administrators** o **Backup Operators** . Inoltre, è necessario eseguire **wbadmin** da un prompt dei comandi con privilegi elevati. (Per aprire un prompt dei comandi con privilegi elevati di rapida **Command prompt** e quindi fare clic su **Esegui come amministratore**.)

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
|-AddTarget|Per Windows Server 2008, specifica il percorso di archiviazione per i backup. È necessario specificare una destinazione per i backup come identificatore del disco (vedere la sezione Osservazioni). Il disco viene formattato prima dell'utilizzo e tutti i dati esistenti su di esso vengono eliminati definitivamente.</br>Per Windows Server 2008 R2 e versioni successive, specifica il percorso di archiviazione per i backup. È necessario specificare la posizione come percorso disco, volume o Universal Naming Convention (UNC) di una cartella condivisa\\remota (\\\<ServerName >\<ShareName >\). Per impostazione predefinita, il backup verrà salvato in: \\ \< \\ <servername>ShareName > \WindowsImageBackup\<ComputerBackedUp >\. Se si specifica un disco, il disco verrà formattato prima dell'utilizzo e tutti i dati esistenti su di esso verranno eliminati definitivamente. Se si specifica una cartella condivisa, non è possibile aggiungere altre posizioni. È possibile specificare una sola cartella condivisa come percorso di archiviazione alla volta.</br>Importante: Se si salva un backup in una cartella condivisa remota, il backup verrà sovrascritto se si utilizza la stessa cartella per eseguire nuovamente il backup dello stesso computer. Inoltre, se l'operazione di backup non riesce, potrebbe avere alcun backup perché il backup precedente verrà sovrascritto, mentre il backup più recente non sarà utilizzabile. Per evitare questo problema creando sottocartelle nella cartella condivisa remota per organizzare i backup. In tal caso, le sottocartelle dovranno avere due volte lo spazio della cartella padre.</br>È possibile specificare una sola posizione in un unico comando. È possibile aggiungere più percorsi di archiviazione di backup di volumi e dischi eseguendo di nuovo il comando.|
|-removetarget|Specifica il percorso di archiviazione che si desidera rimuovere dalla pianificazione del backup esistente. È necessario specificare il percorso come identificatore del disco (vedere la sezione Osservazioni).|
|-pianificazione|Specifica le ore del giorno per la creazione di un backup, formattato in formato HH: MM e delimitato da virgole.|
|-includere|Per Windows Server 2008, specifica l'elenco delimitato da virgole di lettere di unità del volume, punti di montaggio del volume o nomi di volumi basati su GUID da includere nel backup.</br>Per Windows Server 2008 R2and in un secondo momento, specifica l'elenco delimitato da virgole di elementi da includere nel backup. È possibile includere più file, cartelle o volumi. È possibile specificare i percorsi dei volumi utilizzando lettere di unità dei volumi, punti di montaggio dei volumi o nomi dei volumi basati su GUID. Se si usa un nome di volume basato su GUID, deve terminare con una barra rovesciata\)(. Quando si specifica un percorso di un file, è possibile utilizzare il carattere jolly (*) nel nome del file.|
|-nonRecurseInclude|Per Windows Server 2008 R2 e versioni successive, specifica l'elenco di elementi non ricorsivi, delimitati da virgole da includere nel backup. È possibile includere più file, cartelle o volumi. È possibile specificare i percorsi dei volumi utilizzando lettere di unità dei volumi, punti di montaggio dei volumi o nomi dei volumi basati su GUID. Se si usa un nome di volume basato su GUID, deve terminare con una barra rovesciata\)(. Quando si specifica un percorso di un file, è possibile utilizzare il carattere jolly (*) nel nome del file. Deve essere usato solo quando viene usato il parametro-backupTarget.|
|-escludere|Per Windows Server 2008 R2 e versioni successive, specifica l'elenco delimitato da virgole di elementi da escludere dal backup. È possibile escludere i file, cartelle o volumi. È possibile specificare i percorsi dei volumi utilizzando lettere di unità dei volumi, punti di montaggio dei volumi o nomi dei volumi basati su GUID. Se si usa un nome di volume basato su GUID, deve terminare con una barra rovesciata\)(. Quando si specifica un percorso di un file, è possibile utilizzare il carattere jolly (*) nel nome del file.|
|-nonRecurseExclude|Per Windows Server 2008 R2 e versioni successive, specifica l'elenco di elementi non ricorsivi, delimitati da virgole da escludere dal backup. È possibile escludere i file, cartelle o volumi. È possibile specificare i percorsi dei volumi utilizzando lettere di unità dei volumi, punti di montaggio dei volumi o nomi dei volumi basati su GUID. Se si usa un nome di volume basato su GUID, deve terminare con una barra rovesciata\)(. Quando si specifica un percorso di un file, è possibile utilizzare il carattere jolly (*) nel nome del file.|
|-HyperV|Specifica l'elenco delimitato da virgole dei componenti da includere nel backup. L'identificatore può essere un nome di componente o un GUID componente (con o senza parentesi graffe).|
|-systemState|Per Windows 7 e Windows Server 2008 R2 e versioni successive, in viene creato un backup che include lo stato del sistema, oltre a qualsiasi altro elemento specificato con il parametro **-include** . Lo stato del sistema contiene i file di avvio (boot. ini, NDTLDR, NTDetect.com), il registro di sistema di Windows, incluse le impostazioni COM, SYSVOL (criteri di gruppo e script di accesso), il Active Directory e NTDS. DIT nei controller di dominio e, se il servizio certificati è installato, l'archivio certificati. Se il server è installato il ruolo di server Web, IIS Metadirectory sarà inclusa. Se il server fa parte di un cluster, vengono incluse anche Servizio cluster informazioni.|
|-allCritical|Specifica che tutti i volumi critici (volumi che contengono lo stato del sistema operativo) vengono inclusi nei backup. Questo parametro è utile se si sta creando un backup per il ripristino completo del sistema o dello stato del sistema. Deve essere usato solo quando si specifica-backupTarget; in caso contrario, il comando avrà esito negativo. Può essere utilizzato con il **-includono** (opzione).</br>Suggerimento: Il volume di destinazione per un backup del volume critico può essere un'unità locale, ma non può essere uno dei volumi inclusi nel backup.|
|-vssFull|Per Windows Server 2008 R2 e versioni successive, esegue un backup completo utilizzando il Servizio Copia Shadow del volume (VSS). Tutti i file di backup, la cronologia di ogni file viene aggiornata per indicare che è stato eseguito il backup e i registri di backup precedente possono essere troncati. Se questo parametro non viene utilizzato, Wbadmin start backup esegue un backup di copia, ma la cronologia dei file di cui viene eseguito il backup non viene aggiornata.</br>Attenzione: Non utilizzare questo parametro se si utilizza un prodotto diverso da Windows Server Backup per eseguire il backup di applicazioni che si trovano sui volumi inclusi nel backup corrente. In questo modo, è possibile che si interrompa la creazione di backup incrementali, differenziali o di altro tipo, perché la cronologia su cui si basano per determinare la quantità di dati di cui eseguire il backup potrebbe non essere presente e potrebbe eseguire un backup completo inutilmente.|
|-vssCopy|Per Windows Server 2008 R2 e versioni successive, esegue un backup di copia con VSS. Tutti i file vengono sottoposti a backup, ma la cronologia dei file da eseguire il backup non viene aggiornata in modo da conservare tutte le informazioni su quali file modificati, eliminati e così via, nonché qualsiasi file di log dell'applicazione. L'utilizzo di questo tipo di backup non influisce sulla sequenza dei backup incrementali e differenziali che potrebbero verificarsi indipendentemente dal backup di copia. Rappresenta il valore predefinito.</br>Avviso: Non è possibile usare un backup di copia per i backup incrementali o differenziali o i ripristini.|
|-utente|Per Windows Server 2008 R2 e versioni successive, specifica l'utente con autorizzazione di scrittura per la destinazione di archiviazione di backup (se si tratta di una cartella condivisa remota). L'utente deve essere un membro del gruppo Administrators o del gruppo Backup Operators nel computer di cui viene eseguito il backup.|
|-password|Per Windows Server 2008 R2 e versioni successive, specifica la password per il nome utente fornito dal parametro-User.|
|-quiet|Esegue il sottocomando senza alcuna richiesta visualizzata all'utente.|
|-allowDeleteOldBackups|Sovrascrive tutti i backup eseguiti prima dell'aggiornamento del computer.|

## <a name="remarks"></a>Note

Per visualizzare il valore dell'identificatore del disco per i dischi, digitare **Wbadmin get disks**.

## <a name="BKMK_examples"></a>Esempi

Negli esempi seguenti viene illustrato come è possibile utilizzare il comando **Wbadmin enable backup** in diversi scenari di backup:

Scenario #1
- Pianificare i backup delle unità disco rigido e:, d:\MountPoint e \\ \\? \Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
- Salva i file sul DiskID del disco
- Esegui backup ogni giorno alle ore 9:00 e 6:00
  ```
  wbadmin enable backup -addtarget:DiskID -schedule:09:00,18:00 -include:e:,d:\mountpoint,\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
  ```
  Scenario 2 #
- Pianificare i backup della cartella d:\Documents nel percorso \\ \\di rete backupshare\backup1
- Usare le credenziali di rete per l'amministratore di backup Aarn Ekelund (aekel), che è membro del dominio CONTOSOEAST per autenticare l'accesso alla condivisione di rete. La password di Aar è *$3hM9 ^ 5LP*.
- Esegui backup ogni giorno alle ore 12:00 e 7:00
  ```
  wbadmin enable backup –addtarget:\\backupshare\backup1 –include: d:\documents –user:CONTOSOEAST\aekel –password:$3hM9^5lp –schedule:00:00,19:00
  ```
  Scenario 3 #
- Pianificare i backup del volume t: e della cartella d:\Documents sull'unità h:, escludendo la cartella d:\Documents\~tmp
- Eseguire un backup completo usando il Servizio Copia Shadow del volume.
- Esegui backup ogni giorno alle ore 1:00
  ```
  wbadmin enable backup –addtarget:H: –include T:,D:\documents –exclude D:\documents\~tmp –vssfull –schedule:01:00
  ```

#### <a name="additional-references"></a>Altri riferimenti

-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)