---
title: DiskPart
description: Windows Commands Topic for DiskPart, che consente di gestire le unità del computer.
ms.prod: windows-server
ms.technology: storage
author: jasongerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 45fe66e4843b96db8e4593c0e963e4a80dbd22c2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80845474"
---
# <a name="diskpart"></a>DiskPart

>Si applica a: Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 e Windows Server 2008 R2, Windows Server 2008

I comandi DiskPart consentono di gestire le unità del computer (dischi, partizioni, volumi o dischi rigidi virtuali). 

Prima di poter utilizzare i comandi DiskPart, è necessario innanzitutto elencare, quindi selezionare un oggetto per assegnargli lo stato attivo. Quando un oggetto ha lo stato attivo, qualsiasi comando DiskPart digitato agirà su tale oggetto.

## <a name="list-the-available-objects"></a>Elencare gli oggetti disponibili

È possibile elencare gli oggetti disponibili e determinare il numero o la lettera di unità di un oggetto utilizzando:

- `list disk`-Visualizza tutti i dischi del computer.

- `list volume`-Visualizza tutti i volumi nel computer.

- `list partition`-Visualizza le partizioni sul disco con lo stato attivo sul computer.

- `list vdisk`: Visualizza tutti i dischi virtuali nel computer.

Quando si usano i comandi **List** , viene visualizzato un asterisco (\*) accanto all'oggetto con lo stato attivo.

## <a name="determine-focus"></a>Determinare lo stato attivo

Quando si seleziona un oggetto, lo stato attivo rimane su tale oggetto fino a quando non si seleziona un oggetto diverso. Ad esempio, se lo stato attivo è impostato su disco 0 e si seleziona volume 8 sul disco 2, lo stato attivo passa dal disco 0 al disco 2, volume 8.

Alcuni comandi cambiano automaticamente lo stato attivo. Ad esempio, quando si crea una nuova partizione, lo stato attivo passa automaticamente alla nuova partizione.

È possibile assegnare lo stato attivo solo a una partizione sul disco selezionato. Quando una partizione ha lo stato attivo, anche il volume correlato (se presente) dispone dello stato attivo. Quando un volume ha lo stato attivo, il disco e la partizione correlati hanno lo stato attivo anche se il volume è mappato a una singola partizione specifica. In caso contrario, lo stato attivo sul disco e sulla partizione viene perso.

## <a name="diskpart-commands"></a>Comandi DiskPart

Per avviare l'interprete dei comandi DiskPart, al prompt dei comandi digitare:

```
diskpart
```

> [!IMPORTANT]
> Per eseguire DiskPart, è necessario appartenere al gruppo **Administrators** locale o a un gruppo con autorizzazioni simili. 

È possibile eseguire i comandi seguenti nell'interprete dei comandi DiskPart:

| Comando | Descrizione |
| ------- | ----------- |
| [Active](active.md) | Contrassegna la partizione del disco con lo stato attivo, come attiva. |
| [Aggiungi](add.md) | Esegue il mirroring del volume semplice con lo stato attivo per il disco specificato. |
| [Assegnare](assign.md) | Assegna una lettera di unità o un punto di montaggio al volume con lo stato attivo. |
| [Connetti vdisk](attach-vdisk.md) | Collega (talvolta denominato montaggi o superfici) un disco rigido virtuale (VHD) in modo che venga visualizzato nel computer host come unità disco rigido locale. |
| [Attributi](attributes.md) | Visualizza, imposta o cancella gli attributi di un disco o di un volume. |
| [Montaggio automatico](automount.md) | Abilita o Disabilita la funzionalità di montaggio automatico. | 
| [Interruzione](break.md) | Suddivide il volume con mirroring con lo stato attivo in due volumi semplici. |
| [Pulito](clean.md) | Rimuove tutte le partizioni o la formattazione del volume dal disco con lo stato attivo. |
| [Compatta vdisk](compact-vdisk.md) | Riduce le dimensioni fisiche di un file di disco rigido virtuale (VHD) ad espansione dinamica. |
| [Convertire](convert.md) | Converte file allocation table (FAT) e volumi FAT32 al file system NTFS, lasciando inalterate le directory e i file esistenti. |
| [Creare](create.md) | Crea una partizione su un disco, un volume su uno o più dischi o un disco rigido virtuale (VHD). |
| [Elimina](delete.md) | Elimina una partizione o un volume. |
| [Scollega vdisk](detach-vdisk.md) | Arresta la visualizzazione del disco rigido virtuale (VHD) selezionato come unità disco rigido locale nel computer host. |
| [Dettaglio](detail.md) | Visualizza le informazioni relative al disco, alla partizione, al volume o al disco rigido virtuale (VHD) selezionato. |
| [Azioni uscita](exit.md) | Chiude l'interprete dei comandi DiskPart. |
| [Espandi vdisk](expand-vdisk.md) | 
| [Estendere](extend.md) | 
| [Filesystem](filesystems.md) | 
| [Formato](format.md) | 
| [GPT](gpt.md) | 
| [?](help.md) | 
| [Importa](import.md) | 
| [Inattivo](inactive.md) | 
| [Elenco](list.md) | 
| [Unisci vdisk](merge-vdisk.md) | 
| [Offline](offline.md) | 
| [Online](online.md) | 
| [Ripristinare](recover.md) | 
| [REM](rem.md) | 
| [Rimuovi](remove.md) | 
| [Ripristinare](repair.md) | 
| [Ripeti analisi](rescan.md) | 
| [Mantenere](retain.md) | 
| [San](san.md) | 
| [Selezionare](select.md) | 
| [Imposta ID](set-id.md) | 
| [Strizzacervelli](shrink.md) | 
| [UniqueId](uniqueid.md) | 

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Chiave sintassi della riga di comando] (command-line-syntax-key.md

- [Panoramica di gestione disco](https://docs.microsoft.com/windows-server/storage/disk-management/overview-of-disk-management)

- [Cmdlet di archiviazione in Windows PowerShell](https://docs.microsoft.com/powershell/module/storage/)
