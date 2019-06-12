---
title: Eseguire la migrazione da versioni precedenti a Windows Server Essentials o Windows Server Essentials Experience
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
ms.openlocfilehash: 107a20cae83072ee0066ba0a335eb5078341e59b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66432852"
---
# <a name="migrate-from-previous-versions-to-windows-server-essentials-or-windows-server-essentials-experience"></a>Eseguire la migrazione da versioni precedenti a Windows Server Essentials o Windows Server Essentials Experience

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Questa guida descrive come eseguire la migrazione da versioni precedenti di Windows Server Essentials (inclusi Windows Server Essentials, Windows Small Business Server 2011 Standard, Windows Small Business Server 2011 Essentials, Windows e Windows Small Business Server Small Business Server 2008 e Windows Small Business Server 2003) per Windows Server Essentials o Windows Server 2012 R2 con il ruolo esperienza Windows Server Essentials installato.  
  
 **Per gli ambienti con un massimo di 25 utenti e 50 dispositivi**, è possibile seguire i passaggi descritti in questa guida per la migrazione da versioni precedenti di Windows SBS a Windows Server Essentials.  
  
 **Per gli ambienti con un massimo di 100 utenti e 200 dispositivi**, è possibile seguire le stesse linee guida per eseguire la migrazione alle edizioni Standard o Datacenter di Windows Server 2012 R2 con il ruolo esperienza Windows Server Essentials installato.  
  
> [!NOTE]
>  Per evitare problemi durante la migrazione, il team di sviluppo del prodotto di Windows Server Essentials consiglia di leggere il presente documento prima di avviare la migrazione.  
  
## <a name="terms-and-definitions"></a>Termini e definizioni  
 **Server di origine** Il server esistente da cui vengono migrati dati e impostazioni.  
  
 **Server di destinazione** Il nuovo server nel quale vengono migrati dati e impostazioni.  
  
## <a name="migration-process-summary"></a>Riepilogo del processo di migrazione  
 In questa guida alla migrazione sono illustrati i passaggi seguenti:  
  
1. [Passaggio 1: Preparare la migrazione di Server di origine per Windows Server Essentials](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md).  Verificare che il server di origine e la rete siano pronti per la migrazione. In questa sezione viene illustrato come eseguire il backup del server di origine, valutare l'integrità del sistema del server di origine, installare gli aggiornamenti e i Service Pack più recenti e verificare la configurazione della rete.  
  
2. [Passaggio 2: Installare Windows Server Essentials come nuovo controller di dominio replica](Step-2--Install-Windows-Server-Essentials-as-a-new-replica-domain-controller.md). Questa sezione descrive come installare Windows Server Essentials o Windows Server 2012 R2 Standard (con il ruolo esperienza Windows Server Essentials abilitato) come controller di dominio.  
  
3. [Passaggio 3: Aggiungere computer al nuovo server Windows Server Essentials](Step-3--Join-computers-to-the-new-Windows-Server-Essentials-server.md).  In questa sezione illustra come aggiungere i computer client al nuovo server che esegue Windows Server Essentials e aggiornare le impostazioni di criteri di gruppo.  
  
4. [Passaggio 4: Spostare dati e impostazioni per la migrazione di Server di destinazione per Windows Server Essentials](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  In questa sezione vengono fornite informazioni in merito alla migrazione di dati e impostazioni dal server di origine.  
  
5. [Passaggio 5: Abilitare il reindirizzamento cartelle nella migrazione di Server di destinazione per Windows Server Essentials](Step-5--Enable-folder-redirection-on-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  Se il reindirizzamento cartelle è abilitato nel server di origine, è possibile abilitarlo nel server di destinazione e quindi eliminare l'impostazione dei Criteri di gruppo per il reindirizzamento cartelle.  
  
6. [Passaggio 6: Abbassare di livello e rimuovere il Server di origine dalla nuova rete Windows Server Essentials](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md).  Prima di rimuovere il server di origine dalla rete, è necessario forzare un aggiornamento dei Criteri di gruppo e abbassare di livello il server di origine.  
  
7. [Passaggio 7: Eseguire attività post-migrazione per la migrazione di Windows Server Essentials](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md).  Dopo aver completato la migrazione di tutte le impostazioni e i dati a Windows Server Essentials, è possibile mappare i computer autorizzati agli account utente.  
  
8. [Passaggio 8: Eseguire Windows Server Essentials Best Practices Analyzer](Step-8--Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md).  Dopo aver completato la migrazione delle impostazioni e i dati a Windows Server Essentials, è consigliabile eseguire Windows Server Essentials Best Practices Analyzer (BPA).  
  
   Diverse procedure di migrazione richiedono l'apertura di una finestra del prompt dei comandi come amministratore. Le procedure seguenti illustrano come eseguire questa operazione.  
  
###  <a name="BKMK_OpenACommandPromptAsAdmin"></a> Per aprire una finestra del prompt dei comandi nel Server di origine come amministratore  
  
1.  Fare clic su **Start**.  
  
2.  Nella casella di ricerca digitare **cmd**.  
  
3.  Nell'elenco dei risultati fare clic con il pulsante destro del mouse su **cmd**e quindi scegliere **Esegui come amministratore**.  
  
#### <a name="to-open-a-command-prompt-window-on-the-destination-server-as-an-administrator"></a>Per aprire una finestra del prompt dei comandi sul server di destinazione come amministratore  
  
1.  Nella schermata **Start** digitare **cmd** nella casella di ricerca.  
  
2.  Nell'elenco dei risultati fare clic con il pulsante destro del mouse su **cmd**e quindi scegliere **Esegui come amministratore**.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Eseguire la migrazione dei dati del server a Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [Eseguire la migrazione dei dati del server a Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

