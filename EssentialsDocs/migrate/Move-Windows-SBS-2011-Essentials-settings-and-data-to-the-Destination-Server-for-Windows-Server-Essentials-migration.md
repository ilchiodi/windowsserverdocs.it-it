---
title: Spostare dati e impostazioni di Windows SBS 2011 Essentials per la migrazione di Server di destinazione per Windows Server Essentials
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 47548994-9fa0-42e0-afa4-c2ccbd063acb
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 80ef8a196f70a77e149dc8defaacf20a82d576e7
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="move-windows-sbs-2011-essentials-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>Spostare dati e impostazioni di Windows SBS 2011 Essentials per la migrazione di Server di destinazione per Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Spostare dati e impostazioni nel Server di destinazione come indicato di seguito:  
  

1.  [Copiare i dati nel Server di destinazione](Move-Windows-SBS-2011-Essentials-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_CopyData)  
  
2.  [Importare gli account utente di Active Directory nel Dashboard di Windows Server Essentials (facoltativo)](Move-Windows-SBS-2011-Essentials-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_ImportADaccounts)  
  
3.  [Configurare la rete](Move-Windows-SBS-2011-Essentials-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_Network)  
  
4.  [Mappare i computer autorizzati agli account utente](Move-Windows-SBS-2011-Essentials-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_MapPermittedComputers)  
 
##  <a name="BKMK_CopyData"></a>Copiare i dati nel Server di destinazione  
 Prima di copiare i dati dal Server di origine al Server di destinazione, eseguire le attività seguenti:  
  
-   Esaminare l'elenco di cartelle condivise nel Server di origine, incluse le autorizzazioni per ogni cartella. Creare o personalizzare le cartelle nel Server di destinazione per trovare la corrispondenza la struttura di cartelle che si esegue la migrazione dal Server di origine.  
  
-   Esaminare la dimensione di ogni cartella e assicurarsi che il Server di destinazione disponga di sufficiente spazio di archiviazione.  
  
-   Impostare le cartelle condivise nel Server di origine Read-only per inserire tutti gli utenti in modo scrittura nell'unità mentre si copiano i file nel Server di destinazione.  
  
#### <a name="to-copy-data-from-the-source-server-to-the-destination-server"></a>Per copiare i dati dal Server di origine al Server di destinazione  
  
1.  Accedere al Server di destinazione come amministratore di dominio e quindi aprire una finestra di comando.  
  
2.  Al prompt dei comandi, digitare il comando seguente e quindi premere INVIO:  
  
    `robocopy \\<SourceServerName> \<SharedSourceFolderName> \\<DestinationServerName> \<SharedDestinationFolderName> /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`  
  
     Dove:
     - \ < SourceServerName\ > è il nome del Server di origine
     - \ < SharedSourceFolderName\ > è il nome della cartella condivisa nel Server di origine
     - \ < DestinationServerName\ > è il nome del Server di destinazione,
     - \ < SharedDestinationFolderName\ > è la cartella condivisa nel Server di destinazione in cui verranno copiati i dati.  
        
3.  Ripetere il passaggio precedente per ogni cartella condivisa che si esegue la migrazione dal Server di origine.  
  
##  <a name="BKMK_ImportADaccounts"></a>Importare gli account utente di Active Directory nel Dashboard di Windows Server Essentials (facoltativo)  
 Per impostazione predefinita, tutti gli account utente creati nel Server di origine vengono automaticamente migrati nel dashboard in Windows Server Essentials. Tuttavia, la migrazione automatica di un account utente di Active Directory avrà esito negativo se tutte le proprietà non soddisfano i requisiti di migrazione. È possibile utilizzare il seguente cmdlet Windows PowerShell per importare gli utenti di Active Directory.  
  
#### <a name="to-import-an-active-directory-user-account-to-the-windows-server-essentials-dashboard"></a>Per importare un account utente di Active Directory nel Dashboard di Windows Server Essentials  
  
1.  Accedere al Server di destinazione come amministratore di dominio.  
  
2.  Aprire Windows PowerShell come amministratore.  
  
3.  Eseguire il cmdlet seguente, dove `[AD username]`è il nome dell'account utente Active Directory che si desidera importare:  
  
     `Import-WssUser  SamAccountName [AD username]`  
  
##  <a name="BKMK_Network"></a>Configurare la rete  
  
#### <a name="to-configure-the-network"></a>Per configurare la rete  
  
1.  Nel Server di destinazione, aprire il Dashboard.  
  
2.  Nel Dashboard **Home** pagina, fare clic su **installazione**, fare clic su **configurare accesso remoto via Internet**e quindi scegli il **fare clic per configurare accesso remoto via Internet** opzione.  
  
3.  Completare le istruzioni della procedura guidata per configurare il router e i nomi di dominio.  
  
 Se il router non supporta il framework UPnP o se il framework UPnP è disabilitato, accanto al nome del router può verrà visualizzata un'icona di avviso gialla. Assicurarsi che le porte seguenti siano aperte e impostate per l'indirizzo IP del Server di destinazione:  
  
-   Porta 80: Traffico HTTP Web  
  
-   Porta 443: Traffico Web HTTPS  
  
##  <a name="BKMK_MapPermittedComputers"></a>Mappare i computer autorizzati agli account utente  
 Ogni account utente che viene eseguita la migrazione da Windows Small Business Server 2011 Essentials deve essere mappato a uno o più computer.  
  
#### <a name="to-map-user-accounts-to-computers"></a>Per mappare gli account utente ai computer  
  
1.  Aprire il Dashboard di Windows Server Essentials.  
  
2.  Nella barra di spostamento, fare clic su **utenti**.  
  
3.  Nell'elenco di account utente, fare doppio clic su un account utente, quindi fare clic su **Visualizza proprietà account**.  
  
4.  Fare clic su di **accesso remoto via Internet** scheda e quindi fare clic su **Consenti accesso Web remoto e Accedi ai servizi Web**.  
  
5.  Selezionare **cartelle condivise**selezionare **computer**selezionare **home page-collegamenti**, quindi fare clic su **applica**.  
  
6.  Fare clic su di **accesso Computer** scheda e quindi fare clic sul nome del computer in cui si desidera consentire l'accesso.  
  
7.  Ripetere i passaggi 3, 4, 5 e 6 per ogni account utente.  
  
> [!NOTE]
>  Non devi modificare la configurazione del computer client. Viene configurato automaticamente.  
  
> [!NOTE]
>  Dopo aver completato la migrazione, se si verifica un problema quando si crea il primo nuovo account utente nel Server di destinazione, rimuovere l'account utente che è stato aggiunto e crearne uno nuovo.
