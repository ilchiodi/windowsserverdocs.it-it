---
Title: Comandi DiskPart
ms.prod: windows-server-threshold
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: e04af7b6425e208013277d1aaa6f28af62871bcc
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280084"
---
# <a name="diskpart-commands"></a>Comandi DiskPart

Si applica a: Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 e Windows Server 2008 R2, Windows Server 2008

I comandi DiskPart aiutano a gestire le unità del PC (dischi, partizioni, volumi o dischi rigidi virtuali). Prima di poter utilizzare i comandi DiskPart, è necessario prima elencati e quindi selezionare un oggetto per attivarla. Quando un oggetto ha lo stato attivo, qualsiasi comando DiskPart indipendente dai tipi verrà agire su di esso.

È possibile elencare tutti gli oggetti disponibili e determinare un oggetto numero o lettera di unità tramite il **disco elenco, il volume di elenco, elenco partizione**, e **elencare vdisk** comandi. Il **disco elenco, elenco vdisk** e **elenco volume** comandi visualizzano tutti i dischi e volumi nel computer. Tuttavia, il **elenco partizione** comando Visualizza solo le partizioni nel disco con lo stato attivo. Quando si usa la **elenco** comandi, un asterisco (\*) viene visualizzato accanto all'oggetto con lo stato attivo.

Quando si seleziona un oggetto, lo stato attivo rimane su tale oggetto fino a quando non si seleziona un oggetto diverso. Ad esempio, se lo stato attivo è impostato su disco 0 e si seleziona il volume 8 sul disco 2, lo stato attivo passa dal disco 0 per il disco 2, 8 volume. Alcuni comandi di modificano automaticamente lo stato attivo. Ad esempio, quando si crea una nuova partizione, lo stato attivo passa automaticamente alla nuova partizione.

È possibile assegnare solo lo stato attivo a una partizione del disco selezionato. Quando una partizione ha lo stato attivo, il relativo volume, se presente, ha anche lo stato attivo. Quando un volume con lo stato attivo, il disco correlato e partizione ha anche lo stato attivo se il volume viene mappato a una determinata partizione. Se ciò non avviene, lo stato attivo sul disco e partizione viene persa.

## <a name="diskpart-commands"></a>Comandi DiskPart

Per avviare l'interprete dei comandi DiskPart, al prompt dei comandi digitare:

`diskpart`

> [!IMPORTANT]
> L'appartenenza al gruppo **gli amministratori** gruppo o equivalente è il requisito minimo necessario per eseguire DiskPart. 

Nell'interprete dei comandi Diskpart, è possibile eseguire i comandi seguenti:

  - [Active](active.md)  
      
  - [Aggiungi](add.md)  
      
  - [assegnare](assign.md)  
      
  - [collegare vdisk](attach-vdisk.md)  
      
  - [Attributi](attributes.md)  
      
  - [Automount](automount.md)  
      
  - [Break](break.md)  
      
  - [Pulisci](clean.md)  
      
  - [Compatta disco virtuale](compact-vdisk.md)  
      
  - [Convert](convert.md)  
      
  - [creare](create.md)  
      
  - [Elimina](delete.md)  
      
  - [Detach vdisk](detach-vdisk.md)  
      
  - [Dettagli](detail.md)  
      
  - [Azioni uscita](exit.md)  
      
  - [Espandere vdisk](expand-vdisk.md)  
      
  - [Estendere](extend.md)  
      
  - [file System](filesystems.md)  
      
  - [Format](format.md)  
      
  - [GPT](gpt.md)  
      
  - [?](help.md)  
      
  - [Importa](import.md)  
      
  - [Inactive](inactive.md)  
      
  - [Elenco](list.md)  
      
  - [Merge vdisk](merge-vdisk.md)  
      
  - [Offline](offline.md)  
      
  - [Online](online.md)  
      
  - [Recover](recover.md)  
      
  - [REM](rem.md)  
      
  - [Rimuovi](remove.md)  
      
  - [Repair](repair.md)  
      
  - [ripetizione dell'analisi](rescan.md)  
      
  - [Conservare](retain.md)  
      
  - [SAN](san.md)  
      
  - [Selezionare](select.md)  
      
  - [Id del set](set-id.md)  
      
  - [compattazione](shrink.md)  
      
  - [Uniqueid](uniqueid.md)  
      

## <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

[Cmdlet di archiviazione in Windows PowerShell](https://docs.microsoft.com/powershell/module/storage/)
