---
title: Gestire dischi rigidi virtuali
description: In questo articolo viene descritto come gestire dischi rigidi virtuali
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 2e371710752d59ebc7a1f8aa2dad3d9189872c47
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827112"
---
# <a name="manage-virtual-hard-disks-vhd"></a>Gestire dischi rigidi virtuali

> **Si applica a:** Windows 10, Windows 8.1, Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento viene descritto come creare, collegare e scollegare dischi rigidi virtuali con Gestione disco. I dischi rigidi virtuali sono file su disco rigido virtualizzati che, una volta montati, vengono visualizzati ed eseguiti praticamente in modo identico a un disco rigido fisico. Sono più comunemente utilizzati con le macchine virtuali Hyper-V. 

## <a name="viewing-vhds-in-disk-management"></a>Visualizzazione di dischi rigidi virtuali in Gestione disco

I dischi rigidi virtuali vengono visualizzati come dischi fisici in Gestione disco. Quando è stato collegato un disco rigido virtuale (ovvero reso disponibile per l'utilizzo nel sistema), viene visualizzato in blu. Se il disco viene disconnesso (vale a dire, reso non disponibile), la relativa icona ritorna grigia.

## <a name="creating-a-vhd"></a>Creazione di un disco rigido virtuale

> [!NOTE]
> Per completare questi passaggi, devi essere un membro del gruppo **Backup Operators** o **Administrators**.

**Per creare un disco rigido virtuale**

1.  Dal menu **Azione** scegliere **Crea file VHD**.

2.  Nella finestra di dialogo **Crea e collega disco rigido virtuale**, specificare il percorso nel computer fisico in cui archiviare il file VHD e le dimensioni del disco rigido virtuale.

3.  In **Formato disco rigido virtuale** selezionare **A espansione dinamica** o **A dimensione fissa** e quindi fare clic su **OK**.

## <a name="attaching-and-detaching-a-vhd"></a>Collegamento e scollegamento di un disco rigido virtuale

Per rendere disponibile per l'utilizzo un disco rigido virtuale (uno appena creato o un altro disco rigido virtuale esistente): 

1. Nel menu **Azione**, selezionare **Collega file VHD**.

2. Specificare il percorso del disco rigido virtuale, utilizzando un percorso completo.

Per scollegare il disco rigido virtuale, rendendo non disponibili: Fare clic su disco, selezionare **Scollega file VHD**, quindi fare clic su **OK**. Lo scollegamento di un disco rigido virtuale non elimina il disco rigido virtuale o tutti i dati in esso archiviati.

## <a name="additional-considerations"></a>Considerazioni aggiuntive

-   Il percorso che specifica il percorso per il disco rigido virtuale deve essere completo e non può essere il \\directory di Windows.
-   La dimensione minima per un disco rigido virtuale è 3 megabyte (MB).
-   Un disco rigido virtuale può essere solo un disco di base.
-   Poiché un disco rigido virtuale viene inizializzato durante la creazione, la creazione di un disco rigido virtuale di grande dimensione fissa potrebbe richiedere tempo.
