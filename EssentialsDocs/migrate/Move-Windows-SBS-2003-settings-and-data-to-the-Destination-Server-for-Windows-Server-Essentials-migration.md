---
title: Spostare dati e impostazioni di Windows SBS 2003 per la migrazione di Server di destinazione per Windows Server Essentials
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 67087ccb-d820-4642-8ca2-7d2d38714014
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 74375d65845a7a5e9c2d6752bdee49993b8cc791
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="move-windows-sbs-2003-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>Spostare dati e impostazioni di Windows SBS 2003 per la migrazione di Server di destinazione per Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Spostare dati e impostazioni nel Server di destinazione come indicato di seguito:

1.  [Copiare i dati nel Server di destinazione](Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_CopyData)  
  
2.  [Importare gli account utente di Active Directory nel Dashboard di Windows Server Essentials (facoltativo)](Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_ImportADaccounts)  
  
3.  [Rimuovere i vecchi script di accesso (facoltativo)](Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_RemoveScripts)  
  
4.  [Rimuovere legacy Active Directory oggetti Criteri di gruppo (facoltativo)](Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_RemoveLegacyADGPO)  
  
5.  [Configurare la rete](Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_Configure)  
  
6.  [Mappare i computer autorizzati agli account utente](Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_MapPermittedComputers)  
 
##  <a name="BKMK_CopyData"></a>Copiare i dati nel Server di destinazione  
 Prima di copiare i dati dal Server di origine al Server di destinazione, eseguire le attività seguenti:  
  
-   Esaminare l'elenco di cartelle condivise nel Server di origine, incluse le autorizzazioni per ogni cartella. Creare o personalizzare le cartelle nel Server di destinazione per trovare la corrispondenza la struttura di cartelle che si esegue la migrazione dal Server di origine.  
  
-   Esaminare la dimensione di ogni cartella e assicurarsi che il Server di destinazione disponga di sufficiente spazio di archiviazione.  
  
-   Impostare le cartelle condivise nel Server di origine Read-only per inserire tutti gli utenti in modo scrittura nell'unità mentre si copiano i file nel Server di destinazione.  
  
#### <a name="to-copy-data-from-the-source-server-to-the-destination-server"></a>Per copiare i dati dal Server di origine al Server di destinazione  
  
1.  Accedere al Server di destinazione come amministratore di dominio.  
  
2.  Fare clic su **Start**, tipo **cmd** nella casella di ricerca e quindi premere INVIO.  
  
3.  Al prompt dei comandi, digitare il comando seguente e quindi premere INVIO:  
  
    `robocopy \\<SourceServerName> \<SharedSourceFolderName> \\<DestinationServerName> \<SharedDestinationFolderName> /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`  
  
     Dove:
     - \ < SourceServerName\ > è il nome del Server di origine
     - \ < SharedSourceFolderName\ > è il nome della cartella condivisa nel Server di origine
     - \ < DestinationServerName\ > è il nome del Server di destinazione,
     - \ < SharedDestinationFolderName\ > è la cartella condivisa nel Server di destinazione in cui verranno copiati i dati.  
 
4.  Ripetere il passaggio precedente per ogni cartella condivisa che si esegue la migrazione dal Server di origine.  
  
##  <a name="BKMK_ImportADaccounts"></a>Importare gli account utente di Active Directory nel Dashboard di Windows Server Essentials (facoltativo)  
 Per impostazione predefinita, tutti gli account utente creati nel Server di origine vengono automaticamente migrati nel dashboard in Windows Server Essentials. Tuttavia, la migrazione automatica di un account utente di Active Directory avrà esito negativo se alcune proprietà non soddisfano i requisiti di migrazione. È possibile utilizzare il seguente cmdlet Windows PowerShell per importare gli utenti di Active Directory.  
  
#### <a name="to-import-an-active-directory-user-account-to-the-windows-server-essentials-dashboard"></a>Per importare un account utente di Active Directory nel Dashboard di Windows Server Essentials  
  
1.  Accedere al Server di destinazione come amministratore di dominio.  
  
2.  Aprire Windows PowerShell come amministratore.  
  
3.  Eseguire il cmdlet seguente, dove `[AD username]`è il nome dell'account utente Active Directory che si desidera importare:  
  
     `Import-WssUser SamAccountName [AD username]`  
  
##  <a name="BKMK_RemoveScripts"></a>Rimuovere i vecchi script di accesso (facoltativo)  
 Windows SBS 2003 Usa script di accesso per attività come l'installazione di software e la personalizzazione dei desktop.  Windows Server Essentials sostituisce gli script di accesso di Windows SBS 2003 con una combinazione di script di accesso e oggetti Criteri di gruppo.  
  
> [!NOTE]
>  Se gli script di accesso di Windows SBS 2003 sono stati modificati, è consigliabile rinominare gli script per mantenere le personalizzazioni.  
  
> [!NOTE]
>  Gli script di accesso di Windows SBS 2003 si applicano solo agli account utente che sono state aggiunte mediante il **Aggiunta guidata nuovi utenti**.  
  
#### <a name="to-remove-the-windows-sbs-2003-logon-scripts"></a>Per rimuovere gli script di accesso di Windows SBS 2003  
  
1.  Fare clic su **Start**, scegliere **strumenti di amministrazione**, quindi fare clic su **Active Directory Users and Computers**.  
  
2.  In **Active Directory Users and Computers**, espandere la rete e quindi fare clic su **utenti**.  
  
3.  Fare clic su un nome utente, fare clic su **proprietà**, quindi fare clic su di **profilo** scheda.  
  
4.  Eliminare il contenuto del **script di accesso** casella di testo, quindi fare clic su **OK**.  
  
5.  Ripetere i passaggi 3 e 4 per ogni utente.  
  
##  <a name="BKMK_RemoveLegacyADGPO"></a>Rimuovere legacy Active Directory oggetti Criteri di gruppo (facoltativo)  
 Gli oggetti Criteri di gruppo (GPO) sono stati aggiornati per Windows Server Essentials. Sono un soprainsieme oggetti Criteri di gruppo Windows SBS 2003. Per Windows Server Essentials, è necessario eliminare manualmente un numero di filtri di Strumentazione gestione Windows (WMI) e di oggetti Criteri di gruppo di Windows SBS 2003 per evitare conflitti con i filtri WMI e di oggetti Criteri di gruppo di Windows Server Essentials.  
  
> [!NOTE]
>  Se sono stati modificati originale Windows SBS 2003 oggetti Criteri di gruppo, è necessario salvarne le copie in una posizione diversa e quindi eliminarli da Windows SBS 2003.  
  
#### <a name="to-remove-old-group-policy-objects-from-windows-sbs-2003"></a>Per rimuovere i vecchi oggetti Criteri di gruppo da Windows SBS 2003  
  
1.  Accedere al Server di origine con un account amministratore.  
  
2.  Fare clic su **Start**, quindi fare clic su **Gestione Server**.  
  
3.  Nel riquadro di spostamento, fare clic su **gestione avanzata**, fare clic su **Gestione criteri di gruppo**, quindi fare clic su **foresta:***< YourDomainName\ >*.  
  
4.  Fare clic su **domini**, fare clic su *< YourDomainName\ >*, quindi fare clic su **oggetti Criteri di gruppo**.  
  
5.  Fare doppio clic su **controllo criterio di Small Business Server**, fare clic su **eliminare**, quindi fare clic su **OK**.  
  
6.  Ripetere il passaggio 5 per eliminare i seguenti oggetti Criteri di gruppo che si applicano alla rete:  
  
    -   Computer Client di Small Business Server  
  
    -   Criterio Password dominio di Small Business Server  
  
         È consigliabile configurare i criteri password in Windows Server Essentials per imporre password complesse. Per configurare i criteri password, usare il Dashboard, che scrive la configurazione per il criterio dominio predefinito. La configurazione dei criteri password non viene scritta nell'oggetto Small Business Server dominio Password criteri, come in Windows SBS 2003.  
  
    -   Firewall connessione Internet di Small Business Server  
  
    -   Criterio blocco Small Business Server  
  
    -   Criterio assistenza remota Small Business Server  
  
    -   Firewall di Windows Small Business Server  
  
    -   Criterio Computer Client di Small Business Server Update Services  
  
         Questo oggetto Criteri di gruppo sarà presente se si esegue la migrazione da Windows SBS 2003 R2.  
  
    -   Small Business Server aggiornamento criterio impostazioni comuni servizi  
  
         Questo oggetto Criteri di gruppo sarà presente se si esegue la migrazione da Windows SBS 2003 R2.  
  
    -   Criterio Computer Server di Small Business Server Update Services  
  
         Questo oggetto Criteri di gruppo sarà presente se si esegue la migrazione da Windows SBS 2003 R2.  
  
7.  Verificare che tutti i GPO vengono eliminati.  
  
#### <a name="to-remove-wmi-filters-from-windows-sbs-2003"></a>Per rimuovere i filtri WMI da Windows SBS 2003  
  
1.  Accedere al Server di origine con un account amministratore.  
  
2.  Fare clic su **Start**, quindi fare clic su **Gestione Server**.  
  
3.  Nel riquadro di spostamento, fare clic su **gestione avanzata**, fare clic su **Gestione criteri di gruppo**, quindi fare clic su **foresta:***< YourNetworkDomainName\ >*  
  
4.  Fare clic su **domini**, fare clic su *< YourNetworkDomainName\ >*, quindi fare clic su **i filtri WMI**.  
  
5.  Fare doppio clic su **PostSP2**, fare clic su **eliminare**, quindi fare clic su **Sì**.  
  
6.  Fare doppio clic su **PreSP2**, fare clic su **eliminare**, quindi fare clic su **Sì**.  
  
7.  Verificare che questi tre filtri WMI vengono eliminati.  
  
##  <a name="BKMK_Configure"></a>Configurare la rete  
  
#### <a name="to-configure-the-network"></a>Per configurare la rete  
  
1.  Nel Server di destinazione, aprire il Dashboard.  
  
2.  Nel Dashboard **Home** pagina, fare clic su **installazione**, fare clic su **configurare accesso remoto via Internet**e quindi scegli il **fare clic per configurare accesso remoto via Internet** opzione.  
  
3.  Seguire le istruzioni di **configurare accesso remoto via Internet** procedura guidata per configurare il nome di router e il dominio.  
  
 Se il router non supporta il framework UPnP o se il framework UPnP è disabilitato, accanto al nome del router può verrà visualizzata un'icona di avviso gialla. Assicurarsi che le porte seguenti siano aperte e impostate per l'indirizzo IP del Server di destinazione:  
  
-   Porta 80: Traffico HTTP Web  
  
-   Porta 443: Traffico Web HTTPS  
  
> [!NOTE]
>  Se configuri un server di Exchange in locale in un secondo server, è necessario verificare la porta 25 (per SMTP) sia aperta e che venga reindirizzata all'indirizzo IP del server Exchange locale.  
  
##  <a name="BKMK_MapPermittedComputers"></a>Mappare i computer autorizzati agli account utente  
 In Windows SBS 2003, se un utente si connette ad accesso Web remoto, vengono visualizzati tutti i computer nella rete. Ciò può includere computer in cui l'utente non dispone dell'autorizzazione di accesso. In Windows Server Essentials, un utente deve essere assegnato esplicitamente a un computer per poterlo visualizzare in accesso Web remoto. Ogni account utente che viene eseguita la migrazione da Windows SBS 2003 deve essere mappato a uno o più computer.  
  
#### <a name="to-map-user-accounts-to-computers"></a>Per mappare gli account utente ai computer  
  
1.  Nel Server di destinazione, aprire il Dashboard di Windows Server Essentials.  
  
2.  Nella barra di spostamento, fare clic su **utenti**.  
  
3.  Nell'elenco di account utente, fare doppio clic su un account utente, quindi fare clic su **Visualizza proprietà account**.  
  
4.  Fare clic su di **accesso remoto via Internet** scheda e quindi fare clic su **Consenti accesso Web remoto e Accedi ai servizi Web.**.  
  
5.  Fare clic su **cartelle condivise**, fare clic su **computer**, fare clic su **home page-collegamenti**, quindi fare clic su **applica**.  
  
6.  Fare clic su di **accesso Computer** scheda e fare clic sul nome del computer in cui si desidera consentire l'accesso.  
  
7.  Ripetere i passaggi 3, 4, 5 e 6 per ogni account utente.  
  
> [!NOTE]
>  Non devi modificare la configurazione del computer client. Viene configurato automaticamente.  
  
> [!NOTE]
>  Dopo aver completato la migrazione, se si verifica un problema quando si crea il primo nuovo account utente nel Server di destinazione, rimuovere l'account utente che è stato aggiunto e crearne uno nuovo.
