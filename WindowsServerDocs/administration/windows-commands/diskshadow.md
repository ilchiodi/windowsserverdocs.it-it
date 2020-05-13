---
title: Diskshadow
description: Argomento di riferimento per il comando DiskShadow, che è uno strumento che espone la funzionalità offerta dal servizio Copia Shadow del volume (VSS).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e962537d-b759-4368-b6f1-e8391cf7b221
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ae3a4ba57d9c29375c560c300a4e4ead807184fc
ms.sourcegitcommit: aed942d11f1a361fc1d17553a4cf190a864d1268
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/12/2020
ms.locfileid: "83235187"
---
# <a name="diskshadow"></a>Diskshadow

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

DiskShadow. exe è uno strumento che espone la funzionalità offerta dal servizio Copia Shadow del volume (VSS). Per impostazione predefinita, DiskShadow usa un interprete dei comandi interattivo simile a quello di DiskRAID o DiskPart. DiskShadow include anche una modalità di scripting.

> [!NOTE]
> L'appartenenza al gruppo Administrators locale o a un gruppo equivalente è il requisito minimo necessario per eseguire DiskShadow.

## <a name="syntax"></a>Sintassi

Per la modalità interattiva, digitare quanto segue al prompt dei comandi per avviare l'interprete dei comandi di DiskShadow:

```
diskshadow
```

Per la modalità script digitare quanto segue, dove *script. txt* è un file di script contenente i comandi di DiskShadow:

```
diskshadow -s script.txt
```

### <a name="parameters"></a>Parametri

È possibile eseguire i comandi seguenti nell'interprete dei comandi di DiskShadow o tramite un file di script. Come minimo, è necessario **aggiungere** e **creare** solo per creare una copia shadow. In questo modo, tuttavia, le impostazioni relative al contesto e alle opzioni verranno ritirate e verrà creata una copia shadow senza script di esecuzione di backup.

| Comando | Descrizione |
| --------- | ----------- |
| [imposta comando](set_2.md) | Imposta il contesto, le opzioni, la modalità dettagliata e il file di metadati per la creazione di copie shadow. |
| [comando Load Metadata](load-metadata.md) | Carica un file con estensione cab metadati prima di importare una copia shadow trasportabili o carica i metadati del writer in caso di ripristino. |
| [comando writer](writer.md) | Verifica che un writer o un componente sia incluso o escluda un writer o un componente dalla procedura di backup o ripristino. |
| [Aggiungi comando](add.md) | Aggiunge volumi al set di volumi di cui deve essere eseguita la copia shadow o aggiunge alias all'ambiente alias. |
| [Crea comando](create.md) | Avvia il processo di creazione copia shadow, utilizzando le impostazioni correnti di contesto e l'opzione. |
| [comando exec](exec.md) | Esegue un file nel computer locale. |
| [comando BEGIN backup](begin-backup.md) | Avvia una sessione di backup completo. |
| [comando termina backup](end-backup.md) | Termina una sessione di backup completo e genera un evento **BackupComplete** con lo stato del writer appropriato, se necessario. |
| [comando BEGIN Restore](begin-restore.md) | Avvia una sessione di ripristino e genera un evento di **preripristino** per i writer necessari. |
| [termina comando Restore](end-restore.md) | Termina una sessione di ripristino e genera un evento **PostRestore** per i writer necessari. |
| [Reimposta comando](reset.md) | Reimposta DiskShadow sullo stato predefinito. |
| [comando list](list.md) | Elenca i writer, le copie shadow o provider di copie shadow attualmente registrato che il sistema. |
| [comando Elimina ombreggiature](delete-shadows.md) | Consente di eliminare le copie shadow. |
| [comando Import](import.md) | Importa una copia shadow trasportabile da un file di metadati caricato nel sistema. |
| [Mask (comando)](mask.md) | Rimuove le copie shadow dell'hardware importate tramite il comando **Import** . |
| [espone comando](expose.md) | Espone una copia permanente come una lettera di unità, una condivisione o un punto di montaggio. |
| [comando unexpoe](unexpose.md) | Unexposes una copia shadow esposta tramite il **esporre** comando. |
| [Interrompi comando](break_2.md) | Annulla l'associazione di un volume di copia shadow da VSS. |
| [Annulla comando](revert.md) | Ripristina un volume in una copia shadow specificata. |
| [comando Exit](exit.md) | Esce dall'interprete dei comandi o dallo script. |

## <a name="examples"></a>Esempi

Si tratta di una sequenza di comandi di esempio che creerà una copia shadow per il backup. È possibile salvarlo nel file come script. DSH ed eseguirlo utilizzando `diskshadow /s script.dsh` .

Si supponga quanto segue:

- Si dispone di una directory esistente denominata c: \\ diskshadowdata.

- Il volume di sistema è C: e il volume di dati è D:.

- Si dispone di un file backupscript. cmd in c: \\ diskshadowdata.

- Il file backupscript. cmd eseguirà la copia dei dati Shadow p: e q: nell'unità di backup.

È possibile immettere questi comandi manualmente o crearne uno script:

```
#Diskshadow script file
set context persistent nowriters
set metadata c:\diskshadowdata\example.cab
set verbose on
begin backup
add volume c: alias systemvolumeshadow
add volume d: alias datavolumeshadow

create

expose %systemvolumeshadow% p:
expose %datavolumeshadow% q:
exec c:\diskshadowdata\backupscript.cmd
end backup
#End of script
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
