---
title: 'Passaggio 4: Spostare dati e impostazioni nel server di destinazione per la migrazione a Windows Server Essentials'
description: Viene descritto come usare Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: e143df43-e227-4629-a4ab-9f70d9bf6e84
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: dd3ba0b54e24a5fcafb72c970f05224c3606ff3a
ms.sourcegitcommit: 2f072c0c02e3e0deae331ca64b375d63b89d0522
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/14/2020
ms.locfileid: "83404520"
---
# <a name="step-4-move-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>Passaggio 4: Spostare dati e impostazioni nel server di destinazione per la migrazione a Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials

In questa sezione vengono fornite informazioni in merito alla migrazione di dati e impostazioni dal server di origine. Spostare impostazioni e dati nel server di destinazione nel modo seguente:  
  
-   [Copiare i dati nel server di destinazione](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_CopyData)  
  
-   [Configurare la rete](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_Network)  
  
-   [Mappare i computer consentiti agli account utente](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_MapPermittedComputers)  
  
##  <a name="copy-data-to-the-destination-server"></a><a name="BKMK_CopyData"></a>Copiare i dati nel server di destinazione  
 Prima di copiare i dati dal server di origine al server di destinazione, eseguire le attività seguenti:  
  
-   Esaminare l'elenco di cartelle condivise sul server di origine, incluse le autorizzazioni per ogni cartella. Creare o personalizzare le cartelle sul server di destinazione in modo che corrispondano alla struttura di cartelle di cui si sta eseguendo la migrazione dal server di origine.  
  
-   Esaminare la dimensione di ogni cartella e verificare che lo spazio di archiviazione sul server di destinazione sia sufficiente.  
  
-   Impostare le cartelle condivise nel server di origine come di sola lettura per tutti gli utenti per impedire operazioni di scrittura nell'unità durante la copia dei file nel server di destinazione.  
  
-   Non è possibile eseguire la migrazione della cartella **Backup computer client** al server di destinazione. Prima di procedere con la migrazione del server, verificare l'integrità di tutti i computer client. Dopo la migrazione di server, è consigliabile configurare e avviare i backup dei computer client per assicurarsi che i dati di tutti i computer client siano sottoposti a backup.  
  
-   Non è possibile eseguire la migrazione diretta della cartella **backup cronologia file** al server di destinazione a causa delle modifiche alla struttura di cartelle e ai metadati di backup in Windows Server Essentials. È però possibile eseguire la migrazione della cartella **Backup Cronologia file** per uno specifico utente in uno specifico computer. A questo scopo, è necessario individuare la cartella **Dati** nella cartella **Backup Cronologia file** per tale utente e computer, quindi copiare la cartella **Dati** nella cartella **Backup Cronologia file** del server di destinazione.  
  
#### <a name="to-copy-data-from-the-source-server-to-the-destination-server"></a>Per copiare i dati dal server di origine a quello di destinazione  
  
1. Accedere al server di destinazione come amministratore di dominio e quindi aprire una finestra del prompt dei comandi o un prompt dei comandi di Windows PowerShell.  
  
2. Se si usa la finestra del prompt dei comandi, digitare il comando seguente e quindi premere INVIO:  
  
   `robocopy \\<SourceServerName>\<SharedSourceFolderName> "<PathOfTheDestination>\<SharedDestinationFolderName>" /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`
  
    Dove:  
  
   - \<SourceServerName \> è il nome del server di origine  
  
   - \<SharedSourceFolderName \> è il nome della cartella condivisa nel server di origine  
  
   - \<PathOfTheDestination \> è il percorso assoluto in cui si vuole spostare la cartella  
  
   - \<SharedDestinationFolderName \> è la cartella nel server di destinazione in cui verranno copiati i dati  
  
     Ad esempio, `robocopy \\sourceserver\MyData "d:\ServerFolders\MyData" /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`.  
  
3. Se si usa Windows PowerShell, digitare il comando seguente e quindi premere INVIO.  
  
    `Add-Wssfolder  Path \ -Name  -KeepPermission`  
  
4. Ripetere questa procedura per ogni cartella condivisa di cui si deve eseguire la migrazione dal server di origine.  
  
##  <a name="configure-the-network"></a><a name="BKMK_Network"></a>Configurare la rete  
  
#### <a name="to-configure-the-network"></a>Per configurare la rete  
  
1. Nel server di destinazione aprire il dashboard.  
  
2. Nella pagina **Home** del dashboard fare clic su **Configura**, fare clic su **Configura Accesso remoto via Internet** e scegliere l'opzione **Fare clic per configurare Accesso remoto via Internet**.  
  
3. Verrà visualizzata la procedura guidata Configura Accesso remoto via Internet. Completare le istruzioni della procedura guidata per configurare il router e i nomi di dominio.  
  
   Se il router non supporta il framework UPnP o se il framework UPnP è disabilitato, accanto al nome del router verrà visualizzata un'icona di avviso gialla. Verificare che le seguenti porte siano aperte e impostate sull'indirizzo IP del server di destinazione:  
  
-   Porta 80: Traffico Web HTTP  
  
-   Porta 443: Traffico Web HTTPS  
  
> [!NOTE]
>  Se si vuole configurare un nome di dominio pubblico nel server di destinazione, è necessario rilasciare il nome di dominio dal server di origine per evitare conflitti nell'aggiornamento dinamico del DNS.  
  
##  <a name="map-permitted-computers-to-user-accounts"></a><a name="BKMK_MapPermittedComputers"></a>Mappare i computer consentiti agli account utente  
 Ogni account utente di cui è stata eseguita la migrazione da versioni precedenti di Windows Small Business Server o Windows Server Essentials deve essere mappato a uno o più computer.  
  
#### <a name="to-map-user-accounts-to-computers"></a>Per mappare gli account utente ai computer  
  
1.  Aprire il dashboard di Windows Server Essentials.  
  
2.  Sulla barra di spostamento fare clic su **Utenti**.  
  
3.  Nell'elenco di account utente fare clic con il pulsante destro del mouse su un account utente e quindi scegliere **Visualizza proprietà account**.  
  
4.  Fare clic sulla scheda **Accesso remoto via Internet** e quindi fare clic su **Consenti Accesso Web remoto e accedi ai servizi Web**.  
  
5.  Fare clic su **Cartelle condivise**, su **Computer**, su **Home page - Collegamenti** e infine su **Applica**.  
  
6.  Fare clic sulla scheda **Accesso computer** e quindi fare clic sul nome del computer a cui si vuole consentire l'accesso.  
  
7.  Ripetere i passaggi 3, 4, 5 e 6 per ogni account utente.  
  
> [!NOTE]
>  Non è necessario cambiare la configurazione del computer client. Viene configurato automaticamente.  
  
> [!NOTE]
>  Dopo aver completato la migrazione, se si verificano problemi durante la creazione del primo nuovo account utente sul server di destinazione, rimuovere l'account utente aggiunto e quindi ricrearlo.  
  
## <a name="next-steps"></a>Passaggi successivi  
 I dati e le impostazioni sono stati spostati nel server di destinazione. Andare a [passaggio 5: abilitare il reindirizzamento cartelle nel server di destinazione per la migrazione a Windows Server Essentials](Step-5--Enable-folder-redirection-on-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  
  

Per visualizzare tutti i passaggi, vedere [eseguire la migrazione a Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

