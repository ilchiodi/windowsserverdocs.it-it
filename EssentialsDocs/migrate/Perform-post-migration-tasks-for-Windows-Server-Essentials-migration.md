---
title: Eseguire le attività di post-migrazione per Windows Server Essentials migration1
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f2d236a4-0d62-4961-9d1f-332054e06f6d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 535a547ded55cb4afc0942259eadf5222a815274
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821022"
---
# <a name="perform-post-migration-tasks-for-windows-server-essentials-migration1"></a>Eseguire le attività di post-migrazione per Windows Server Essentials migration1

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Le attività seguenti aiutano a completare la configurazione del server di destinazione con alcune impostazioni uguali a quelle del server di origine. Durante il processo di migrazione è possibile che alcune di queste impostazioni siano state disabilitate sul server di origine e di conseguenza non ne sia stata eseguita la migrazione al server di destinazione. Oppure sono passaggi di configurazione facoltativi che si vuole eseguire.  
  

-   [Eliminare le voci DNS del Server di origine](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_DeleteDNSEntries)  
  
-   [Condividere line-of-business e altre cartelle di dati dell'applicazione](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_ShareLineOfBusinessAndOtherApplications)  
  
-   [Risolvere i problemi relativi al computer client al termine della migrazione](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_FixClientComputerIssuesAfterMigrating)  
  
-   [Concedere al gruppo Administrators interno il diritto di accesso come processo batch](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_AdminGroup)  

-   [Eliminare le voci DNS del Server di origine](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_DeleteDNSEntries)  
  
-   [Condividere line-of-business e altre cartelle di dati dell'applicazione](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_ShareLineOfBusinessAndOtherApplications)  
  
-   [Risolvere i problemi relativi al computer client al termine della migrazione](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_FixClientComputerIssuesAfterMigrating)  
  
-   [Concedere al gruppo Administrators interno il diritto di accesso come processo batch](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_AdminGroup)  

  
##  <a name="BKMK_DeleteDNSEntries"></a> Eliminare le voci DNS del Server di origine  
 Anche dopo aver rimosso le autorizzazioni per il server di origine, il server Domain Name Service (DNS) potrebbe contenere voci che puntano al server di origine. Eliminare queste voci DNS.  
  
#### <a name="to-delete-dns-entries-that-point-to-the-source-server"></a>Per eliminare le voci relative a DNS che fanno riferimento al server di origine  
  
1.  Nel server di destinazione aprire **Gestore DNS**.  
  
2.  In Gestore DNS fare clic con il pulsante destro del mouse sul nome server, scegliere **Proprietà**e quindi fare clic sulla scheda **Server d'inoltro** .  
  
3.  Determinare se nell'elenco di server d'inoltro c'è una voce che punta al server di origine. Se c'è, fare clic su **Modifica** e quindi eliminare tale voce nella finestra **Modifica server d'inoltro**.  
  
4.  In **Gestore DNS**espandere il nome server e quindi espandere **Zone di ricerca diretta**.  
  
5.  Per ogni zona di ricerca diretta fare clic con il pulsante destro del mouse sulla zona, scegliere **Proprietà** e quindi fare clic sulla scheda **Server dei nomi**.  
  
6.  Nella casella **Server dei nomi** fare clic su una voce che punta al server di origine, fare clic su **Rimuovi** e quindi fare clic su **OK**.  
  
7.  Ripetere i passaggi 5 e 6 finché tutti i puntatori al server di origine non sono stati rimossi.  
  
8.  Fare clic su **OK** per chiudere la finestra di dialogo **Proprietà**.  
  
9. Nella console **Gestore DNS** espandere **Zone di ricerca inversa**.  
  
10. Ripetere i passaggi da 6 a 9 per rimuovere tutte le zone di ricerca inversa che fanno riferimento al server di origine.  
  
##  <a name="BKMK_ShareLineOfBusinessAndOtherApplications"></a> Condividere line-of-business e altre cartelle di dati dell'applicazione  
 È necessario configurare le autorizzazioni delle cartelle condivise e le autorizzazioni NTFS per le cartelle di dati delle applicazioni LOB e di altre applicazioni copiate nel server di destinazione. Dopo aver impostato le autorizzazioni, le cartelle condivise vengono visualizzate nel Dashboard di Windows Server Essentials nel **archiviazione** sezione.  
  
 Se si usa uno script di accesso che esegue il mapping delle unità alle cartelle condivise, sarà necessario aggiornare lo script per consentire il mapping alle unità nel server di destinazione.  
  
##  <a name="BKMK_FixClientComputerIssuesAfterMigrating"></a> Risolvere i problemi relativi al computer client al termine della migrazione  
 Se esegue la migrazione a Windows Server Essentials da Windows Small Business Server 2003 Premium Edition con Microsoft Internet Security e Acceleration (ISA) Server installato, i computer client nella rete hanno ancora Microsoft Firewall Client e Internet Finestra di esplorazione configurato per utilizzare un server proxy.  
  
 Questo causa problemi di connettività nei computer client, perché il server proxy non esiste più. Se è presente un altro server proxy configurato, i computer client continuano a usare il server che esegue Windows SBS 2003 per il server proxy. Per risolvere questo problema, è necessario riconfigurare Internet Explorer in modo che non usi un server proxy o che usi il nuovo server proxy.  
  
#### <a name="to-reconfigure-internet-explorer"></a>Per riconfigurare Internet Explorer  
  
1.  In Internet Explorer fare clic su **Strumenti**e quindi su **Opzioni Internet**.  
  
2.  Fare clic sulla scheda **Connessioni**, su **Impostazioni LAN** e quindi eseguire una delle operazioni seguenti:  
  
    -   Se non si userà un server proxy nella rete, deselezionare tutte le caselle di controllo nella finestra di dialogo **Impostazioni rete locale (LAN)**.  
  
    -   Se si vuole usare un nuovo server proxy nella rete:  
  
        1.  Nella finestra di dialogo **Impostazioni rete locale (LAN)** deselezionare le caselle di controllo nella sezione **Configurazione automatica**.  
  
        2.  Nella sezione **Server proxy** verificare che entrambe le caselle di controllo siano selezionate.  
  
        3.  Nella casella **Indirizzo** digitare il nome di dominio completo (FQDN) del server proxy.  
  
        4.  Nella casella **Porta** digitare **80**.  
  
3.  Fare clic su **OK** due volte.  
  
4.  Accedere a un sito Web per verificare che le impostazioni di connessione siano corrette.  
  
##  <a name="BKMK_AdminGroup"></a> Concedere al gruppo Administrators interno il diritto di accesso come processo batch  
 Dopo la migrazione di un dominio Windows Small Business Server 2003 esistente a Windows Server Essentials, è consigliabile concedere al gruppo Administrators interno il diritto di accedere come processo batch. Verificare che il gruppo Administrators interno abbia ancora il diritto di accedere come processo batch al server di destinazione. Questo diritto serve agli amministratori per eseguire un avviso nel server di destinazione senza accedervi.  
  
#### <a name="to-give-the-built-in-administrators-group-the-right-to-log-on-as-a-batch-job"></a>Per concedere al gruppo Administrators interno il diritto di accedere come processo batch  
  
1.  Nel server di destinazione aprire lo strumento di amministrazione **Gestione Criteri di gruppo**.  
  
2.  Nel **Gestione criteri di gruppo** albero della Console, espandere **foresta:** *< nomeserver\>*, domini e infine il server.  
  
3.  Espandere **Controller di dominio**, fare clic con il pulsante destro del mouse su **Criterio controller di dominio predefiniti** e quindi scegliere **Modifica**.  
  
4.  Nelle **Editor Gestione criteri di gruppo**, fare clic su **criterio controller di dominio predefiniti ***< ServerName\>*** criteri**, quindi espandere  **Configurazione computer**.  
  
5.  Espandere **Criteri**, **Impostazioni di Windows** e infine **Impostazioni di protezione**.  
  
6.  Nell'albero **Impostazioni di protezione** espandere **Criteri locali** e quindi fare clic su **Assegnazione diritti utente**.  
  
7.  Nel riquadro risultati fare clic con il pulsante destro del mouse su **Accesso come processo batch** e quindi scegliere Proprietà.  
  
8.  Nella pagina delle proprietà di **Accesso come processo batch** fare clic su **Aggiungi utente o gruppo**.  
  
9. Nella finestra di dialogo **Aggiungi utente o gruppo** fare clic su **Sfoglia**.  
  
10. Nella finestra di dialogo **Seleziona utenti, computer o gruppi** digitare **Administrators**.  
  
11. Fare clic su **Controlla nomi** per verificare che il gruppo Administrators interno sia visualizzato e quindi fare clic su **OK** tre volte per salvare l'impostazione.  
  
## <a name="see-also"></a>Vedere anche  
  

-   [Eseguire la migrazione da Windows SBS 2003](Migrate-Windows-Small-Business-Server-2003-to-Windows-Server-Essentials.md)  
  
-   [Eseguire la migrazione di dati del Server a Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [Eseguire la migrazione da Windows SBS 2003](../migrate/Migrate-Windows-Small-Business-Server-2003-to-Windows-Server-Essentials.md)  
  
-   [Eseguire la migrazione di dati del Server a Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

