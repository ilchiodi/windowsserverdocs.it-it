---
title: Eseguire la migrazione di Windows Server Essentials a un nuovo Hardware
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f695ae90-3160-407b-bebf-9e460f22c86d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 61106439a63a75143a9cca0989c70370adfedd38
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="migrate-windows-server-essentials-to-new-hardware"></a>Eseguire la migrazione di Windows Server Essentials a un nuovo Hardware

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Questa guida descrive come eseguire la migrazione di un dominio Windows Server® 2012 Essentials esistente a Windows Server Essentials su hardware nuovo e quindi trasferire le impostazioni e i dati. Questa guida viene inoltre descritto come rimuovere il server esistente dalla rete Windows Server Essentials dopo aver completato la migrazione.  
  
> [!NOTE]
>  Per evitare problemi durante la migrazione, il team di sviluppo del prodotto Windows Server Essentials consiglia di leggere questo documento prima di iniziare la migrazione.  
  
> [!NOTE]

>  Per eseguire la migrazione di dati del server per la versione più recente di Windows Server Essentials, vedere [la migrazione a Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).  

  
## <a name="additional-resources"></a>Risorse aggiuntive  
 Per i collegamenti a informazioni aggiuntive, strumenti e risorse della community che consentono di agevolare il processo di migrazione, visitare il [migrazione di Windows Small Business Server](https://go.microsoft.com/fwlink/?LinkId=217520) sito Web.  
  
## <a name="terms-and-definitions"></a>Termini e definizioni  
 **Server di origine:** il server esistente da cui vengono migrati dati e impostazioni.  
  
 **Server di destinazione:** il nuovo server nel quale vengono migrati dati e impostazioni.  
  
## <a name="migration-process-summary"></a>Riepilogo del processo di migrazione  
 Questa Guida alla migrazione include i passaggi seguenti:  
  

1.  [Preparare la migrazione di Server di origine per Windows Server Essentials](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md).  È necessario assicurarsi che il Server di origine e la rete siano pronti per la migrazione. In questa sezione illustra la backup dei Server di origine, la valutazione dell'integrità di sistema del Server di origine, installare il service pack e le correzioni più recenti e verificare la configurazione di rete.  
  
2.  [Installare Windows Server Essentials in modalità di migrazione](Install-Windows-Server-Essentials-in-migration-mode.md).  In questa sezione descrive i passaggi da eseguire per installare Windows Server Essentials nel Server di destinazione in modalità di migrazione.  
  
3.  [Aggiungere computer al nuovo server Windows Server Essentials](Join-computers-to-the-new-Windows-Server-Essentials-server.md).  Questa sezione viene spiegato come aggiungere i computer client al nuovo server Windows Server Essentials e aggiornare le impostazioni di criteri di gruppo.  
  
4.  [Spostare dati e impostazioni per la migrazione di Server di destinazione per Windows Server Essentials](Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  In questa sezione fornisce informazioni sulla migrazione di dati e impostazioni dal Server di origine.  
  
5.  [Configurare il reindirizzamento cartelle nel Server di destinazione Windows Server Essentials](Configure-folder-redirection-on-the-Windows-Server-Essentials-Destination-Server.md).  Se il reindirizzamento cartelle è abilitato nel Server di origine, è possibile abilitare il reindirizzamento cartelle nel Server di destinazione e quindi eliminare la vecchia impostazione di criteri di gruppo reindirizzamento cartelle.  
  
6.  [Abbassare di livello e rimuovere il Server di origine dalla nuova rete Windows Server Essentials](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md).  Prima di rimuovere il Server di origine dalla rete, è necessario forzare un aggiornamento di criteri di gruppo e abbassare di livello il Server di origine.  
  
7.  [Eseguire attività post-migrazione per la migrazione di Windows Server Essentials](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md).  Dopo aver completato la migrazione di tutti i dati e le impostazioni di Windows Server Essentials, si desidera mappare i computer autorizzati agli account utente.  
  
8.  [Eseguire Windows Server Essentials Best Practices Analyzer](Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md).  Dopo aver completato la migrazione delle impostazioni e i dati a Windows Server Essentials, è consigliabile scaricare ed eseguire Windows Server Essentials BPA.  
  
 Molte delle procedure di migrazione è necessario aprire una finestra del prompt dei comandi come amministratore.  
  
#### <a name="to-open-a-command-prompt-window-as-an-administrator"></a>Per aprire una finestra del prompt dei comandi come amministratore  
  
1.  Nel **Start** dello schermo, nella casella di ricerca, digitare **cmd**.  
  
2.  Nell'elenco dei risultati, fare doppio clic su **cmd**, quindi fare clic su **Esegui come amministratore**.
