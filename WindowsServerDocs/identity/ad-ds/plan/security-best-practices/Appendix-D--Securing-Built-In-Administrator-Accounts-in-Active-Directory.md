---
ms.assetid: 11f36f2b-9981-4da0-9e7c-4eca78035f37
title: Appendice D-protezione degli account amministratore predefiniti in Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: cc91ccd00c951863f4f9802f3d669e36ff3f3d9a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367842"
---
# <a name="appendix-d-securing-built-in-administrator-accounts-in-active-directory"></a>Appendice D: Protezione degli account amministratore predefiniti in Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-d-securing-built-in-administrator-accounts-in-active-directory"></a>Appendice D: Protezione degli account amministratore predefiniti in Active Directory  
In ogni dominio in Active Directory, un account amministratore viene creato come parte della creazione del dominio. Per impostazione predefinita, questo account è membro dei gruppi Domain Admins e Administrators nel dominio e, se il dominio è il dominio radice della foresta, l'account è anche membro del gruppo Enterprise Admins.

L'uso dell'account amministratore di un dominio deve essere riservato solo per le attività di compilazione iniziali e possibilmente per gli scenari di ripristino di emergenza. Per assicurarsi che un account amministratore possa essere utilizzato per eseguire le riparazioni nel caso in cui non sia possibile utilizzare altri account, è consigliabile non modificare l'appartenenza predefinita dell'account amministratore in qualsiasi dominio della foresta. Al contrario, è necessario proteggere l'account amministratore in ogni dominio della foresta, come descritto nella sezione seguente e in dettaglio nelle istruzioni dettagliate che seguono. 

> [!NOTE]
> Questa guida è stata usata per consigliare la disabilitazione dell'account. Questa operazione è stata rimossa perché il ripristino della foresta white paper utilizza l'account amministratore predefinito. Il motivo è che questo è l'unico account che consente l'accesso senza un server di catalogo globale.


#### <a name="controls-for-built-in-administrator-accounts"></a>Controlli per gli account amministratore predefinito  
Per l'account Administrator predefinito in ogni dominio della foresta, è necessario configurare le impostazioni seguenti:  

-   Abilitare l' **account è sensibile e non è possibile delegare** il flag per l'account.  

-   Abilitare la **Smart Card è necessaria per il flag di accesso interattivo per** l'account.  

-   Configurare gli oggetti Criteri di gruppo per limitare l'utilizzo dell'account amministratore nei sistemi aggiunti a un dominio:  

    -   In uno o più oggetti Criteri di gruppo creati e collegati alle unità organizzative workstation e server membro in ogni dominio, aggiungere l'account amministratore di ciascun dominio ai seguenti diritti utente in **computer Computer\criteri\impostazioni Windows\Impostazioni protezione\Criteri Policies \ Assegnazioni diritti utente**:  

        -   Nega accesso al computer dalla rete  

        -   Nega accesso come processo batch  

        -   Nega accesso come servizio  

        -   Nega accesso tramite Servizi Desktop remoto  

> [!NOTE]  
> Quando si aggiungono account a questa impostazione, è necessario specificare se si sta configurando account amministratore locale o account amministratore di dominio. Ad esempio, per aggiungere l'account amministratore del dominio NWTRADERS a questi diritti nega, è necessario digitare l'account come NWTRADERS\Administrator oppure passare all'account amministratore per il dominio NWTRADERS. Se si digita "Administrator" in queste impostazioni di diritti utente nel Editor oggetti Criteri di gruppo, si limiterà l'account amministratore locale in ogni computer a cui viene applicato l'oggetto Criteri di gruppo.
>   
> È consigliabile limitare l'account amministratore locale sul server e workstation membri allo stesso modo come account di amministratore di dominio. Pertanto, è necessario aggiungere in genere l'account Administrator per ogni dominio nella foresta e l'account di amministratore per il computer locale per queste impostazioni di diritti utente. La schermata seguente mostra un esempio di configurazione di questi diritti utente per bloccare l'account di amministratore locale e account di amministratore del dominio di eseguire accessi che non sono necessari per questi account.  


![protezione degli account amministratore predefiniti](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_23.gif)  

-   Configurare gli oggetti Criteri di gruppo per limitare gli account amministratore nei controller di dominio  
    -   In ogni dominio della foresta, l'oggetto Criteri di gruppo controller di dominio predefinito o un criterio collegato all'unità organizzativa controller di dominio deve essere modificato per aggiungere l'account amministratore di ciascun dominio ai seguenti diritti utente in **computer Computer\criteri\impostazioni Settings \ Sicurezza protezione\Criteri locali\Assegnazione diritti assegnati**:   
        -   Nega accesso al computer dalla rete  

        -   Nega accesso come processo batch  

        -   Nega accesso come servizio  

        -   Nega accesso tramite Servizi Desktop remoto  

> [!NOTE]  
> Queste impostazioni garantiscono che l'account amministratore predefinito del dominio non possa essere utilizzato per connettersi a un controller di dominio, anche se l'account, se abilitato, può accedere localmente ai controller di dominio. Poiché solo questo account deve essere abilitato e utilizzato negli scenari di ripristino di emergenza, si prevede che l'accesso fisico per almeno un controller di dominio sarà disponibile o che è possibile utilizzare altri account con autorizzazioni per accedere ai controller di dominio in modalità remota.  

-   Configurare il controllo degli account amministratore  

    Dopo avere protetto account amministratore di ciascun dominio e disattivato, è necessario configurare il controllo per monitorare le modifiche all'account. Se l'account è abilitato, la password viene reimpostata o vengono apportate altre modifiche all'account, gli avvisi devono essere inviati agli utenti o ai team responsabili dell'amministrazione di Active Directory, oltre ai team di risposta agli eventi imprevisti nell'organizzazione.  

#### <a name="step-by-step-instructions-to-secure-built-in-administrator-accounts-in-active-directory"></a>Istruzioni dettagliate per la protezione degli account amministratore predefiniti in Active Directory  

1.  In **Server Manager**fare clic su **strumenti**e quindi su **Active Directory utenti e computer**.  

2.  Per evitare attacchi che sfruttano la delega per usare le credenziali dell'account su altri sistemi, seguire questa procedura:  

    1.  Fare clic con il pulsante destro del mouse sull'account **Administrator** e scegliere **Proprietà**.  

    2.  Fare clic sulla scheda **account** .  

    3.  In **Opzioni account**selezionare **account è sensibile e non può essere delegato** come indicato nella schermata seguente e fare clic su **OK**.  

        ![protezione degli account amministratore predefiniti](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_24.gif)  

3.  Per abilitare la **Smart Card obbligatoria per il flag di accesso interattivo** nell'account, seguire questa procedura:  

    1.  Fare clic con il pulsante destro del mouse sull'account **Administrator** e scegliere **Proprietà**.  

    2.  Fare clic sulla scheda **account** .  

    3.  In opzioni **account** selezionare la **Smart Card obbligatoria per il flag di accesso interattivo** come indicato nella schermata seguente e fare clic su **OK**.  

        ![protezione degli account amministratore predefiniti](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_25.gif) 

##### <a name="configuring-gpos-to-restrict-administrator-accounts-at-the-domain-level"></a>Configurazione degli oggetti Criteri di gruppo per limitare gli account amministratore a livello di dominio  

> [!WARNING]  
> Questo oggetto Criteri di gruppo non deve mai essere collegato a livello di dominio perché può rendere inutilizzabile l'account amministratore predefinito, anche negli scenari di ripristino di emergenza.  

1.  In **Server Manager**fare clic su **strumenti**e quindi su **Gestione criteri di gruppo**.  

2.  Nell'albero della console espandere <Forest> \ domini @ no__t-1 @ no__t-2, quindi **criteri di gruppo oggetti** (dove <Forest> è il nome della foresta e <Domain> è il nome del dominio in cui si desidera creare il criteri di gruppo).  

3.  Nell'albero della console fare clic con il pulsante destro del mouse su **criteri di gruppo oggetti**, quindi scegliere **nuovo**.  

    ![protezione degli account amministratore predefiniti](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_27.gif)  

4.  Nella finestra di dialogo **nuovo oggetto Criteri** di gruppo digitare <GPO Name> e fare clic su **OK** (dove <GPO Name> è il nome dell'oggetto Criteri di gruppo) come indicato nella schermata seguente.  

    ![protezione degli account amministratore predefiniti](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_28.gif)  

5.  Nel riquadro dei dettagli fare clic con il pulsante destro del mouse su <GPO Name> e scegliere **modifica**.  

6.  Passare a **computer Computer\criteri\impostazioni Windows\Impostazioni protezione\Criteri criteri**e fare clic su **assegnazione diritti utente**.  

    ![protezione degli account amministratore predefiniti](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_29.gif)  

7.  Configurare i diritti utente in modo da impedire all'account Administrator di accedere ai server e alle workstation dei membri tramite la rete seguendo questa procedura:  

    1.  Fare doppio clic su **Nega accesso al computer dalla rete** e selezionare **Definisci le impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

    3.  Digitare **Administrator**, fare clic su **Controlla nomi**e quindi su **OK**. Verificare che l'account venga visualizzato nel formato <DomainName> \ nomeutente come indicato nella schermata seguente.  

        ![protezione degli account amministratore predefiniti](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_30.gif)  

    4.  Fare clic su **OK**e di nuovo su **OK** .  

8.  Configurare i diritti utente per impedire all'account Administrator di accedere come processo batch eseguendo le operazioni seguenti:  

    1.  Fare doppio clic su **Nega accesso come processo batch** e selezionare **Definisci queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

    3.  Digitare **Administrator**, fare clic su **Controlla nomi**e quindi su **OK**. Verificare che l'account venga visualizzato nel formato <DomainName> \ nomeutente come indicato nella schermata seguente.  

        ![protezione degli account amministratore predefiniti](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_31.gif)  

    4.  Fare clic su **OK**e di nuovo su **OK** .  

9. Configurare i diritti utente per impedire all'account Administrator di accedere come servizio effettuando le operazioni seguenti:  

    1.  Fare doppio clic su **Nega accesso come servizio** e selezionare **Definisci queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

    3.  Digitare **Administrator**, fare clic su **Controlla nomi**e quindi su **OK**. Verificare che l'account venga visualizzato nel formato <DomainName> \ nomeutente come indicato nella schermata seguente.  

        ![protezione degli account amministratore predefiniti](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_32.gif)  

    4.  Fare clic su **OK**e di nuovo su **OK** .  

10. Configurare i diritti utente per impedire all'account BA di accedere ai server membri e alle workstation tramite Servizi Desktop remoto eseguendo le operazioni seguenti:  

    1.  Fare doppio clic su **Nega accesso tramite Servizi Desktop remoto** e selezionare **Definisci queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

    3.  Digitare **Administrator**, fare clic su **Controlla nomi**e quindi su **OK**. Verificare che l'account venga visualizzato nel formato <DomainName> \ nomeutente come indicato nella schermata seguente.  

        ![protezione degli account amministratore predefiniti](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_33.gif)  

    4.  Fare clic su **OK**e di nuovo su **OK** .  

11. Per uscire da **Editor gestione criteri di gruppo**, fare clic su **file**e quindi su **Esci**.  

12. In **gestione criteri di gruppo**collegare l'oggetto Criteri di gruppo al server membro e alle unità organizzative della workstation effettuando le operazioni seguenti:  

    1.  Passare al <Forest> \ Domains @ no__t-1 @ no__t-2 (dove <Forest> è il nome della foresta e <Domain> è il nome del dominio in cui si vuole impostare il Criteri di gruppo).  

    2.  Fare clic con il pulsante destro del mouse sull'unità organizzativa a cui verrà applicato l'oggetto Criteri di gruppo e scegliere **collega un oggetto Criteri**di gruppo  

        ![protezione degli account amministratore predefiniti](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_34.gif)  

    3.  Selezionare l'oggetto Criteri di gruppo creato e fare clic su **OK**.  

        ![protezione degli account amministratore predefiniti](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_35.gif)  

    4.  Creare collegamenti a tutte le altre ou che contengono workstation.  

    5.  Creare collegamenti a tutte le altre ou che contengono server membro.  

> [!IMPORTANT]  
> Quando si aggiunge l'account Administrator a queste impostazioni, è necessario specificare se si sta configurando un account amministratore locale o un account amministratore di dominio mediante l'etichetta degli account. Ad esempio, per aggiungere l'account amministratore del dominio TAILSPINTOYS a questi diritti nega, è necessario passare all'account Administrator per il dominio TAILSPINTOYS, che verrebbe visualizzato come TAILSPINTOYS\Administrator. Se si digita "Administrator" in queste impostazioni di diritti utente nel Editor oggetti Criteri di gruppo, si limiterà l'account amministratore locale in ogni computer a cui viene applicato l'oggetto Criteri di gruppo, come descritto in precedenza.  

#### <a name="verification-steps"></a>Passaggi di verifica  
I passaggi di verifica descritti di seguito sono specifici di Windows 8 e Windows Server 2012.  

##### <a name="verify-smart-card-is-required-for-interactive-logon-account-option"></a>Verificare l'opzione per l'account "Smart Card obbligatoria per l'accesso interattivo"  

1.  Da qualsiasi server membro o workstation interessato dalle modifiche dell'oggetto Criteri di gruppo, tentare di accedere in modo interattivo al dominio usando l'account amministratore predefinito del dominio. Dopo il tentativo di accesso, verrà visualizzata una finestra di dialogo simile alla seguente.  

![protezione degli account amministratore predefiniti](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_36.gif)  

##### <a name="verify-account-is-disabled-account-option"></a>Opzione account "account disabilitato"  

1.  Da qualsiasi server membro o workstation interessato dalle modifiche dell'oggetto Criteri di gruppo, tentare di accedere in modo interattivo al dominio usando l'account amministratore predefinito del dominio. Dopo il tentativo di accesso, verrà visualizzata una finestra di dialogo simile alla seguente.  

![protezione degli account amministratore predefiniti](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_37.gif)  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Verificare le impostazioni dell'oggetto Criteri di gruppo "Nega accesso a questo computer dalla rete"  
Da qualsiasi server membro o workstation che non è influenzato dall'oggetto Criteri di gruppo, ad esempio un Jump server, tenta di accedere a un server membro o a una workstation sulla rete interessata dall'oggetto Criteri di gruppo modificato. Per verificare le impostazioni dell'oggetto Criteri di gruppo, provare a eseguire il mapping dell'unità di sistema utilizzando il comando **net use** attenendosi alla procedura seguente:  

1.  Accedere al dominio utilizzando l'account Administrator predefinito del dominio.  

2.  Con il mouse, spostare il puntatore nell'angolo superiore destro o inferiore destro dello schermo. Quando viene visualizzata la barra degli **accessi** , fare clic su **Cerca**.  

3.  Nella casella di **ricerca** Digitare **prompt**dei comandi, fare clic con il pulsante destro del mouse su **prompt dei comandi**e quindi scegliere **Esegui come amministratore** per aprire un prompt dei comandi con privilegi elevati.  

4.  Quando viene richiesto di approvare l'elevazione, fare clic su **Sì**.  

    ![protezione degli account amministratore predefiniti](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_38.gif)  

5.  Nella finestra del **prompt dei comandi** Digitare **net use \\ @ no__t-3 @ No__t-4Server Name @ no__t-5\c $** , dove \<Server Name @ no__t-7 è il nome del server membro o della workstation a cui si sta tentando di accedere attraverso la rete.  

6.  Lo screenshot seguente mostra il messaggio di errore che dovrebbe essere visualizzato.  

    ![protezione degli account amministratore predefiniti](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_39.gif)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Verificare le impostazioni dell'oggetto Criteri di gruppo "Nega accesso come processo batch"  

Accedere localmente da qualsiasi server membro o workstation interessato dall'oggetto Criteri di gruppo.  

###### <a name="create-a-batch-file"></a>Creazione di un file batch  

1.  Con il mouse, spostare il puntatore nell'angolo superiore destro o inferiore destro dello schermo. Quando viene visualizzata la barra degli **accessi** , fare clic su **Cerca**.  

2.  Nella casella di **ricerca** Digitare **blocco note**e fare clic su **blocco note**.  

3.  In **blocco note**Digitare **dir c:** .  

4.  Fare clic su **file** e quindi su **Salva con nome**.  

5.  Nel campo **filename** Digitare **@no__t 2. bat** (dove <Filename> è il nome del nuovo file batch).  

###### <a name="schedule-a-task"></a>Pianificare un'attività  

1.  Con il mouse, spostare il puntatore nell'angolo superiore destro o inferiore destro dello schermo. Quando viene visualizzata la barra degli **accessi** , fare clic su **Cerca**.  

2.  Nella casella di **ricerca** Digitare **utilità di pianificazione**e fare clic su **utilità di pianificazione**.  

    > [!NOTE]  
    > Nei computer che eseguono Windows 8, nella casella di ricerca digitare **Pianifica attività**, quindi fare clic su **Pianifica attività**.  

3.  In **utilità di pianificazione**fare clic su **azione**e quindi su **Crea attività**.  

4.  Nella finestra di dialogo **Crea attività** Digitare **<Task Name>** (dove **<Task Name>** è il nome della nuova attività).  

5.  Fare clic sulla scheda **azioni** e quindi su **nuovo**.  

6.  In **azione:** selezionare **Avvia programma**.  

7.  In **programma/script:** fare clic su **Sfoglia**, individuare e selezionare il file batch creato nella sezione "creare un file batch" e fare clic su **Apri**.  

8.  Fare clic su **OK**.  

9. Fare clic sulla scheda **Generale**.  

10. In opzioni di **sicurezza** fare clic su **modifica utente o gruppo**.  

11. Digitare il nome dell'account BA a livello di dominio, fare clic su **Controlla nomi**e quindi su **OK**.  

12. Selezionare **Esegui se l'utente è connesso o meno** e non **archiviare la password**. L'attività avrà accesso solo alle risorse del computer locale.  

13. Fare clic su **OK**.  

14. Verrà visualizzata una finestra di dialogo in cui vengono richieste le credenziali dell'account utente per l'esecuzione dell'attività.  

15. Dopo aver immesso le credenziali, fare clic su **OK**.  

16. Verrà visualizzata una finestra di dialogo simile alla seguente.  

    ![protezione degli account amministratore predefiniti](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_40.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Verificare le impostazioni dell'oggetto Criteri di gruppo "Nega accesso come servizio"  

1.  Accedere localmente da qualsiasi server membro o workstation interessato dall'oggetto Criteri di gruppo.  

2.  Con il mouse, spostare il puntatore nell'angolo superiore destro o inferiore destro dello schermo. Quando viene visualizzata la barra degli **accessi** , fare clic su **Cerca**.  

3.  Nella casella di **ricerca** Digitare **Services**, quindi fare clic su **Servizi**.  

4.  Individuare e fare doppio clic su **spooler di stampa**.  

5.  Fare clic sulla scheda **Connessione**.  

6.  In **Accedi come:** selezionare **questo account**.  

7.  Fare clic su **Sfoglia**, digitare il nome dell'account BA a livello di dominio, fare clic su **Controlla nomi**e quindi su **OK**.  

8.  In **password:** e **Conferma password:** digitare la password dell'account amministratore e fare clic su **OK**.  

9. Fare clic su **OK** altre tre volte.  

10. Fare clic con il pulsante destro del mouse sul **servizio spooler di stampa** e scegliere **Riavvia**.  

11. Quando il servizio viene riavviato, viene visualizzata una finestra di dialogo simile alla seguente.  

    ![protezione degli account amministratore predefiniti](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_41.gif)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>Ripristinare le modifiche al servizio spooler di stampa  

1.  Accedere localmente da qualsiasi server membro o workstation interessato dall'oggetto Criteri di gruppo.  

2.  Con il mouse, spostare il puntatore nell'angolo superiore destro o inferiore destro dello schermo. Quando viene visualizzata la barra degli **accessi** , fare clic su **Cerca**.  

3.  Nella casella di **ricerca** Digitare **Services**, quindi fare clic su **Servizi**.  

4.  Individuare e fare doppio clic su **spooler di stampa**.  

5.  Fare clic sulla scheda **Connessione**.  

6.  In **Accedi come:** selezionare l'account di **sistema locale** e fare clic su **OK**.  

##### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>Verificare le impostazioni dell'oggetto Criteri di gruppo "Nega accesso tramite Servizi Desktop remoto"

1.  Con il mouse, spostare il puntatore nell'angolo superiore destro o inferiore destro dello schermo. Quando viene visualizzata la barra degli **accessi** , fare clic su **Cerca**.  

2.  Nella casella di **ricerca** Digitare **Connessione desktop remoto**e fare clic su **Connessione desktop remoto**.  

3.  Nel campo **computer** Digitare il nome del computer a cui si desidera connettersi, quindi fare clic su **Connetti**. È anche possibile digitare l'indirizzo IP anziché il nome del computer.  

4.  Quando richiesto, specificare le credenziali per il nome dell'account BA a livello di dominio.  

5.  Verrà visualizzata una finestra di dialogo simile alla seguente.  

    ![protezione degli account amministratore predefiniti](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_42.gif)  
