---
title: diskshadow
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e962537d-b759-4368-b6f1-e8391cf7b221
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b2c5648235a1c856c6aef09621e2381e74d08d70
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869182"
---
# <a name="diskshadow"></a>diskshadow

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

DiskShadow.exe è uno strumento che espone le funzionalità offerte dalla copia shadow del volume del servizio \(VSS\). Per impostazione predefinita, diskshadow utilizza un interprete dei comandi interattivo simile a quello di diskraid o DiskPart. DiskShadow è inoltre disponibile una modalità gestibile tramite script.  
  
> [!NOTE]  
> L'appartenenza al gruppo Administrators locale o equivalente è il requisito minimo necessario per eseguire diskshadow.  
  
Per esempi di come usare i comandi diskshadow, vedere [esempi](#BKMK_examples).  
  
## <a name="syntax"></a>Sintassi  
per la modalità interattiva, digitare quanto segue al prompt dei comandi per avviare l'interprete dei comandi diskshadow:  
  
```  
diskshadow  
```  
  
per la modalità di script, digitare quanto segue, dove *script.txt* è un file di script contenente comandi diskshadow:  
  
```  
diskshadow -s script.txt  
```  
  
## <a name="diskshadow-commands"></a>comandi DiskShadow  
In diskshadow l'interprete dei comandi o tramite un file di script, è possibile eseguire i comandi seguenti:  
  
|Parametro|Descrizione|  
|-------|--------|  
|[set_2](set_2.md)|Imposta il contesto, opzioni, la modalità dettagliata e file di metadati per la creazione di copie shadow.|  
|[Simulare il ripristino](simulate-restore.md)|Testa un coinvolgimento dell'agente di scrittura nelle sessioni di ripristino nel computer senza emettere **PreRestore** oppure **PostRestore** per gli autori di eventi.|  
|[Caricare i metadati](load-metadata.md)|Carica un file con estensione cab metadati prima di importare una copia shadow trasportabili o carica i metadati del writer in caso di ripristino.|  
|[writer](writer.md)|verifica che un writer o un componente è incluso o esclude un writer o un componente della procedura di backup o ripristino.|  
|[add_1](add_1.md)|Aggiunge volumi per il set di volumi che devono essere replicati mediante copiata shadow, o gli alias per l'ambiente di alias.|  
|[create_1](create_1.md)|Avvia il processo di creazione copia shadow, utilizzando le impostazioni correnti di contesto e l'opzione.|  
|[exec](exec.md)|esegue un file nel computer locale.|  
|[Avviare il backup](begin-backup.md)|Avvia una sessione di backup completo.|  
|[Backup di fine](end-backup.md)|Termina una sessione di backup completo e problemi di un **Backupcomplete** evento con lo stato del writer appropriato, se necessario.|  
|[Inizio ripristino](begin-restore.md)|Avvia una sessione di ripristino e i problemi di un **PreRestore** eventi per gli scrittori coinvolti.|  
|[Ripristino di fine](end-restore.md)|Termina una sessione di ripristino e i problemi di un **PostRestore** eventi per gli scrittori coinvolti.|  
|[reset](reset.md)|Reimposta diskshadow allo stato predefinito.|  
|[list](list.md)|Elenca i writer, le copie shadow o provider di copie shadow attualmente registrato che sono nel sistema.|  
|[eliminare le ombreggiature](delete-shadows.md)|Elimina le copie shadow.|  
|[import](import.md)|Importa una copia shadow trasportabili da un file dei metadati caricati nel sistema.|  
|[mask](mask.md)|Rimuove le copie shadow hardware che sono state importate usando il **importare** comando.|  
|[expose](expose.md)|espone una copia permanente come una lettera di unità, una condivisione o un punto di montaggio.|  
|[unexpose](unexpose.md)|unexposes una copia shadow esposta tramite il **esporre** comando.|  
|[break_2](break_2.md)|Rimuove l'associazione di un volume copia shadow da VSS.|  
|[revert](revert.md)|Ripristina un volume in una copia shadow specificata.|  
|[exit_1](exit_1.md)|esce da diskshadow.|  
  
## <a name="remarks"></a>Note  
  
-   come minimo, solo **aggiungere** e **creare** sono necessari per creare una copia shadow. Tuttavia, si perderanno le impostazioni di contesto e l'opzione, sarà un backup di copia e solo creerà una copia shadow con nessuno script di esecuzione di backup.  
  
## <a name="BKMK_examples"></a>Esempi  
Si tratta di una sequenza di esempio di comandi che verrà creata una copia shadow per il backup. Può essere salvato in un file con script.dsh ed eseguita con diskshadow \/script.dsh s  
  
Si presupponga quanto segue:  
  
-   Si dispone di una directory esistente denominata c:\\diskshadowdata.  
  
-   Il volume di sistema è c: e il volume di dati è il d:.  
  
-   Si dispone di un file backupscript.cmd in c:\\diskshadowdata.  
  
-   Il file backupscript.cmd eseguirà la copia di p: dati di ombreggiatura e d: per l'unità di backup.  
  
È possibile immettere questi comandi manualmente o uno script:  
  
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
  
#### <a name="additional-references"></a>Riferimenti aggiuntivi  
[Chiave sintassi della riga di comando](command-line-syntax-key.md)  
  

