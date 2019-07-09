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
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/17/2019
ms.locfileid: "63751729"
---
# <a name="move-disks-to-another-computer"></a>Spostare dischi in un altro computer

> **Si applica a:** Windows 10, Windows 8.1, Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questa sezione descrive i passaggi da eseguire per spostare dischi in un altro computer e le relative considerazioni. È consigliabile stampare questa procedura o scriverne i passaggi prima di tentare di spostare dischi da un computer a un altro.

> [!NOTE]
> Per completare questi passaggi, devi essere un membro del gruppo **Backup Operators** o **Administrators**.

## <a name="verify-volume-health"></a>Verificare l'integrità del volume

Usa Gestione disco per assicurarti che lo stato dei volumi sui dischi sia **Integro**. Se lo stato non è **Integro**, ripara i volumi prima di spostare i dischi.

Per verificare lo stato dei volumi, nel menu **Visualizza** controlla la colonna **Stato** nella visualizzazione **Elenco volumi** oppure sotto le informazioni sul file system e le **dimensioni del volume** in **Visualizzazione grafica**.

## <a name="uninstall-the-disks"></a>Disinstallare i dischi

Disinstalla i dischi che vuoi spostare tramite Gestione dispositivi.

**Per disinstallare i dischi**

1.  Apri Gestione dispositivi in Gestione computer.

2.  Nell'elenco dei dispositivi fai doppio clic su **Unità disco**.

3.  Fai clic con il pulsante destro del mouse sui dischi da disinstallare e quindi scegli **Disinstalla**.

4.  Nella finestra di dialogo **Conferma rimozione dispositivo** fare clic su **OK**.

## <a name="remove-dynamic-disks"></a>Rimuovere dischi dinamici

1. Se i dischi da spostare sono dischi dinamici, in Gestione disco fai clic con il pulsante destro del mouse sui dischi da spostare e quindi scegli **Rimuovi disco**.

2. Dopo la rimozione dei dischi dinamici o lo spostamento di dischi di base, puoi eseguirne la disconnessione fisica. Se i dischi sono esterni, ora puoi scollegarli dal computer. Se i dischi sono interni, spegni il computer e quindi rimuovili fisicamente.

## <a name="install-disks-in-the-new-computer"></a>Installare i dischi nel nuovo computer

1. Se i dischi sono esterni, collegali al computer. Se i dischi sono interni, assicurati che il computer sia spento e quindi installa fisicamente i dischi nel computer.

2. Avvia il computer che contiene i dischi spostati e segui le istruzioni nella finestra di dialogo Trovato nuovo hardware.

## <a name="detect-new-disks"></a>Rilevare i nuovi dischi

1. Nel nuovo computer apri Gestione disco. 
2. Fai clic su **Azione** e quindi su **Ripeti analisi dischi**.
3. Fai clic con il pulsante destro del mouse su tutti i dischi contrassegnati come **Esterno**. 
4. Fai clic su **Importazione dischi esterni** e quindi segui le istruzioni visualizzate.

## <a name="additional-considerations"></a>Considerazioni aggiuntive

-   Quando vengono spostati in un altro computer, i volumi di base ricevono la successiva lettera di unità disponibile in tale computer. 
-   I volumi dinamici mantengono la lettera di unità che avevano nel computer precedente. Se un volume dinamico non aveva una lettera di unità nel computer precedente, non riceverà una lettera di unità quando viene spostato in un altro computer. Se la lettera di unità è già usata nel computer in cui viene spostato un volume, il volume riceverà la successiva lettera di unità disponibile.

-   Se un amministratore ha usato il comando **mountvol /n** o **diskpart automount** per impedire l'aggiunta di nuovi volumi al sistema, i volumi spostati da un altro computer non verranno montati e non riceveranno una lettera di unità. Per usare il volume, devi montarlo manualmente e assegnargli una lettera di unità con Gestione disco o i comandi **DiskPart** e **mountvol**.

-   Se sposti i volumi con spanning, striping, mirroring o RAID-5, è consigliabile spostare insieme tutti i dischi che contengono il volume. In caso contrario, i volumi sui dischi non possono passare allo stato online e non saranno accessibili se non per l'eliminazione.

-   Puoi spostare più dischi da computer diversi in un computer installando i dischi, aprendo Gestione disco, facendo clic con il pulsante destro del mouse su uno qualsiasi dei nuovi dischi e quindi scegliendo **Importazione dischi esterni**. Per importare più dischi da computer diversi, importa sempre tutti i dischi da un computer alla volta. Se ad esempio vuoi spostare dischi da due computer, importa tutti i dischi dal primo computer e quindi importa tutti i dischi dal secondo computer.

-   Gestione disco descrive la condizione dei volumi sui dischi prima che vengano importati. Leggi attentamente queste informazioni. Se si verificano problemi, queste informazioni indicheranno cosa accadrà a ogni volume su tali dischi dopo che i dischi sono stati importati.

-   Se sposti un disco GPT (GUID Partition Table, tabella di partizione GUID) contenente il sistema operativo Windows in un computer basato su x86 o x64, puoi accedere ai dati, ma non puoi eseguire l'avvio da tale sistema operativo.