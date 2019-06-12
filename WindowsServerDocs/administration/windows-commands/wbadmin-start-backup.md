---
title: Comando Wbadmin start backup
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 56f3e752-d99a-4c3d-8e97-10303c37dd78
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2ac602506960b92333750e7a37692c44c92aae22
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440271"
---
# <a name="wbadmin-start-backup"></a>Comando Wbadmin start backup



Crea un backup utilizzando i parametri specificati. Se viene specificato alcun parametro e aver creato un backup giornaliero pianificato, il sottocomando crea il backup utilizzando le impostazioni per il backup pianificato. Se vengono specificati parametri, viene creato un backup di copia del servizio Copia Shadow del Volume (VSS) e non aggiornerà la cronologia dei file che vengono sottoposti a backup.

Per creare un backup unico con questo sottocomando, è necessario essere un membro del **gruppo Backup Operators** gruppo o **amministratori** gruppo, oppure è necessario che siano state delegate le autorizzazioni appropriate. Inoltre, è necessario eseguire **wbadmin** da un prompt dei comandi con privilegi elevati. (Per aprire un prompt dei comandi con privilegi elevati di rapida **Command prompt** e quindi fare clic su **Esegui come amministratore**.)

Per esempi di come utilizzare questo sottocomando, vedere [esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

Sintassi per Windows ° Vista e Windows Server 2008:
```
wbadmin start backup
[-backupTarget:{<BackupTargetLocation> | <TargetNetworkShare>}]
[-include:<VolumesToInclude>]
[-allCritical]
[-noVerify]
[-user:<UserName>]
[-password:<Password>]
[-noinheritAcl]
[-vssFull]
[-quiet]
```
Sintassi per Windows 7 e Windows Server 2008 R2 e versioni successive:
```
Wbadmin start backup
[-backupTarget:{<BackupTargetLocation> | <TargetNetworkShare>}]
[-include:<ItemsToInclude>]
[-nonRecurseInclude:<ItemsToInclude>]
[-exclude:<ItemsToExclude>]
[-nonRecurseExclude:<ItemsToExclude>]
[-allCritical]
[-systemState]
[-noVerify]
[-user:<UserName>]
[-password:<Password>]
[-noInheritAcl]
[-vssFull | -vssCopy] 
[-quiet]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|-backupTarget|Specifica il percorso di archiviazione per il backup. Richiede una lettera di unità disco rigido (f), un percorso basato su GUID volume nel formato \\ \\? \Volume{GUID}}, o un percorso Universal Naming Convention (UNC) in una cartella condivisa remota (\\\\\<nomeserver > \<nomecondivisione >\). Per impostazione predefinita, il backup verrà salvato in: \\ \\ <servername> \<nomecondivisione >\** WindowsImageBackup * *\\<ComputerBackedUp>\.</br>Importante: Se si salva una copia di backup in una cartella condivisa remota, tale backup verrà sovrascritto se si usa la stessa cartella di backup anche in questo caso lo stesso computer. Inoltre, se l'operazione di backup non riesce, potrebbe avere alcun backup perché il backup precedente verrà sovrascritto, mentre il backup più recente non sarà utilizzabile. Per evitare questo problema creando sottocartelle nella cartella condivisa remota per organizzare i backup. In questo caso, le sottocartelle saranno necessario il doppio dello spazio della cartella padre.|
|-includere|Per Windows ° Vista e Windows Server 2008, specifica l'elenco delimitato da virgole di lettere di unità, punti di montaggio del volume o i nomi dei volumi basati su GUID da includere nel backup. Questo parametro deve essere utilizzato solo quando il **- backupTarget** parametro viene utilizzato.</br>Per Windows 7 e Windows Server 2008 R2 e versioni successive, specifica l'elenco delimitato da virgole di elementi da includere nel backup. È possibile includere più file, cartelle o volumi. È possibile specificare i percorsi dei volumi utilizzando lettere di unità dei volumi, punti di montaggio dei volumi o nomi dei volumi basati su GUID. Se si utilizza un nome di volume basato su GUID, deve terminare con una barra rovesciata (\\). È possibile utilizzare il carattere jolly (\*) nel nome del file quando si specifica un percorso di un file. Deve essere utilizzato solo quando il **- backupTarget** parametro viene utilizzato.|
|-escludere|Per Windows 7 e Windows Server 2008 R2 e versioni successive, specifica l'elenco delimitato da virgole di elementi da escludere dal backup. È possibile escludere i file, cartelle o volumi. È possibile specificare i percorsi dei volumi utilizzando lettere di unità dei volumi, punti di montaggio dei volumi o nomi dei volumi basati su GUID. Se si utilizza un nome di volume basato su GUID, deve terminare con una barra rovesciata (\\). È possibile utilizzare il carattere jolly (\*) nel nome del file quando si specifica un percorso di un file. Deve essere utilizzato solo quando il **- backupTarget** parametro viene utilizzato.|
|-nonRecurseInclude|Per Windows 7 e Windows Server 2008 R2 e versioni successive, specifica di non ricorsiva, un elenco delimitato da virgole di elementi da includere nel backup. È possibile includere più file, cartelle o volumi. È possibile specificare i percorsi dei volumi utilizzando lettere di unità dei volumi, punti di montaggio dei volumi o nomi dei volumi basati su GUID. Se si utilizza un nome di volume basato su GUID, deve terminare con una barra rovesciata (\\). È possibile utilizzare il carattere jolly (\*) nel nome del file quando si specifica un percorso di un file. Deve essere utilizzato solo quando il **- backupTarget** parametro viene utilizzato.|
|-nonRecurseExclude|Per Windows 7 e Windows Server 2008 R2 e versioni successive, specifica di non ricorsiva, un elenco delimitato da virgole di elementi da escludere dal backup. È possibile escludere i file, cartelle o volumi. È possibile specificare i percorsi dei volumi utilizzando lettere di unità dei volumi, punti di montaggio dei volumi o nomi dei volumi basati su GUID. Se si utilizza un nome di volume basato su GUID, deve terminare con una barra rovesciata (\\). È possibile utilizzare il carattere jolly (\*) nel nome del file quando si specifica un percorso di un file. Deve essere utilizzato solo quando il **- backupTarget** parametro viene utilizzato.|
|-allCritical|Specifica che tutti i volumi critici (volumi che contengono lo stato del sistema operativo) vengono inclusi nei backup. Questo parametro è utile se si sta creando una copia di backup per ripristino bare metal. Deve essere utilizzato solo quando **- backupTarget** è specificato, in caso contrario il comando avrà esito negativo. Può essere utilizzato con il **-includono** (opzione).</br>Suggerimento: Il volume di destinazione per il backup del volume critico può essere un'unità locale, ma non può essere uno dei volumi inclusi nel backup.|
|-systemState|Per Windows 7 e Windows Server 2008 R2 e versioni successive, viene creato un backup che include lo stato del sistema oltre a qualsiasi altro elemento specificato con il **-include** parametro. Lo stato del sistema contiene i file di avvio (Boot. ini, NDTLDR, NTDetect.com), il Registro di sistema di Windows comprese le impostazioni di COM, la cartella SYSVOL (criteri di gruppo e script di accesso), Active Directory e NTDS. DIT sui controller di dominio e, se è installato il servizio certificati, il certificato nell'archivio. Se il server è installato il ruolo di server Web, IIS Metadirectory sarà inclusa. Se il server è parte di un cluster, informazioni sul servizio Cluster verranno inoltre incluse.|
|-noVerify|Specifica che i backup salvati su supporti rimovibili (ad esempio un DVD) non vengono verificati gli errori. Se non si utilizza questo parametro, salvati su supporti rimovibili vengono verificati gli errori.|
|-utente|Se il backup viene salvato in una cartella condivisa remota, specifica il nome utente con autorizzazione di scrittura nella cartella.|
|-password|Specifica la password per il nome utente fornito dal parametro **-utente**.|
|-noInheritAcl|Applica le autorizzazioni elenco (ACL) di controllo di accesso che corrispondono alle credenziali specificate per il **-utente** e **-password** parametri \\ \\ \< NomeServer >\<nomecondivisione > \WindowsImageBackup\<ComputerBackedUp > \ (la cartella che contiene il backup). Per accedere ai backup in un secondo momento, è necessario utilizzare le credenziali o essere un membro del gruppo Administrators o del gruppo Backup Operators nel computer con la cartella condivisa. Se **- noInheritAcl** non è usata, vengono applicate le autorizzazioni ACL dalla cartella condivisa remota di \<ComputerBackedUp > cartella per impostazione predefinita, in modo che chiunque abbia accesso alla cartella condivisa remota può accedere il backup.|
|-vssFull|Esegue un backup completo mediante il servizio Copia Shadow del Volume (VSS). Tutti i file di backup, la cronologia di ogni file viene aggiornata per indicare che è stato eseguito il backup e i registri di backup precedente possono essere troncati. Se questo parametro non viene utilizzato **wbadmin start backup** consente a un backup di copia, ma la cronologia dei file non viene eseguito il backup viene aggiornato.</br>Attenzione: Non utilizzare questo parametro se si usa un prodotto diverso da Windows Server Backup per eseguire il backup di applicazioni che si trovano i volumi inclusi nel backup corrente. Tale operazione così può provocare problemi di incrementali, differenziali o altro tipo di backup che crea l'altro prodotto di backup perché la cronologia che sono affidarsi a determinare la quantità di dati di backup potrebbe essere mancante ed è possibile eseguire una procedura completa di backup inutilmente.|
|-vssCopy|Per Windows 7 e Windows Server 2008 R2 e versioni successive, esegue un copia backup tramite VSS. Tutti i file vengono sottoposti a backup, ma la cronologia dei file da eseguire il backup non viene aggiornata in modo da conservare tutte le informazioni su quali file modificati, eliminati e così via, nonché qualsiasi file di log dell'applicazione. Con questo tipo di backup non influenza la sequenza di backup incrementale e backup differenziali che possono verificarsi indipendente da questo backup di copia. Rappresenta il valore predefinito.</br>Avviso: Un copia backup non è utilizzabile per il backup incrementale o differenziale o Ripristina.|
|-quiet|Esegue il sottocomando senza alcuna richiesta visualizzata all'utente.|

## <a name="BKMK_examples"></a>Esempi

Gli esempi seguenti illustrano come il **wbadmin start backup** comando possa essere usato in diversi scenari di backup:

Scenario di #1
- Creare un backup dei volumi e:, d:\mountpoint, e \\ \\? \Volume{cc566d14-4410-11d9-9d93-806e6f6e6963}
- Salvare il backup del volume f:
  ```
  wbadmin start backup -backupTarget:f: -include:e:,d:\mountpoint,\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
  ```
  Scenario 2 #
- Eseguire un backup unico di *f:\folder1* e *h:\folder2* volume *unità d:* .
- Eseguire il backup dello stato del sistema
- Eseguire il backup di copia in modo che il backup differenziale in genere pianificato non è interessato.
  ```
  wbadmin start backup –backupTarget:d: -include:g\folder1,h:\folder2 –systemstate -vsscopy
  ```
  Scenario 3 #
- Eseguire un backup unico di *d:\folder1* che deve eseguire il backup non in modo ricorsivo.
- Il backup della cartella nel percorso di rete  *\\ \\backupshare\backup1*
- Limitare l'accesso per il backup per i membri del **amministratori** o **gruppo Backup Operators** gruppo.
  ```
  wbadmin start backup –backupTarget: \\backupshare\backup1 -noinheritacl -nonrecurseinclude:d:\folder1
  ```

#### <a name="additional-references"></a>Altri riferimenti

-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
