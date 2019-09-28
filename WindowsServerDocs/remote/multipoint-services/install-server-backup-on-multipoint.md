---
title: Installare il backup del server nel server MultiPoint
description: Vengono illustrati i passaggi per installare gli strumenti di backup e ripristino
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e4331370-ba07-4529-92ab-db14a41bfc3b
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 933a24ee91fa1f5ccbe31ff4cb722a7c3eb54e4b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395115"
---
# <a name="install-server-backup-on-your-multipoint-server"></a>Installare il backup del server nel server MultiPoint
Si consiglia di prendere in considerazione un piano di backup e ripristino per i server MultiPoint.
  
Un piano di backup e ripristino efficace è importante per qualsiasi ambiente di dimensioni. Windows Server Backup è una funzionalità di Windows Server 2016 che fornisce un set di procedure guidate e altri strumenti per l'esecuzione di attività di base di backup e ripristino per il server in cui è installato. È possibile utilizzare Windows Server Backup per eseguire il backup di un server completo (tutti i volumi), di volumi selezionati, dello stato del sistema o di specifici file o cartelle, nonché di creare un backup che è possibile utilizzare per ricompilare il sistema.  
  
È possibile ripristinare volumi, cartelle, file, determinate applicazioni e lo stato del sistema. Per le emergenze, ad esempio gli errori del disco rigido, è possibile ricompilare un sistema da zero o utilizzando hardware alternativo. A tale scopo, è necessario disporre di un backup del server completo o dei soli volumi contenenti i file del sistema operativo e l'ambiente di ripristino di Windows. Questo ripristina il sistema completo nel vecchio sistema o in un nuovo disco rigido.  
  
Una funzionalità chiave di Windows Server Backup è la possibilità di pianificare l'esecuzione automatica dei backup.  
  
Utilizzare le procedure seguenti per impostare il tipo di backup richiesto.  
  
## <a name="install-backup-and-recovery-tools"></a>Installare gli strumenti di backup e ripristino  
  
1.  Dalla schermata **Start** aprire **Server Manager**.  
  
2.  Fare clic su **Aggiungi ruoli e funzionalità** per avviare l'aggiunta guidata ruoli. Quindi fare clic su **Avanti** dopo aver esaminato le note **prima di iniziare** .  
  
3.  Selezionare l'opzione di **installazione basata su ruoli o basata su funzionalità** e quindi fare clic su **Avanti**.  
  
4.  Selezionare il computer locale che si sta gestendo, quindi fare clic su **Avanti**.  
  
    Verrà visualizzata l'Aggiunta guidata funzionalità.  
  
5.  Nella pagina **Selezione funzionalità** espandere Windows Server Backup funzionalità, selezionare le caselle di controllo per **Windows Server Backup** e **gli strumenti da riga di comando**e quindi fare clic su **Avanti**.  
  
    > [!NOTE]  
    > In alternativa, se si desidera semplicemente installare lo snap-in e lo strumento da riga di comando Wbadmin, espandere **Windows Server Backup funzionalità**, quindi selezionare la casella di controllo **Windows Server Backup** solo, assicurarsi che la casella di controllo **strumenti da riga di comando** sia deselezionata.  
  
6.  Nella pagina **Conferma selezioni** per l'installazione rivedere le scelte effettuate e quindi fare clic su **Installa**.  
  
    Se si verificano errori durante l'installazione, nella pagina **Risultati installazione** si noteranno gli errori.  
  
7.  Una volta completata l'installazione, dovrebbe essere possibile accedere a questi strumenti di backup e ripristino:  
  
    -   Per aprire lo snap-in Windows Server Backup, nella schermata **Start** Digitare **backup**, quindi fare clic su **Windows Server Backup** nei risultati.  
  
    -   Per avviare lo strumento Wbadmin e visualizzare la sintassi per i comandi: Nella schermata **Start** Digitare **Command**. Nei risultati fare clic con il pulsante destro del mouse su **prompt dei comandi**, scegliere **Esegui come amministratore** nella parte inferiore della pagina e quindi fare clic su **Sì** alla richiesta di conferma. Al prompt dei comandi digitare **Wbadmin/?** e premere INVIO. Verranno visualizzate la sintassi dei comandi e le descrizioni per lo strumento.  
  
## <a name="configure-backups-using-windows-server-backup"></a>Configurare i backup con Windows Server Backup  
  
-   Seguire le istruzioni nel [backup del server](https://technet.microsoft.com/library/cc753528.aspx). 