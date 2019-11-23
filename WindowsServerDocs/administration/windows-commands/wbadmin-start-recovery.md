---
title: wbadmin Avvia ripristino
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 52381316-a0fa-459f-b6a6-01e31fb21612
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: edb287573dc76619502faf58018f48c464140629
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362355"
---
# <a name="wbadmin-start-recovery"></a>wbadmin Avvia ripristino



Esegue un'operazione di ripristino in base ai parametri specificati.

Per eseguire un ripristino con il sottocomando, è necessario essere un membro del **Backup Operators** gruppo o **amministratori** gruppo, oppure è necessario che siano state delegate le autorizzazioni appropriate. Inoltre, è necessario eseguire **wbadmin** da un prompt dei comandi con privilegi elevati. (Per aprire un prompt dei comandi con privilegi elevati, fare clic su **Avviare**, fare doppio clic su **prompt dei comandi**, quindi fare clic su **Esegui come amministratore**.)

Per esempi di come utilizzare questo sottocomando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
wbadmin start recovery
-version:<VersionIdentifier>
-items:{<VolumesToRecover> | <AppsToRecover> | <FilesOrFoldersToRecover>}
-itemtype:{Volume | App | File}
[-backupTarget:{<VolumeHostingBackup> | <NetworkShareHostingBackup>}]
[-machine:<BackupMachineName>]
[-recoveryTarget:{<TargetVolumeForRecovery> | <TargetPathForRecovery>}]
[-recursive]
[-overwrite:{Overwrite | CreateCopy | Skip}]
[-notRestoreAcl]
[-skipBadClusterCheck]
[-noRollForward]
[-quiet]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|-versione|Specifica l'identificatore di versione del backup da ripristinare in MM/GG/AAAA-formato hh: mm. Se non si conosce l'identificatore di versione, digitare **wbadmin ottenere versioni**.|
|-articoli|Specifica un elenco delimitato da virgole di volumi, applicazioni, file o cartelle per ripristinare.</br>-Se **- itemtype** è **Volume**, è possibile specificare solo un singolo volume, fornendo la lettera di unità, il punto di montaggio del volume o il nome basato su GUID del volume.</br>-Se **- itemtype** è **App**, è possibile specificare una sola applicazione. Per essere ripristinato, l'applicazione deve essere registrato con Windows Server Backup. È inoltre possibile utilizzare il valore **ADIFM** per ripristinare un'installazione di Active Directory. Per ulteriori informazioni, vedere la sezione Note.</br>-Se **- itemtype** è **File**, è possibile specificare file o cartelle, ma dovrebbero far parte dello stesso volume e dovrebbero essere nella stessa cartella padre.|
|-itemtype|Specifica tipo di elementi da ripristinare. Deve essere **Volume**, **App**, o **File**.|
|-backupTarget|Specifica il percorso di archiviazione che contiene il backup che si desidera ripristinare. Questo parametro è utile quando la posizione è diversa da in cui sono in genere archiviati i backup del computer.|
|-machine|Specifica il nome del computer in cui si desidera ripristinare il processo di backup. Questo parametro è utile quando più computer sottoposte a backup nello stesso percorso. Deve essere utilizzato quando il **- backupTarget** parametro specificato.|
|-recoveryTarget|Specifica il percorso da ripristinare. Questo parametro è utile se questo percorso è diverso da quella che è stato eseguito il backup. Può inoltre essere utilizzato per operazioni di ripristino di volumi, file o applicazioni. Se si desidera ripristinare un volume, è possibile specificare la lettera di unità del volume del volume alternativo. Se si desidera ripristinare un file o un'applicazione, è possibile specificare un percorso di ripristino alternativo.|
|-recursive|Valido solo quando il ripristino dei file. Ripristina i file nelle cartelle e tutti i file subordinati nelle cartelle specificate. Per impostazione predefinita, vengono recuperati solo i file che si trovano direttamente nelle cartelle specificate.|
|-sovrascrivere|Valido solo quando il ripristino dei file. Specifica l'azione da intraprendere quando esiste un file che viene recuperato già nello stesso percorso.</br>-   **Ignora** causa Windows Server backup ignorare il file esistente e continuare con il ripristino del file successivo.</br>-   **CreateCopy** fa in modo che Windows Server Backup crei una copia del file esistente in modo che il file esistente non venga modificato.</br>-   **sovrascrivere** causa Windows Server Backup sovrascrivere il file esistente con il file dal backup.|
|-notRestoreAcl|Valido solo quando il ripristino dei file. Specifica da non ripristinare gli elenchi di controllo di accesso (ACL) della protezione dei file in fase di ripristino dal backup. Per impostazione predefinita, vengono ripristinati gli ACL di protezione (il valore predefinito è **true)** . Se questo parametro viene utilizzato, gli elenchi ACL per i file ripristinati verranno ereditate dalla posizione in cui vengono ripristinati i file.|
|-skipBadClusterCheck|Valido solo durante il ripristino di volumi. Ignora il controllo dei dischi che si esegue il ripristino per informazioni sul cluster non valida. Se si ripristina un server alternativo o hardware, si consiglia di non utilizzare questo parametro. È possibile eseguire manualmente il comando **/b chkdsk** su tali dischi in qualsiasi momento per verificare la presenza di cluster danneggiati e aggiornare di conseguenza le informazioni sul file system.</br>Importante: fino a quando non si esegue **chkdsk** come descritto, i cluster danneggiati segnalati nel sistema ripristinato potrebbero non essere accurati.|
|-noRollForward|Valido solo durante il ripristino di applicazioni. Consente di ripristino temporizzato nel precedente di un'applicazione se è selezionata la versione più recente dal backup. Per altre versioni dell'applicazione che non sono il ripristino temporizzato nel precedente più recente, viene eseguito come il valore predefinito.|
|-quiet|Esegue il sottocomando senza alcuna richiesta visualizzata all'utente.|

## <a name="remarks"></a>Osservazioni

-   Per visualizzare un elenco di elementi che sono disponibili per il ripristino da una versione di backup specifico, utilizzare **wbadmin ottenere elementi**. Se un volume non è una lettera di unità o punto di montaggio al momento del backup, il sottocomando restituirà un nome di volume basato su GUID da utilizzare per il ripristino del volume.
-   Quando il **- itemtype** è **App**, è possibile utilizzare un valore di **ADIFM** per **-elemento** per eseguire un'installazione da supporto operazione per recuperare tutti i dati correlati necessari per servizi di dominio Active Directory. **ADIFM** Crea una copia del database di Active Directory, Registro di sistema e lo stato SYSVOL e quindi Salva queste informazioni nella posizione specificata da **- recoveryTarget**. Utilizzare questo parametro solo quando **- recoveryTarget** specificato.

>     [!NOTE]
>     Before using **wbadmin** to perform an install from media operation, you should consider using the **ntdsutil** command because **ntdsutil** only copies the minimum amount of data needed, and it uses a more secure data transport method.

## <a name="BKMK_Examples"></a>Esempi

Per eseguire un ripristino del backup da 31 marzo 2013, eseguito alle 9:00, del volume d:, digitare:
```
wbadmin start recovery -version:03/31/2013-09:00 -itemType:Volume -items:d:
```
Per eseguire un ripristino di unità d del backup da 31 marzo 2013, eseguito alle 9:00, del Registro di sistema, digitare:
```
wbadmin start recovery -version:03/31/2013-09:00 -itemType:App -items:Registry -recoverytarget:d:\
```
Per eseguire un ripristino del backup da 31 marzo 2013, eseguito alle 9:00, i d:\folder e le cartelle subordinate a d:\folder, digitare:
```
wbadmin start recovery -version:03/31/2013-09:00 -itemType:File -items:d:\folder -recursive
```
Per eseguire un ripristino del backup dal 31 marzo 2013, eseguito alle ore 9:00, del volume \\\\? \Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\, tipo:
```
wbadmin start recovery -version:03/31/2013-09:00 -itemType:Volume 
-items:\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
```
Per eseguire un ripristino del backup dal 30 aprile 2013, eseguito alle ore 9:00, della cartella condivisa \\\\servername\share da Server01, digitare:
```
wbadmin start recovery -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Inizio WBFileRecovery](https://technet.microsoft.com/library/jj902457.aspx) cmdlet
-   [Inizio WBHyperVRecovery](https://technet.microsoft.com/library/jj902463.aspx) cmdlet
-   [Inizio WBSystemStateRecovery](https://technet.microsoft.com/library/jj902449.aspx) cmdlet
-   [Inizio WBVolumeRecovery](https://technet.microsoft.com/library/jj902470.aspx) cmdlet