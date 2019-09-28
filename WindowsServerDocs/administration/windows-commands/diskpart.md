---
title: Comandi DiskPart
ms.prod: windows-server
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 0826b773927f09cc846fb1cfdf4d5dfbf75d5cca
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377834"
---
# <a name="diskpart-commands"></a>Comandi DiskPart

Si applica a: Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 e Windows Server 2008 R2, Windows Server 2008

I comandi DiskPart consentono di gestire le unità del PC (dischi, partizioni, volumi o dischi rigidi virtuali). Prima di poter utilizzare i comandi DiskPart, è necessario innanzitutto elencare, quindi selezionare un oggetto per assegnargli lo stato attivo. Quando un oggetto ha lo stato attivo, qualsiasi comando DiskPart digitato agirà su tale oggetto.

È possibile elencare gli oggetti disponibili e determinare il numero o la lettera di unità di un oggetto usando i comandi **list disk, list volume, List Partition**ed **List vdisk** . I comandi **list disk, List vdisk** ed List **volume** visualizzano tutti i dischi e i volumi del computer. Tuttavia, il comando **List Partition** Visualizza solo le partizioni sul disco con lo stato attivo. Quando si usano i comandi **List** , viene visualizzato un asterisco (\*) accanto all'oggetto con lo stato attivo.

Quando si seleziona un oggetto, lo stato attivo rimane su tale oggetto fino a quando non si seleziona un oggetto diverso. Ad esempio, se lo stato attivo è impostato su disco 0 e si seleziona volume 8 sul disco 2, lo stato attivo passa dal disco 0 al disco 2, volume 8. Alcuni comandi cambiano automaticamente lo stato attivo. Ad esempio, quando si crea una nuova partizione, lo stato attivo passa automaticamente alla nuova partizione.

È possibile assegnare lo stato attivo solo a una partizione sul disco selezionato. Quando una partizione ha lo stato attivo, anche il volume correlato (se presente) dispone dello stato attivo. Quando un volume ha lo stato attivo, il disco e la partizione correlati hanno lo stato attivo anche se il volume è mappato a una singola partizione specifica. In caso contrario, lo stato attivo sul disco e sulla partizione viene perso.

## <a name="diskpart-commands"></a>Comandi DiskPart

Per avviare l'interprete dei comandi DiskPart, al prompt dei comandi digitare:

`diskpart`

> [!IMPORTANT]
> L'appartenenza al gruppo **Administrators** locale o a un gruppo equivalente è il requisito minimo necessario per eseguire Diskpart. 

È possibile eseguire i comandi seguenti nell'interprete dei comandi DiskPart:

  - [Active](active.md)  
      
  - [Aggiungi](add.md)  
      
  - [Assegnare](assign.md)  
      
  - [Connetti vdisk](attach-vdisk.md)  
      
  - [Attributi](attributes.md)  
      
  - [Montaggio automatico](automount.md)  
      
  - [Interruzione](break.md)  
      
  - [Pulito](clean.md)  
      
  - [Compatta vdisk](compact-vdisk.md)  
      
  - [Convertire](convert.md)  
      
  - [Creare](create.md)  
      
  - [Elimina](delete.md)  
      
  - [Scollega vdisk](detach-vdisk.md)  
      
  - [Dettaglio](detail.md)  
      
  - [Azioni uscita](exit.md)  
      
  - [Espandi vdisk](expand-vdisk.md)  
      
  - [Estendere](extend.md)  
      
  - [Filesystem](filesystems.md)  
      
  - [Formato](format.md)  
      
  - [GPT](gpt.md)  
      
  - [?](help.md)  
      
  - [Importa](import.md)  
      
  - [Inattivo](inactive.md)  
      
  - [Elenco](list.md)  
      
  - [Unisci vdisk](merge-vdisk.md)  
      
  - [Offline](offline.md)  
      
  - [Online](online.md)  
      
  - [Ripristinare](recover.md)  
      
  - [REM](rem.md)  
      
  - [Rimuovi](remove.md)  
      
  - [Ripristinare](repair.md)  
      
  - [Ripeti analisi](rescan.md)  
      
  - [Mantenere](retain.md)  
      
  - [San](san.md)  
      
  - [Selezionare](select.md)  
      
  - [Imposta ID](set-id.md)  
      
  - [Strizzacervelli](shrink.md)  
      
  - [UniqueId](uniqueid.md)  
      

## <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

[Cmdlet di archiviazione in Windows PowerShell](https://docs.microsoft.com/powershell/module/storage/)
