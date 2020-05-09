---
title: diskpart
description: Argomento di riferimento per l'interprete dei comandi DiskPart, che consente di gestire le unità del computer.
ms.prod: windows-server
ms.technology: storage
author: jasongerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 8b6d36e428daaefd7cf26e42170442373fc7551c
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2020
ms.locfileid: "82992565"
---
# <a name="diskpart"></a>diskpart

> Si applica a: Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 e Windows Server 2008 R2, Windows Server 2008

L'interprete dei comandi DiskPart consente di gestire le unità del computer (dischi, partizioni, volumi o dischi rigidi virtuali).

Prima di poter utilizzare i comandi **DiskPart** , è necessario innanzitutto elencare, quindi selezionare un oggetto per assegnargli lo stato attivo. Quando un oggetto ha lo stato attivo, qualsiasi comando DiskPart digitato agirà su tale oggetto.

## <a name="list-available-objects"></a>Elenca oggetti disponibili

È possibile elencare gli oggetti disponibili e determinare il numero o la lettera di unità di un oggetto utilizzando:

- `list disk`-Visualizza tutti i dischi del computer.

- `list volume`-Visualizza tutti i volumi nel computer.

- `list partition`-Visualizza le partizioni sul disco con lo stato attivo sul computer.

- `list vdisk`-Visualizza tutti i dischi virtuali nel computer.

Dopo aver eseguito i comandi **List** , viene visualizzato un asterisco (*) accanto all'oggetto con lo stato attivo.

## <a name="determine-focus"></a>Determinare lo stato attivo

Quando si seleziona un oggetto, lo stato attivo rimane su tale oggetto fino a quando non si seleziona un oggetto diverso. Ad esempio, se lo stato attivo è impostato su disco 0 e si seleziona volume 8 sul disco 2, lo stato attivo passa dal disco 0 al disco 2, volume 8.

Alcuni comandi cambiano automaticamente lo stato attivo. Ad esempio, quando si crea una nuova partizione, lo stato attivo passa automaticamente alla nuova partizione.

È possibile assegnare lo stato attivo solo a una partizione sul disco selezionato. Quando una partizione ha lo stato attivo, anche il volume correlato (se presente) ha lo stato attivo. Quando un volume ha lo stato attivo, il disco e la partizione correlati hanno lo stato attivo anche se il volume è mappato a una singola partizione specifica. In caso contrario, lo stato attivo sul disco e sulla partizione viene perso.

## <a name="syntax"></a>Sintassi

Per avviare l'interprete dei comandi DiskPart, al prompt dei comandi digitare:

```
diskpart <parameter>
```

> [!IMPORTANT]
> Per eseguire Diskpart, è necessario appartenere al gruppo **Administrators** locale o a un gruppo con autorizzazioni simili.

### <a name="parameters"></a>Parametri

È possibile eseguire i comandi seguenti dall'interprete dei comandi DiskPart:

| Comando | Descrizione |
| ------- | ----------- |
| [active](active.md) | Contrassegna la partizione del disco con lo stato attivo, come attiva. |
| [add](add.md) | Esegue il mirroring del volume semplice con lo stato attivo per il disco specificato. |
| [assign](assign.md) | Assegna una lettera di unità o un punto di montaggio al volume con lo stato attivo. |
| [attach vdisk](attach-vdisk.md) | Collega (talvolta denominato montaggi o superfici) un disco rigido virtuale (VHD) in modo che venga visualizzato nel computer host come unità disco rigido locale. |
| [attributes](attributes.md) | Visualizza, imposta o cancella gli attributi di un disco o di un volume. |
| [automount](automount.md) | Abilita o Disabilita la funzionalità di montaggio automatico. |
| [break](break.md) | Suddivide il volume con mirroring con lo stato attivo in due volumi semplici. |
| [clean](clean.md) | Rimuove tutte le partizioni o la formattazione del volume dal disco con lo stato attivo. |
| [compatta vdisk](compact-vdisk.md) | Riduce le dimensioni fisiche di un file di disco rigido virtuale (VHD) ad espansione dinamica. |
| [convert](convert.md) | Converte file allocation table (FAT) e volumi FAT32 al file system NTFS, lasciando inalterate le directory e i file esistenti. |
| [creare](create.md) | Crea una partizione su un disco, un volume su uno o più dischi o un disco rigido virtuale (VHD). |
| [delete](delete.md) | Elimina una partizione o un volume. |
| [Scollega vdisk](detach-vdisk.md) | Arresta la visualizzazione del disco rigido virtuale (VHD) selezionato come unità disco rigido locale nel computer host. |
| [dettagli](detail.md) | Visualizza le informazioni relative al disco, alla partizione, al volume o al disco rigido virtuale (VHD) selezionato. |
| [exit](exit.md) | Esce dall'interprete dei comandi Diskpart. |
| [Espandi vdisk](expand-vdisk.md) | Espande un disco rigido virtuale (VHD) per le dimensioni specificate. |
| [estendere](extend.md) | Estende il volume o la partizione con lo stato attivo, insieme al relativo file system, nello spazio libero (non allocato) su un disco. |
| [filesystem](filesystems.md) | Visualizza informazioni sul file system corrente del volume con lo stato attivo e vengono elencati i sistemi di file supportati per la formattazione del volume. |
| [format](format.md) | Formatta un disco per accettare i file di Windows. |
| [GPT](gpt.md) | Assegna gli attributi GPT alla partizione con lo stato attivo sui dischi di base della tabella di partizione GUID (GPT). |
| [help](help.md) | Visualizza un elenco di comandi disponibili o le informazioni della Guida dettagliate su un comando specificato. |
| [import](import.md) | Importa un gruppo di dischi esterni nel gruppo di dischi del computer locale. |
| [inattivo](inactive.md) | Contrassegna la partizione di sistema o la partizione di avvio con lo stato attivo come inattivo sui dischi MBR (master boot record) di base. |
| [list](list.md) | Visualizza un elenco di dischi, di partizioni in un disco, di volumi in un disco o di dischi rigidi virtuali (VHD). |
| [Unisci vdisk](merge-vdisk.md) | Unisce un disco rigido virtuale differenze (VHD) con il VHD padre corrispondente. |
| [offline](offline.md) | Accetta un volume o il disco online sullo stato offline. |
| [online](online.md) | Accetta un volume o il disco non in linea allo stato online. |
| [recover](recover.md) | Aggiorna lo stato di tutti i dischi in un gruppo di dischi, tenta di ripristinare i dischi in un gruppo di dischi non valido e risincronizza i volumi con mirroring e i volumi RAID-5 con dati non aggiornati. |
| [rem](rem.md) | Consente di aggiungere commenti a uno script. |
| [rimozione](remove.md) | Rimuove un punto di montaggio o lettera di unità da un volume. |
| [ripristinare](repair.md) | Ripristina il volume RAID-5 con lo stato attivo sostituendo l'area del disco non riuscita con il disco dinamico specificato. |
| [Ripeti analisi](rescan.md) | Individua i nuovi dischi aggiunti al computer. |
| [mantenere](retain.md) | Prepara un volume semplice dinamico esistente da utilizzare come un avvio o il volume di sistema. |
| [San](san.md) | Visualizza o imposta i criteri San (Storage Area Network) per il sistema operativo. |
| [Selezionare](select.md) | Sposta lo stato attivo su un disco, una partizione, un volume o disco rigido virtuale (VHD). |
| [imposta ID](set-id.md) | Modifica il campo del tipo di partizione per la partizione con lo stato attivo. |
| [shrink](shrink.md) | Riduce la dimensione del volume selezionato in base al valore specificato. |
| [UniqueId](uniqueid.md) | Visualizza o imposta il GUID partizione GPT (tabella) identificatore firma o del master avvio MBR (record) per il disco con lo stato attivo. |

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Panoramica di Gestione disco](https://docs.microsoft.com/windows-server/storage/disk-management/overview-of-disk-management)

- [Cmdlet di archiviazione in Windows PowerShell](https://docs.microsoft.com/powershell/module/storage/)
