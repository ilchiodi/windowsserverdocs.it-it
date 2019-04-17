---
title: 'Passaggio 4: Spostare dati e impostazioni per la migrazione di Server di destinazione per Windows Server Essentials'
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e143df43-e227-4629-a4ab-9f70d9bf6e84
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: db1169a5415039498718a7988c711d068945b4b7
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="step-4-move-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>Passaggio 4: Spostare dati e impostazioni per la migrazione di Server di destinazione per Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

In questa sezione fornisce informazioni sulla migrazione di dati e impostazioni dal Server di origine. Spostare dati e impostazioni nel Server di destinazione come indicato di seguito:  
  
-   [Copiare i dati nel Server di destinazione](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_CopyData)  
  
-   [Configurare la rete](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_Network)  
  
-   [Mappare i computer autorizzati agli account utente](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_MapPermittedComputers)  
  
##  <a name="BKMK_CopyData"></a>Copiare i dati nel Server di destinazione  
 Prima di copiare i dati dal Server di origine al Server di destinazione, eseguire le attività seguenti:  
  
-   Esaminare l'elenco di cartelle condivise nel Server di origine, incluse le autorizzazioni per ogni cartella. Creare o personalizzare le cartelle nel Server di destinazione per trovare la corrispondenza la struttura di cartelle che si esegue la migrazione dal Server di origine.  
  
-   Esaminare la dimensione di ogni cartella e assicurarsi che il Server di destinazione disponga di sufficiente spazio di archiviazione.  
  
-   Impostare le cartelle condivise nel Server di origine sola lettura per tutti gli utenti in modo che non scrittura avvengono sul disco rigido mentre si copiano i file nel Server di destinazione.  
  
-   Il **Backup dei Computer Client** cartella non può essere migrata nel Server di destinazione. Prima della migrazione del server, assicurarsi che tutti i computer client siano integri. Dopo la migrazione del server, è consigliabile configurare e avviare i backup dei computer client per assicurarsi che i backup dei dati per tutti i computer client.  
  
-   Il **backup cronologia File** cartella non può essere migrata direttamente al Server di destinazione a causa di modifiche cartella metadati struttura e il backup in Windows Server Essentials. Tuttavia, è possibile eseguire la migrazione di **backup cronologia File** cartella per uno specifico utente in un computer specifico. A tale scopo, è necessario individuare il **dati** nella cartella di **backup cronologia File** cartella per tale utente e computer, quindi copiare **dati** cartella per il **backup cronologia File** cartella nel Server di destinazione.  
  
#### <a name="to-copy-data-from-the-source-server-to-the-destination-server"></a>Per copiare i dati dal Server di origine al Server di destinazione  
  
1.  Accedere al Server di destinazione come amministratore di dominio e quindi aprire una finestra del prompt dei comandi o un prompt dei comandi di Windows PowerShell.  
  
2.  Se si utilizza la finestra del prompt dei comandi, digitare il comando seguente e quindi premere INVIO:  
  
    `robocopy \\<SourceServerName>\<SharedSourceFolderName> "<PathOfTheDestination>\<SharedDestinationFolderName>" /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`
  
     Dove:  
  
    -   \ < SourceServerName\ > è il nome del Server di origine  
  
    -   \ < SharedSourceFolderName\ > è il nome della cartella condivisa nel Server di origine  
  
    -   \ < PathOfTheDestination\ > è il percorso assoluto in cui si desidera spostare la cartella  
  
    -   \ < SharedDestinationFolderName\ > è la cartella nel Server di destinazione in cui verranno copiati i dati  
  
     Ad esempio, `robocopy \\sourceserver\MyData "d:\ServerFolders\MyData" /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`.  
  
3.  Se si utilizza Windows PowerShell, digitare il comando seguente e quindi premere INVIO.  
  
     `Add-Wssfolder  Path \ -Name  -KeepPermission`  
  
4.  Ripetere questa procedura per ogni cartella condivisa che si esegue la migrazione dal Server di origine.  
  
##  <a name="BKMK_Network"></a>Configurare la rete  
  
#### <a name="to-configure-the-network"></a>Per configurare la rete  
  
1.  Nel Server di destinazione, aprire il Dashboard.  
  
2.  Nel Dashboard **Home** pagina, fare clic su **installazione**, fare clic su **configurare accesso remoto via Internet**e quindi scegli il **fare clic per configurare accesso remoto via Internet** opzione.  
  
3.  Verrà visualizzata la configurazione guidata accesso remoto via Internet. Completare le istruzioni della procedura guidata per configurare il router e i nomi di dominio.  
  
 Se il router non supporta il framework UPnP o se il framework UPnP è disabilitato, accanto al nome del router può verrà visualizzata un'icona di avviso gialla. Assicurarsi che le porte seguenti siano aperte e impostate per l'indirizzo IP del Server di destinazione:  
  
-   Porta 80: Traffico HTTP Web  
  
-   Porta 443: Traffico Web HTTPS  
  
> [!NOTE]
>  Se si desidera configurare un nome di dominio pubblico nel Server di destinazione, è necessario rilasciare il nome di dominio dal Server di origine per evitare conflitti nell'aggiornamento dinamico del DNS.  
  
##  <a name="BKMK_MapPermittedComputers"></a>Mappare i computer autorizzati agli account utente  
 Ogni account utente che viene eseguita la migrazione da versioni precedenti di Windows Small Business Server o Windows Server Essentials deve essere mappato a uno o più computer.  
  
#### <a name="to-map-user-accounts-to-computers"></a>Per mappare gli account utente ai computer  
  
1.  Aprire il Dashboard di Windows Server Essentials.  
  
2.  Nella barra di spostamento, fare clic su **utenti**.  
  
3.  Nell'elenco di account utente, fare doppio clic su un account utente, quindi fare clic su **Visualizza proprietà account**.  
  
4.  Fare clic su di **accesso remoto via Internet** scheda e quindi fare clic su **Consenti accesso Web remoto e Accedi ai servizi Web**.  
  
5.  Fare clic su **cartelle condivise**, fare clic su**computer**, fare clic su **home page-collegamenti**, quindi fare clic su **applica**.  
  
6.  Fare clic su di **accesso Computer** scheda e quindi fare clic sul nome del computer in cui si desidera consentire l'accesso.  
  
7.  Ripetere i passaggi 3, 4, 5 e 6 per ogni account utente.  
  
> [!NOTE]
>  Non devi modificare la configurazione del computer client. Viene configurato automaticamente.  
  
> [!NOTE]
>  Dopo aver completato la migrazione, se si verifica un problema quando si crea il primo nuovo account utente nel Server di destinazione, rimuovere l'account utente che è stato aggiunto e crearne uno nuovo.  
  
## <a name="next-steps"></a>Passaggi successivi  
 È stato spostato dati e impostazioni nel Server di destinazione. Passare quindi a [passaggio 5: abilitare il reindirizzamento cartelle nella migrazione di Server di destinazione per Windows Server Essentials](Step-5--Enable-folder-redirection-on-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  
  

Per visualizzare tutti i passaggi, vedere [la migrazione a Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

