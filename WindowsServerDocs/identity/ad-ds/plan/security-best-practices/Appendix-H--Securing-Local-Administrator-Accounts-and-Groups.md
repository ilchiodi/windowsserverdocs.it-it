---
ms.assetid: ea015cbc-dea9-4c72-a9d8-d6c826d07608
title: Appendice H-protezione degli account e dei gruppi dell'amministratore locale
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 7e0cff62851250009d8af6ec7d87ec8191dcaec0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408639"
---
# <a name="appendix-h-securing-local-administrator-accounts-and-groups"></a>Appendice H: Protezione di account e gruppi di amministratori locali

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-h-securing-local-administrator-accounts-and-groups"></a>Appendice H: Protezione di account e gruppi di amministratori locali  
In tutte le versioni di Windows attualmente in supporto "Mainstream", l'account Administrator locale è disabilitato per impostazione predefinita, che rende inutilizzabile per pass-the-hash e altri attacchi contro il furto di credenziali dell'account. Tuttavia, negli ambienti che contengono sistemi operativi legacy o in cui sono stati abilitati gli account di amministratore locale, è possibile usare questi account come descritto in precedenza per propagare la compromissione tra i server membri e le workstation. Ogni account amministratore locale e gruppo deve essere protetto come descritto nelle istruzioni dettagliate riportate di seguito.  

Per informazioni dettagliate sulle considerazioni relative alla protezione dei gruppi predefiniti Administrator (BA), vedere [implementazione dei modelli amministrativi con privilegi minimi](../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md).  

#### <a name="controls-for-local-administrator-accounts"></a>Controlli per gli account di amministratore locale  
Per l'account amministratore locale in ogni dominio della foresta, è necessario configurare le impostazioni seguenti:  

-   Configurare gli oggetti Criteri di gruppo per limitare l'utilizzo dell'account amministratore del dominio nei sistemi aggiunti a un dominio  
    -   In uno o più oggetti Criteri di gruppo creati e collegati alle unità organizzative workstation e server membro in ogni dominio, aggiungere l'account amministratore ai seguenti diritti utente in **computer Computer\criteri\impostazioni Windows\Impostazioni protezione\Criteri locali\Assegnazione diritti Assegnazioni**:  

        -   Nega accesso al computer dalla rete  

        -   Nega accesso come processo batch  

        -   Nega accesso come servizio  

        -   Nega accesso tramite Servizi Desktop remoto  

#### <a name="step-by-step-instructions-to-secure-local-administrators-groups"></a>Istruzioni dettagliate per la protezione dei gruppi di amministratori locali  

###### <a name="configuring-gpos-to-restrict-administrator-account-on-domain-joined-systems"></a>Configurazione degli oggetti Criteri di gruppo per limitare l'account amministratore nei sistemi aggiunti a un dominio  

1.  In **Server Manager**fare clic su **strumenti**e quindi su **Gestione criteri di gruppo**.  

2.  Nell'albero della console espandere <Forest> \ Domains @ no__t-1 @ no__t-2, quindi **criteri di gruppo oggetti** (dove <Forest> è il nome della foresta e <Domain> è il nome del dominio in cui si desidera impostare il criteri di gruppo).  

3.  Nell'albero della console fare clic con il pulsante destro del mouse su **criteri di gruppo oggetti**, quindi scegliere **nuovo**.  

    ![proteggere gli account di amministratore locale e i gruppi](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_101.png)  

4.  Nella finestra di dialogo **nuovo oggetto Criteri** di gruppo digitare **<GPO Name>** , quindi fare clic su **OK** (dove <GPO Name> è il nome dell'oggetto Criteri di gruppo).  

    ![proteggere gli account di amministratore locale e i gruppi](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_102.png)  

5.  Nel riquadro dei dettagli fare clic con il pulsante destro del mouse su **<GPO Name>** , quindi scegliere **modifica**.  

6.  Passare a **computer Computer\criteri\impostazioni Windows\Impostazioni protezione\Criteri criteri**e fare clic su **assegnazione diritti utente**.  

    ![proteggere gli account di amministratore locale e i gruppi](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_103.png)  

7.  Configurare i diritti utente per impedire all'account amministratore locale di accedere ai server membri e alle workstation in rete eseguendo le operazioni seguenti:  

    1.  Fare doppio clic su **Nega accesso al computer dalla rete** e selezionare **Definisci le impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo**, digitare il nome utente dell'account amministratore locale e fare clic su **OK**. Il nome utente sarà **Administrator**, il valore predefinito quando viene installato Windows.  

        ![proteggere gli account di amministratore locale e i gruppi](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_104.png)  

    3.  Fare clic su OK.  

        > [!IMPORTANT]  
        > Quando si aggiunge l'account Administrator a queste impostazioni, è necessario specificare se si sta configurando un account amministratore locale o un account amministratore di dominio mediante l'etichetta degli account. Ad esempio, per aggiungere l'account amministratore del dominio TAILSPINTOYS a questi diritti nega, è necessario passare all'account Administrator per il dominio TAILSPINTOYS, che verrebbe visualizzato come TAILSPINTOYS\Administrator. Se si digita **Administrator** in queste impostazioni di diritti utente nel Editor oggetti Criteri di gruppo, si limiterà l'account amministratore locale in ogni computer a cui viene applicato l'oggetto Criteri di gruppo, come descritto in precedenza.  

8.  Configurare i diritti utente per impedire all'account amministratore locale di accedere come processo batch eseguendo le operazioni seguenti:  

    1.  Fare doppio clic su **Nega accesso come processo batch** e selezionare **Definisci queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo**, digitare il nome utente dell'account amministratore locale e fare clic su **OK**. Il nome utente sarà **Administrator**, il valore predefinito quando viene installato Windows.  

        ![proteggere gli account di amministratore locale e i gruppi](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_105.png)  

    3.  Fare clic su **OK**.  

        > [!IMPORTANT]  
        > Quando si aggiunge l'account Administrator a queste impostazioni, è necessario specificare se si sta configurando l'account Administrator locale o l'account amministratore di dominio mediante l'assegnazione di etichette agli account. Ad esempio, per aggiungere l'account amministratore del dominio TAILSPINTOYS a questi diritti nega, è necessario passare all'account Administrator per il dominio TAILSPINTOYS, che verrebbe visualizzato come TAILSPINTOYS\Administrator. Se si digita **Administrator** in queste impostazioni di diritti utente nel Editor oggetti Criteri di gruppo, si limiterà l'account amministratore locale in ogni computer a cui viene applicato l'oggetto Criteri di gruppo, come descritto in precedenza.  

9. Configurare i diritti utente per impedire all'account amministratore locale di accedere come servizio effettuando le operazioni seguenti:  

    1.  Fare doppio clic su **Nega accesso come servizio** e selezionare **Definisci queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo**, digitare il nome utente dell'account amministratore locale e fare clic su **OK**. Il nome utente sarà **Administrator**, il valore predefinito quando viene installato Windows.  

        ![proteggere gli account di amministratore locale e i gruppi](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_106.png)  

    3.  Fare clic su **OK**.  

        > [!IMPORTANT]  
        > Quando si aggiunge l'account Administrator a queste impostazioni, è necessario specificare se si sta configurando l'account Administrator locale o l'account amministratore di dominio mediante l'assegnazione di etichette agli account. Ad esempio, per aggiungere l'account amministratore del dominio TAILSPINTOYS a questi diritti nega, è necessario passare all'account Administrator per il dominio TAILSPINTOYS, che verrebbe visualizzato come TAILSPINTOYS\Administrator. Se si digita **Administrator** in queste impostazioni di diritti utente nel Editor oggetti Criteri di gruppo, si limiterà l'account amministratore locale in ogni computer a cui viene applicato l'oggetto Criteri di gruppo, come descritto in precedenza.  

10. Configurare i diritti utente per impedire all'account amministratore locale di accedere ai server membri e alle workstation tramite Servizi Desktop remoto eseguendo le operazioni seguenti:  

    1.  Fare doppio clic su **Nega accesso tramite Servizi Desktop remoto** e selezionare **Definisci queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo**, digitare il nome utente dell'account amministratore locale e fare clic su **OK**. Il nome utente sarà **Administrator**, il valore predefinito quando viene installato Windows.  

        ![proteggere gli account di amministratore locale e i gruppi](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_107.png)  

    3.  Fare clic su **OK**.  

        > [!IMPORTANT]  
        > Quando si aggiunge l'account Administrator a queste impostazioni, è necessario specificare se si sta configurando l'account Administrator locale o l'account amministratore di dominio mediante l'assegnazione di etichette agli account. Ad esempio, per aggiungere l'account amministratore del dominio TAILSPINTOYS a questi diritti nega, è necessario passare all'account Administrator per il dominio TAILSPINTOYS, che verrebbe visualizzato come TAILSPINTOYS\Administrator. Se si digita **Administrator** in queste impostazioni di diritti utente nel Editor oggetti Criteri di gruppo, si limiterà l'account amministratore locale in ogni computer a cui viene applicato l'oggetto Criteri di gruppo, come descritto in precedenza.  

11. Per uscire da **Editor gestione criteri di gruppo**, fare clic su **file**e quindi su **Esci**.  

12. In **gestione criteri di gruppo**collegare l'oggetto Criteri di gruppo al server membro e alle unità organizzative della workstation effettuando le operazioni seguenti:  

    1.  Passare al <Forest> \ Domains @ no__t-1 @ no__t-2 (dove <Forest> è il nome della foresta e <Domain> è il nome del dominio in cui si vuole impostare il Criteri di gruppo).  

    2.  Fare clic con il pulsante destro del mouse sull'unità organizzativa a cui verrà applicato l'oggetto Criteri di gruppo e scegliere **collega un oggetto Criteri**di gruppo  

        ![proteggere gli account di amministratore locale e i gruppi](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_108.png)  

    3.  Selezionare l'oggetto Criteri di gruppo creato e fare clic su **OK**.  

        ![proteggere gli account di amministratore locale e i gruppi](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_109.png)  

    4.  Creare collegamenti a tutte le altre ou che contengono workstation.  

    5.  Creare collegamenti a tutte le altre ou che contengono server membro.  

#### <a name="verification-steps"></a>Passaggi di verifica  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Verificare le impostazioni dell'oggetto Criteri di gruppo "Nega accesso a questo computer dalla rete"  

Da qualsiasi server membro o workstation che non è influenzato dall'oggetto Criteri di gruppo, ad esempio un Jump server, tenta di accedere a un server membro o a una workstation sulla rete interessata dall'oggetto Criteri di gruppo modificato. Per verificare le impostazioni dell'oggetto Criteri di gruppo, provare a eseguire il mapping dell'unità di sistema utilizzando il comando **net use** .  

1.  Accedere localmente a qualsiasi server membro o workstation non interessato dall'oggetto Criteri di gruppo modificato.  

2.  Con il mouse, spostare il puntatore nell'angolo superiore destro o inferiore destro dello schermo. Quando viene visualizzata la barra degli **accessi** , fare clic su **Cerca**.  

3.  Nella casella di **ricerca** Digitare **prompt**dei comandi, fare clic con il pulsante destro del mouse su **prompt dei comandi**e quindi scegliere **Esegui come amministratore** per aprire un prompt dei comandi con privilegi elevati.  

4.  Quando viene richiesto di approvare l'elevazione, fare clic su **Sì**.  

    ![proteggere gli account di amministratore locale e i gruppi](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_110.png)  

5.  Nella finestra del **prompt dei comandi** Digitare **net use \\ @ no__t-3 @ no__t-4\c $/User: <Server Name> \ Administrator**, dove <Server Name> è il nome del server membro o della workstation a cui si sta tentando di accedere attraverso la rete.  

    > [!NOTE]  
    > Le credenziali dell'amministratore locale devono provenire dallo stesso sistema a cui si sta tentando di accedere attraverso la rete.  

6.  Lo screenshot seguente mostra il messaggio di errore che dovrebbe essere visualizzato.  

    ![proteggere gli account di amministratore locale e i gruppi](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_111.png)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Verificare le impostazioni dell'oggetto Criteri di gruppo "Nega accesso come processo batch"  
Accedere localmente da qualsiasi server membro o workstation interessato dall'oggetto Criteri di gruppo.  

###### <a name="create-a-batch-file"></a>Creazione di un file batch  

1.  Con il mouse, spostare il puntatore nell'angolo superiore destro o inferiore destro dello schermo. Quando viene visualizzata la barra degli **accessi** , fare clic su **Cerca**.  

2.  Nella casella di **ricerca** Digitare **blocco note**e fare clic su **blocco note**.  

3.  In **blocco note**Digitare **dir c:** .  

4.  Fare clic su **file**e quindi su **Salva con nome**.  

5.  Nella casella **nome file** Digitare **@no__t 2. bat** (dove <Filename> è il nome del nuovo file batch).  

###### <a name="schedule-a-task"></a>Pianificare un'attività  

1.  Con il mouse, spostare il puntatore nell'angolo superiore destro o inferiore destro dello schermo. Quando viene visualizzata la barra degli **accessi** , fare clic su **Cerca**.  

2.  Nella casella di **ricerca** digitare utilità di pianificazione e fare clic su **utilità di pianificazione**.  

    > [!NOTE]  
    > Nei computer che eseguono Windows 8, nella casella di **ricerca** Digitare **Pianifica attività**, quindi fare clic su **Pianifica attività**.  

3.  Fare clic su **azione**e quindi su **Crea attività**.  

4.  Nella finestra di dialogo **Crea attività** Digitare **<Task Name>** (dove <Task Name> è il nome della nuova attività).  

5.  Fare clic sulla scheda **azioni** e quindi su **nuovo**.  

6.  Nel campo **azione** fare clic su **Avvia programma**.  

7.  Nel campo **programma/script** fare clic su **Sfoglia**, individuare e selezionare il file batch creato nella sezione **creare un file batch** e fare clic su **Apri**.  

8.  Fare clic su **OK**.  

9. Fare clic sulla scheda **Generale**.  

10. Nel campo **Opzioni di sicurezza** fare clic su **modifica utente o gruppo**.  

11. Digitare il nome dell'account amministratore locale del sistema, fare clic su **Controlla nomi**e quindi su **OK**.  

12. Selezionare **Esegui se l'utente è connesso o meno** e non **archiviare la password**. L'attività avrà accesso solo alle risorse del computer locale.  

13. Fare clic su **OK**.  

14. Verrà visualizzata una finestra di dialogo in cui vengono richieste le credenziali dell'account utente per l'esecuzione dell'attività.  

15. Dopo aver immesso le credenziali, fare clic su **OK**.  

16. Verrà visualizzata una finestra di dialogo simile alla seguente.  

    ![proteggere gli account di amministratore locale e i gruppi](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_112.png)  

###### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Verificare le impostazioni dell'oggetto Criteri di gruppo "Nega accesso come servizio"  

1.  Accedere localmente da qualsiasi server membro o workstation interessato dall'oggetto Criteri di gruppo.  

2.  Con il mouse, spostare il puntatore nell'angolo superiore destro o inferiore destro dello schermo. Quando viene visualizzata la barra degli **accessi** , fare clic su **Cerca**.  

3.  Nella casella di **ricerca** Digitare **Services**, quindi fare clic su **Servizi**.  

4.  Individuare e fare doppio clic su **spooler di stampa**.  

5.  Fare clic sulla scheda **Connessione**.  

6.  In campo **Accedi come** fare clic su **questo account**.  

7.  Fare clic su **Sfoglia**, digitare l'account amministratore locale del sistema, fare clic su **Controlla nomi**e quindi su **OK**.  

8.  Nei campi **password** e **Conferma password** digitare la password dell'account selezionato, quindi fare clic su **OK**.  

9. Fare clic su **OK** altre tre volte.  

10. Fare clic con il pulsante destro del mouse su **spooler di stampa** e scegliere **Riavvia**.  

11. Quando il servizio viene riavviato, viene visualizzata una finestra di dialogo simile alla seguente.  

    ![proteggere gli account di amministratore locale e i gruppi](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_113.png)  

###### <a name="revert-changes-to-the-printer-spooler-service"></a>Ripristinare le modifiche al servizio spooler di stampa  

1.  Accedere localmente da qualsiasi server membro o workstation interessato dall'oggetto Criteri di gruppo.  

2.  Con il mouse, spostare il puntatore nell'angolo superiore destro o inferiore destro dello schermo. Quando viene visualizzata la barra degli **accessi** , fare clic su **Cerca**.  

3.  Nella casella di **ricerca** Digitare **Services**, quindi fare clic su **Servizi**.  

4.  Individuare e fare doppio clic su **spooler di stampa**.  

5.  Fare clic sulla scheda **Connessione**.  

6.  Nel campo **Accedi come**: selezionare **SystemAccount locale**e fare clic su **OK**.  

###### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>Verificare le impostazioni dell'oggetto Criteri di gruppo "Nega accesso tramite Servizi Desktop remoto"  

1.  Con il mouse, spostare il puntatore nell'angolo superiore destro o inferiore destro dello schermo. Quando viene visualizzata la barra degli **accessi** , fare clic su **Cerca**.  

2.  Nella casella di **ricerca** Digitare **Connessione desktop remoto**e fare clic su **Connessione desktop remoto**.  

3.  Nel campo **computer** Digitare il nome del computer a cui si desidera connettersi, quindi fare clic su **Connetti**. È anche possibile digitare l'indirizzo IP anziché il nome del computer.  

4.  Quando richiesto, specificare le credenziali per l'account **amministratore** locale del sistema.  

5.  Verrà visualizzata una finestra di dialogo simile alla seguente.  

    ![proteggere gli account di amministratore locale e i gruppi](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_114.png)  
