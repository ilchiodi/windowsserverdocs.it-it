---
ms.assetid: 11f36f2b-9981-4da0-9e7c-4eca78035f37
title: 'Appendice D: protezione predefiniti Administrator account in Active Directory'
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 51e503f55ee0fca1f1a53339de555fd213c69296
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870682"
---
# <a name="appendix-d-securing-built-in-administrator-accounts-in-active-directory"></a>Appendice D: Protezione predefiniti Administrator account in Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-d-securing-built-in-administrator-accounts-in-active-directory"></a>Appendice D: Protezione predefiniti Administrator account in Active Directory  
In ogni dominio in Active Directory, un account amministratore viene creato come parte della creazione del dominio. Questo account è per impostazione predefinita un membro dei gruppi Domain Admins e amministratori del dominio e se il dominio è il dominio radice della foresta, l'account è anche membro del gruppo Enterprise Admins.

Uso di account di amministratore del dominio deve essere riservata solo per le attività di compilazione iniziali e, possibilmente, scenari di ripristino di emergenza. Per garantire che un account amministratore è utilizzabile per operazioni di ripristino nel caso in cui non è possibile utilizzare nessun altro account, non è necessario modificare l'appartenenza predefinita dell'account amministratore in qualsiasi dominio nella foresta. Al contrario, è necessario proteggere l'account di amministratore in ogni dominio nella foresta, come descritto nella sezione seguente e dettagliati nelle istruzioni dettagliate che seguono. 

> [!NOTE]
> Questa Guida utilizzata per consigliare la disabilitazione dell'account. Questo è stato rimosso come il white paper recovery dell'insieme di strutture Usa l'account amministratore predefinito. Infatti, questo è l'unico che consente l'accesso senza un Server di catalogo globale.


#### <a name="controls-for-built-in-administrator-accounts"></a>Controlli per gli account amministratore predefinito  
Per l'account Administrator predefinito in ogni dominio nella foresta, è necessario configurare le impostazioni seguenti:  

-   Abilitare la **Account è sensibile e non può essere delegata** flag all'account.  

-   Abilitare la **Smart card è necessaria per l'accesso interattivo** flag all'account.  

-   Configurare oggetti Criteri di gruppo per limitare l'uso dell'account Administrator in sistemi appartenenti a un dominio:  

    -   Uno o più oggetti Criteri di gruppo creare e collegare workstation e server membro unità organizzative in ogni dominio, aggiungere account di amministratore di ciascun dominio per i seguenti diritti utente in **Computer Configurazione computer\Criteri\Impostazioni computer\Impostazioni di Windows\Impostazioni protezione\Criteri Locali\Assegnazione diritti utente assegnazioni**:  

        -   Nega accesso al computer dalla rete  

        -   Nega accesso come processo batch  

        -   Nega accesso come servizio  

        -   Nega accesso tramite Servizi Desktop remoto  

> [!NOTE]  
> Quando si aggiungono account per questa impostazione, è necessario specificare se si sta configurando gli account amministratore locali o gli account amministratore di dominio. Ad esempio, per aggiungere l'account di amministratore del dominio NWTRADERS a queste negare diritti, è necessario digitare l'account come NWTRADERS\Administrator o cercare l'account amministratore per il dominio NWTRADERS. Se si digita "Administrator" in queste impostazioni diritti utente in Editor oggetti Criteri di gruppo, si limiterà l'account amministratore locale su ciascun computer in cui viene applicato l'oggetto Criteri di gruppo.
>   
> È consigliabile limitare l'account amministratore locale sul server e workstation membri allo stesso modo come account di amministratore di dominio. Pertanto, è necessario aggiungere in genere l'account Administrator per ogni dominio nella foresta e l'account di amministratore per il computer locale per queste impostazioni di diritti utente. La schermata seguente mostra un esempio di configurazione di questi diritti utente per bloccare l'account di amministratore locale e account di amministratore del dominio di eseguire accessi che non sono necessari per questi account.  


![protezione degli account amministratore predefinito](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_23.gif)  

-   Configurare oggetti Criteri di gruppo per limitare gli account di amministratore nei controller di dominio  
    -   In ogni dominio nella foresta, il controller di dominio predefinito o un criterio collegato ai controller di dominio unità Organizzativa deve essere modificato per aggiungere account di amministratore di ciascun dominio per i seguenti diritti utente in **Configurazione computer\Criteri\Impostazioni di Computer Computer\Impostazioni di Windows\Impostazioni sicurezza\Criteri locali\Assegnazione diritti utente assegnazioni**:   
        -   Nega accesso al computer dalla rete  

        -   Nega accesso come processo batch  

        -   Nega accesso come servizio  

        -   Nega accesso tramite Servizi Desktop remoto  

> [!NOTE]  
> Queste impostazioni garantirà l'account Administrator predefinito del dominio non può essere utilizzato per connettersi a un controller di dominio, anche se l'account, se abilitata, può accedere localmente ai controller di dominio. Poiché solo questo account deve essere abilitato e utilizzato negli scenari di ripristino di emergenza, si prevede che l'accesso fisico per almeno un controller di dominio sarà disponibile o che è possibile utilizzare altri account con autorizzazioni per accedere ai controller di dominio in modalità remota.  

-   Configurare il controllo dell'account amministratore  

    Dopo avere protetto account amministratore di ciascun dominio e disattivato, è necessario configurare il controllo per monitorare le modifiche all'account. Se l'account è abilitato, viene reimpostata la password o vengono apportate altre modifiche all'account, è necessario inviare avvisi per gli utenti o team responsabile dell'amministrazione di Active Directory, oltre ai team di risposta agli eventi imprevisti dell'organizzazione.  

#### <a name="step-by-step-instructions-to-secure-built-in-administrator-accounts-in-active-directory"></a>Istruzioni dettagliate per la protezione predefiniti Administrator account in Active Directory  

1.  Nelle **Server Manager**, fare clic su **Tools**, fare clic su **Active Directory Users and Computers**.  

2.  Per impedire gli attacchi che sfruttano la delega a usare le credenziali dell'account in altri sistemi, eseguire la procedura seguente:  

    1.  Fare doppio clic il **Administrator** dell'account e fare clic su **proprietà**.  

    2.  Scegliere il **Account** scheda.  

    3.  Sotto **opzioni dell'Account**, selezionare **Account è sensibile e non possono essere delegate** flag come indicato nello screenshot seguente e fare clic su **OK**.  

        ![protezione degli account amministratore predefinito](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_24.gif)  

3.  Per abilitare la **Smart card è necessaria per l'accesso interattivo** flag per l'account, procedere come segue:  

    1.  Fare doppio clic il **Administrator** dell'account e selezionare **proprietà**.  

    2.  Scegliere il **Account** scheda.  

    3.  Sotto **conto** opzioni, selezionare la **Smart card è necessaria per l'accesso interattivo** flag come indicato nello screenshot seguente e fare clic su **OK**.  

        ![protezione degli account amministratore predefinito](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_25.gif) 

##### <a name="configuring-gpos-to-restrict-administrator-accounts-at-the-domain-level"></a>Configurazione di oggetti Criteri di gruppo per limitare l'account di amministratore a livello di dominio  

> [!WARNING]  
> Questo oggetto Criteri di gruppo non deve mai essere collegata a livello di dominio poiché l'account Administrator predefinito potrebbe rendere inutilizzabile, anche in scenari di ripristino di emergenza.  

1.  Nelle **Server Manager**, fare clic su **Tools**, fare clic su **Gestione criteri di gruppo**.  

2.  Nell'albero della console, espandere <Forest>\Domains\\<Domain>, quindi **oggetti Criteri di gruppo** (dove <Forest> è il nome della foresta e <Domain> è il nome del dominio in cui si desidera creare i criteri di gruppo).  

3.  Nell'albero della console, fare doppio clic su **oggetti Criteri di gruppo**, fare clic su **New**.  

    ![protezione degli account amministratore predefinito](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_27.gif)  

4.  Nel **nuovo GPO** finestra di dialogo, digitare <GPO Name>e fare clic su **OK** (dove <GPO Name> è il nome di questo oggetto Criteri di gruppo) come indicato nello screenshot seguente.  

    ![protezione degli account amministratore predefinito](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_28.gif)  

5.  Nel riquadro dei dettagli, fare doppio clic su <GPO Name>, fare clic su **modifica**.  

6.  Passare a **Computer Configurazione computer\Criteri\Impostazioni di Windows\Impostazioni Protezione\criteri**, fare clic su **Assegnazione diritti utente**.  

    ![protezione degli account amministratore predefinito](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_29.gif)  

7.  Configurare i diritti utente per impedire che l'account amministratore l'accesso a server membri e workstation in rete eseguendo queste operazioni:  

    1.  Fare doppio clic su **Nega l'accesso al computer dalla rete** e selezionare **Definisci queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

    3.  Tipo di **Administrator**, fare clic su **Controlla nomi**, fare clic su **OK**. Verificare che l'account viene visualizzato <DomainName>\Username formato come indicato nello screenshot seguente.  

        ![protezione degli account amministratore predefinito](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_30.gif)  

    4.  Fare clic su **OK**, e **OK** nuovamente.  

8.  Configurare i diritti utente per evitare che l'account amministratore in grado di accedere come processo batch nel modo seguente:  

    1.  Fare doppio clic su **Nega accesso come processo batch** e selezionare **Definisci queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

    3.  Tipo di **Administrator**, fare clic su **Controlla nomi**, fare clic su **OK**. Verificare che l'account viene visualizzato <DomainName>\Username formato come indicato nello screenshot seguente.  

        ![protezione degli account amministratore predefinito](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_31.gif)  

    4.  Fare clic su **OK**, e **OK** nuovamente.  

9. Configurare i diritti utente per evitare che l'account amministratore in grado di accedere come servizio effettuando le operazioni seguenti:  

    1.  Fare doppio clic su **Nega accesso come servizio** e selezionare **Definisci queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

    3.  Tipo di **Administrator**, fare clic su **Controlla nomi**, fare clic su **OK**. Verificare che l'account viene visualizzato <DomainName>\Username formato come indicato nello screenshot seguente.  

        ![protezione degli account amministratore predefinito](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_32.gif)  

    4.  Fare clic su **OK**, e **OK** nuovamente.  

10. Configurare i diritti utente per impedire che l'account BA l'accesso a server membri e workstation tramite Servizi Desktop remoto eseguendo le operazioni seguenti:  

    1.  Fare doppio clic su **Nega accesso tramite Servizi Desktop remoto** e selezionare **Definisci queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

    3.  Tipo di **Administrator**, fare clic su **Controlla nomi**, fare clic su **OK**. Verificare che l'account viene visualizzato <DomainName>\Username formato come indicato nello screenshot seguente.  

        ![protezione degli account amministratore predefinito](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_33.gif)  

    4.  Fare clic su **OK**, e **OK** nuovamente.  

11. Per uscire **Editor Gestione criteri di gruppo**, fare clic su **File**, fare clic su **uscire**.  

12. Nelle **Gestione criteri di gruppo**, collegare l'oggetto Criteri di gruppo per il server membro e workstation unità organizzative eseguendo le operazioni seguenti:  

    1.  Passare il <Forest>\Domains\\ <Domain> (dove <Forest> è il nome della foresta e <Domain> è il nome del dominio in cui si desidera impostare i criteri di gruppo).  

    2.  Fare doppio clic su unità Organizzativa che oggetto Criteri di gruppo verranno applicati a e fare clic su **collegarne un esistente**.  

        ![protezione degli account amministratore predefinito](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_34.gif)  

    3.  Selezionare l'oggetto Criteri di gruppo è stato creato e fare clic su **OK**.  

        ![protezione degli account amministratore predefinito](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_35.gif)  

    4.  Creare collegamenti a tutte le altre unità organizzative che includono le workstation.  

    5.  Creare collegamenti a tutte le altre unità organizzative che includono server membri.  

> [!IMPORTANT]  
> Quando si aggiunge l'account di amministratore a queste impostazioni, specificare se si sta configurando un account amministratore locale o un account di amministratore di dominio dal modo in cui etichettare gli account. Ad esempio, per aggiungere l'account di amministratore del dominio TAILSPINTOYS a queste negare diritti, andare all'account di amministratore per il dominio TAILSPINTOYS, che viene visualizzato come TAILSPINTOYS\Administrator. Se si digita "Administrator" in queste impostazioni diritti utente in Editor oggetti Criteri di gruppo, si limiterà l'account amministratore locale su ciascun computer in cui viene applicato l'oggetto Criteri di gruppo, come descritto in precedenza.  

#### <a name="verification-steps"></a>Passaggi di verifica  
I passaggi di verifica descritti in questo argomento sono specifici di Windows 8 e Windows Server 2012.  

##### <a name="verify-smart-card-is-required-for-interactive-logon-account-option"></a>Verificare "Smart card è necessaria per l'accesso interattivo" opzione relativa all'Account  

1.  Da qualsiasi server membro o una workstation interessate dalle modifiche dell'oggetto Criteri di gruppo, tenta di accedere in modo interattivo al dominio usando l'account Administrator predefinito del dominio. Dopo aver tentato l'accesso, verrà visualizzata una finestra di dialogo simile al seguente.  

![protezione degli account amministratore predefinito](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_36.gif)  

##### <a name="verify-account-is-disabled-account-option"></a>Verificare "Account is disabled" opzione relativa all'Account  

1.  Da qualsiasi server membro o una workstation interessate dalle modifiche dell'oggetto Criteri di gruppo, tenta di accedere in modo interattivo al dominio usando l'account Administrator predefinito del dominio. Dopo aver tentato l'accesso, verrà visualizzata una finestra di dialogo simile al seguente.  

![protezione degli account amministratore predefinito](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_37.gif)  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Verificare le impostazioni oggetto Criteri di gruppo "Nega accesso al computer dalla rete"  
Da qualsiasi server membro o una workstation che non è interessato dalle modifiche dell'oggetto Criteri di gruppo (ad esempio un jump server), tentare di accedere a un server membro o una workstation in rete che è interessata dalle modifiche dell'oggetto Criteri di gruppo. Per verificare le impostazioni di GPO, provare a eseguire il mapping di unità di sistema usando il **NET USE** comando attenendosi alla procedura seguente:  

1.  Accedere al dominio tramite l'account Administrator predefinito del dominio.  

2.  Con il mouse, spostare il puntatore del mouse nell'angolo superiore destro o inferiore destro dello schermo. Quando la **accessi** barra viene visualizzata, fare clic su **ricerca**.  

3.  Nel **ricerca** , digitare **prompt dei comandi**, fare doppio clic su **prompt dei comandi**, quindi fare clic su **Esegui come amministratore** aprire con privilegi elevati prompt dei comandi.  

4.  Quando viene richiesto di approvare l'elevazione dei privilegi, fare clic su **Sì**.  

    ![protezione degli account amministratore predefinito](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_38.gif)  

5.  Nel **prompt dei comandi** finestra, digitare **net utilizzare \\ \\ \<nome del Server\>\c$**, dove \<nome Server\> è la nome del server membri o workstation che si sta tentando di accedere attraverso la rete.  

6.  Lo screenshot seguente mostra il messaggio di errore che deve essere visualizzato.  

    ![protezione degli account amministratore predefinito](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_39.gif)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Verificare le impostazioni oggetto Criteri di gruppo "Nega accesso come processo batch"  

Da qualsiasi server membro o una workstation interessate dalle modifiche dell'oggetto Criteri di gruppo, accedere localmente.  

###### <a name="create-a-batch-file"></a>Creare un File Batch  

1.  Con il mouse, spostare il puntatore del mouse nell'angolo superiore destro o inferiore destro dello schermo. Quando la **accessi** barra viene visualizzata, fare clic su **ricerca**.  

2.  Nel **ricerca** , digitare **blocco note**, fare clic su **Notepad**.  

3.  Nelle **Notepad**, digitare **dir c:**.  

4.  Fare clic su **File** e fare clic su **Salva con nome**.  

5.  Nel **nomefile** digitare  **<Filename>bat** (dove <Filename> è il nome del nuovo file batch).  

###### <a name="schedule-a-task"></a>Pianificare un'attività  

1.  Con il mouse, spostare il puntatore del mouse nell'angolo superiore destro o inferiore destro dello schermo. Quando la **accessi** barra viene visualizzata, fare clic su **ricerca**.  

2.  Nel **ricerca** , digitare **utilità di pianificazione**, fare clic su **utilità di pianificazione**.  

    > [!NOTE]  
    > Nei computer che eseguono Windows 8, nella casella di ricerca, digitare **pianificare le attività**, fare clic su **pianificare attività**.  

3.  Sul **utilità di pianificazione**, fare clic su **azione**, fare clic su **Crea attività**.  

4.  Nel **attività di creazione** finestra di dialogo, digitare **<Task Name>** (dove **<Task Name>** è il nome della nuova attività).  

5.  Scegliere il **azioni** scheda e fare clic su **New**.  

6.  Sotto **azione:**, selezionare **avviare un programma**.  

7.  Sotto **programma/script:**, fare clic su **Sfoglia**individuare e selezionare il file batch creato nella sezione "Creare un File Batch" e fare clic su **Open**.  

8.  Fare clic su **OK**.  

9. Fare clic sulla scheda **Generale**.  

10. Sotto **sicurezza** opzioni, fare clic su **Cambia utente o gruppo**.  

11. Al clic a livello di dominio, digitare il nome dell'account BA **Controlla nomi**, fare clic su **OK**.  

12. Selezionare **eseguire anche se l'utente è connesso o non** e **non archiviare password**. L'attività sarà solo possibile accedere alle risorse del computer locale.  

13. Fare clic su **OK**.  

14. Verrà visualizzata una finestra di dialogo, account utente che richiede le credenziali per eseguire l'attività.  

15. Dopo aver immesso le credenziali, fare clic su **OK**.  

16. Verrà visualizzata una finestra di dialogo simile alla seguente.  

    ![protezione degli account amministratore predefinito](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_40.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Verificare le impostazioni oggetto Criteri di gruppo "Nega accesso come servizio"  

1.  Da qualsiasi server membro o una workstation interessate dalle modifiche dell'oggetto Criteri di gruppo, accedere localmente.  

2.  Con il mouse, spostare il puntatore del mouse nell'angolo superiore destro o inferiore destro dello schermo. Quando la **accessi** barra viene visualizzata, fare clic su **ricerca**.  

3.  Nel **ricerca** , digitare **services**, fare clic su **Services**.  

4.  Individuare e fare doppio clic su **Spooler di stampa**.  

5.  Fare clic sulla scheda **Connessione**.  

6.  Sotto **Accedi come:**, selezionare **questo account**.  

7.  Fare clic su **esplorare**, digitare il nome dell'account BA a livello di dominio, fare clic su **Controlla nomi**, fare clic su **OK**.  

8.  Sotto **Password:** e **Conferma password:**, digitare la password dell'account amministratore e fare clic su **OK**.  

9. Fare clic su **OK** altre tre volte.  

10. Fare doppio clic il **servizio Spooler di stampa** e selezionare **riavviare**.  

11. Quando il servizio viene riavviato, verrà visualizzata una finestra di dialogo simile alla seguente.  

    ![protezione degli account amministratore predefinito](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_41.gif)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>Annullare le modifiche per il servizio Spooler di stampa  

1.  Da qualsiasi server membro o una workstation interessate dalle modifiche dell'oggetto Criteri di gruppo, accedere localmente.  

2.  Con il mouse, spostare il puntatore del mouse nell'angolo superiore destro o inferiore destro dello schermo. Quando la **accessi** barra viene visualizzata, fare clic su **ricerca**.  

3.  Nel **ricerca** , digitare **services**, fare clic su **Services**.  

4.  Individuare e fare doppio clic su **Spooler di stampa**.  

5.  Fare clic sulla scheda **Connessione**.  

6.  Sotto **Accedi come:**, selezionare la **LocalSystem** dell'account e fare clic su **OK**.  

##### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>Verificare le impostazioni oggetto Criteri di gruppo "Nega accesso tramite Servizi Desktop remoto"

1.  Con il mouse, spostare il puntatore del mouse nell'angolo superiore destro o inferiore destro dello schermo. Quando la **accessi** barra viene visualizzata, fare clic su **ricerca**.  

2.  Nel **ricerca** , digitare **connessione desktop remoto**, fare clic su **connessione Desktop remoto**.  

3.  Nel **Computer** digitare il nome del computer in cui si desidera connettersi a e fare clic su **Connect**. (È anche possibile digitare l'indirizzo IP anziché il nome del computer.)  

4.  Quando richiesto, fornire le credenziali per il nome dell'account BA a livello di dominio.  

5.  Verrà visualizzata una finestra di dialogo simile alla seguente.  

    ![protezione degli account amministratore predefinito](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_42.gif)  
