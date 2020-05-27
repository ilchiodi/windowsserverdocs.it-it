---
title: comando Wbadmin start sysrecovery
description: Argomento di riferimento per Wbadmin start sysrecovery, che esegue un ripristino di sistema (ripristino bare metal) utilizzando i parametri specificati.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 95b8232f-7c42-452b-838e-15b0cf6faebe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a7c32818461c46a7d582029ec127c574d6a8f3e3
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83819111"
---
# <a name="wbadmin-start-sysrecovery"></a>comando Wbadmin start sysrecovery



Esegue un ripristino del sistema (ripristino bare metal) utilizzando i parametri specificati.

> [!NOTE]
> Il sottocomando può essere eseguito solo da ambiente ripristino Windows e non è elencata per impostazione predefinita nel testo dell'utilizzo di **Wbadmin**. Per ulteriori informazioni, vedere [Cenni preliminari su Ambiente ripristino Windows (Windows RE)](https://technet.microsoft.com/library/hh825173.aspx).

Per eseguire un ripristino del sistema con il sottocomando, è necessario essere un membro del **Backup Operators** gruppo o **amministratori** gruppo, oppure è necessario che siano state delegate le autorizzazioni appropriate.

## <a name="syntax"></a>Sintassi

```
wbadmin start sysrecovery
-version:<VersionIdentifier>
-backupTarget:{<BackupDestinationVolume> | <NetworkShareHostingBackup>}
[-machine:<BackupMachineName>]
[-restoreAllVolumes]
[-recreateDisks]
[-excludeDisks]
[-skipBadClusterCheck]
[-quiet]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|-version|Specifica l'identificatore di versione per il backup da ripristinare nella MM/GG/AAAA-formato hh: mm. Se non si conosce l'identificatore di versione, digitare **wbadmin ottenere versioni**.|
|-backupTarget|Specifica il percorso di archiviazione che contiene il backup o il backup che si desidera ripristinare. Questo parametro è utile quando il percorso di archiviazione è diverso da in cui sono in genere archiviati i backup del computer.|
|-machine|Specifica il nome del computer in cui si desidera ripristinare. Questo parametro è utile quando più computer sottoposte a backup nello stesso percorso. Deve essere utilizzato quando il **- backupTarget** parametro specificato.|
|-restoreAllVolumes|Ripristina tutti i volumi del backup selezionato. Se questo parametro viene omesso, vengono recuperati solo critici (volumi contenenti i componenti del sistema di stato e il sistema operativo). Questo parametro è utile quando è necessario ripristinare i volumi non critici durante il ripristino di sistema.|
|-recreateDisks|Ripristina una configurazione del disco allo stato esistente al momento della creazione del backup.</br>Avviso: questo parametro elimina tutti i dati in volumi che ospitano i componenti del sistema operativo. Inoltre potrebbe eliminare dati da volumi di dati.|
|-excludeDisks|Valido solo quando viene specificato con il **- recreateDisks** parametro e deve essere di input come elenco delimitato da virgole di identificatori disco (come indicato nell'output di **wbadmin ottenere dischi**). Esclusi i dischi non sono partizionati o formattati. Questo parametro consente di conservare i dati nei dischi che si preferisce non modificati durante l'operazione di ripristino.|
|-skipBadClusterCheck|Ignora il controllo dei dischi di ripristino per informazioni sul cluster non valida. Se esegue il ripristino a un server alternativo o hardware, si consiglia di non utilizzare questo parametro. È possibile eseguire manualmente **/b chkdsk** presenti sui dischi di ripristino in qualsiasi momento per verificare la presenza di cluster danneggiati e aggiornare di conseguenza le informazioni sul file system.</br>Avviso: fino a quando non si esegue **chkdsk** come descritto, il cluster danneggiati segnalati nel sistema ripristinato potrebbero non essere accurati.|
|-quiet|Esegue il comando senza alcuna richiesta visualizzata all'utente.|

## <a name="examples"></a>Esempi

Per avviare le informazioni di ripristino dalla copia di backup è stato eseguito il 31 marzo 2013 alle 9:00, si trova sull'unità d:, tipo:
```
wbadmin start sysrecovery -version:03/31/2013-09:00 -backupTarget:d:
```
Per avviare il recupero delle informazioni dal backup eseguito il 30 aprile 2013 alle ore 9:00, che si trova nella cartella condivisa \\ \\ servername\shared: per Server01, digitare:
```
wbadmin start sysrecovery -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Get-WBBareMetalRecovery](https://technet.microsoft.com/library/jj902461.aspx) cmdlet