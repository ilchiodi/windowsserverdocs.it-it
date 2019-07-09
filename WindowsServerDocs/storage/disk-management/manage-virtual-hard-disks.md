---
title: Gestire dischi rigidi virtuali
description: Questo articolo descrive come gestire dischi rigidi virtuali
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 2e371710752d59ebc7a1f8aa2dad3d9189872c47
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/17/2019
ms.locfileid: "63751759"
---
# <a name="manage-virtual-hard-disks-vhd"></a>Gestire dischi rigidi virtuali

> **Si applica a:** Windows 10, Windows 8.1, Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento descrive come creare e rendere visibili e non visibili dischi rigidi virtuali con Gestione disco. I dischi rigidi virtuali sono file su disco rigido virtualizzati che, una volta montati, vengono visualizzati ed eseguiti praticamente in modo identico a un disco rigido fisico. Sono usati più comunemente con le macchine virtuali Hyper-V. 

## <a name="viewing-vhds-in-disk-management"></a>Visualizzazione di dischi rigidi virtuali in Gestione disco

I dischi rigidi virtuali vengono visualizzati come dischi fisici in Gestione disco. Dopo che un disco rigido virtuale è stato reso visibile (ovvero reso disponibile per l'uso nel sistema), viene visualizzato in blu. Se il disco viene reso non visibile (ovvero non disponibile), la relativa icona diventa di nuovo grigia.

## <a name="creating-a-vhd"></a>Creazione di un disco rigido virtuale

> [!NOTE]
> Per completare questi passaggi, devi essere un membro del gruppo **Backup Operators** o **Administrators**.

**Per creare un disco rigido virtuale**

1.  Scegli **Crea file VHD** dal menu **Azione**.

2.  Nella finestra di dialogo **Crea e collega disco rigido virtuale** specifica il percorso nel computer fisico in cui vuoi archiviare il file VHD e le dimensioni del disco rigido virtuale.

3.  In **Formato disco rigido virtuale** selezionare **A espansione dinamica** o **A dimensione fissa** e quindi fare clic su **OK**.

## <a name="attaching-and-detaching-a-vhd"></a>Come rendere visibile o non visibile un disco rigido virtuale

Per rendere disponibile per l'uso un disco rigido virtuale (appena creato o già esistente): 

1. Scegli **Collega file VHD** dal menu **Azione**.

2. Specifica la posizione del disco rigido virtuale usando un percorso completo.

Per rendere non visibile il disco rigido virtuale, ovvero non disponibile: fai clic con il pulsante destro del mouse sul disco, scegli **Scollega file VHD** e quindi fai clic su **OK**. Quando un disco rigido virtuale viene reso non visibile, non viene eliminato e non vengono eliminati i dati in esso archiviati.

## <a name="additional-considerations"></a>Considerazioni aggiuntive

-   Il percorso che specifica la posizione del disco rigido virtuale deve essere completo e non può essere nella directory \\Windows.
-   La dimensione minima per un disco rigido virtuale è 3 megabyte (MB).
-   Un disco rigido virtuale può essere solo un disco di base.
-   Poiché un disco rigido virtuale viene inizializzato durante la creazione, la creazione di un grande disco rigido virtuale a dimensione fissa potrebbe richiedere tempo.
