---
title: "Passaggio 7: Eseguire le attività post-migrazione per la migrazione di Windows Server Essentials"
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d382e3fd-d393-4bd0-883f-db50104a969f
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6a61d28f29097bcb6993a471587f4cc1ae0bcc3f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="step-7-perform-post-migration-tasks-for-the-windows-server-essentials-migration"></a>Passaggio 7: Eseguire le attività post-migrazione per la migrazione di Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Le attività seguenti consentono di completare la configurazione del Server di destinazione con alcune delle stesse impostazioni nel Server di origine. Si potrebbe essere disabilitato alcune di queste impostazioni nel Server di origine durante il processo di migrazione, in modo che non sono stati migrati al Server di destinazione.  
  
1.  [Eliminare le voci DNS del Server di origine](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md#BKMK_DeleteDNSEntries)  
  
2.  [Condividere line-of-business e altre cartelle di dati applicazione](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md#BKMK_ShareLineOfBusinessAndOtherApplications)  
  
##  <a name="BKMK_DeleteDNSEntries"></a>Eliminare le voci DNS del Server di origine  
 Dopo rimuovere le autorizzazioni del Server di origine, il server del servizio DNS (Domain Name) potrebbe contenere voci che puntano al Server di origine. Eliminare queste voci DNS.  
  
#### <a name="to-delete-dns-entries-that-point-to-the-source-server"></a>Per eliminare le voci DNS che fanno riferimento al Server di origine  
  
1.  Nel Server di destinazione, aprire **gestore DNS**.  
  
2.  In Gestore DNS, fare clic sul nome del server, fare clic su **proprietà**, quindi fare clic su di **server d'inoltro** scheda.  
  
3.  Determinare se è presente una voce dell'elenco di server d'inoltro che punta al Server di origine. Se è presente, fare clic su **modifica**e quindi eliminare tale voce nel **Modifica server d'inoltro** finestra.  
  
4.  In **gestore DNS**, espandere il nome del server e quindi espandere **zone di ricerca diretta**.  
  
5.  Per ogni zona di ricerca diretta, fare clic sulla zona, fare clic su **proprietà**, quindi fare clic su di **server dei nomi** scheda.  
  
6.  Selezionare una voce nel **server dei nomi** casella di testo che punta al Server di origine, fare clic su **rimuovere**, quindi fare clic su **OK**.  
  
7.  Ripetere i passaggi 5 e 6 fino a quando non vengono rimossi tutti i puntatori al Server di origine.  
  
8.  Fare clic su **OK** per chiudere la **proprietà** finestra.  
  
9. In **gestore DNS**, espandere **zone di ricerca inversa**.  
  
10. Ripetere i passaggi da 6 a 9 per rimuovere tutte le zone di ricerca inversa che fanno riferimento al Server di origine.  
  
##  <a name="BKMK_ShareLineOfBusinessAndOtherApplications"></a>Condividere line-of-business e altre cartelle di dati applicazione  
 È necessario impostare le autorizzazioni delle cartelle condivise e le autorizzazioni NTFS per line-of-business e altre cartelle di dati dell'applicazione che è stato copiato nel Server di destinazione. Dopo aver impostato le autorizzazioni, le cartelle condivise vengono visualizzate nel Dashboard nel **archiviazione** scheda.  
  
 Se si utilizza uno script di accesso per il mapping delle unità alle cartelle condivise, è necessario aggiornare lo script da eseguire il mapping di unità nel Server di destinazione.  
  
## <a name="next-steps"></a>Passaggi successivi  
 Sono state eseguite le attività post-migrazione per la migrazione di Windows Server Essentials. Passare quindi a [passaggio 8: eseguire Windows Server Essentials Best Practices Analyzer](Step-8--Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md).  
  

Per visualizzare tutti i passaggi, vedere [la migrazione a Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

