---
title: 'Passaggio 7: Eseguire le attività post-migrazione per la migrazione a Windows Server Essentials'
description: Viene descritto come usare Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: d382e3fd-d393-4bd0-883f-db50104a969f
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 3101a7bdc42ef754e5aafa87a8758172c42824bb
ms.sourcegitcommit: 2f072c0c02e3e0deae331ca64b375d63b89d0522
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/14/2020
ms.locfileid: "83404505"
---
# <a name="step-7-perform-post-migration-tasks-for-the-windows-server-essentials-migration"></a>Passaggio 7: Eseguire le attività post-migrazione per la migrazione a Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials

Le attività seguenti aiutano a completare la configurazione del server di destinazione con alcune impostazioni uguali a quelle del server di origine. Durante il processo di migrazione è possibile che alcune di queste impostazioni siano state disabilitate sul server di origine e di conseguenza non ne sia stata eseguita la migrazione al server di destinazione.  
  
1.  [Eliminare le voci DNS per il server di origine](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md#BKMK_DeleteDNSEntries)  
  
2.  [Condividere le cartelle di dati di applicazioni LOB e di altro tipo](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md#BKMK_ShareLineOfBusinessAndOtherApplications)  
  
##  <a name="delete-dns-entries-for-the-source-server"></a><a name="BKMK_DeleteDNSEntries"></a>Elimina le voci DNS per il server di origine  
 Anche dopo aver rimosso le autorizzazioni per il server di origine, il server Domain Name Service (DNS) potrebbe contenere voci che puntano al server di origine. Eliminare queste voci DNS.  
  
#### <a name="to-delete-dns-entries-that-point-to-the-source-server"></a>Per eliminare le voci relative a DNS che fanno riferimento al server di origine  
  
1.  Nel server di destinazione aprire **Gestore DNS**.  
  
2.  In Gestore DNS fare clic con il pulsante destro del mouse sul nome server, scegliere **Proprietà** e quindi fare clic sulla scheda **Server d'inoltro**.  
  
3.  Determinare se nell'elenco di server d'inoltro c'è una voce che punta al server di origine. Se c'è, fare clic su **Modifica** e quindi eliminare tale voce nella finestra **Modifica server d'inoltro**.  
  
4.  In **Gestore DNS** espandere il nome server e quindi espandere **Zone di ricerca diretta**.  
  
5.  Per ogni zona di ricerca diretta fare clic con il pulsante destro del mouse sulla zona, scegliere **Proprietà** e quindi fare clic sulla scheda **Server dei nomi**.  
  
6.  Nella casella di testo **Server dei nomi** selezionare una voce che punta al server di origine, fare clic su **Rimuovi** e quindi fare clic su **OK**.  
  
7.  Ripetere i passaggi 5 e 6 finché tutti i puntatori al server di origine non sono stati rimossi.  
  
8.  Fare clic su **OK** per chiudere la finestra di dialogo **Proprietà**.  
  
9. In **Gestore DNS** espandere **Zone di ricerca inversa**.  
  
10. Ripetere i passaggi da 6 a 9 per rimuovere tutte le zone di ricerca inversa che fanno riferimento al server di origine.  
  
##  <a name="share-line-of-business-and-other-application-data-folders"></a><a name="BKMK_ShareLineOfBusinessAndOtherApplications"></a>Condividere le cartelle di dati delle applicazioni line-of-business e di altro settore  
 È necessario configurare le autorizzazioni delle cartelle condivise e le autorizzazioni NTFS per le cartelle di dati delle applicazioni LOB e di altre applicazioni copiate nel server di destinazione. Dopo aver impostato le autorizzazioni, le cartelle condivise vengono visualizzate nel dashboard nella scheda **Archiviazione**.  
  
 Se si usa uno script di accesso che esegue il mapping delle unità alle cartelle condivise, sarà necessario aggiornare lo script per consentire il mapping alle unità nel server di destinazione.  
  
## <a name="next-steps"></a>Passaggi successivi  
 Sono state eseguite le attività post-migrazione per la migrazione a Windows Server Essentials. A questo [punto, andare al passaggio 8: eseguire il Best Practices Analyzer di Windows Server Essentials](Step-8--Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md).  
  

Per visualizzare tutti i passaggi, vedere [eseguire la migrazione a Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

