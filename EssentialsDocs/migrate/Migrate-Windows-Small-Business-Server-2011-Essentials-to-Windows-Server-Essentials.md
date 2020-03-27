---
title: Eseguire la migrazione da Windows Small Business Server 2011 Essentials a Windows Server Essentials
description: Viene descritto come usare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 32fc90d8-31c5-4c7e-9fe3-483cf3c35f78
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 101a0e2e6e9e20c5a33e9daf2ec97c5b0984cb66
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318909"
---
# <a name="migrate-windows-small-business-server-2011-essentials-to-windows-server-essentials"></a>Eseguire la migrazione da Windows Small Business Server 2011 Essentials a Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Questa guida descrive come eseguire la migrazione di un dominio Windows Small Business Server 2011 Essentials esistente a Windows Server® 2012 Essentials e quindi eseguire la migrazione delle impostazioni e dei dati. Questa guida descrive anche come rimuovere il server esistente dalla rete di Windows Server Essentials al termine della migrazione.  
  
> [!NOTE]
>  Per evitare problemi durante la migrazione, il team di sviluppo del prodotto Windows Server Essentials consiglia di leggere questo documento prima di iniziare la migrazione.  
> 
> [!NOTE]
> 
>  Per eseguire la migrazione dei dati del server alla versione più recente di Windows Server Essentials, vedere [eseguire la migrazione a Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).  
> 
>  Per eseguire la migrazione dei dati del server alla versione più recente di Windows Server Essentials, vedere [eseguire la migrazione a Windows Server Essentials](../migrate/Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).  

  
## <a name="additional-resources"></a>Risorse aggiuntive  
 Visitare il sito Web relativo alla [migrazione di Windows Small Business Server](https://go.microsoft.com/fwlink/?LinkId=217520) per accedere a informazioni aggiuntive, strumenti e risorse della community che consentono di agevolare il processo di migrazione.  
  
## <a name="terms-and-definitions"></a>Termini e definizioni  
 **Server di origine:** Server esistente da cui si esegue la migrazione delle impostazioni e dei dati.  
  
 **Server di destinazione:** Il nuovo server in cui si esegue la migrazione delle impostazioni e dei dati.  
  
## <a name="migration-process-summary"></a>Riepilogo del processo di migrazione  
 In questa Guida alla migrazione sono illustrati i passaggi seguenti:  
  

1.  [Preparare il server di origine per la migrazione a Windows Server Essentials](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md).  Verificare che il server di origine e la rete siano pronti per la migrazione. In questa sezione viene illustrato come eseguire il backup del server di origine, valutare l'integrità del sistema del server di origine, installare gli aggiornamenti e i Service Pack più recenti e verificare la configurazione della rete.  
  
2.  [Installare Windows Server Essentials in modalità di migrazione](Install-Windows-Server-Essentials-in-migration-mode.md).  Questa sezione descrive i passaggi da eseguire per installare Windows Server Essentials nel server di destinazione in modalità di migrazione.  
  
3.  [Aggiungere i computer al nuovo server di Windows Server Essentials](Join-computers-to-the-new-Windows-Server-Essentials-server.md).  Questa sezione illustra l'aggiunta di computer client al nuovo server di Windows Server Essentials e l'aggiornamento delle impostazioni di Criteri di gruppo.  
  
4.  [Spostare i dati e le impostazioni di SBS 2011 Essentials nel server di destinazione](Move-Windows-SBS-2011-Essentials-to-the-Destination-Server-for-migration.md).  In questa sezione vengono fornite informazioni in merito alla migrazione di dati e impostazioni dal server di origine.  
  
5.  [Abilitare il reindirizzamento cartelle nel server di destinazione Windows Server Essentials](Enable-folder-redirection-on-the-Windows-Server-Essentials-Destination-Server.md).  Se il reindirizzamento cartelle è abilitato nel server di origine, è possibile abilitarlo nel server di destinazione e quindi eliminare l'impostazione dei Criteri di gruppo per il reindirizzamento cartelle.  
  
6.  [Abbassare di e rimuovere il server di origine dalla nuova rete Windows Server Essentials](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md).  Prima di rimuovere il server di origine dalla rete, è necessario forzare un aggiornamento dei Criteri di gruppo e abbassare di livello il server di origine.  
  
7.  [Eseguire le attività post-migrazione per la migrazione a Windows Server Essentials](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md).  Al termine della migrazione di tutte le impostazioni e i dati in Windows Server Essentials, è possibile eseguire il mapping dei computer autorizzati agli account utente.  
  
8.  [Eseguire il Best Practices Analyzer di Windows Server Essentials](Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md).  Al termine della migrazione delle impostazioni e dei dati in Windows Server Essentials, è necessario scaricare ed eseguire il BPA di Windows Server Essentials.  

1.  [Preparare il server di origine per la migrazione a Windows Server Essentials](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md).  Verificare che il server di origine e la rete siano pronti per la migrazione. In questa sezione viene illustrato come eseguire il backup del server di origine, valutare l'integrità del sistema del server di origine, installare gli aggiornamenti e i Service Pack più recenti e verificare la configurazione della rete.  
  
2.  [Installare Windows Server Essentials in modalità di migrazione](../migrate/Install-Windows-Server-Essentials-in-migration-mode.md).  Questa sezione descrive i passaggi da eseguire per installare Windows Server Essentials nel server di destinazione in modalità di migrazione.  
  
3.  [Aggiungere i computer al nuovo server di Windows Server Essentials](../migrate/Join-computers-to-the-new-Windows-Server-Essentials-server.md).  Questa sezione illustra l'aggiunta di computer client al nuovo server di Windows Server Essentials e l'aggiornamento delle impostazioni di Criteri di gruppo.  
  
4.  [Spostare i dati e le impostazioni di SBS 2011 Essentials nel server di destinazione](../migrate/Move-Windows-SBS-2011-Essentials-to-the-Destination-Server-for-migration.md).  In questa sezione vengono fornite informazioni in merito alla migrazione di dati e impostazioni dal server di origine.  
  
5.  [Abilitare il reindirizzamento cartelle nel server di destinazione Windows Server Essentials](../migrate/Enable-folder-redirection-on-the-Windows-Server-Essentials-Destination-Server.md).  Se il reindirizzamento cartelle è abilitato nel server di origine, è possibile abilitarlo nel server di destinazione e quindi eliminare l'impostazione dei Criteri di gruppo per il reindirizzamento cartelle.  
  
6.  [Abbassare di e rimuovere il server di origine dalla nuova rete Windows Server Essentials](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md).  Prima di rimuovere il server di origine dalla rete, è necessario forzare un aggiornamento dei Criteri di gruppo e abbassare di livello il server di origine.  
  
7.  [Eseguire le attività post-migrazione per la migrazione a Windows Server Essentials](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md).  Al termine della migrazione di tutte le impostazioni e i dati in Windows Server Essentials, è possibile eseguire il mapping dei computer autorizzati agli account utente.  
  
8.  [Eseguire il Best Practices Analyzer di Windows Server Essentials](../migrate/Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md).  Al termine della migrazione delle impostazioni e dei dati in Windows Server Essentials, è necessario scaricare ed eseguire il BPA di Windows Server Essentials.  

  
 Diverse procedure di migrazione richiedono l'apertura di una finestra del prompt dei comandi come amministratore.  
  
###  <a name="to-open-a-command-prompt-window-on-the-source-server-as-an-administrator"></a><a name="BKMK_OpenACommandPromptAsAdmin"></a>Per aprire una finestra del prompt dei comandi nel server di origine come amministratore  
  
1.  Fare clic su **Start**.  
  
2.  Nella casella di ricerca digitare **cmd**.  
  
3.  Nell'elenco dei risultati fare clic con il pulsante destro del mouse su **cmd** e quindi scegliere **Esegui come amministratore**.  
  
#### <a name="to-open-a-command-prompt-window-on-the-destination-server-as-an-administrator"></a>Per aprire una finestra del prompt dei comandi sul server di destinazione come amministratore  
  
1.  Nella schermata **Start** digitare **cmd** nella casella di ricerca.  
  
2.  Nell'elenco dei risultati fare clic con il pulsante destro del mouse su **cmd** e quindi scegliere **Esegui come amministratore**.
