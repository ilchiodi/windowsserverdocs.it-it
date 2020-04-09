---
title: Eseguire la migrazione da Windows Server Essentials a un nuovo hardware
description: Viene descritto come usare Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: f695ae90-3160-407b-bebf-9e460f22c86d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: ac55c523f05128715ac76cf538375987ab764b5b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852534"
---
# <a name="migrate-windows-server-essentials-to-new-hardware"></a>Eseguire la migrazione da Windows Server Essentials a un nuovo hardware

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Questa guida descrive come eseguire la migrazione di un dominio di Windows Server&reg; 2012 Essentials esistente a Windows Server Essentials in un nuovo hardware e quindi eseguire la migrazione delle impostazioni e dei dati. Questa guida descrive anche come rimuovere il server esistente dalla rete di Windows Server Essentials al termine della migrazione.  
  
> [!NOTE]
>  Per evitare problemi durante la migrazione, il team di sviluppo del prodotto Windows Server Essentials consiglia di leggere questo documento prima di iniziare la migrazione.  
> 
> [!NOTE]
> 
>  Per eseguire la migrazione dei dati del server alla versione più recente di Windows Server Essentials, vedere [eseguire la migrazione a Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).  

  
## <a name="additional-resources"></a>Risorse aggiuntive  
 Visitare il sito Web relativo alla [migrazione di Windows Small Business Server](https://go.microsoft.com/fwlink/?LinkId=217520) per accedere a informazioni aggiuntive, strumenti e risorse della community che consentono di agevolare il processo di migrazione.  
  
## <a name="terms-and-definitions"></a>Termini e definizioni  
 **Server di origine:** Server esistente da cui si esegue la migrazione delle impostazioni e dei dati.  
  
 **Server di destinazione:** Il nuovo server in cui si esegue la migrazione delle impostazioni e dei dati.  
  
## <a name="migration-process-summary"></a>Riepilogo del processo di migrazione  
 In questa Guida alla migrazione sono illustrati i passaggi seguenti:  
  

1. [Preparare il server di origine per la migrazione a Windows Server Essentials](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md).  Verificare che il server di origine e la rete siano pronti per la migrazione. In questa sezione viene illustrato come eseguire il backup del server di origine, valutare l'integrità del sistema del server di origine, installare gli aggiornamenti e i Service Pack più recenti e verificare la configurazione della rete.  
  
2. [Installare Windows Server Essentials in modalità di migrazione](Install-Windows-Server-Essentials-in-migration-mode.md).  Questa sezione descrive i passaggi da eseguire per installare Windows Server Essentials nel server di destinazione in modalità di migrazione.  
  
3. [Aggiungere i computer al nuovo server di Windows Server Essentials](Join-computers-to-the-new-Windows-Server-Essentials-server.md).  Questa sezione illustra l'aggiunta di computer client al nuovo server di Windows Server Essentials e l'aggiornamento delle impostazioni di Criteri di gruppo.  
  
4. [Spostare dati e impostazioni nel server di destinazione per la migrazione a Windows Server Essentials](Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  In questa sezione vengono fornite informazioni in merito alla migrazione di dati e impostazioni dal server di origine.  
  
5. [Configurare il reindirizzamento cartelle nel server di destinazione Windows Server Essentials](Configure-folder-redirection-on-the-Windows-Server-Essentials-Destination-Server.md).  Se il reindirizzamento cartelle è abilitato nel server di origine, è possibile abilitarlo nel server di destinazione e quindi eliminare l'impostazione dei Criteri di gruppo per il reindirizzamento cartelle.  
  
6. [Abbassare di e rimuovere il server di origine dalla nuova rete Windows Server Essentials](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md).  Prima di rimuovere il server di origine dalla rete, è necessario forzare un aggiornamento dei Criteri di gruppo e abbassare di livello il server di origine.  
  
7. [Eseguire le attività post-migrazione per la migrazione a Windows Server Essentials](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md).  Al termine della migrazione di tutte le impostazioni e i dati in Windows Server Essentials, è possibile eseguire il mapping dei computer autorizzati agli account utente.  
  
8. [Eseguire il Best Practices Analyzer di Windows Server Essentials](Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md).  Al termine della migrazione delle impostazioni e dei dati in Windows Server Essentials, è necessario scaricare ed eseguire il BPA di Windows Server Essentials.  
  
   Diverse procedure di migrazione richiedono l'apertura di una finestra del prompt dei comandi come amministratore.  
  
#### <a name="to-open-a-command-prompt-window-as-an-administrator"></a>Per aprire una finestra Prompt dei comandi come amministratore  
  
1.  Nella schermata **Start** digitare **cmd** nella casella di ricerca.  
  
2.  Nell'elenco dei risultati fare clic con il pulsante destro del mouse su **cmd** e quindi scegliere **Esegui come amministratore**.
