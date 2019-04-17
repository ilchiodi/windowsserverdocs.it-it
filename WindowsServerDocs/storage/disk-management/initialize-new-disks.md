---
title: Inizializzare nuovi dischi
description: Questo articolo descrive come inizializzare nuovi dischi
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 587553746d45eceab654efd4d120038088d32991
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="initialize-new-disks"></a>Inizializzare nuovi dischi

> **Si applica a:** Windows 10, Windows 8.1, Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

> [!NOTE]
> Per completare questi passaggi, è necessario essere membri almeno del gruppo **Backup Operators** o **Administrators**.

## <a name="to-initialize-new-disks"></a>Per inizializzare nuovi dischi
1.  In Gestione disco fare clic con il pulsante destro del mouse sul disco desiderato e quindi scegliere **Inizializza disco**.

2.  Nella finestra di dialogo **Inizializza disco** selezionare il disco o i dischi da inizializzare. È possibile scegliere se utilizzare MBR (Record di avvio principale, Master Boot Record) o GPT (Tabella di partizione GUID, GUID Partition Table) come stile di partizione.

## <a name="additional-considerations"></a>Considerazioni aggiuntive

-   I nuovi dischi vengono contrassegnati come **Non inizializzato**. Prima di poter utilizzare un disco, è necessario inizializzarlo. Se si avvia Gestione disco dopo l'aggiunta di un disco, viene visualizzata l'inizializzazione guidata del disco in modo che sia possibile inizializzare il disco.

> [!NOTE]
> Il nuovo disco viene inizializzato come disco di base.

