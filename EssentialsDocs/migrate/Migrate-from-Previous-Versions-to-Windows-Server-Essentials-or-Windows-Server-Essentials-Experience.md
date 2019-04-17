---
title: Eseguire la migrazione da versioni precedenti a Windows Server Essentials o esperienza Windows Server Essentials
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/20116
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2974fb3a-5150-43fd-a73f-3e5074eb5d03
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 213ee4304d9d4ebdb7580f7f78fdaca78aa454c9
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="migrate-from-previous-versions-to-windows-server-essentials-or-windows-server-essentials-experience"></a>Eseguire la migrazione da versioni precedenti a Windows Server Essentials o esperienza Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Questa guida descrive come eseguire la migrazione da versioni precedenti di Windows Small Business Server e Windows Server Essentials (inclusi Windows Server Essentials, Windows Small Business Server 2011 Standard, Windows Small Business Server 2011 Essentials, Windows Small Business Server 2008 e Windows Small Business Server 2003) a Windows Server Essentials o Windows Server 2012 R2 con il ruolo esperienza Windows Server Essentials installato.  
  
 **Per gli ambienti con un massimo di 25 utenti e 50 dispositivi**, è possibile seguire i passaggi descritti in questa guida per eseguire la migrazione da versioni precedenti di Windows SBS a Windows Server Essentials.  
  
 **Per gli ambienti con un massimo di 100 utenti e 200 dispositivi**, è possibile seguire le stesse linee guida per eseguire la migrazione alle edizioni Standard o Datacenter di Windows Server 2012 R2 con il ruolo esperienza Windows Server Essentials installato.  
  
> [!NOTE]
>  Per evitare problemi durante la migrazione, il team di sviluppo del prodotto Windows Server Essentials consiglia di leggere questo documento prima di iniziare la migrazione.  
  
## <a name="terms-and-definitions"></a>Termini e definizioni  
 **Server di origine** il server esistente da cui vengono migrati dati e impostazioni.  
  
 **Server di destinazione** il nuovo server nel quale vengono migrati dati e impostazioni.  
  
## <a name="migration-process-summary"></a>Riepilogo del processo di migrazione  
 Questa Guida alla migrazione include i passaggi seguenti:  
  
1.  [Passaggio 1: Preparare la migrazione di Server di origine per Windows Server Essentials](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md).  È necessario assicurarsi che il Server di origine e la rete siano pronti per la migrazione. In questa sezione illustra la backup dei Server di origine, la valutazione dell'integrità di sistema del Server di origine, installare il service pack e le correzioni più recenti e verificare la configurazione di rete.  
  
2.  [Passaggio 2: Installare Windows Server Essentials come nuovo controller di dominio replica](Step-2--Install-Windows-Server-Essentials-as-a-new-replica-domain-controller.md). In questa sezione viene descritto come installare Windows Server Essentials o Windows Server 2012 R2 Standard (con il ruolo esperienza Windows Server Essentials abilitato) come controller di dominio.  
  
3.  [Passaggio 3: Aggiungere i computer al nuovo server Windows Server Essentials](Step-3--Join-computers-to-the-new-Windows-Server-Essentials-server.md).  In questa sezione viene illustrato come aggiungere i computer client al nuovo server che esegue Windows Server Essentials e aggiornare le impostazioni di criteri di gruppo.  
  
4.  [Passaggio 4: Spostare dati e impostazioni per la migrazione di Server di destinazione per Windows Server Essentials](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  In questa sezione fornisce informazioni sulla migrazione di dati e impostazioni dal Server di origine.  
  
5.  [Passaggio 5: Abilitare il reindirizzamento cartelle nella migrazione di Server di destinazione per Windows Server Essentials](Step-5--Enable-folder-redirection-on-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  Se il reindirizzamento cartelle è abilitato nel Server di origine, è possibile abilitare il reindirizzamento cartelle nel Server di destinazione e quindi eliminare la vecchia impostazione di criteri di gruppo reindirizzamento cartelle.  
  
6.  [Passaggio 6: Abbassare di livello e rimuovere il Server di origine dalla nuova rete Windows Server Essentials](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md).  Prima di rimuovere il Server di origine dalla rete, è necessario forzare un aggiornamento di criteri di gruppo e abbassare di livello il Server di origine.  
  
7.  [Passaggio 7: Eseguire le attività post-migrazione per la migrazione di Windows Server Essentials](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md).  Dopo aver completato la migrazione di tutti i dati e le impostazioni di Windows Server Essentials, si desidera mappare i computer autorizzati agli account utente.  
  
8.  [Passaggio 8: Eseguire Windows Server Essentials Best Practices Analyzer](Step-8--Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md).  Dopo aver completato la migrazione delle impostazioni e i dati a Windows Server Essentials, è necessario eseguire il Windows Server Essentials Best Practices Analyzer (BPA).  
  
 Molte delle procedure di migrazione è necessario aprire una finestra del prompt dei comandi come amministratore. Le procedure seguenti illustrano come eseguire questa operazione.  
  
###  <a name="BKMK_OpenACommandPromptAsAdmin"></a>Per aprire una finestra del prompt dei comandi nel Server di origine come amministratore  
  
1.  Fare clic su **Start**.  
  
2.  Nella casella di ricerca, digitare **cmd**.  
  
3.  Nell'elenco dei risultati, fare doppio clic su **cmd**, quindi fare clic su **Esegui come amministratore**.  
  
#### <a name="to-open-a-command-prompt-window-on-the-destination-server-as-an-administrator"></a>Per aprire una finestra del prompt dei comandi nel Server di destinazione come amministratore  
  
1.  Nel **Start** dello schermo, nella casella di ricerca, digitare **cmd**.  
  
2.  Nell'elenco dei risultati, fare doppio clic su **cmd**, quindi fare clic su **Esegui come amministratore**.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Eseguire la migrazione di dati del Server a Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [Eseguire la migrazione di dati del Server a Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

