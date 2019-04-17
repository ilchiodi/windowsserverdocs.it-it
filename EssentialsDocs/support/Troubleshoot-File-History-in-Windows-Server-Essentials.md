---
title: Risolvere i problemi di cronologia File in Windows Server Essentials
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ed062945-27e9-4572-b1bb-6c8cf1b9c2f4
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 34442565b54b089064c1fa19317a24f591e44fda
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="troubleshoot-file-history-in-windows-server-essentials"></a>Risolvere i problemi di cronologia File in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials 
  
## <a name="troubleshoot-issues-with-user-file-history-backups"></a>Risolvere i problemi con il backup di cronologia File utente  
 Potrebbero verificarsi i problemi seguenti durante la gestione di backup di cronologia File per un utente o un computer in cui è stato aggiunto a un server che esegue Windows Server Essentials.  
  
### <a name="file-history-data-is-not-automatically-deleted"></a>Dati di cronologia file non viene eliminati automaticamente  
 I dati di cronologia File potrebbero non essere eliminati automaticamente se:  
  
-   Quando si elimina un account utente, puoi scegliere di non eliminare l'account utente s dati di cronologia File e scegliere di eliminare i dati manualmente.  
  
-   Quando si tenta di eliminare i dati di cronologia File, i dati di cronologia File sono in uso da un altro processo.  
  
 Per risolvere questo problema, è necessario eliminare manualmente la cronologia File mediante la procedura seguente:  
  
####  <a name="BKMK_manuallyDelete"></a>Per eliminare manualmente il backup di cronologia File per un utente o un computer  
  
1.  Accedere al server come amministratore.  
  
2.  Eseguire Esplora File come amministratore.  
  
3.  Passare alla cartella backup cronologia File. Il percorso predefinito è il backup di cronologia C:\ServerFolders\File.  
  
4.  Eliminare la cartella condivisa che contiene il backup di cronologia File:  
  
    -   Per eliminare la cronologia file per un utente, eliminare la cronologia File sottocartella di backup che include il nome s.  
  
    -   Per eliminare la cronologia file per un computer, eliminare la cronologia File sottocartella di backup che ha il nome del computer. Se un utente ha disattivato < MyComputer01\ > dopo aver iniziato a usare il nuovo computer portatile, < MyComputer02\ >, ad esempio, è possibile eliminare C:\ServerFolders\File cronologia Backups\\ < MyAccount\ > \ < MyComputer01\ > dopo aver verificato con l'utente ha trasferito tutti i file e cartelle nel nuovo computer portatile e non è necessario che la cronologia file in futuro.  
  
### <a name="cannot-apply-file-history-setting-to-a-new-user"></a>Non è possibile applicare l'impostazione di cronologia File a un nuovo utente  
 Se si aggiunge un nuovo utente il cui nome è identico al nome utente di un utente che è stato eliminato da Windows Server Essentials, la configurazione di cronologia File per il nuovo utente potrebbe non riuscire a causa di un conflitto di denominazione quando Windows Server Essentials tenta di creare una cartella per archiviare cronologia file del nuovo utente. Per risolvere questo problema, è possibile rinominare la cartella Cronologia File per l'utente eliminato.  
  
##### <a name="to-locate-user-file-history-on-the-server"></a>Per individuare cronologia file utente sul server  
  
1.  Accedere al server come amministratore.  
  
2.  Nel Dashboard di Windows Server Essentials, fare clic su **archiviazione**.  
  
3.  Nel **cartelle Server** scheda, annotare il percorso della cartella backup cronologia File. Il percorso predefinito è %SystemDrive%\ServerFolders\File Backups\\ cronologia.  
  
##### <a name="to-resolve-file-history-issues-for-a-new-user-with-a-name-conflict"></a>Per risolvere i problemi di cronologia file per un nuovo utente con conflitto relativo al nome  
  
1.  Accedere al server come amministratore.  
  
2.  Eseguire Esplora File come amministratore.  
  
3.  Passare alla cartella backup cronologia File. Il percorso predefinito è il backup di cronologia C:\ServerFolders\File.  
  
     La cartella backup cronologia File contiene una sottocartella per ogni account utente che è stato aggiunto a Windows Server Essentials. Ad esempio, la cronologia file per l'utente John Smith vengono memorizzata nella sottocartella cronologia file\indro.  
  
4.  Rinominare la sottocartella per l'utente che è stato eliminato, ad esempio, * * < *UserName*> _eliminato * *. Se non è più necessario cronologia file dell'utente, è possibile eliminare la cartella.  
  

5.  È ora possibile aggiungere il nuovo utente. Per istruzioni, vedere aggiungere un account utente? in [gestire gli account utente](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md).  
  
### <a name="a-user-account-was-removed-but-the-users-file-history-remains"></a>Un account utente è stato rimosso, ma rimane cronologia file dell'utente  
 In alcuni casi l'amministratore di rete potrebbe scegliere di rimuovere un utente o computer dal server di, ma di mantenere la cronologia di File di backup per un uso futuro. Quando non è più necessario la cronologia file, rimuovere la cartella backup cronologia File per l'utente o il computer dalle cartelle condivise sul server. A tale scopo, vedere [di eliminare manualmente il backup di cronologia File per un utente o un computer](Troubleshoot-File-History-in-Windows-Server-Essentials.md#BKMK_manuallyDelete).  

5.  È ora possibile aggiungere il nuovo utente. Per istruzioni, vedere aggiungere un account utente? in [gestire gli account utente](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md).  
  
### <a name="a-user-account-was-removed-but-the-users-file-history-remains"></a>Un account utente è stato rimosso, ma rimane cronologia file dell'utente  
 In alcuni casi l'amministratore di rete potrebbe scegliere di rimuovere un utente o computer dal server di, ma di mantenere la cronologia di File di backup per un uso futuro. Quando non è più necessario la cronologia file, rimuovere la cartella backup cronologia File per l'utente o il computer dalle cartelle condivise sul server. A tale scopo, vedere [di eliminare manualmente il backup di cronologia File per un utente o un computer](../support/Troubleshoot-File-History-in-Windows-Server-Essentials.md#BKMK_manuallyDelete).  

  
## <a name="see-also"></a>Vedere anche  
  
-   [Gestire il Backup dei Client](../manage/Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md)  
  

-   [Supporto per Windows Server Essentials](Support-Windows-Server-Essentials.md)

-   [Supporto per Windows Server Essentials](../support/Support-Windows-Server-Essentials.md)

