---
title: Installare Server Backup in MultiPoint server
description: Illustra i passaggi per installare gli strumenti di backup e ripristino
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e4331370-ba07-4529-92ab-db14a41bfc3b
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 51932c5f0796cfd757d3322e10c17de2a3081f4c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832492"
---
# <a name="install-server-backup-on-your-multipoint-server"></a>Installare Server Backup in MultiPoint server
È consigliabile considerare l'ipotesi di un piano di backup e ripristino per i server MultiPoint.
  
Un buon piano di backup e ripristino è importante per qualsiasi ambiente di dimensioni. Windows Server Backup è una funzionalità di Windows Server 2016 che fornisce un set di procedure guidate e altri strumenti per eseguire semplici attività di backup e ripristino per il server in cui è installato. È possibile utilizzare Windows Server Backup per eseguire il backup di un intero server (tutti i volumi), volumi selezionati, dello stato del sistema, o specifico file o delle cartelle e per creare un backup che è possibile usare per ricostruire il sistema.  
  
È possibile ripristinare volumi, cartelle, file, determinate applicazioni e lo stato del sistema. Inoltre, per emergenze, ad esempio gli errori del disco rigido, è possibile ricompilare un sistema da zero o usando hardware alternativo. A tale scopo, è necessario disporre di un backup del server completo o dei soli volumi contenenti i file del sistema operativo e l'ambiente ripristino Windows. Ciò consente di ripristinare il sistema completo su quello precedente o in un nuovo disco rigido.  
  
Una funzionalità chiave di Windows Server Backup è la possibilità di pianificare i backup per l'esecuzione automatica.  
  
Utilizzare le procedure seguenti per impostare il tipo di backup che è necessario.  
  
## <a name="install-backup-and-recovery-tools"></a>Installare gli strumenti di backup e ripristino  
  
1.  Dal **avviare** schermata, aprire **Server Manager**.  
  
2.  Fare clic su **Aggiungi ruoli e funzionalità** per avviare l'aggiunta guidata ruoli. Quindi fare clic su **successivo** dopo aver verificato le **prima di iniziare** note.  
  
3.  Selezionare il **in base al ruolo o funzionalità basati su installazione** opzione e quindi fare clic su **successivo**.  
  
4.  Selezionare il computer locale che si sta gestendo e fare clic su **successivo**.  
  
    Verrà visualizzata l'Aggiunta guidata funzionalità.  
  
5.  Nel **Selezione funzionalità** pagina, espandere le funzionalità di Windows Server Backup, selezionare le caselle di controllo **Windows Server Backup** e **Command-line Tools**e quindi fare clic su  **Avanti**.  
  
    > [!NOTE]  
    > O, se si desidera installare solo lo snap-in e lo strumento da riga di comando Wbadmin, espandere **funzionalità di Windows Server Backup**e quindi selezionare la **Windows Server Backup** solo casella di controllo, assicurarsi che il **Command-line Tools** casella di controllo è deselezionata.  
  
6.  Nel **Conferma selezioni per l'installazione** pagina, rivedere le scelte effettuate e quindi fare clic su **installare**.  
  
    Se si verificano errori durante l'installazione, il **i risultati dell'installazione** noti gli errori di pagina.  
  
7.  Dopo l'installazione viene completata correttamente, deve essere in grado di accedere a questi strumenti di backup e ripristino:  
  
    -   Per aprire il Windows Server Backup snap-in, scegliere il **avviare** digitare **backup**e quindi fare clic su **Windows Server Backup** nei risultati.  
  
    -   Per avviare lo strumento Wbadmin e visualizzare la sintassi per i relativi comandi: Nel **avviare** digitare **comando**. Nei risultati, fare doppio clic su **prompt dei comandi**, fare clic su **Esegui come amministratore** nella parte inferiore della pagina e quindi fare clic su **Sì** alla richiesta di conferma. Al prompt dei comandi, digitare **wbadmin /?** e premere INVIO. Dovrebbe essere sintassi dei comandi e le descrizioni per lo strumento.  
  
## <a name="configure-backups-using-windows-server-backup"></a>Configurare i backup con Windows Server Backup  
  
-   Seguire le indicazioni fornite in [Backing Up Your Server](https://technet.microsoft.com/library/cc753528.aspx). 