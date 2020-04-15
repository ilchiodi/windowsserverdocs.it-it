---
title: Assegnare un percorso di cartella del punto di montaggio a un'unità
description: Questo articolo descrive come assegnare un percorso di cartella del punto di montaggio (anziché una lettera di unità) a un'unità.
ms.date: 06/07/2019
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: b2fda216b57fbf036ce20c40b4c8b38d44404f3c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80815534"
---
# <a name="assign-a-mount-point-folder-path-to-a-drive"></a>Assegnare un percorso di cartella del punto di montaggio a un'unità

> **Si applica a:** Windows 10, Windows 8.1, Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puoi usare Gestione disco per assegnare un percorso di cartella del punto di montaggio (anziché una lettera di unità) all'unità. I percorsi di cartella del punto di montaggio sono disponibili solo in cartelle vuote in volumi NTFS di base o dinamici.

## <a name="assigning-a-mount-point-folder-path-to-a-drive"></a>Assegnazione di un percorso di cartella del punto di montaggio a un'unità

> [!NOTE]
> Per completare questi passaggi, devi essere un membro del gruppo **Backup Operators** o **Administrators**.

#### <a name="to-assign-a-mount-point-folder-path-to-a-drive-by-using-the-windows-interface"></a>Per assegnare un percorso di cartella del punto di montaggio a un'unità tramite l'interfaccia di Windows

1.  In Gestione disco fai clic con il pulsante destro del mouse sulla partizione o sul volume in cui vuoi assegnare il percorso di cartella del punto di montaggio. 
2. Fai clic su **Cambia lettera e percorso di unità** e quindi scegli **Aggiungi**. 
3. Fai clic su **Monta in questa cartella vuota NTFS**.
4. Digita il percorso di una cartella vuota in un volume NTFS oppure fai clic su **Sfoglia** per individuare la cartella.

#### <a name="to-assign-a-mount-point-folder-path-to-a-drive-using-a-command-line"></a>Per assegnare un percorso di cartella del punto di montaggio a un'unità tramite la riga di comando

1.  Aprire un prompt dei comandi e digitare `diskpart`.

2.  Al prompt **DISKPART** digita `list volume`, prendendo nota del numero del volume cui vuoi assegnare il percorso.

3.  Al prompt **DISKPART** digita `select volume <volumenumber>`. 

4. Seleziona il volume semplice *volumenumber* cui vuoi assegnare il percorso.

5.  Al prompt **DISKPART** digita `assign [mount=<path>]`.

#### <a name="to-remove-a-mount-point-folder-path-to-a-drive"></a>Per rimuovere un percorso di cartella del punto di montaggio da un'unità

-   Per rimuovere il percorso di cartella del punto di montaggio, fai clic sul percorso e quindi su **Rimuovi**.

| Value | Description |
| --- | --- |
| **list volume** | Visualizza un elenco di volumi di base e dinamici su tutti i dischi. |
| **select volume**        | Seleziona il volume specificato, dove <em>volumenumber</em> è il numero del volume, assegnando lo stato attivo a questo volume. Se non è specificato alcun volume, il comando **select** visualizza un elenco di volumi correnti con stato attivo. Puoi specificare il volume in base al numero, alla lettera di unità o al percorso di cartella del punto di montaggio. Su un disco di base la selezione di un volume imposta anche lo stato attivo della partizione corrispondente.|
| **assign** | <ul><li> Assegna una lettera di unità o un percorso di cartella del punto di montaggio al volume con lo stato attivo. Se non viene specificato alcun percorso di cartella del punto di montaggio o lettera di unità, viene assegnata la lettera di unità disponibile successiva. Se la lettera di unità o il percorso di cartella del punto di montaggio è già in uso, viene generato un errore.</li>  <li>Con il comando **assign** puoi modificare la lettera di unità associata a un'unità rimovibile.</li> <li> Non puoi assegnare lettere di unità a volumi di avvio o volumi che contengono file di paging. Inoltre, non puoi assegnare una lettera di unità a una partizione OEM (Original Equipment Manufacturer), a una partizione di sistema EFI o a qualsiasi partizione GPT diversa da una partizione dati di base.</li></ul> |
| **mount=** <em>path</em> | Specifica una cartella NTFS vuota esistente in cui si troverà l'unità montata.  |

## <a name="additional-considerations"></a>Altre considerazioni

-   Se stai amministrando un computer locale o remoto, puoi selezionare le cartelle NTFS in questo computer.
-   I percorsi di cartella del punto di montaggio sono disponibili solo in cartelle vuote in volumi NTFS di base o dinamici.
-   Per modificare un percorso di cartella del punto di montaggio, rimuovilo e quindi crea un nuovo percorso di cartella utilizzando la nuova posizione. Non puoi modificare direttamente il percorso di cartella del punto di montaggio.
-   Nell'assegnare un percorso di cartella del punto di montaggio a un'unità, usa **Visualizzatore eventi** per verificare la presenza nel Registro di sistema di errori o avvertenze per qualsiasi Servizio cluster che indica errori nel percorso di cartella del punto di montaggio. Questi errori verranno elencati come **ClusSvc** nella colonna **Origine** e **Risorsa disco fisico** nella colonna **Categoria**.
-   Puoi anche creare un'unità montata tramite il comando [mountvol](https://go.microsoft.com/fwlink/?linkid=64111).

## <a name="see-also"></a>Vedere anche
-   [Notazione della sintassi della riga di comando](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)


