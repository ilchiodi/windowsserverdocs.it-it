---
title: "Eseguire le attività post-migrazione per Windows Server Essentials migration1"
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
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="perform-post-migration-tasks-for-windows-server-essentials-migration1"></a>Eseguire le attività post-migrazione per Windows Server Essentials migration1

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Le attività seguenti consentono di completare la configurazione del Server di destinazione con alcune delle stesse impostazioni nel Server di origine. Si potrebbe essere disabilitato alcune di queste impostazioni nel Server di origine durante il processo di migrazione, in modo che non sono stati migrati al Server di destinazione. Oppure sono passaggi di configurazione facoltativo che vuoi eseguire.  
  

-   [Eliminare le voci DNS del Server di origine](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_DeleteDNSEntries)  
  
-   [Condividere line-of-business e altre cartelle di dati applicazione](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_ShareLineOfBusinessAndOtherApplications)  
  
-   [Risolvere i problemi dei computer client dopo la migrazione](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_FixClientComputerIssuesAfterMigrating)  
  
-   [Concedere al gruppo Administrators interno il diritto di accedere come processo batch](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_AdminGroup)  

-   [Eliminare le voci DNS del Server di origine](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_DeleteDNSEntries)  
  
-   [Condividere line-of-business e altre cartelle di dati applicazione](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_ShareLineOfBusinessAndOtherApplications)  
  
-   [Risolvere i problemi dei computer client dopo la migrazione](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_FixClientComputerIssuesAfterMigrating)  
  
-   [Concedere al gruppo Administrators interno il diritto di accedere come processo batch](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_AdminGroup)  

  
##  <a name="BKMK_DeleteDNSEntries"></a>Eliminare le voci DNS del Server di origine  
 Dopo rimuovere le autorizzazioni del Server di origine, il server del servizio DNS (Domain Name) potrebbe contenere voci che puntano al Server di origine. Eliminare queste voci DNS.  
  
#### <a name="to-delete-dns-entries-that-point-to-the-source-server"></a>Per eliminare le voci DNS che fanno riferimento al Server di origine  
  
1.  Nel Server di destinazione, aprire **gestore DNS**.  
  
2.  In Gestore DNS, fare clic sul nome del server, fare clic su **proprietà**, quindi fare clic su di **server d'inoltro** scheda.  
  
3.  Determinare se è presente una voce dell'elenco di server d'inoltro che punta al Server di origine. Se è presente, fare clic su **modifica**e quindi eliminare tale voce nel **Modifica server d'inoltro** finestra.  
  
4.  In **gestore DNS**, espandere il nome del server e quindi espandere **zone di ricerca diretta**.  
  
5.  Per ogni zona di ricerca diretta, fare clic sulla zona, fare clic su **proprietà**, quindi fare clic su di **server dei nomi** scheda.  
  
6.  Fare clic su una voce di **server dei nomi** casella che punta al Server di origine, fare clic su **rimuovere**e quindi fare clic su **OK**.  
  
7.  Ripetere i passaggi 5 e 6 fino a quando non vengono rimossi tutti i puntatori al Server di origine.  
  
8.  Fare clic su **OK** per chiudere la **proprietà** finestra.  
  
9. Nel **gestore DNS** espandere **zone di ricerca inversa**.  
  
10. Ripetere i passaggi da 6 a 9 per rimuovere tutte le zone di ricerca inversa che fanno riferimento al Server di origine.  
  
##  <a name="BKMK_ShareLineOfBusinessAndOtherApplications"></a>Condividere line-of-business e altre cartelle di dati applicazione  
 È necessario impostare le autorizzazioni delle cartelle condivise e le autorizzazioni NTFS per line-of-business e altre cartelle di dati dell'applicazione che è stato copiato nel Server di destinazione. Dopo aver impostato le autorizzazioni, le cartelle condivise vengono visualizzate nel Dashboard di Windows Server Essentials nel **archiviazione** sezione.  
  
 Se si utilizza uno script di accesso per il mapping delle unità alle cartelle condivise, è necessario aggiornare lo script da eseguire il mapping di unità nel Server di destinazione.  
  
##  <a name="BKMK_FixClientComputerIssuesAfterMigrating"></a>Risolvere i problemi dei computer client dopo la migrazione  
 Se si esegue la migrazione a Windows Server Essentials da Windows Small Business Server 2003 Premium Edition con Microsoft Internet Security e Acceleration (ISA) Server installato, i computer client nella rete hanno ancora Microsoft Firewall Client e Internet Explorer configurati per utilizzare un server proxy.  
  
 Questo causa problemi di connettività del client computer, perché il server proxy non esiste più. Se è presente un altro server proxy configurato, i computer client continuano a utilizzare il server che esegue Windows SBS 2003 per il server proxy. Per risolvere questo problema, è necessario riconfigurare Internet Explorer di non utilizzare un server proxy o usare il nuovo server proxy.  
  
#### <a name="to-reconfigure-internet-explorer"></a>Per riconfigurare Internet Explorer  
  
1.  In Internet Explorer, fare clic su **strumenti**, quindi fare clic su **Opzioni Internet**.  
  
2.  Fare clic su di **connessioni** scheda, fare clic su **impostazioni LAN**, quindi effettuare una delle operazioni seguenti:  
  
    -   Se non si utilizza un server proxy nella rete, deselezionare tutte le caselle di controllo di **Impostazioni rete locale (LAN)** la finestra di dialogo.  
  
    -   Se si desidera utilizzare un nuovo server proxy nella rete:  
  
        1.  Nel **Impostazioni rete locale (LAN)** la finestra di dialogo, deselezionare le caselle di controllo nel **configurazione automatica** sezione.  
  
        2.  Nel **server Proxy** sezione, verificare che siano selezionate entrambe le caselle di controllo.  
  
        3.  Nel **indirizzo**, digitare il nome di dominio completo (FQDN) del server proxy.  
  
        4.  Nel **porta** digitare **80**.  
  
3.  Fare clic su **OK** due volte.  
  
4.  Passare a un sito Web per verificare che le impostazioni di connessione siano corrette.  
  
##  <a name="BKMK_AdminGroup"></a>Concedere al gruppo Administrators interno il diritto di accedere come processo batch  
 Dopo la migrazione di un dominio Windows Small Business Server 2003 esistente a Windows Server Essentials, è consigliabile concedere al gruppo Administrators interno il diritto di accedere come processo batch. Verificare che il gruppo Administrators interno abbia ancora il diritto di accedere come processo batch al Server di destinazione. Gli amministratori devono questo diritto per eseguire un avviso nel Server di destinazione senza effettuare l'accesso.  
  
#### <a name="to-give-the-built-in-administrators-group-the-right-to-log-on-as-a-batch-job"></a>Per assegnare predefinito del gruppo Administrators il diritto di accedere come processo batch  
  
1.  Nel Server di destinazione, aprire il **Gestione criteri di gruppo** strumento di amministrazione.  
  
2.  Nel **Gestione criteri di gruppo** albero della Console, espandere **foresta:***< ServerName\ >*, domini e infine il server.  
  
3.  Espandere **i controller di dominio**, fare doppio clic su **criterio controller di dominio predefiniti**, quindi fare clic su **modifica**.  
  
4.  In **Editor Gestione criteri di gruppo**, fare clic su **criterio controller di dominio predefiniti***< ServerName\ >***criteri**, quindi espandere **configurazione Computer**.  
  
5.  Espandere **criteri**, espandere **le impostazioni di Windows**, quindi espandere **le impostazioni di sicurezza**.  
  
6.  Nel **le impostazioni di sicurezza** espandere **criteri locali**, quindi fare clic su **Assegnazione diritti utente**.  
  
7.  Nel riquadro dei risultati, fare doppio clic su **accedere come processo batch**, quindi fare clic su proprietà.  
  
8.  Nel **accedere come processo batch proprietà** pagina, fare clic su **Aggiungi utente o gruppo**.  
  
9. Nel **Aggiungi utente o gruppo** la finestra di dialogo, fare clic su **Sfoglia**.  
  
10. Nel **Seleziona utenti, computer o gruppi** la finestra di dialogo, digitare **amministratori**.  
  
11. Fare clic su **Controlla nomi** per verificare che il gruppo Administrators interno viene visualizzata e quindi fare clic su **OK** tre volte per salvare l'impostazione.  
  
## <a name="see-also"></a>Vedere anche  
  

-   [Eseguire la migrazione da Windows SBS 2003](Migrate-Windows-Small-Business-Server-2003-to-Windows-Server-Essentials.md)  
  
-   [Eseguire la migrazione di dati del Server a Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [Eseguire la migrazione da Windows SBS 2003](../migrate/Migrate-Windows-Small-Business-Server-2003-to-Windows-Server-Essentials.md)  
  
-   [Eseguire la migrazione di dati del Server a Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

