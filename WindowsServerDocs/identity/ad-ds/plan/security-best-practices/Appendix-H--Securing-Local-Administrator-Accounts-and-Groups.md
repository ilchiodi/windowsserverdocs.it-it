---
ms.assetid: ea015cbc-dea9-4c72-a9d8-d6c826d07608
title: Appendice H - protezione dei gruppi e gli account amministratore locale
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 222e6725456bb76267240467469e97c5b64fc888
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="appendix-h-securing-local-administrator-accounts-and-groups"></a>Appendice h: protezione Local Administrator account e gruppi

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-h-securing-local-administrator-accounts-and-groups"></a>Appendice h: protezione Local Administrator account e gruppi  
In tutte le versioni di Windows attualmente coperte dal supporto mainstream, l'account Administrator locale è disabilitato per impostazione predefinita, che rende inutilizzabile per pass-the-hash e altri attacchi contro il furto di credenziali di account. Tuttavia, in ambienti che contengono i sistemi operativi legacy o in cui sono stati abilitati gli account amministratore locali, questi account possono essere utilizzati come descritto in precedenza per propagare i compromessi tra le workstation e server membri. Ogni account amministratore locale e il gruppo deve essere protetto come descritto nelle istruzioni dettagliate che seguono.  

Per informazioni dettagliate sulle considerazioni relative nella protezione dei gruppi di amministratore predefiniti (BA), vedere [implementazione di modelli amministrativi con privilegi minimi](../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md).  

#### <a name="controls-for-local-administrator-accounts"></a>Controlli per gli account amministratore locale  
Per l'account amministratore locale in ogni dominio della foresta, è necessario configurare le impostazioni seguenti:  

-   Configurare oggetti Criteri di gruppo per limitare l'utilizzo dell'account di amministratore del dominio in sistemi appartenenti a un dominio  
    -   Uno o più oggetti Criteri di gruppo creare e collegare workstation e server membro unità organizzative in ogni dominio, aggiungere l'account amministratore per i seguenti diritti utente in **Configurazione computer\Criteri\Impostazioni di Windows\Impostazioni protezione\Criteri Settings\nome diritti**:  

        -   Nega accesso al computer dalla rete  

        -   Nega accesso come processo batch  

        -   Nega accesso come servizio  

        -   Nega accesso tramite Servizi Desktop remoto  

#### <a name="step-by-step-instructions-to-secure-local-administrators-groups"></a>Istruzioni dettagliate per la protezione dei gruppi Administrators locale  

###### <a name="configuring-gpos-to-restrict-administrator-account-on-domain-joined-systems"></a>Configurazione di oggetti Criteri di gruppo per limitare l'Account Administrator in sistemi appartenenti a un dominio  

1.  In **Server Manager**, fare clic su **strumenti**e fare clic su **Gestione criteri di gruppo**.  

2.  Nell'albero della console, espandere <Forest>\Domains\\<Domain>e quindi **oggetti Criteri di gruppo** (in cui <Forest> è il nome dell'insieme di strutture e <Domain> è il nome del dominio in cui si desidera impostare i criteri di gruppo).  

3.  Nell'albero della console fare doppio clic su **oggetti Criteri di gruppo**e fare clic su **New**.  

    ![account amministratore locale sicura e gruppi](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_101.png)  

4.  Nel **nuovo oggetto Criteri di gruppo** la finestra di dialogo, digitare ** <GPO Name> **e fare clic su **OK** (in cui <GPO Name> è il nome di questo oggetto Criteri di gruppo).  

    ![account amministratore locale sicura e gruppi](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_102.png)  

5.  Nel riquadro dei dettagli fare doppio clic su ** <GPO Name> **e fare clic su **modifica**.  

6.  Passare a **Computer Configurazione computer\Criteri\Impostazioni di Windows\Impostazioni Protezione\criteri**e fare clic su **Assegnazione diritti utente**.  

    ![account amministratore locale sicura e gruppi](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_103.png)  

7.  Configurare i diritti utente impedire l'account amministratore locale di accedere al server membri e workstation sulla rete eseguendo le operazioni seguenti:  

    1.  Fare doppio clic su **negare l'accesso al computer dalla rete** e seleziona **definire queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo**, digitare il nome utente dell'account amministratore locale e fare clic su **OK**. Questo nome utente sarà **amministratore**, il valore predefinito quando è installato Windows.  

        ![account amministratore locale sicura e gruppi](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_104.png)  

    3.  Fare clic su OK.  

        > [!IMPORTANT]  
        > Quando si aggiunge l'account amministratore per queste impostazioni, specificare se si configura un account amministratore locale o un account di amministratore di dominio da come creare un'etichetta per gli account. Ad esempio, per aggiungere l'account di amministratore del dominio TAILSPINTOYS a queste negare diritti, accedete all'account amministratore per il dominio TAILSPINTOYS, che vengono visualizzati come TAILSPINTOYS\Administrator. Se si digita **amministratore** in queste impostazioni diritti utente in Editor criteri di gruppo, si limiterà l'account amministratore locale su ciascun computer in cui viene applicato l'oggetto Criteri di gruppo, come descritto in precedenza.  

8.  Configurare i diritti utente per impedire che l'account Administrator locale l'accesso come processo batch effettuando le operazioni seguenti:  

    1.  Fare doppio clic su **Nega accesso come processo batch** e seleziona **definire queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo**, digitare il nome utente dell'account amministratore locale e fare clic su **OK**. Questo nome utente sarà **amministratore**, il valore predefinito quando è installato Windows.  

        ![account amministratore locale sicura e gruppi](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_105.png)  

    3.  Fare clic su **OK**.  

        > [!IMPORTANT]  
        > Quando si aggiunge l'account amministratore per queste impostazioni, specificare se si configura account amministratore locale o un account amministratore di dominio da come creare un'etichetta per gli account. Ad esempio, per aggiungere l'account di amministratore del dominio TAILSPINTOYS a queste negare diritti, accedete all'account amministratore per il dominio TAILSPINTOYS, che vengono visualizzati come TAILSPINTOYS\Administrator. Se si digita **amministratore** in queste impostazioni diritti utente in Editor criteri di gruppo, si limiterà l'account amministratore locale su ciascun computer in cui viene applicato l'oggetto Criteri di gruppo, come descritto in precedenza.  

9. Configurare i diritti utente per impedire che l'account Administrator locale l'accesso come servizio effettuando le operazioni seguenti:  

    1.  Fare doppio clic su **Nega accesso come servizio** e seleziona **definire queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo**, digitare il nome utente dell'account amministratore locale e fare clic su **OK**. Questo nome utente sarà **amministratore**, il valore predefinito quando è installato Windows.  

        ![account amministratore locale sicura e gruppi](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_106.png)  

    3.  Fare clic su **OK**.  

        > [!IMPORTANT]  
        > Quando si aggiunge l'account amministratore per queste impostazioni, specificare se si configura account amministratore locale o un account amministratore di dominio da come creare un'etichetta per gli account. Ad esempio, per aggiungere l'account di amministratore del dominio TAILSPINTOYS a queste negare diritti, accedete all'account amministratore per il dominio TAILSPINTOYS, che vengono visualizzati come TAILSPINTOYS\Administrator. Se si digita **amministratore** in queste impostazioni diritti utente in Editor criteri di gruppo, si limiterà l'account amministratore locale su ciascun computer in cui viene applicato l'oggetto Criteri di gruppo, come descritto in precedenza.  

10. Configurare i diritti utente impedire l'account amministratore locale di accedere al server membri e workstation tramite Servizi Desktop remoto eseguendo le operazioni seguenti:  

    1.  Fare doppio clic su **Nega accesso tramite Servizi Desktop remoto** e seleziona **definire queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo**, digitare il nome utente dell'account amministratore locale e fare clic su **OK**. Questo nome utente sarà **amministratore**, il valore predefinito quando è installato Windows.  

        ![account amministratore locale sicura e gruppi](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_107.png)  

    3.  Fare clic su **OK**.  

        > [!IMPORTANT]  
        > Quando si aggiunge l'account amministratore per queste impostazioni, specificare se si configura account amministratore locale o un account amministratore di dominio da come creare un'etichetta per gli account. Ad esempio, per aggiungere l'account di amministratore del dominio TAILSPINTOYS a queste negare diritti, accedete all'account amministratore per il dominio TAILSPINTOYS, che vengono visualizzati come TAILSPINTOYS\Administrator. Se si digita **amministratore** in queste impostazioni diritti utente in Editor criteri di gruppo, si limiterà l'account amministratore locale su ciascun computer in cui viene applicato l'oggetto Criteri di gruppo, come descritto in precedenza.  

11. Per uscire da **Editor Gestione criteri di gruppo**, fare clic su **File**e fare clic su **uscire**.  

12. In **Gestione criteri di gruppo**, collegare l'oggetto Criteri di gruppo per il server membro e workstation unità organizzative effettuando le operazioni seguenti:  

    1.  Passare al <Forest>\Domains\\<Domain> (in cui <Forest> è il nome dell'insieme di strutture e <Domain> è il nome del dominio in cui si desidera impostare i criteri di gruppo).  

    2.  Fare doppio clic su oggetto Criteri di gruppo verranno applicate a e fare clic su, l'unità Organizzativa **collegare un oggetto Criteri di gruppo esistente**.  

        ![account amministratore locale sicura e gruppi](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_108.png)  

    3.  Selezionare l'oggetto Criteri di gruppo creato e fare clic su **OK**.  

        ![account amministratore locale sicura e gruppi](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_109.png)  

    4.  Creare collegamenti a tutte le altre unità organizzative contenenti le workstation.  

    5.  Creare collegamenti a tutte le altre unità organizzative contenenti i server membri.  

#### <a name="verification-steps"></a>Passaggi di verifica  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Verificare le impostazioni oggetto Criteri di gruppo "Nega accesso al computer dalla rete"  

Da qualsiasi server membro o una workstation in cui non è interessata dall'oggetto Criteri di gruppo viene modificato (ad esempio un jump server), tentare di accedere a un server membro o una workstation in rete che è interessata dall'oggetto Criteri di gruppo viene modificato. Per verificare le impostazioni oggetto Criteri di gruppo, tentare di eseguire il mapping di unità di sistema utilizzando il **NET USE** comando.  

1.  Accedere localmente a qualsiasi server membro o una workstation in cui non è interessata dall'oggetto Criteri di gruppo viene modificato.  

2.  Con il mouse, sposta il puntatore nell'angolo superiore destro o in basso a destra dello schermo. Quando il **accessi** barra viene visualizzata, fare clic su **ricerca**.  

3.  Nel **ricerca** digitare **prompt dei comandi**, fare doppio clic su **prompt dei comandi**, quindi fare clic su **Esegui come amministratore** per aprire un prompt dei comandi con privilegi elevati.  

4.  Quando viene richiesto di approvare l'elevazione dei privilegi, fare clic su **Sì**.  

    ![account amministratore locale sicura e gruppi](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_110.png)  

5.  Nel **prompt dei comandi** finestra, digitare **net uso \ \ \<Server Name>\c$ /user:<Server Name>\Administrator**, dove <Server Name> è il nome del server membri o workstation che si sta tentando di accedere attraverso la rete.  

    > [!NOTE]  
    > Le credenziali di amministratore locale devono essere dallo stesso sistema che si sta tentando di accedere attraverso la rete.  

6.  La schermata seguente mostra il messaggio di errore che dovrebbe essere visualizzato.  

    ![account amministratore locale sicura e gruppi](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_111.png)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Verificare le impostazioni oggetto Criteri di gruppo "Nega accesso come processo batch"  
Da qualsiasi server membro o una workstation interessati dalle modifiche dell'oggetto Criteri di gruppo, accedere localmente.  

###### <a name="create-a-batch-file"></a>Creare un File Batch  

1.  Con il mouse, sposta il puntatore nell'angolo superiore destro o in basso a destra dello schermo. Quando il **accessi** barra viene visualizzata, fare clic su **ricerca**.  

2.  Nel **ricerca** digitare **blocco note**e fare clic su **blocco note**.  

3.  In **blocco note**, tipo **c: dir**.  

4.  Fare clic su **File**e fare clic su **Salva con nome**.  

5.  Nel **nome File** digitare ** <Filename>. bat** (in cui <Filename> è il nome del nuovo file batch).  

###### <a name="schedule-a-task"></a>Pianificare un'attività  

1.  Con il mouse, sposta il puntatore nell'angolo superiore destro o in basso a destra dello schermo. Quando il **accessi** barra viene visualizzata, fare clic su **ricerca**.  

2.  Nel **ricerca** casella, digitare l'utilità di pianificazione e fare clic su **utilità di pianificazione**.  

    > [!NOTE]  
    > Nei computer che eseguono Windows 8, nel **ricerca** digitare **Pianifica attività**e fare clic su **Pianifica attività**.  

3.  Fare clic su **azione**e fare clic su **Crea attività**.  

4.  Nel **Crea attività** la finestra di dialogo, digitare ** <Task Name> ** (in cui <Task Name> è il nome della nuova attività).  

5.  Fare clic su di **azioni** scheda e fare clic su **New**.  

6.  Nel **azione** campo, fare clic su **avviare un programma**.  

7.  Nel **programma o script** campo, fare clic su **Sfoglia**, individuare e selezionare il file batch creato nel **creare un File Batch** sezione e fare clic su **aprire**.  

8.  Fare clic su **OK**.  

9. Fare clic su di **generale** scheda.  

10. Nel **opzioni di protezione** campo, fare clic su **Cambia utente o gruppo**.  

11. Digitare il nome dell'account amministratore locale del sistema, fare clic su **Controlla nomi**e fare clic su **OK**.  

12. Selezionare **eseguire se l'utente è connesso o non** e **non archiviare password**. L'attività sarà solo possibile accedere alle risorse del computer locale.  

13. Fare clic su **OK**.  

14. Dovrebbe essere visualizzata una finestra di dialogo, account utente che richiede le credenziali necessarie per eseguire l'attività.  

15. Dopo aver immesso le credenziali, fare clic su **OK**.  

16. Dovrebbe essere visualizzata una finestra di dialogo simile al seguente.  

    ![account amministratore locale sicura e gruppi](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_112.png)  

###### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Verificare le impostazioni oggetto Criteri di gruppo "Nega accesso come servizio"  

1.  Da qualsiasi server membro o una workstation interessati dalle modifiche dell'oggetto Criteri di gruppo, accedere localmente.  

2.  Con il mouse, sposta il puntatore nell'angolo superiore destro o in basso a destra dello schermo. Quando il **accessi** barra viene visualizzata, fare clic su **ricerca**.  

3.  Nel **ricerca** digitare **servizi**e fare clic su **servizi**.  

4.  Individuare e fare doppio clic su **Spooler di stampa**.  

5.  Fare clic su di **accesso** scheda.  

6.  In **accedere come** campo, fare clic su **questo account**.  

7.  Fare clic su **Sfoglia**, digitare l'account amministratore locale del sistema, fare clic su **Controlla nomi**e fare clic su **OK**.  

8.  Nel **Password** e **Conferma password** campi, digitare la password dell'account selezionato e scegliere **OK**.  

9. Fare clic su **OK** tre volte.  

10. Fare doppio clic su **Spooler di stampa** e fare clic su **riavviare**.  

11. Quando viene riavviato il servizio, verrà visualizzata una finestra di dialogo simile al seguente.  

    ![account amministratore locale sicura e gruppi](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_113.png)  

###### <a name="revert-changes-to-the-printer-spooler-service"></a>Ripristinare le modifiche per il servizio Spooler di stampa  

1.  Da qualsiasi server membro o una workstation interessati dalle modifiche dell'oggetto Criteri di gruppo, accedere localmente.  

2.  Con il mouse, sposta il puntatore nell'angolo superiore destro o in basso a destra dello schermo. Quando il **accessi** barra viene visualizzata, fare clic su **ricerca**.  

3.  Nel **ricerca** digitare **servizi**e fare clic su **servizi**.  

4.  Individuare e fare doppio clic su **Spooler di stampa**.  

5.  Fare clic su di **accesso** scheda.  

6.  Nel **accedere come**: campi, selezionare **Systemaccount locale**e fare clic su **OK**.  

###### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>Verificare le impostazioni oggetto Criteri di gruppo "Nega accesso tramite Servizi Desktop remoto"  

1.  Con il mouse, sposta il puntatore nell'angolo superiore destro o in basso a destra dello schermo. Quando il **accessi** barra viene visualizzata, fare clic su **ricerca**.  

2.  Nel **ricerca** digitare **connessione desktop remoto**e fare clic su **connessione Desktop remoto**.  

3.  Nel **Computer** , digitare il nome del computer che si desidera connettersi e fare clic su **Connetti**. (È anche possibile digitare l'indirizzo IP anziché il nome del computer.)  

4.  Quando richiesto, fornire le credenziali per il sistema locale del **amministratore** account.  

5.  Dovrebbe essere visualizzata una finestra di dialogo simile al seguente.  

    ![account amministratore locale sicura e gruppi](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_114.png)  
