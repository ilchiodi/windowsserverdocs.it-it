---
title: DiskPart
description: Windows Commands Topic for **DiskPart**, che consente di gestire le unità del computer.
ms.prod: windows-server
ms.technology: storage
author: jasongerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: eb2921d8da4a4a29c4f700107ef5b6d7bfb41481
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122539"
---
# <a name="diskpart"></a>DiskPart

>Si applica a: Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 e Windows Server 2008 R2, Windows Server 2008

I comandi DiskPart consentono di gestire le unità del computer (dischi, partizioni, volumi o dischi rigidi virtuali).

Prima di poter utilizzare i comandi DiskPart, è necessario innanzitutto elencare, quindi selezionare un oggetto per assegnargli lo stato attivo. Quando un oggetto ha lo stato attivo, qualsiasi comando DiskPart digitato agirà su tale oggetto.

## <a name="list-available-objects"></a>Elenca oggetti disponibili

È possibile elencare gli oggetti disponibili e determinare il numero o la lettera di unità di un oggetto utilizzando:

- `list disk`-Visualizza tutti i dischi del computer.

- `list volume`-Visualizza tutti i volumi nel computer.

- `list partition`-Visualizza le partizioni sul disco con lo stato attivo sul computer.

- `list vdisk`: Visualizza tutti i dischi virtuali nel computer.

Quando si usano i comandi **List** , viene visualizzato un asterisco (*) accanto all'oggetto con lo stato attivo.

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

È possibile eseguire i comandi seguenti dall'interprete dei comandi DiskPart:

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
| [Espandi vdisk](expand-vdisk.md) | Espande un disco rigido virtuale (VHD) per le dimensioni specificate. |
| [Estendere](extend.md) | Estende il volume o la partizione con lo stato attivo, insieme al relativo file system, nello spazio libero (non allocato) su un disco. |
| [Filesystem](filesystems.md) | Visualizza informazioni sul file system corrente del volume con lo stato attivo e vengono elencati i sistemi di file supportati per la formattazione del volume. |
| [Formato](format.md) | Formatta un disco per accettare i file di Windows. |
| [GPT](gpt.md) | Assegna gli attributi GPT alla partizione con lo stato attivo sui dischi di base della tabella di partizione GUID (GPT). |
| [?](help.md) | Visualizza un elenco di comandi disponibili o le informazioni della Guida dettagliate su un comando specificato. |
| [Importa](import.md) | Importa un gruppo di dischi esterni nel gruppo di dischi del computer locale. |
| [Inattivo](inactive.md) | Contrassegna la partizione di sistema o la partizione di avvio con lo stato attivo come inattivo sui dischi MBR (master boot record) di base. |
| [Elenco](list.md) | Visualizza un elenco di dischi, di partizioni in un disco, di volumi in un disco o di dischi rigidi virtuali (VHD). |
| [Unisci vdisk](merge-vdisk.md) | Unisce un disco rigido virtuale differenze (VHD) con il VHD padre corrispondente. |
| [Offline](offline.md) | Accetta un volume o il disco online sullo stato offline. |
| [Online](online.md) | Accetta un volume o il disco non in linea allo stato online. |
| [Ripristinare](recover.md) | Aggiorna lo stato di tutti i dischi in un gruppo di dischi, tenta di ripristinare i dischi in un gruppo di dischi non valido e risincronizza i volumi con mirroring e i volumi RAID-5 con dati non aggiornati. |
| [REM](rem.md) | Consente di aggiungere commenti a uno script. |
| [Rimuovi](remove.md) | Rimuove un punto di montaggio o lettera di unità da un volume. |
| [Ripristinare](repair.md) | Ripristina il volume RAID-5 con lo stato attivo sostituendo l'area del disco non riuscita con il disco dinamico specificato. |
| [Ripeti analisi](rescan.md) | Individua i nuovi dischi aggiunti al computer. |
| [Mantenere](retain.md) | Prepara un volume semplice dinamico esistente da utilizzare come un avvio o il volume di sistema. |
| [San](san.md) | Visualizza o imposta i criteri San (Storage Area Network) per il sistema operativo. |
| [Selezionare](select.md) | Sposta lo stato attivo su un disco, una partizione, un volume o disco rigido virtuale (VHD). |
| [Imposta ID](set-id.md) | Modifica il campo del tipo di partizione per la partizione con lo stato attivo. |
| [Strizzacervelli](shrink.md) | Riduce la dimensione del volume selezionato in base al valore specificato. |
| [UniqueId](uniqueid.md) | Visualizza o imposta il GUID partizione GPT (tabella) identificatore firma o del master avvio MBR (record) per il disco con lo stato attivo. |

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Panoramica di gestione disco](https://docs.microsoft.com/windows-server/storage/disk-management/overview-of-disk-management)

- [Cmdlet di archiviazione in Windows PowerShell](https://docs.microsoft.com/powershell/module/storage/)
