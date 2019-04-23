---
title: Spostare dischi in un altro computer
description: Questo articolo illustra come spostare dischi in un altro computer
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 6b235ce8e5b936940629d5977a17bbc729efbe82
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854002"
---
# <a name="move-disks-to-another-computer"></a>Spostare dischi in un altro computer

> **Si applica a:** Windows 10, Windows 8.1, Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questa sezione descrive i passaggi da eseguire per spostare dischi in un altro computer e le relative considerazioni. È consigliabile stampare questa procedura o scriverne i passaggi prima di tentare di spostare dischi da un computer a un altro.

> [!NOTE]
> Per completare questi passaggi, devi essere un membro del gruppo **Backup Operators** o **Administrators**.

## <a name="verify-volume-health"></a>Verificare l'integrità del volume

Utilizzare Gestione disco per assicurarsi che lo stato dei volumi sui dischi sia **integro**. Se lo stato non è **Integro**, ripristinare i volumi prima di spostare i dischi.

Per verificare lo stato del volume, dal menu **Visualizza**, controllare la colonna **Stato** nella vista **Elenco volumi** o nelle **dimensioni del volume** e le informazioni sul file system in **Visualizzazione grafica**.

## <a name="uninstall-the-disks"></a>Disinstallare i dischi

Disinstallare i dischi che si desidera spostare mediante Gestione dispositivi.

**Per disinstallare i dischi**

1.  Aprire Gestione dispositivi in Gestione computer.

2.  Nell'elenco dei dispositivi, fare doppio clic su **Unità disco**.

3.  Fare clic con il pulsante destro del mouse sui dischi da disinstallare e quindi fare clic su **Disinstalla**.

4.  Nella finestra di dialogo **Conferma rimozione dispositivo** fare clic su **OK**.

## <a name="remove-dynamic-disks"></a>Rimuovere dischi dinamici

1. Se i dischi che si desidera spostare sono dinamici dischi, in Gestione disco, fare clic con il pulsante destro del mouse sui dischi che si desidera spostare e quindi fare clic su **Rimuovi disco**.

2. Dopo la rimozione di dischi dinamici o se si stanno spostando dischi di base, è possibile eseguirne la disconnessione fisica. Se i dischi sono esterni, è ora possibile scollegarli dal computer. Se i dischi sono interni, spegnere il computer e quindi rimuoverli fisicamente.

## <a name="install-disks-in-the-new-computer"></a>Installare i dischi nel nuovo computer

1. Se i dischi sono esterni, collegarli al computer. Se i dischi sono interni, assicurarsi che il computer sia disattivato e quindi installare i dischi in tale computer.

2. Avviare il computer che contiene i dischi spostati e seguire le istruzioni nella finestra di dialogo Nuovo Hardware.

## <a name="detect-new-disks"></a>Rilevare i nuovi dischi

1. Sul nuovo computer, aprire Gestione disco. 
2. Fare clic su **Azione**, quindi su **Ripeti analisi dischi**.
3. Fare clic con il pulsante destro del mouse su tutti i dischi contrassegnati come **Esterno**. 
4. Fare clic su **Importazione dischi esterni** e quindi seguire le istruzioni visualizzate.

## <a name="additional-considerations"></a>Considerazioni aggiuntive

-   Quando vengono spostati in un altro computer, i volumi di base ricevono la successiva lettera di unità disponibile in tale computer. 
-   I volumi dinamici mantengono la lettera di unità che avevano nel computer precedente. Se un volume dinamico non aveva una lettera di unità nel computer precedente, non riceverà una lettera di unità quando viene spostato in un altro computer. Se la lettera di unità è già utilizzata nel computer in cui viene spostato un volume, il volume riceve la successiva lettera di unità disponibile.

-   Se un amministratore ha utilizzato il comando **mountvol /n** o **diskpart automount** per impedire l'aggiunta di nuovi volumi al sistema, i volumi spostati da un altro computer sono non vengono montati e non ricevono una lettera di unità. Per utilizzare il volume, è necessario montare il volume manualmente e assegnare una lettera di unità con Gestione disco o i comandi **DiskPart** e **mountvol**.

-   Se vengono spostati volumi con spanning, archiviati con striping, con mirroring o RAID-5, si consiglia di spostare insieme tutti i dischi che contengono il volume. In caso contrario, i volumi nei dischi non possono passare allo stato online e non saranno accessibili se non per eliminarli.

-   È possibile spostare più dischi da diversi computer a un computer installando i dischi, aprendo Gestione disco, facendo clic con il pulsante destro del mouse su uno qualsiasi dei nuovi dischi e quindi facendo clic su **Importazione dischi esterni**. Quando si importano più dischi da computer diversi, importare sempre tutti i dischi da un computer alla volta. Ad esempio, se si desidera spostare i dischi da due computer, importare tutti i dischi dal primo computer e quindi importare tutti i dischi dal secondo computer.

-   Gestione disco descrive la condizione dei volumi nei dischi prima che vengano importati. Leggere attentamente queste informazioni. Se si verificano problemi, queste informazioni informeranno su cosa accadrà a ciascun volume su tali dischi dopo che i dischi sono stati importati.

-   Se si sposta un disco GPT (Tabella di partizione GUID, GUID Partition Table) contenente il sistema operativo Windows in un computer basato su x86 o x64, è possibile accedere ai dati, ma non è possibile eseguire l'avvio da tale sistema operativo.