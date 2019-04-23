---
ms.assetid: f643099e-f9c6-476f-9378-5a9228c39b33
title: Appendice E - protezione dei gruppi Enterprise Admins in Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 976eb8c7159c8349b72bee05a5248b5cc116d96b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856722"
---
# <a name="appendix-e-securing-enterprise-admins-groups-in-active-directory"></a>Appendice E: Protezione dei gruppi Enterprise Admins in Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-e-securing-enterprise-admins-groups-in-active-directory"></a>Appendice E: Protezione dei gruppi Enterprise Admins in Active Directory  
Il gruppo Enterprise Admins (EA), che si trova nel dominio radice della foresta, deve contenere alcun utente su base giornaliera, con la possibile eccezione dell'account di amministratore del dominio radice, purché sia impostata come descritto in [appendice d: Protezione predefiniti Administrator account in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

Enterprise Admins sono, per impostazione predefinita, i membri del gruppo Administrators in ogni dominio nella foresta. È necessario non rimuovere il gruppo EA dal gruppo Administrators in ogni dominio perché in caso di uno scenario di ripristino di emergenza foresta diritti EA saranno probabilmente necessario. Gruppo Enterprise Admins della foresta deve essere protetto come descritto nelle istruzioni dettagliate che seguono.  

Per il gruppo Enterprise Admins nella foresta:  

1.  Nel GPO collegati alle unità organizzative che contiene i server membri e workstation in ogni dominio, deve essere aggiunte al gruppo Enterprise Admins per i seguenti diritti utente in **Computer Configurazione computer\Criteri\Impostazioni computer\Impostazioni di Windows\Impostazioni protezione\Criteri Policies\ Assegnazione diritti utente**:  

    -   Nega accesso al computer dalla rete  

    -   Nega accesso come processo batch  

    -   Nega accesso come servizio  

    -   Nega accesso locale  

    -   Nega accesso tramite Servizi Desktop remoto  

2.  Configurare il controllo per l'invio di avvisi se vengono apportate modifiche per le proprietà o l'appartenenza al gruppo Enterprise Admins.  

### <a name="step-by-step-instructions-for-removing-all-members-from-the-enterprise-admins-group"></a>Istruzioni dettagliate per la rimozione di tutti i membri dal gruppo Enterprise Admins  

1.  Nelle **Server Manager**, fare clic su **Tools**, fare clic su **Active Directory Users and Computers**.  

2.  Se non si gestisce il dominio radice della foresta, nell'albero della console, fare doppio clic su <Domain>, quindi fare clic su **Cambia dominio** (dove <Domain> è il nome del dominio attualmente si sta amministrando).  

    ![proteggere enterprise admin gruppi](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_43.gif)  

3.  Nel **Cambia dominio** finestra di dialogo, fare clic su **Sfoglia**, selezionare il dominio radice della foresta e fare clic su **OK**.  

    ![proteggere enterprise admin gruppi](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_44.gif)  

4.  Per rimuovere tutti i membri dal gruppo EA:  

    1.  Fare doppio clic il **Enterprise Admins** di gruppo e quindi fare clic sui **membri** scheda.  

        ![proteggere enterprise admin gruppi](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_45.gif)  

    2.  Selezionare un membro del gruppo, fare clic su **rimuovere**, fare clic su **Yes**, fare clic su **OK**.  

5.  Ripetere il passaggio 2 fino a quando non sono stati rimossi tutti i membri del gruppo EA.  

### <a name="step-by-step-instructions-to-secure-enterprise-admins-in-active-directory"></a>Istruzioni dettagliate per la protezione di Enterprise Admins in Active Directory  

1.  Nelle **Server Manager**, fare clic su **Tools**, fare clic su **Gestione criteri di gruppo**.  

2.  Nell'albero della console, espandere <Forest>\Domains\\<Domain>, quindi **oggetti Criteri di gruppo** (dove <Forest> è il nome della foresta e <Domain> è il nome del dominio in cui si desidera impostare i criteri di gruppo).  

    > [!NOTE]  
    > In una foresta che contiene più domini, un oggetto Criteri di gruppo simili deve essere creata in ogni dominio che richiede che il gruppo Enterprise Admins essere protetto.  

3.  Nell'albero della console, fare doppio clic su **oggetti Criteri di gruppo**, fare clic su **New**.  

    ![proteggere enterprise admin gruppi](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_46.gif)  

4.  Nel **nuovo GPO** finestra di dialogo, digitare <GPO Name>, fare clic su **OK** (in cui <GPO Name> è il nome di questo oggetto Criteri di gruppo).  

    ![proteggere enterprise admin gruppi](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_47.gif)  

5.  Nel riquadro dei dettagli, fare doppio clic su <GPO Name>, fare clic su **modifica**.  

6.  Passare a **Computer Configurazione computer\Criteri\Impostazioni di Windows\Impostazioni Protezione\criteri**, fare clic su **Assegnazione diritti utente**.  

    ![proteggere enterprise admin gruppi](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_48.gif)  

7.  Configurare i diritti utente per impedire ai membri del gruppo Enterprise Admins di accedere ai server membri e workstation in rete eseguendo queste operazioni:  

    1.  Fare doppio clic su **Nega l'accesso al computer dalla rete** e selezionare **Definisci queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

    3.  Tipo di **Enterprise Admins**, fare clic su **Controlla nomi**, fare clic su **OK**.  

        ![proteggere enterprise admin gruppi](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_49.gif)  

    4.  Fare clic su **OK**, e **OK** nuovamente.  

8.  Configurare i diritti utente per impedire ai membri del gruppo Enterprise Admins in grado di accedere come processo batch nel modo seguente:  

    1.  Fare doppio clic su **Nega accesso come processo batch** e selezionare **Definisci queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

        > [!NOTE]  
        > In una foresta che contiene più domini, fare clic su **posizioni** e selezionare il dominio radice della foresta.  

    3.  Tipo di **Enterprise Admins**, fare clic su **Controlla nomi**, fare clic su **OK**.  

        ![proteggere enterprise admin gruppi](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_50.gif)  

    4.  Fare clic su **OK**, e **OK** nuovamente.  

9. Configurare i diritti utente per impedire ai membri del gruppo EA in grado di accedere come servizio effettuando le operazioni seguenti:  

    1.  Fare doppio clic su **all'opzione Nega accesso come servizio** e selezionare **Definisci queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e quindi fare clic su **Sfoglia**.  

        > [!NOTE]  
        > In una foresta che contiene più domini, fare clic su **posizioni** e selezionare il dominio radice della foresta.  

    3.  Tipo di **Enterprise Admins**, fare clic su **Controlla nomi**, fare clic su **OK**.  

        ![proteggere enterprise admin gruppi](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_51.gif)  

    4.  Fare clic su **OK**, e **OK** nuovamente.  

10. Configurare i diritti utente per impedire ai membri del gruppo Enterprise Admins in grado di accedere localmente ai server membri e workstation nel modo seguente:  

    1.  Fare doppio clic su **Nega accesso locale** e selezionare **Definisci queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e quindi fare clic su **Sfoglia**.  

        > [!NOTE]  
        > In una foresta che contiene più domini, fare clic su **posizioni** e selezionare il dominio radice della foresta.  

    3.  Tipo di **Enterprise Admins**, fare clic su **Controlla nomi**, fare clic su **OK**.  

        ![proteggere enterprise admin gruppi](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_52.gif)  

    4.  Fare clic su **OK**, e **OK** nuovamente.  

11. Configurare i diritti utente per impedire ai membri del gruppo Enterprise Admins di accedere ai server membri e workstation tramite Servizi Desktop remoto eseguendo le operazioni seguenti:  

    1.  Fare doppio clic su **Nega accesso tramite Servizi Desktop remoto** e selezionare **Definisci queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e quindi fare clic su **Sfoglia**.  

        > [!NOTE]  
        > In una foresta che contiene più domini, fare clic su **posizioni** e selezionare il dominio radice della foresta.  

    3.  Tipo di **Enterprise Admins**, fare clic su **Controlla nomi**, fare clic su **OK**.  

        ![proteggere enterprise admin gruppi](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_53.gif)  

    4.  Fare clic su **OK**, e **OK** nuovamente.  

12. Per uscire **Editor Gestione criteri di gruppo**, fare clic su **File**, fare clic su **uscire**.  

13. Nelle **Gestione criteri di gruppo**, collegare l'oggetto Criteri di gruppo per il server membro e workstation unità organizzative eseguendo le operazioni seguenti:  

    1.  Passare il <Forest>\Domains\\ <Domain> (dove <Forest> è il nome della foresta e <Domain> è il nome del dominio in cui si desidera impostare i criteri di gruppo).  

    2.  Fare doppio clic su unità Organizzativa che oggetto Criteri di gruppo verranno applicati a e fare clic su **collegarne un esistente**.  

        ![proteggere enterprise admin gruppi](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_54.gif)  

    3.  Selezionare l'oggetto Criteri di gruppo appena creato e fare clic su **OK**.  

        ![proteggere enterprise admin gruppi](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_55.gif)  

    4.  Creare collegamenti a tutte le altre unità organizzative che includono le workstation.  

    5.  Creare collegamenti a tutte le altre unità organizzative che includono server membri.  

    6.  In una foresta che contiene più domini, un oggetto Criteri di gruppo simili deve essere creata in ogni dominio che richiede che il gruppo Enterprise Admins essere protetto.  

> [!IMPORTANT]  
> Se jump server vengono utilizzati per amministrare i controller di dominio e Active Directory, verificare che jump server si trovano in un'unità Organizzativa a cui non è collegato oggetti Criteri di gruppo.  

### <a name="verification-steps"></a>Passaggi di verifica  

#### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Verificare le impostazioni oggetto Criteri di gruppo "Nega accesso al computer dalla rete"  
Da qualsiasi server membro o una workstation che non è interessato dalle modifiche dell'oggetto Criteri di gruppo (ad esempio, "jump server"), tentare di accedere a un server membro o una workstation in rete che è interessata dalle modifiche dell'oggetto Criteri di gruppo. Per verificare le impostazioni di GPO, provare a eseguire il mapping di unità di sistema usando il **NET USE** comando attenendosi alla procedura seguente:  

1.  Accedere in locale usando un account membro del gruppo EA.  

2.  Con il mouse, spostare il puntatore del mouse nell'angolo superiore destro o inferiore destro dello schermo. Quando la **accessi** barra viene visualizzata, fare clic su **ricerca**.  

3.  Nel **ricerca** , digitare **prompt dei comandi**, fare doppio clic su **prompt dei comandi**, quindi fare clic su **Esegui come amministratore** aprire con privilegi elevati prompt dei comandi.  

4.  Quando viene richiesto di approvare l'elevazione dei privilegi, fare clic su **Sì**.  

    ![proteggere enterprise admin gruppi](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_56.gif)  

5.  Nel **prompt dei comandi** finestra, digitare **net utilizzare \\ \\ \<nome del Server\>\c$**, dove \<nome Server\> è la nome del server membri o workstation che si sta provando ad accedere attraverso la rete.  

6.  Lo screenshot seguente mostra il messaggio di errore che deve essere visualizzato.  

    ![proteggere enterprise admin gruppi](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_57.gif)  

#### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Verificare le impostazioni oggetto Criteri di gruppo "Nega accesso come processo batch"  

Da qualsiasi server membro o una workstation interessate dalle modifiche dell'oggetto Criteri di gruppo, accedere localmente.  

##### <a name="create-a-batch-file"></a>Creare un File Batch  

1.  Con il mouse, spostare il puntatore del mouse nell'angolo superiore destro o inferiore destro dello schermo. Quando la **accessi** barra viene visualizzata, fare clic su **ricerca**.  

2.  Nel **ricerca** , digitare **blocco note**, fare clic su **Notepad**.  

3.  Nelle **Notepad**, digitare **dir c:**.  

4.  Fare clic su **File**, fare clic su **Salva con nome**.  

5.  Nel **File** nome, digitare  **<Filename>bat** (dove <Filename> è il nome del nuovo file batch).  

##### <a name="schedule-a-task"></a>Pianificare un'attività  

1.  Con il mouse, spostare il puntatore del mouse nell'angolo superiore destro o inferiore destro dello schermo. Quando la **accessi** barra viene visualizzata, fare clic su **ricerca**.  

2.  Nel **ricerca** , digitare **utilità di pianificazione**, fare clic su **utilità di pianificazione**.  

    > [!NOTE]  
    > Nei computer che eseguono Windows 8, nelle **ricerca** , digitare **pianificare le attività**, fare clic su **pianificare attività**.  

3.  Fare clic su **azione**, fare clic su **Crea attività**.  

4.  Nel **attività di creazione** finestra di dialogo, digitare **<Task Name>** (dove <Task Name> è il nome della nuova attività).  

5.  Scegliere il **azioni** scheda e fare clic su **New**.  

6.  Nel **azione** campi, selezionare **avviare un programma**.  

7.  Sotto **programma/script**, fare clic su **Sfoglia**individuare e selezionare il file batch creato nel **creare un File Batch** sezione e fare clic su **aprire**.  

8.  Fare clic su **OK**.  

9. Fare clic sulla scheda **Generale**.  

10. Nel **opzioni di sicurezza** campo, fare clic su **Cambia utente o gruppo**.  

11. Digitare il nome di un account membro del gruppo EA, fare clic su **Controlla nomi**, fare clic su **OK**.  

12. Selezionare **eseguire anche se l'utente è connesso o non** e selezionare **non archiviare password**. L'attività sarà solo possibile accedere alle risorse del computer locale.  

13. Fare clic su **OK**.  

14. Verrà visualizzata una finestra di dialogo, account utente che richiede le credenziali per eseguire l'attività.  

15. Dopo aver immesso le credenziali, fare clic su **OK**.  

16. Verrà visualizzata una finestra di dialogo simile alla seguente.  

    ![proteggere enterprise admin gruppi](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_58.gif)  

#### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Verificare le impostazioni oggetto Criteri di gruppo "Nega accesso come servizio"  

1.  Da qualsiasi server membro o una workstation interessate dalle modifiche dell'oggetto Criteri di gruppo, accedere localmente.  

2.  Con il mouse, spostare il puntatore del mouse nell'angolo superiore destro o inferiore destro dello schermo. Quando la **accessi** barra viene visualizzata, fare clic su **ricerca**.  

3.  Nel **ricerca** , digitare **services**, fare clic su **Services**.  

4.  Individuare e fare doppio clic su **Spooler di stampa**.  

5.  Fare clic sulla scheda **Connessione**.  

6.  Sotto **accedere come**, selezionare **questo account**.  

7.  Fare clic su **esplorare**, digitare il nome di un account membro del gruppo EA, fare clic su **Controlla nomi**, fare clic su **OK**.  

8.  Sotto **Password:** e **Conferma password**, digitare la password dell'account selezionato e fare clic su **OK**.  

9. Fare clic su **OK** altre tre volte.  

10. Fare doppio clic il **Spooler di stampa** del servizio e selezionare **riavviare**.  

11. Quando il servizio viene riavviato, verrà visualizzata una finestra di dialogo simile alla seguente.  

    ![proteggere enterprise admin gruppi](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_59.gif)  

#### <a name="revert-changes-to-the-printer-spooler-service"></a>Annullare le modifiche per il servizio Spooler di stampa  

1.  Da qualsiasi server membro o una workstation interessate dalle modifiche dell'oggetto Criteri di gruppo, accedere localmente.  

2.  Con il mouse, spostare il puntatore del mouse nell'angolo superiore destro o inferiore destro dello schermo. Quando la **accessi** barra viene visualizzata, fare clic su **ricerca**.  

3.  Nel **ricerca** , digitare **services**, fare clic su **Services**.  

4.  Individuare e fare doppio clic su **Spooler di stampa**.  

5.  Fare clic sulla scheda **Connessione**.  

6.  Sotto **accedere come**, selezionare la **LocalSystem** dell'account e fare clic su **OK**.  

#### <a name="verify-deny-log-on-locally-gpo-settings"></a>Verificare le impostazioni oggetto Criteri di gruppo "Nega accesso locale"  

1.  Da qualsiasi server membro o una workstation interessate dalle modifiche dell'oggetto Criteri di gruppo, tenta di accedere in locale usando un account membro del gruppo EA. Verrà visualizzata una finestra di dialogo simile alla seguente.  

    ![proteggere enterprise admin gruppi](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_60.gif)  

#### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>Verificare le impostazioni oggetto Criteri di gruppo "Nega accesso tramite Servizi Desktop remoto"  

1.  Con il mouse, spostare il puntatore del mouse nell'angolo superiore destro o inferiore destro dello schermo. Quando la **accessi** barra viene visualizzata, fare clic su **ricerca**.  

2.  Nel **ricerca** , digitare **connessione desktop remoto**, quindi fare clic su **connessione Desktop remoto**.  

3.  Nel **Computer** digitare il nome del computer in cui si desidera connettersi a e quindi fare clic su **Connect**. (È anche possibile digitare l'indirizzo IP anziché il nome del computer.)  

4.  Quando richiesto, fornire le credenziali per un account membro del gruppo EA.  

5.  Verrà visualizzata una finestra di dialogo simile alla seguente.  

    ![proteggere enterprise admin gruppi](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_61.gif)  
