---
ms.assetid: 11f36f2b-9981-4da0-9e7c-4eca78035f37
title: Appendice D - protezione predefiniti Administrator account in Active Directory
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 2878e661e1bb93fcdc3161c46b73d8e4baec76d2
ms.sourcegitcommit: 2782a80a916f8432c030af76e860a72f08481899
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2018
---
# <a name="appendix-d-securing-built-in-administrator-accounts-in-active-directory"></a>Appendice d: protezione predefiniti Administrator account in Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-d-securing-built-in-administrator-accounts-in-active-directory"></a>Appendice d: protezione predefiniti Administrator account in Active Directory  
In ogni dominio in Active Directory, un account amministratore viene creato come parte della creazione del dominio. Questo account è per impostazione predefinita un membro dei gruppi di amministratori e Domain Admins nel dominio, e se il dominio è il dominio radice della foresta, l'account sia anche membro del gruppo Enterprise Admins.

Utilizzo dell'account di amministratore del dominio deve essere riservata solo per attività di compilazione iniziali e, possibilmente, scenari di ripristino di emergenza. Per garantire un account amministratore può essere utilizzato per operazioni di ripristino nel caso in cui è non possibile utilizzare nessun altro account, è consigliabile non modificare l'appartenenza predefinita dell'account amministratore in qualsiasi dominio nella foresta. Al contrario, è necessario proteggere l'account di amministratore in ogni dominio nella foresta, come descritto nella sezione seguente e dettagliate le istruzioni che seguono. 

> [!NOTE]
> Questa guida è utilizzata per consigliamo di disabilitare l'account. Questo è stato rimosso come il white paper ripristino foresta Usa l'account administrator predefinito. Il motivo è, questo è l'unico account che consente l'accesso senza un Server di catalogo globale.


#### <a name="controls-for-built-in-administrator-accounts"></a>Controlli per gli account amministratore predefinito  
Per l'account amministratore predefinito in ogni dominio della foresta, è necessario configurare le impostazioni seguenti:  

-   Abilitare il **Account è sensibile e non può essere delegato** flag all'account.  

-   Abilitare il **Smart card è necessaria per l'accesso interattivo** flag all'account.  

-   Configurare oggetti Criteri di gruppo per limitare l'utilizzo dell'account Administrator in sistemi appartenenti a un dominio:  

    -   Uno o più oggetti Criteri di gruppo creare e collegare workstation e server membro unità organizzative in ogni dominio, aggiungere account di amministratore di ciascun dominio per i seguenti diritti utente in **Configurazione computer\Criteri\Impostazioni di Windows\Impostazioni protezione\Criteri Settings\nome diritti**:  

        -   Nega accesso al computer dalla rete  

        -   Nega accesso come processo batch  

        -   Nega accesso come servizio  

        -   Nega accesso tramite Servizi Desktop remoto  

> [!NOTE]  
> Quando si aggiungono account questa impostazione, è necessario specificare se si configura account amministratore locale o un account amministratore di dominio. Ad esempio, per aggiungere l'account di amministratore del dominio NWTRADERS a queste negare diritti, è necessario digitare l'account come NWTRADERS\Administrator, o selezionare l'account amministratore per il dominio NWTRADERS. Se si digita "Administrator" in queste impostazioni diritti utente in Editor criteri di gruppo, si limiterà l'account amministratore locale su ciascun computer in cui viene applicato l'oggetto Criteri di gruppo.
>   
> È consigliabile limitare l'account amministratore locale sul server e workstation membri allo stesso modo come account di amministratore basati su dominio. Pertanto, è necessario aggiungere in genere l'account Administrator per ogni dominio nella foresta e l'account di amministratore per il computer locale per queste impostazioni diritti utente. La schermata seguente mostra un esempio di configurazione di questi diritti utente per bloccare l'account amministratore locale e account di amministratore del dominio di eseguire accessi che non sono necessari per questi account.  


![protezione degli account amministratore predefinito](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_23.gif)  

-   Configurare oggetti Criteri di gruppo per limitare l'account di amministratore nei controller di dominio  
    -   In ogni dominio nella foresta, i controller di dominio predefinito o un criterio collegato ai controller di dominio, unità Organizzativa deve essere modificato per aggiungere i seguenti diritti utente in account di amministratore di ciascun dominio **Configurazione computer\Criteri\Impostazioni di Windows\Impostazioni protezione\Criteri Settings\nome diritti**:   
        -   Nega accesso al computer dalla rete  

        -   Nega accesso come processo batch  

        -   Nega accesso come servizio  

        -   Nega accesso tramite Servizi Desktop remoto  

> [!NOTE]  
> Queste impostazioni garantirà l'account Administrator predefinito del dominio non può essere utilizzato per connettersi a un controller di dominio, anche se l'account, se abilitata, può accedere localmente ai controller di dominio. Poiché solo questo account deve essere abilitato e utilizzato negli scenari di ripristino di emergenza, si prevede che accesso fisico per almeno un controller di dominio sarà disponibile o che è possibile utilizzare altri account con autorizzazioni per accedere in remoto i controller di dominio.  

-   Configurare il controllo dell'account amministratore  

    Dopo avere protetto account amministratore di ciascun dominio e disattivato, è necessario configurare il controllo per monitorare le modifiche all'account. Se l'account è abilitato, viene reimpostata la password o vengono apportate altre modifiche all'account, è necessario inviare avvisi per gli utenti o team responsabile dell'amministrazione di Active Directory, oltre ai team di risposta agli eventi imprevisti nell'organizzazione.  

#### <a name="step-by-step-instructions-to-secure-built-in-administrator-accounts-in-active-directory"></a>Istruzioni dettagliate per la protezione predefiniti Administrator account in Active Directory  

1.  In **Server Manager**, fare clic su **strumenti**e fare clic su **Active Directory Users and Computers**.  

2.  Per impedire gli attacchi che sfruttano la delega a usare le credenziali dell'account in altri sistemi, eseguire i passaggi seguenti:  

    1.  Fare doppio clic su di **amministratore** account e fare clic su **proprietà**.  

    2.  Fare clic su di **Account** scheda.  

    3.  In **opzioni Account**selezionare **Account è sensibile e non può essere delegato** flag come indicato nella schermata seguente e fare clic su **OK**.  

        ![protezione degli account amministratore predefinito](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_24.gif)  

3.  Per abilitare il **Smart card è necessaria per l'accesso interattivo** flag per l'account, eseguire i passaggi seguenti:  

    1.  Fare doppio clic su di **amministratore** account e selezionare **proprietà**.  

    2.  Fare clic su di **Account** scheda.  

    3.  In **Account** opzioni, selezionate il **Smart card è necessaria per l'accesso interattivo** flag come indicato nella schermata seguente e fare clic su **OK**.  

        ![protezione degli account amministratore predefinito](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_25.gif) 

##### <a name="configuring-gpos-to-restrict-administrator-accounts-at-the-domain-level"></a>Configurazione di oggetti Criteri di gruppo per limitare l'account di amministratore a livello di dominio  

> [!WARNING]  
> Questo oggetto Criteri di gruppo non deve essere collegata mai a livello di dominio poiché rende l'account Administrator predefinito inutilizzabile, anche in scenari di ripristino di emergenza.  

1.  In **Server Manager**, fare clic su **strumenti**e fare clic su **Gestione criteri di gruppo**.  

2.  Nell'albero della console, espandere <Forest>\Domains\\<Domain>e quindi **oggetti Criteri di gruppo** (in cui <Forest> è il nome dell'insieme di strutture e <Domain> è il nome del dominio in cui si desidera creare i criteri di gruppo).  

3.  Nell'albero della console fare doppio clic su **oggetti Criteri di gruppo**e fare clic su **New**.  

    ![protezione degli account amministratore predefinito](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_27.gif)  

4.  Nel **nuovo oggetto Criteri di gruppo** la finestra di dialogo, digitare <GPO Name>e fare clic su **OK** (in cui <GPO Name> è il nome di questo oggetto Criteri di gruppo) come indicato nella schermata seguente.  

    ![protezione degli account amministratore predefinito](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_28.gif)  

5.  Nel riquadro dei dettagli fare doppio clic su <GPO Name>e fare clic su **modifica**.  

6.  Passare a **Computer Configurazione computer\Criteri\Impostazioni di Windows\Impostazioni Protezione\criteri**e fare clic su **Assegnazione diritti utente**.  

    ![protezione degli account amministratore predefinito](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_29.gif)  

7.  Configurare i diritti utente impedire l'account amministratore di accedere al server membri e workstation sulla rete eseguendo le operazioni seguenti:  

    1.  Fare doppio clic su **negare l'accesso al computer dalla rete** e seleziona **definire queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

    3.  Tipo **amministratore**, fare clic su **Controlla nomi**e fare clic su **OK**. Verificare che l'account viene visualizzato <DomainName>\Username formato come indicato nella schermata seguente.  

        ![protezione degli account amministratore predefinito](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_30.gif)  

    4.  Fare clic su **OK**, e **OK** nuovamente.  

8.  Configurare i diritti utente per impedire l'account amministratore di accesso come processo batch effettuando le operazioni seguenti:  

    1.  Fare doppio clic su **Nega accesso come processo batch** e seleziona **definire queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

    3.  Tipo **amministratore**, fare clic su **Controlla nomi**e fare clic su **OK**. Verificare che l'account viene visualizzato <DomainName>\Username formato come indicato nella schermata seguente.  

        ![protezione degli account amministratore predefinito](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_31.gif)  

    4.  Fare clic su **OK**, e **OK** nuovamente.  

9. Configurare i diritti utente per impedire l'account amministratore di accesso come servizio effettuando le operazioni seguenti:  

    1.  Fare doppio clic su **Nega accesso come servizio** e seleziona **definire queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

    3.  Tipo **amministratore**, fare clic su **Controlla nomi**e fare clic su **OK**. Verificare che l'account viene visualizzato <DomainName>\Username formato come indicato nella schermata seguente.  

        ![protezione degli account amministratore predefinito](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_32.gif)  

    4.  Fare clic su **OK**, e **OK** nuovamente.  

10. Configurare i diritti utente per impedire che l'account BA l'accesso a server membri e workstation tramite Servizi Desktop remoto eseguendo le operazioni seguenti:  

    1.  Fare doppio clic su **Nega accesso tramite Servizi Desktop remoto** e seleziona **definire queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

    3.  Tipo **amministratore**, fare clic su **Controlla nomi**e fare clic su **OK**. Verificare che l'account viene visualizzato <DomainName>\Username formato come indicato nella schermata seguente.  

        ![protezione degli account amministratore predefinito](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_33.gif)  

    4.  Fare clic su **OK**, e **OK** nuovamente.  

11. Per uscire da **Editor Gestione criteri di gruppo**, fare clic su **File**e fare clic su **uscire**.  

12. In **Gestione criteri di gruppo**, collegare l'oggetto Criteri di gruppo per il server membro e workstation unità organizzative effettuando le operazioni seguenti:  

    1.  Passare al <Forest>\Domains\\<Domain> (in cui <Forest> è il nome dell'insieme di strutture e <Domain> è il nome del dominio in cui si desidera impostare i criteri di gruppo).  

    2.  Fare doppio clic su oggetto Criteri di gruppo verranno applicate a e fare clic su, l'unità Organizzativa **collegare un oggetto Criteri di gruppo esistente**.  

        ![protezione degli account amministratore predefinito](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_34.gif)  

    3.  Selezionare l'oggetto Criteri di gruppo creato e fare clic su **OK**.  

        ![protezione degli account amministratore predefinito](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_35.gif)  

    4.  Creare collegamenti a tutte le altre unità organizzative contenenti le workstation.  

    5.  Creare collegamenti a tutte le altre unità organizzative contenenti i server membri.  

> [!IMPORTANT]  
> Quando si aggiunge l'account amministratore per queste impostazioni, specificare se si configura un account amministratore locale o un account di amministratore di dominio da come creare un'etichetta per gli account. Ad esempio, per aggiungere l'account di amministratore del dominio TAILSPINTOYS a queste negare diritti, accedete all'account amministratore per il dominio TAILSPINTOYS, che vengono visualizzati come TAILSPINTOYS\Administrator. Se si digita "Administrator" in queste impostazioni diritti utente in Editor criteri di gruppo, si limiterà l'account amministratore locale su ciascun computer in cui viene applicato l'oggetto Criteri di gruppo, come descritto in precedenza.  

#### <a name="verification-steps"></a>Passaggi di verifica  
I passaggi di verifica descritti in questo argomento sono specifici di Windows 8 e Windows Server 2012.  

##### <a name="verify-smart-card-is-required-for-interactive-logon-account-option"></a>Verificare "Smart card è necessaria per l'accesso interattivo" opzione Account  

1.  Da qualsiasi server membro o una workstation interessati dall'oggetto Criteri di gruppo viene modificato, tentare di accedere in modo interattivo al dominio utilizzando l'account Administrator predefinito del dominio. Dopo il tentativo di accesso, verrà visualizzata una finestra di dialogo simile al seguente.  

![protezione degli account amministratore predefinito](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_36.gif)  

##### <a name="verify-account-is-disabled-account-option"></a>Verificare il "Account è disabilitato" opzione Account  

1.  Da qualsiasi server membro o una workstation interessati dall'oggetto Criteri di gruppo viene modificato, tentare di accedere in modo interattivo al dominio utilizzando l'account Administrator predefinito del dominio. Dopo il tentativo di accesso, verrà visualizzata una finestra di dialogo simile al seguente.  

![protezione degli account amministratore predefinito](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_37.gif)  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Verificare le impostazioni oggetto Criteri di gruppo "Nega accesso al computer dalla rete"  
Da qualsiasi server membro o una workstation in cui non è interessata dall'oggetto Criteri di gruppo viene modificato (ad esempio un jump server), tentare di accedere a un server membro o una workstation in rete che è interessata dall'oggetto Criteri di gruppo viene modificato. Per verificare le impostazioni oggetto Criteri di gruppo, tentare di eseguire il mapping di unità di sistema utilizzando il **NET USE** comando eseguendo i passaggi seguenti:  

1.  Accedere al dominio utilizzando l'account Administrator predefinito del dominio.  

2.  Con il mouse, sposta il puntatore nell'angolo superiore destro o in basso a destra dello schermo. Quando il **accessi** barra viene visualizzata, fare clic su **ricerca**.  

3.  Nel **ricerca** digitare **prompt dei comandi**, fare doppio clic su **prompt dei comandi**, quindi fare clic su **Esegui come amministratore** per aprire un prompt dei comandi con privilegi elevati.  

4.  Quando viene richiesto di approvare l'elevazione dei privilegi, fare clic su **Sì**.  

    ![protezione degli account amministratore predefinito](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_38.gif)  

5.  Nel **prompt dei comandi** finestra, digitare **net uso \ \ \<Server Name>\c$**, dove <Server Name> è il nome del server membri o workstation che si sta tentando di accedere attraverso la rete.  

6.  La schermata seguente mostra il messaggio di errore che dovrebbe essere visualizzato.  

    ![protezione degli account amministratore predefinito](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_39.gif)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Verificare le impostazioni oggetto Criteri di gruppo "Nega accesso come processo batch"  

Da qualsiasi server membro o una workstation interessati dalle modifiche dell'oggetto Criteri di gruppo, accedere localmente.  

###### <a name="create-a-batch-file"></a>Creare un File Batch  

1.  Con il mouse, sposta il puntatore nell'angolo superiore destro o in basso a destra dello schermo. Quando il **accessi** barra viene visualizzata, fare clic su **ricerca**.  

2.  Nel **ricerca** digitare **blocco note**e fare clic su **blocco note**.  

3.  In **blocco note**, tipo **c: dir**.  

4.  Fare clic su **File** e fare clic su **Salva con nome**.  

5.  Nel **Filename** digitare ** <Filename>. bat** (in cui <Filename> è il nome del nuovo file batch).  

###### <a name="schedule-a-task"></a>Pianificare un'attività  

1.  Con il mouse, sposta il puntatore nell'angolo superiore destro o in basso a destra dello schermo. Quando il **accessi** barra viene visualizzata, fare clic su **ricerca**.  

2.  Nel **ricerca** digitare **utilità di pianificazione**e fare clic su **utilità di pianificazione**.  

    > [!NOTE]  
    > Nei computer che eseguono Windows 8, nella casella di ricerca, digitare **Pianifica attività**e fare clic su **Pianifica attività**.  

3.  In **utilità di pianificazione**, fare clic su **azione**e fare clic su **Crea attività**.  

4.  Nel **Crea attività** la finestra di dialogo, digitare ** <Task Name> ** (in cui ** <Task Name> ** è il nome della nuova attività).  

5.  Fare clic su di **azioni** scheda e fare clic su **New**.  

6.  In **azione:**selezionare **avviare un programma**.  

7.  In **programma o script:**, fare clic su **Sfoglia**, individuare e selezionare il file batch creato nella sezione "Creare un File Batch", quindi fare clic su **aprire**.  

8.  Fare clic su **OK**.  

9. Fare clic su di **generale** scheda.  

10. In **sicurezza** opzioni, fare clic su **Cambia utente o gruppo**.  

11. Digitare il nome dell'account BA al livello di dominio, fare clic su **Controlla nomi**e fare clic su **OK**.  

12. Selezionare **eseguire se l'utente è connesso o non** e **non archiviare password**. L'attività sarà solo possibile accedere alle risorse del computer locale.  

13. Fare clic su **OK**.  

14. Dovrebbe essere visualizzata una finestra di dialogo, account utente che richiede le credenziali necessarie per eseguire l'attività.  

15. Dopo aver immesso le credenziali, fare clic su **OK**.  

16. Dovrebbe essere visualizzata una finestra di dialogo simile al seguente.  

    ![protezione degli account amministratore predefinito](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_40.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Verificare le impostazioni oggetto Criteri di gruppo "Nega accesso come servizio"  

1.  Da qualsiasi server membro o una workstation interessati dalle modifiche dell'oggetto Criteri di gruppo, accedere localmente.  

2.  Con il mouse, sposta il puntatore nell'angolo superiore destro o in basso a destra dello schermo. Quando il **accessi** barra viene visualizzata, fare clic su **ricerca**.  

3.  Nel **ricerca** digitare **servizi**e fare clic su **servizi**.  

4.  Individuare e fare doppio clic su **Spooler di stampa**.  

5.  Fare clic su di **accesso** scheda.  

6.  In **accedere come:**selezionare **questo account**.  

7.  Fare clic su **Sfoglia**, digitare il nome dell'account BA a livello di dominio, fare clic su **Controlla nomi**e fare clic su **OK**.  

8.  In **Password:** e **Conferma password:**, digita la password dell'account amministratore e fare clic su **OK**.  

9. Fare clic su **OK** tre volte.  

10. Fare doppio clic su di **servizio Spooler di stampa** e seleziona **riavviare**.  

11. Quando viene riavviato il servizio, verrà visualizzata una finestra di dialogo simile al seguente.  

    ![protezione degli account amministratore predefinito](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_41.gif)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>Ripristinare le modifiche per il servizio Spooler di stampa  

1.  Da qualsiasi server membro o una workstation interessati dalle modifiche dell'oggetto Criteri di gruppo, accedere localmente.  

2.  Con il mouse, sposta il puntatore nell'angolo superiore destro o in basso a destra dello schermo. Quando il **accessi** barra viene visualizzata, fare clic su **ricerca**.  

3.  Nel **ricerca** digitare **servizi**e fare clic su **servizi**.  

4.  Individuare e fare doppio clic su **Spooler di stampa**.  

5.  Fare clic su di **accesso** scheda.  

6.  In **accedere come:**, selezionare il **sistema locale** account, quindi scegliere **OK**.  

##### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>Verificare le impostazioni oggetto Criteri di gruppo "Nega accesso tramite Servizi Desktop remoto"

1.  Con il mouse, sposta il puntatore nell'angolo superiore destro o in basso a destra dello schermo. Quando il **accessi** barra viene visualizzata, fare clic su **ricerca**.  

2.  Nel **ricerca** digitare **connessione desktop remoto**e fare clic su **connessione Desktop remoto**.  

3.  Nel **Computer** , digitare il nome del computer che si desidera connettersi e fare clic su **Connetti**. (È anche possibile digitare l'indirizzo IP anziché il nome del computer.)  

4.  Quando richiesto, fornire le credenziali per il nome dell'account BA a livello di dominio.  

5.  Dovrebbe essere visualizzata una finestra di dialogo simile al seguente.  

    ![protezione degli account amministratore predefinito](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_42.gif)  
