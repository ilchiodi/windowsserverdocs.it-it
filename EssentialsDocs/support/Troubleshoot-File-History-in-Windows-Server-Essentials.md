---
title: Risoluzione dei problemi di Cronologia file in Windows Server Essentials
description: Viene descritto come usare Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: ed062945-27e9-4572-b1bb-6c8cf1b9c2f4
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 7ef53e350c999206c074fa0d9633947a32d0f9ac
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852239"
---
# <a name="troubleshoot-file-history-in-windows-server-essentials"></a>Risoluzione dei problemi di Cronologia file in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials 
  
## <a name="troubleshoot-issues-with-user-file-history-backups"></a>Risoluzione dei problemi relativi ai backup di Cronologia file dell'utente  
 Potrebbero verificarsi i problemi seguenti durante la gestione dei backup di Cronologia file per un utente o un computer aggiunto a un server che esegue Windows Server Essentials.  
  
### <a name="file-history-data-is-not-automatically-deleted"></a>I dati di Cronologia file non vengono eliminati automaticamente  
 I dati di Cronologia file potrebbero non essere eliminati automaticamente se:  
  
- Quando si elimina un account utente, si sceglie di non eliminare i dati di cronologia file dell'account utente e si sceglie di eliminare manualmente i dati.  
  
- Quando si prova a eliminare i dati di Cronologia file, questi dati sono usati da un altro processo.  
  
  Per risolvere questo problema, è necessario eliminare manualmente la Cronologia file mediante la procedura seguente:  
  
####  <a name="to-manually-delete-file-history-backups-for-a-user-or-a-computer"></a><a name="BKMK_manuallyDelete"></a>Per eliminare manualmente i backup di cronologia file per un utente o un computer  
  
1.  Accedere al server come amministratore.  
  
2.  Eseguire Esplora file come amministratore.  
  
3.  Passare alla cartella Backup Cronologia file. Il percorso predefinito è C:\ServerFolders\Backup Cronologia file.  
  
4.  Eliminare la cartella condivisa in cui viene archiviato il backup di Cronologia file:  
  
    -   Per eliminare la cronologia file per un utente, eliminare la cartella di backup di cronologia file con il nome dell'utente.  
  
    -   Per eliminare la cronologia file per un computer, eliminare la sottocartella di backup di Cronologia file con il nome del computer. Se, ad esempio, un utente è stato ritirato < MyComputer01\> dopo che ha iniziato a lavorare sul nuovo computer portatile, < MyComputer02\>, è necessario eliminare i backup della cronologia di C:\serverfolders\backup\\< account\>\\< MyComputer01\> dopo aver verificato con l'utente che ha trasferito tutti i file e le cartelle al nuovo portatile e non ha bisogno della cronologia file in futuro.  
  
### <a name="cannot-apply-file-history-setting-to-a-new-user"></a>Non è possibile applicare l'impostazione di Cronologia file a un nuovo utente  
 Se si aggiunge un nuovo utente il cui nome è identico al nome di un utente che è stato eliminato da Windows Server Essentials, la configurazione di Cronologia file per il nuovo utente potrebbe non riuscire a causa di un conflitto di denominazione quando Windows Server Essentials prova a creare una cartella per archiviare la cronologia file del nuovo utente. Per risolvere questo problema, è possibile rinominare la cartella di Cronologia file per l'utente eliminato.  
  
##### <a name="to-locate-user-file-history-on-the-server"></a>Per individuare la cronologia file dell'utente nel server  
  
1.  Accedere al server come amministratore.  
  
2.  Nel dashboard di Windows Server Essentials fare clic su **Archiviazione**.  
  
3.  Nella scheda **Cartelle server** annotare il percorso della cartella Backup Cronologia file. Il percorso predefinito è%SystemDrive%\ServerFolders\File cronologia backup\\.  
  
##### <a name="to-resolve-file-history-issues-for-a-new-user-with-a-name-conflict"></a>Per risolvere i problemi relativi alla cronologia file per un nuovo utente con un conflitto di nomi  
  
1.  Accedere al server come amministratore.  
  
2.  Eseguire Esplora file come amministratore.  
  
3.  Passare alla cartella Backup Cronologia file. Il percorso predefinito è C:\ServerFolders\Backup Cronologia file.  
  
     La cartella Backup Cronologia file contiene una sottocartella per ogni account utente che è stato aggiunto a Windows Server Essentials. Ad esempio, la cronologia file per l'utente Indro Neri viene archiviata nella sottocartella Backup Cronologia file\Indro Neri.  
  
4.  Rinominare la sottocartella per l'utente eliminato, ad esempio **<*nome utente*> _Deleted**. Se la cronologia file dell'utente non è più necessaria, è possibile eliminare la cartella.  
  

5.  È ora possibile aggiungere il nuovo utente. Per istruzioni, vedere Aggiungere un account utente? in [Gestisci account utente](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md).  
  
### <a name="a-user-account-was-removed-but-the-users-file-history-remains"></a>È stato rimosso un account utente, ma rimane la cronologia file dell'utente  
 In alcuni casi, l'amministratore di rete potrebbe scegliere di rimuovere un utente o un computer dal server, ma di mantenere il backup di Cronologia file per utilizzi futuri. Quando la cronologia file non sarà più necessaria, rimuovere la cartella Backup Cronologia file per l'utente o il computer dalle cartelle condivise nel server. A tale scopo, vedere [To manually delete File History backups for a user or a computer](Troubleshoot-File-History-in-Windows-Server-Essentials.md#BKMK_manuallyDelete).  

5. È ora possibile aggiungere il nuovo utente. Per istruzioni, vedere Aggiungere un account utente? in [Gestisci account utente](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md).  
  
### <a name="a-user-account-was-removed-but-the-users-file-history-remains"></a>È stato rimosso un account utente, ma rimane la cronologia file dell'utente  
 In alcuni casi, l'amministratore di rete potrebbe scegliere di rimuovere un utente o un computer dal server, ma di mantenere il backup di Cronologia file per utilizzi futuri. Quando la cronologia file non sarà più necessaria, rimuovere la cartella Backup Cronologia file per l'utente o il computer dalle cartelle condivise nel server. A tale scopo, vedere [To manually delete File History backups for a user or a computer](../support/Troubleshoot-File-History-in-Windows-Server-Essentials.md#BKMK_manuallyDelete).  

  
## <a name="see-also"></a>Vedere anche  
  
-   [Gestione del backup del client](../manage/Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md)  
  

-   [Supportare Windows Server Essentials](Support-Windows-Server-Essentials.md)

-   [Supportare Windows Server Essentials](../support/Support-Windows-Server-Essentials.md)

