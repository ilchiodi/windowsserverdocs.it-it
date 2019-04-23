---
title: Assegnare un percorso di cartella del punto di montaggio a un'unità.
description: Questo articolo descrive come assegnare un percorso di cartella del punto di montaggio (anziché una lettera di unità) a un'unità.
keywords: virtualizzazione, sicurezza, malware
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: a25dac6e49d6e9aee2efa043999162262c5bb791
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870872"
---
# <a name="assign-a-mount-point-folder-path-to-a-drive"></a>Assegnare un percorso di cartella del punto di montaggio a un'unità

> **Si applica a:** Windows 10, Windows 8.1, Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È possibile usare Gestione disco per assegnare un percorso di cartella del punto di montaggio (anziché una lettera di unità) all'unità. I percorsi delle cartelle del punto di montaggio sono disponibili solo nelle cartelle vuote in volumi NTFS base o dinamici.

## <a name="assigning-a-mount-point-folder-path-to-a-drive"></a>Assegnazione di un percorso di cartella del punto di montaggio a un'unità

-   [Tramite l'interfaccia di Windows](#BKMK_WINUI)
-   [Tramite la riga di comando](#BKMK_CMD)

> [!NOTE]
> Per completare questi passaggi, devi essere un membro del gruppo **Backup Operators** o **Administrators**.

**Per assegnare un percorso di cartella del punto di montaggio a un'unità tramite l'interfaccia Windows:**
<a id="BKMK_WINUI"></a>

1.  In Gestione disco, fare clic con il pulsante destro del mouse sulla partizione o volume in cui si desidera assegnare il percorso della cartella del punto di montaggio. 
2. Fare clic su **Cambia lettera e percorso di unità** e quindi fare clic su **Aggiungi**. 
3. Fare clic su **Monta in questa cartella vuota NTFS**.
4. Digitare il percorso per una cartella vuota in un volume NTFS oppure fare clic su **Sfoglia** per individuarla.

<a id="BKMK_CMD"></a>
#### <a name="to-assign-a-mount-point-folder-path-to-a-drive-using-a-command-line"></a>Per assegnare un percorso di cartella del punto di montaggio a un'unità tramite la riga di comando
1.  Aprire un prompt dei comandi e digitare `diskpart`.

2.  Al prompt **DISKPART** digitare `list volume`, prendendo nota del numero del volume su cui si desidera assegnare il percorso.

3.  Al prompt **DISKPART** digita `select volume <volumenumber>`. 

4. Selezionare il volume semplice *volumenumber* a cui si desidera assegnare il percorso.

5.  Al prompt **DISKPART** digita `assign [mount=<path>]`.

#### <a name="to-remove-a-mount-point-folder-path-to-a-drive"></a>Per rimuovere un percorso di cartella del punto di montaggio da un'unità

-   Per rimuovere il percorso di cartella del punto di montaggio, fare clic su di esso e quindi fare clic su **Rimuovi**.

<br />

| Value | Descrizione |
| --- | --- |
| <p>**volume di elenco**</p> | <p>Visualizza un elenco di volumi di base e dinamici su tutti i dischi.</p> |
| <p>**Selezionare volume**</p>        | <p>Seleziona il volume specificato, dove <em>volumenumber</em> è il numero del volume, assegnandogli lo stato attivo. Se non è specificato alcun volume, il comando **select** visualizza un elenco di volumi correnti con stato attivo. È possibile specificare il volume in base al numero, alla lettera di unità o al percorso di cartella del punto di montaggio. Su un disco di base la selezione di un volume imposta anche lo stato attivo della partizione corrispondente.</p>|
| <p>**assign**</p> | <p><ul><li> Assegna una lettera di unità o un percorso di cartella del punto di montaggio al volume con stato attivo. Se non viene specificato alcun percorso di cartella del punto di montaggio o lettera di unità, viene assegnata la lettera di unità disponibile successiva. Se la lettera di unità o il percorso di cartella del punto di montaggio è già in uso, viene generato un errore.</li> </p> <p><li>Il comando **assign** può essere usato per modificare la lettera di unità associata a un'unità rimovibile.</li> </p><p><li> Non è possibile assegnare lettere di unità a volumi di avvio, o volumi che contengono file di paging. Inoltre, non è possibile assegnare una lettera di unità a una partizione OEM (Original Equipment Manufacturer), partizione di sistema EFI o a qualsiasi partizione GPT diversa da una partizione dati di base.</p></li></ul> |
| <p>**mount=** <em>path</em></p> | <p>Specifica una cartella NTFS vuota esistente in cui risiederà l'unità montata.</p>  |

## <a name="additional-considerations"></a>Considerazioni aggiuntive

-   Se si sta amministrando un computer locale o remoto, è possibile sfogliare le cartelle NTFS in tale computer.
-   I percorsi delle cartelle del punto di montaggio sono disponibili solo nelle cartelle vuote in volumi NTFS base o dinamici.
-   Per modificare un percorso di cartella del punto di montaggio, rimuoverlo e quindi creare un nuovo percorso di cartella utilizzando la nuova posizione. Non è possibile modificare direttamente il percorso di cartella del punto di montaggio.
-   Quando si assegna un percorso di cartella del punto di montaggio a un'unità, usare **Visualizzatore eventi** per verificare la presenza nel Registro di sistema di errori o avvertenze per il servizio Cluster che indicano errori nel percorso di cartella del punto di montaggio. Tali errori verranno elencati come **ClusSvc** nella colonna **Origine** e **Risorsa disco fisico** nella colonna **Categoria**.
-   È inoltre possibile creare un'unità montata utilizzando il comando [mountvol](https://go.microsoft.com/fwlink/?linkid=64111).

## <a name="see-also"></a>Vedere anche
-   [Notazione della sintassi della riga di comando](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)


