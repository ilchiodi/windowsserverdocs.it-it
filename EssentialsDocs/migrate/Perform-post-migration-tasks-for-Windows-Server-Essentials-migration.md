---
title: Eseguire le attività post-migrazione per Windows Server Essentials migration1
description: Viene descritto come usare Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: f2d236a4-0d62-4961-9d1f-332054e06f6d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: b5093772e22fc95a19e800db5c83dec261e7b63a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852425"
---
# <a name="perform-post-migration-tasks-for-windows-server-essentials-migration1"></a>Eseguire le attività post-migrazione per Windows Server Essentials migration1

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Le attività seguenti aiutano a completare la configurazione del server di destinazione con alcune impostazioni uguali a quelle del server di origine. Durante il processo di migrazione è possibile che alcune di queste impostazioni siano state disabilitate sul server di origine e di conseguenza non ne sia stata eseguita la migrazione al server di destinazione. Oppure sono passaggi di configurazione facoltativi che si vuole eseguire.  
  

-   [Elimina le voci DNS del server di origine](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_DeleteDNSEntries)  
  
-   [Condividere le cartelle di dati delle applicazioni line-of-business e di altro settore](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_ShareLineOfBusinessAndOtherApplications)  
  
-   [Risolvere i problemi dei computer client dopo la migrazione](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_FixClientComputerIssuesAfterMigrating)  
  
-   [Assegnare al gruppo Administrators predefinito il diritto di accesso come processo batch](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_AdminGroup)  

-   [Elimina le voci DNS del server di origine](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_DeleteDNSEntries)  
  
-   [Condividere le cartelle di dati delle applicazioni line-of-business e di altro settore](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_ShareLineOfBusinessAndOtherApplications)  
  
-   [Risolvere i problemi dei computer client dopo la migrazione](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_FixClientComputerIssuesAfterMigrating)  
  
-   [Assegnare al gruppo Administrators predefinito il diritto di accesso come processo batch](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_AdminGroup)  

  
##  <a name="delete-dns-entries-of-the-source-server"></a><a name="BKMK_DeleteDNSEntries"></a>Elimina le voci DNS del server di origine  
 Anche dopo aver rimosso le autorizzazioni per il server di origine, il server Domain Name Service (DNS) potrebbe contenere voci che puntano al server di origine. Eliminare queste voci DNS.  
  
#### <a name="to-delete-dns-entries-that-point-to-the-source-server"></a>Per eliminare le voci relative a DNS che fanno riferimento al server di origine  
  
1.  Nel server di destinazione aprire **Gestore DNS**.  
  
2.  In Gestore DNS fare clic con il pulsante destro del mouse sul nome server, scegliere **Proprietà** e quindi fare clic sulla scheda **Server d'inoltro**.  
  
3.  Determinare se nell'elenco di server d'inoltro c'è una voce che punta al server di origine. Se c'è, fare clic su **Modifica** e quindi eliminare tale voce nella finestra **Modifica server d'inoltro**.  
  
4.  In **Gestore DNS** espandere il nome server e quindi espandere **Zone di ricerca diretta**.  
  
5.  Per ogni zona di ricerca diretta fare clic con il pulsante destro del mouse sulla zona, scegliere **Proprietà** e quindi fare clic sulla scheda **Server dei nomi**.  
  
6.  Nella casella **Server dei nomi** fare clic su una voce che punta al server di origine, fare clic su **Rimuovi** e quindi fare clic su **OK**.  
  
7.  Ripetere i passaggi 5 e 6 finché tutti i puntatori al server di origine non sono stati rimossi.  
  
8.  Fare clic su **OK** per chiudere la finestra di dialogo **Proprietà**.  
  
9. Nella console **Gestore DNS** espandere **Zone di ricerca inversa**.  
  
10. Ripetere i passaggi da 6 a 9 per rimuovere tutte le zone di ricerca inversa che fanno riferimento al server di origine.  
  
##  <a name="share-line-of-business-and-other-application-data-folders"></a><a name="BKMK_ShareLineOfBusinessAndOtherApplications"></a>Condividere le cartelle di dati delle applicazioni line-of-business e di altro settore  
 È necessario configurare le autorizzazioni delle cartelle condivise e le autorizzazioni NTFS per le cartelle di dati delle applicazioni LOB e di altre applicazioni copiate nel server di destinazione. Dopo aver impostato le autorizzazioni, le cartelle condivise vengono visualizzate nel dashboard di Windows Server Essentials nella sezione **archiviazione** .  
  
 Se si usa uno script di accesso che esegue il mapping delle unità alle cartelle condivise, sarà necessario aggiornare lo script per consentire il mapping alle unità nel server di destinazione.  
  
##  <a name="fix-client-computer-issues-after-migrating"></a><a name="BKMK_FixClientComputerIssuesAfterMigrating"></a>Risolvere i problemi dei computer client dopo la migrazione  
 Se si esegue la migrazione a Windows Server Essentials da Windows Small Business Server 2003 Premium Edition con Microsoft Internet Security and Acceleration (ISA) Server installato, i computer client nella rete hanno ancora Microsoft Firewall client e Internet Visualizzatore configurato per l'utilizzo di un server proxy.  
  
 Questo causa problemi di connettività nei computer client, perché il server proxy non esiste più. Se è stato configurato un server proxy diverso, i computer client continuano a usare il server che esegue Windows SBS 2003 per il server proxy. Per risolvere questo problema, è necessario riconfigurare Internet Explorer in modo che non usi un server proxy o che usi il nuovo server proxy.  
  
#### <a name="to-reconfigure-internet-explorer"></a>Per riconfigurare Internet Explorer  
  
1.  In Internet Explorer fare clic su **Strumenti** e quindi su **Opzioni Internet**.  
  
2.  Fare clic sulla scheda **Connessioni**, su **Impostazioni LAN** e quindi eseguire una delle operazioni seguenti:  
  
    -   Se non si userà un server proxy nella rete, deselezionare tutte le caselle di controllo nella finestra di dialogo **Impostazioni rete locale (LAN)** .  
  
    -   Se si vuole usare un nuovo server proxy nella rete:  
  
        1.  Nella finestra di dialogo **Impostazioni rete locale (LAN)** deselezionare le caselle di controllo nella sezione **Configurazione automatica**.  
  
        2.  Nella sezione **Server proxy** verificare che entrambe le caselle di controllo siano selezionate.  
  
        3.  Nella casella **Indirizzo** digitare il nome di dominio completo (FQDN) del server proxy.  
  
        4.  Nella casella **Porta** digitare **80**.  
  
3.  Fare clic su **OK** due volte.  
  
4.  Accedere a un sito Web per verificare che le impostazioni di connessione siano corrette.  
  
##  <a name="give-the-built-in-administrators-group-the-right-to-log-on-as-a-batch-job"></a><a name="BKMK_AdminGroup"></a>Assegnare al gruppo Administrators predefinito il diritto di accesso come processo batch  
 Dopo aver eseguito la migrazione di un dominio Windows Small Business Server 2003 esistente a Windows Server Essentials, è necessario assegnare al gruppo Administrators predefinito il diritto di accedere come processo batch. Verificare che il gruppo Administrators interno abbia ancora il diritto di accedere come processo batch al server di destinazione. Questo diritto serve agli amministratori per eseguire un avviso nel server di destinazione senza accedervi.  
  
#### <a name="to-give-the-built-in-administrators-group-the-right-to-log-on-as-a-batch-job"></a>Per concedere al gruppo Administrators interno il diritto di accedere come processo batch  
  
1. Nel server di destinazione aprire lo strumento di amministrazione **Gestione Criteri di gruppo**.  
  
2. Nell'albero della console di **gestione criteri di gruppo** espandere **foresta:** *< nomeserver\>* , espandere domini, quindi espandere il server.  
  
3. Espandere **Controller di dominio**, fare clic con il pulsante destro del mouse su **Criterio controller di dominio predefiniti** e quindi scegliere **Modifica**.  
  
4. In **Editor gestione criteri di gruppo**fare clic su **criterio controller di dominio predefiniti**<em>< nomeserver\></em> **criteri**, quindi espandere **Configurazione computer**.  
  
5. Espandere **Criteri**, **Impostazioni di Windows** e infine **Impostazioni di protezione**.  
  
6. Nell'albero **Impostazioni di protezione** espandere **Criteri locali** e quindi fare clic su **Assegnazione diritti utente**.  
  
7. Nel riquadro risultati fare clic con il pulsante destro del mouse su **Accesso come processo batch** e quindi scegliere Proprietà.  
  
8. Nella pagina delle proprietà di **Accesso come processo batch** fare clic su **Aggiungi utente o gruppo**.  
  
9. Nella finestra di dialogo **Aggiungi utente o gruppo** fare clic su **Sfoglia**.  
  
10. Nella finestra di dialogo **Seleziona utenti, computer o gruppi** digitare **Administrators**.  
  
11. Fare clic su **Controlla nomi** per verificare che il gruppo Administrators interno sia visualizzato e quindi fare clic su **OK** tre volte per salvare l'impostazione.  
  
## <a name="see-also"></a>Vedere anche  
  

-   [Eseguire la migrazione da Windows SBS 2003](Migrate-Windows-Small-Business-Server-2003-to-Windows-Server-Essentials.md)  
  
-   [Eseguire la migrazione dei dati del server a Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [Eseguire la migrazione da Windows SBS 2003](../migrate/Migrate-Windows-Small-Business-Server-2003-to-Windows-Server-Essentials.md)  
  
-   [Eseguire la migrazione dei dati del server a Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

