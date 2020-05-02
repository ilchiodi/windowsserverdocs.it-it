---
title: DiskShadow
description: Argomento di riferimento per DiskShadow, uno strumento che espone la funzionalità offerta dal servizio Copia Shadow del volume (VSS).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e962537d-b759-4368-b6f1-e8391cf7b221
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: db1b1602bcbde41c2d92af925ff819ef390220e1
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719429"
---
# <a name="diskshadow"></a>DiskShadow

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

DiskShadow. exe è uno strumento che espone la funzionalità offerta dal servizio Copia Shadow del volume (VSS). Per impostazione predefinita, DiskShadow usa un interprete dei comandi interattivo simile a quello di DiskRAID o DiskPart. DiskShadow include anche una modalità di scripting.  
  
> [!NOTE]  
> L'appartenenza al gruppo Administrators locale o a un gruppo equivalente è il requisito minimo necessario per eseguire DiskShadow.  
  

## <a name="syntax"></a>Sintassi  
per la modalità interattiva, digitare quanto segue al prompt dei comandi per avviare l'interprete dei comandi di DiskShadow:  
  
```  
diskshadow  
```  
  
per la modalità script digitare quanto segue, dove *script. txt* è un file di script contenente i comandi di DiskShadow:  
  
```  
diskshadow -s script.txt  
```  
  
### <a name="parameters"></a>Parametri  
È possibile eseguire i comandi seguenti nell'interprete dei comandi di DiskShadow o tramite un file script:  
  
|Parametro|Descrizione|  
|-------|--------|  
|[set_2](set_2.md)|Imposta il contesto, le opzioni, la modalità dettagliata e il file di metadati per la creazione di copie shadow.|  
|[Simula ripristino](simulate-restore.md)|Verifica il coinvolgimento del writer nelle sessioni di ripristino nel computer senza inviare eventi di **preripristino** o di **ripristino** a writer.|  
|[Caricare i metadati](load-metadata.md)|Carica un file con estensione cab metadati prima di importare una copia shadow trasportabili o carica i metadati del writer in caso di ripristino.|  
|[writer](writer.md)|Verifica che un writer o un componente sia incluso o escluda un writer o un componente dalla procedura di backup o ripristino.|  
|[add](add.md)|aggiunge volumi al set di volumi di cui deve essere eseguita la copia shadow o aggiunge alias all'ambiente alias.|  
|[create_1](create_1.md)|avvia il processo di creazione della copia shadow, usando il contesto e le impostazioni dell'opzione correnti.|  
|[exec](exec.md)|esegue un file nel computer locale.|  
|[Avvia backup](begin-backup.md)|avvia una sessione di backup completo.|  
|[Backup di fine](end-backup.md)|Termina una sessione di backup completo e genera un evento **BackupComplete** con lo stato del writer appropriato, se necessario.|  
|[Avvia ripristino](begin-restore.md)|avvia una sessione di ripristino e genera un evento di **preripristino** per i writer necessari.|  
|[Ripristino di fine](end-restore.md)|Termina una sessione di ripristino e i problemi di un **PostRestore** eventi per gli scrittori coinvolti.|  
|[reset](reset.md)|Reimposta DiskShadow sullo stato predefinito.|  
|[list](list.md)|elenca i writer, le copie shadow o i provider di copie shadow attualmente registrati presenti nel sistema.|  
|[Elimina ombre](delete-shadows.md)|Elimina le copie shadow.|  
|[import](import.md)|importa una copia shadow trasportabile da un file di metadati caricato nel sistema.|  
|[mask](mask.md)|rimuove le copie shadow dell'hardware importate tramite il comando **Import** .|  
|[esporre](expose.md)|espone una copia shadow permanente come una lettera di unità, una condivisione o un punto di montaggio.|  
|[volume x:.](unexpose.md)|non espone una copia shadow esposta tramite il comando **Expose** .|  
|[break_2](break_2.md)|Annulla l'associazione di un volume di copia shadow da VSS.|  
|[ripristinare](revert.md)|Ripristina un volume a una copia shadow specificata.|  
|[exit_1](exit_1.md)|esce da DiskShadow.|  
  
## <a name="remarks"></a>Osservazioni  
  
-   Come minimo, è necessario **aggiungere** e **creare** solo per creare una copia shadow. In questo modo, tuttavia, le impostazioni relative al contesto e alle opzioni saranno un backup di copia e verrà creata solo una copia shadow senza script di esecuzione di backup.  
  
## <a name="examples"></a>Esempi  
Si tratta di una sequenza di comandi di esempio che creerà una copia shadow per il backup. È possibile salvarlo nel file come script. DSH ed eseguirlo con DiskShadow \/s script. DSH  
  
Si supponga quanto segue:  
  
-   Si dispone di una directory esistente denominata c\\: diskshadowdata.  
  
-   Il volume di sistema è C: e il volume di dati è D:.  
  
-   Si dispone di un file backupscript. cmd in c\\: diskshadowdata.  
  
-   Il file backupscript. cmd eseguirà la copia dei dati Shadow p: e q: nell'unità di backup.  
  
È possibile immettere questi comandi manualmente o crearne uno script:  
  
```  
#diskshadow script file  
set context persistent nowriters  
set metadata c:\diskshadowdata\example.cab  
set verbose on  
begin backup  
add volume c: alias Systemvolumeshadow  
add volume d: alias Datavolumeshadow  
  
create  
  
expose %Systemvolumeshadow% p:  
expose %Datavolumeshadow% q:  
exec c:\diskshadowdata\backupscript.cmd  
end backup  
#End of script  
```  
  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
  

