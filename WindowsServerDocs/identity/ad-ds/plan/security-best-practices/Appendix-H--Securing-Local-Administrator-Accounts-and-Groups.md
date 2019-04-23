---
ms.assetid: ea015cbc-dea9-4c72-a9d8-d6c826d07608
title: 'Appendice H: protezione Local Administrator account e gruppi'
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 71eea3f623968172076708dbea34d5bbf4a07684
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858692"
---
# <a name="appendix-h-securing-local-administrator-accounts-and-groups"></a>Appendice h: Protezione dell'account di amministratore locale e i gruppi

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-h-securing-local-administrator-accounts-and-groups"></a>Appendice h: Protezione dell'account di amministratore locale e i gruppi  
In tutte le versioni di Windows attualmente in supporto "Mainstream", l'account Administrator locale è disabilitato per impostazione predefinita, che rende inutilizzabile per pass-the-hash e altri attacchi contro il furto di credenziali dell'account. Tuttavia, negli ambienti che contengono sistemi operativi legacy o in cui sono stati abilitati gli account amministratore locali, questi account possono essere utilizzati come descritto in precedenza per propagare i compromessi tra le workstation e server membri. Ogni account amministratore locale e il gruppo deve essere protetto come descritto nelle istruzioni dettagliate che seguono.  

Per informazioni dettagliate sulle considerazioni relative nella protezione dei gruppi predefiniti Administrator (BA), vedere [implementazione di modelli amministrativi con privilegi minimi](../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md).  

#### <a name="controls-for-local-administrator-accounts"></a>Controlli per gli account di amministratore locale  
Per l'account amministratore locale in ogni dominio nella foresta, è necessario configurare le impostazioni seguenti:  

-   Configurare oggetti Criteri di gruppo per limitare l'uso dell'account di amministratore del dominio in sistemi appartenenti a un dominio  
    -   Uno o più oggetti Criteri di gruppo creare e collegare workstation e server membro unità organizzative in ogni dominio, aggiungere l'account amministratore per i seguenti diritti utente in **Computer Configurazione computer\Criteri\Impostazioni computer\Impostazioni di Windows\Impostazioni protezione\Criteri Policies\ Assegnazione diritti utente**:  

        -   Nega accesso al computer dalla rete  

        -   Nega accesso come processo batch  

        -   Nega accesso come servizio  

        -   Nega accesso tramite Servizi Desktop remoto  

#### <a name="step-by-step-instructions-to-secure-local-administrators-groups"></a>Istruzioni dettagliate per proteggere i gruppi di amministratori locali  

###### <a name="configuring-gpos-to-restrict-administrator-account-on-domain-joined-systems"></a>Configurazione di oggetti Criteri di gruppo per limitare l'Account Administrator in sistemi appartenenti a un dominio  

1.  Nelle **Server Manager**, fare clic su **Tools**, fare clic su **Gestione criteri di gruppo**.  

2.  Nell'albero della console, espandere <Forest>\Domains\\<Domain>, quindi **oggetti Criteri di gruppo** (dove <Forest> è il nome della foresta e <Domain> è il nome del dominio in cui si desidera impostare i criteri di gruppo).  

3.  Nell'albero della console, fare doppio clic su **oggetti Criteri di gruppo**, fare clic su **New**.  

    ![gli account amministratore locale sicura e gruppi](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_101.png)  

4.  Nel **nuovo GPO** finestra di dialogo, digitare **<GPO Name>**, fare clic su **OK** (in cui <GPO Name> è il nome di questo oggetto Criteri di gruppo).  

    ![gli account amministratore locale sicura e gruppi](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_102.png)  

5.  Nel riquadro dei dettagli, fare doppio clic su **<GPO Name>**, fare clic su **modifica**.  

6.  Passare a **Computer Configurazione computer\Criteri\Impostazioni di Windows\Impostazioni Protezione\criteri**, fare clic su **Assegnazione diritti utente**.  

    ![gli account amministratore locale sicura e gruppi](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_103.png)  

7.  Configurare i diritti utente per impedire che l'account Administrator locale l'accesso a server membri e workstation in rete eseguendo queste operazioni:  

    1.  Fare doppio clic su **Nega l'accesso al computer dalla rete** e selezionare **Definisci queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo**, digitare il nome utente dell'account amministratore locale e fare clic su **OK**. Questo nome utente sarà **amministratore**, il valore predefinito quando Windows è installato.  

        ![gli account amministratore locale sicura e gruppi](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_104.png)  

    3.  Fare clic su OK.  

        > [!IMPORTANT]  
        > Quando si aggiunge l'account di amministratore a queste impostazioni, specificare se si sta configurando un account amministratore locale o un account di amministratore di dominio dal modo in cui etichettare gli account. Ad esempio, per aggiungere l'account di amministratore del dominio TAILSPINTOYS a queste negare diritti, andare all'account di amministratore per il dominio TAILSPINTOYS, che viene visualizzato come TAILSPINTOYS\Administrator. Se si digita **amministratore** in queste impostazioni diritti utente in Editor oggetti Criteri di gruppo, si limiterà l'account amministratore locale su ciascun computer in cui viene applicato l'oggetto Criteri di gruppo, come descritto in precedenza.  

8.  Configurare i diritti utente per evitare che l'account amministratore locale in grado di accedere come processo batch nel modo seguente:  

    1.  Fare doppio clic su **Nega accesso come processo batch** e selezionare **Definisci queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo**, digitare il nome utente dell'account amministratore locale e fare clic su **OK**. Questo nome utente sarà **amministratore**, il valore predefinito quando Windows è installato.  

        ![gli account amministratore locale sicura e gruppi](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_105.png)  

    3.  Fare clic su **OK**.  

        > [!IMPORTANT]  
        > Quando si aggiunge l'account di amministratore a queste impostazioni, specificare se si configura account amministratore locale o un account amministratore di dominio dal modo in cui etichettare gli account. Ad esempio, per aggiungere l'account di amministratore del dominio TAILSPINTOYS a queste negare diritti, andare all'account di amministratore per il dominio TAILSPINTOYS, che viene visualizzato come TAILSPINTOYS\Administrator. Se si digita **amministratore** in queste impostazioni diritti utente in Editor oggetti Criteri di gruppo, si limiterà l'account amministratore locale su ciascun computer in cui viene applicato l'oggetto Criteri di gruppo, come descritto in precedenza.  

9. Configurare i diritti utente per evitare che l'account amministratore locale in grado di accedere come servizio effettuando le operazioni seguenti:  

    1.  Fare doppio clic su **Nega accesso come servizio** e selezionare **Definisci queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo**, digitare il nome utente dell'account amministratore locale e fare clic su **OK**. Questo nome utente sarà **amministratore**, il valore predefinito quando Windows è installato.  

        ![gli account amministratore locale sicura e gruppi](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_106.png)  

    3.  Fare clic su **OK**.  

        > [!IMPORTANT]  
        > Quando si aggiunge l'account di amministratore a queste impostazioni, specificare se si configura account amministratore locale o un account amministratore di dominio dal modo in cui etichettare gli account. Ad esempio, per aggiungere l'account di amministratore del dominio TAILSPINTOYS a queste negare diritti, andare all'account di amministratore per il dominio TAILSPINTOYS, che viene visualizzato come TAILSPINTOYS\Administrator. Se si digita **amministratore** in queste impostazioni diritti utente in Editor oggetti Criteri di gruppo, si limiterà l'account amministratore locale su ciascun computer in cui viene applicato l'oggetto Criteri di gruppo, come descritto in precedenza.  

10. Configurare i diritti utente per impedire che l'account Administrator locale l'accesso a server membri e workstation tramite Servizi Desktop remoto eseguendo le operazioni seguenti:  

    1.  Fare doppio clic su **Nega accesso tramite Servizi Desktop remoto** e selezionare **Definisci queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo**, digitare il nome utente dell'account amministratore locale e fare clic su **OK**. Questo nome utente sarà **amministratore**, il valore predefinito quando Windows è installato.  

        ![gli account amministratore locale sicura e gruppi](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_107.png)  

    3.  Fare clic su **OK**.  

        > [!IMPORTANT]  
        > Quando si aggiunge l'account di amministratore a queste impostazioni, specificare se si configura account amministratore locale o un account amministratore di dominio dal modo in cui etichettare gli account. Ad esempio, per aggiungere l'account di amministratore del dominio TAILSPINTOYS a queste negare diritti, andare all'account di amministratore per il dominio TAILSPINTOYS, che viene visualizzato come TAILSPINTOYS\Administrator. Se si digita **amministratore** in queste impostazioni diritti utente in Editor oggetti Criteri di gruppo, si limiterà l'account amministratore locale su ciascun computer in cui viene applicato l'oggetto Criteri di gruppo, come descritto in precedenza.  

11. Per uscire **Editor Gestione criteri di gruppo**, fare clic su **File**, fare clic su **uscire**.  

12. Nelle **Gestione criteri di gruppo**, collegare l'oggetto Criteri di gruppo per il server membro e workstation unità organizzative eseguendo le operazioni seguenti:  

    1.  Passare il <Forest>\Domains\\ <Domain> (dove <Forest> è il nome della foresta e <Domain> è il nome del dominio in cui si desidera impostare i criteri di gruppo).  

    2.  Fare doppio clic su unità Organizzativa che oggetto Criteri di gruppo verranno applicati a e fare clic su **collegarne un esistente**.  

        ![gli account amministratore locale sicura e gruppi](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_108.png)  

    3.  Selezionare l'oggetto Criteri di gruppo è stato creato e fare clic su **OK**.  

        ![gli account amministratore locale sicura e gruppi](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_109.png)  

    4.  Creare collegamenti a tutte le altre unità organizzative che includono le workstation.  

    5.  Creare collegamenti a tutte le altre unità organizzative che includono server membri.  

#### <a name="verification-steps"></a>Passaggi di verifica  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Verificare le impostazioni oggetto Criteri di gruppo "Nega accesso al computer dalla rete"  

Da qualsiasi server membro o una workstation che non è interessato dalle modifiche dell'oggetto Criteri di gruppo (ad esempio un jump server), tentare di accedere a un server membro o una workstation in rete che è interessata dalle modifiche dell'oggetto Criteri di gruppo. Per verificare le impostazioni di GPO, provare a eseguire il mapping di unità di sistema usando il **NET USE** comando.  

1.  Accesso locale a qualsiasi server membro o una workstation in cui non è interessato dalle modifiche dell'oggetto Criteri di gruppo.  

2.  Con il mouse, spostare il puntatore del mouse nell'angolo superiore destro o inferiore destro dello schermo. Quando la **accessi** barra viene visualizzata, fare clic su **ricerca**.  

3.  Nel **ricerca** , digitare **prompt dei comandi**, fare doppio clic su **prompt dei comandi**, quindi fare clic su **Esegui come amministratore** aprire con privilegi elevati prompt dei comandi.  

4.  Quando viene richiesto di approvare l'elevazione dei privilegi, fare clic su **Sì**.  

    ![gli account amministratore locale sicura e gruppi](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_110.png)  

5.  Nel **prompt dei comandi** finestra, digitare **net utilizzare \\ \\ <Server Name>\c$ /user:<Server Name>\Administrator**, dove <Server Name> è il nome del membro Server o workstation che si sta provando ad accedere attraverso la rete.  

    > [!NOTE]  
    > Le credenziali di amministratore locale devono essere compreso lo stesso sistema che si sta provando ad accedere attraverso la rete.  

6.  Lo screenshot seguente mostra il messaggio di errore che deve essere visualizzato.  

    ![gli account amministratore locale sicura e gruppi](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_111.png)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Verificare le impostazioni oggetto Criteri di gruppo "Nega accesso come processo batch"  
Da qualsiasi server membro o una workstation interessate dalle modifiche dell'oggetto Criteri di gruppo, accedere localmente.  

###### <a name="create-a-batch-file"></a>Creare un File Batch  

1.  Con il mouse, spostare il puntatore del mouse nell'angolo superiore destro o inferiore destro dello schermo. Quando la **accessi** barra viene visualizzata, fare clic su **ricerca**.  

2.  Nel **ricerca** , digitare **blocco note**, fare clic su **Notepad**.  

3.  Nelle **Notepad**, digitare **dir c:**.  

4.  Fare clic su **File**, fare clic su **Salva con nome**.  

5.  Nel **nome File** , digitare  **<Filename>bat** (dove <Filename> è il nome del nuovo file batch).  

###### <a name="schedule-a-task"></a>Pianificare un'attività  

1.  Con il mouse, spostare il puntatore del mouse nell'angolo superiore destro o inferiore destro dello schermo. Quando la **accessi** barra viene visualizzata, fare clic su **ricerca**.  

2.  Nel **ricerca** casella, digitare utilità di pianificazione e fare clic su **utilità di pianificazione**.  

    > [!NOTE]  
    > Nei computer che eseguono Windows 8, nelle **ricerca** , digitare **pianificare le attività**, fare clic su **pianificare attività**.  

3.  Fare clic su **azione**, fare clic su **Crea attività**.  

4.  Nel **attività di creazione** finestra di dialogo, digitare **<Task Name>** (dove <Task Name> è il nome della nuova attività).  

5.  Scegliere il **azioni** scheda e fare clic su **New**.  

6.  Nel **azione** campo, fare clic su **avviare un programma**.  

7.  Nel **programma/script** campo, fare clic su **Sfoglia**individuare e selezionare il file batch creato nel **creare un File Batch** sezione e fare clic su **aprire**.  

8.  Fare clic su **OK**.  

9. Fare clic sulla scheda **Generale**.  

10. Nel **opzioni di sicurezza** campo, fare clic su **Cambia utente o gruppo**.  

11. Digitare il nome dell'account amministratore locale del sistema, fare clic su **Controlla nomi**, fare clic su **OK**.  

12. Selezionare **eseguire anche se l'utente è connesso o non** e **non archiviare password**. L'attività sarà solo possibile accedere alle risorse del computer locale.  

13. Fare clic su **OK**.  

14. Verrà visualizzata una finestra di dialogo, account utente che richiede le credenziali per eseguire l'attività.  

15. Dopo aver immesso le credenziali, fare clic su **OK**.  

16. Verrà visualizzata una finestra di dialogo simile alla seguente.  

    ![gli account amministratore locale sicura e gruppi](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_112.png)  

###### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Verificare le impostazioni oggetto Criteri di gruppo "Nega accesso come servizio"  

1.  Da qualsiasi server membro o una workstation interessate dalle modifiche dell'oggetto Criteri di gruppo, accedere localmente.  

2.  Con il mouse, spostare il puntatore del mouse nell'angolo superiore destro o inferiore destro dello schermo. Quando la **accessi** barra viene visualizzata, fare clic su **ricerca**.  

3.  Nel **ricerca** , digitare **services**, fare clic su **Services**.  

4.  Individuare e fare doppio clic su **Spooler di stampa**.  

5.  Fare clic sulla scheda **Connessione**.  

6.  Nelle **accedere come** campo, fare clic su **questo account**.  

7.  Fare clic su **esplorare**digitare l'account amministratore locale del sistema, fare clic su **Controlla nomi**, fare clic su **OK**.  

8.  Nel **Password** e **Conferma password** campi, digitare la password dell'account selezionato, quindi scegliere **OK**.  

9. Fare clic su **OK** altre tre volte.  

10. Fare doppio clic su **Spooler di stampa** e fare clic su **riavviare**.  

11. Quando il servizio viene riavviato, verrà visualizzata una finestra di dialogo simile alla seguente.  

    ![gli account amministratore locale sicura e gruppi](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_113.png)  

###### <a name="revert-changes-to-the-printer-spooler-service"></a>Annullare le modifiche per il servizio Spooler di stampa  

1.  Da qualsiasi server membro o una workstation interessate dalle modifiche dell'oggetto Criteri di gruppo, accedere localmente.  

2.  Con il mouse, spostare il puntatore del mouse nell'angolo superiore destro o inferiore destro dello schermo. Quando la **accessi** barra viene visualizzata, fare clic su **ricerca**.  

3.  Nel **ricerca** , digitare **services**, fare clic su **Services**.  

4.  Individuare e fare doppio clic su **Spooler di stampa**.  

5.  Fare clic sulla scheda **Connessione**.  

6.  Nel **accedere come**: campo, selezionare **Systemaccount locale**, fare clic su **OK**.  

###### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>Verificare le impostazioni oggetto Criteri di gruppo "Nega accesso tramite Servizi Desktop remoto"  

1.  Con il mouse, spostare il puntatore del mouse nell'angolo superiore destro o inferiore destro dello schermo. Quando la **accessi** barra viene visualizzata, fare clic su **ricerca**.  

2.  Nel **ricerca** , digitare **connessione desktop remoto**, fare clic su **connessione Desktop remoto**.  

3.  Nel **Computer** digitare il nome del computer in cui si desidera connettersi a e fare clic su **Connect**. (È anche possibile digitare l'indirizzo IP anziché il nome del computer.)  

4.  Quando richiesto, fornire credenziali di sistema locale **amministratore** account.  

5.  Verrà visualizzata una finestra di dialogo simile alla seguente.  

    ![gli account amministratore locale sicura e gruppi](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_114.png)  
