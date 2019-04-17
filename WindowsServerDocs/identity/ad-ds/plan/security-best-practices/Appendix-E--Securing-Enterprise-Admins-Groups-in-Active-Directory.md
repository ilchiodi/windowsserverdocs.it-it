---
ms.assetid: f643099e-f9c6-476f-9378-5a9228c39b33
title: Appendice E - protezione dei gruppi Enterprise Admins in Active Directory
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 714bc0ab3fe15d09f4ccabb3f35d9b4519e5459c
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="appendix-e-securing-enterprise-admins-groups-in-active-directory"></a>Appendice e: protezione dei gruppi Enterprise Admins in Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-e-securing-enterprise-admins-groups-in-active-directory"></a>Appendice e: protezione dei gruppi Enterprise Admins in Active Directory  
Il gruppo Enterprise Admins (EA), che si trova nel dominio radice della foresta, deve contenere alcun utente su base giornaliera, con la possibile eccezione dell'account di amministratore del dominio radice, purché sia impostata come descritto in [appendice d: protezione predefiniti Administrator account in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

Enterprise Admins sono, per impostazione predefinita, i membri del gruppo Administrators in ogni dominio nella foresta. Non è necessario rimuovere il gruppo EA dal gruppo Administrators in ogni dominio perché in caso di uno scenario di ripristino di emergenza foresta diritti EA sarà probabilmente necessari. Gruppo Enterprise Admins della foresta deve essere protetto come descritto nelle istruzioni dettagliate che seguono.  

Per il gruppo Enterprise Admins nella foresta:  

1.  Oggetti Criteri di gruppo collegati alle unità organizzative che contiene i server membri e workstation in ogni dominio, deve essere aggiunto il gruppo Enterprise Admins per i seguenti diritti utente in **Configurazione computer\Criteri\Impostazioni di Windows\Impostazioni protezione\Criteri Settings\nome diritti**:  

    -   Nega accesso al computer dalla rete  

    -   Nega accesso come processo batch  

    -   Nega accesso come servizio  

    -   Nega accesso locale  

    -   Nega accesso tramite Servizi Desktop remoto  

2.  Configurare il controllo per l'invio di avvisi se vengono apportate modifiche per le proprietà o l'appartenenza al gruppo Enterprise Admins.  

### <a name="step-by-step-instructions-for-removing-all-members-from-the-enterprise-admins-group"></a>Istruzioni dettagliate per la rimozione di tutti i membri dal gruppo Enterprise Admins  

1.  In **Server Manager**, fare clic su **strumenti**e fare clic su **Active Directory Users and Computers**.  

2.  Se non si gestisce il dominio radice della foresta, nell'albero della console, fare doppio clic su <Domain>, quindi fare clic su **Cambia dominio** (in cui <Domain> è il nome del dominio attualmente si sta amministrando).  

    ![proteggere enterprise admin gruppi](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_43.gif)  

3.  Nel **Cambia dominio** la finestra di dialogo, fare clic su **Sfoglia**, selezionare il dominio radice della foresta e fare clic su **OK**.  

    ![proteggere enterprise admin gruppi](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_44.gif)  

4.  Per rimuovere tutti i membri dal gruppo EA:  

    1.  Fare doppio clic il **Enterprise Admins** di gruppo e quindi fare clic su di **membri** scheda.  

        ![proteggere enterprise admin gruppi](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_45.gif)  

    2.  Selezionare un membro del gruppo, fare clic su **rimuovere**, fare clic su **Sì**e fare clic su **OK**.  

5.  Ripetere il passaggio 2 fino a quando non sono stati rimossi tutti i membri del gruppo EA.  

### <a name="step-by-step-instructions-to-secure-enterprise-admins-in-active-directory"></a>Istruzioni dettagliate per la protezione di Enterprise Admins in Active Directory  

1.  In **Server Manager**, fare clic su **strumenti**e fare clic su **Gestione criteri di gruppo**.  

2.  Nell'albero della console, espandere <Forest>\Domains\\<Domain>e quindi **oggetti Criteri di gruppo** (in cui <Forest> è il nome dell'insieme di strutture e <Domain> è il nome del dominio in cui si desidera impostare i criteri di gruppo).  

    > [!NOTE]  
    > In una foresta che contiene più domini, un oggetto Criteri di gruppo simili deve essere creato in ogni dominio che richiede che il gruppo Enterprise Admins essere protetti.  

3.  Nell'albero della console fare doppio clic su **oggetti Criteri di gruppo**e fare clic su **New**.  

    ![proteggere enterprise admin gruppi](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_46.gif)  

4.  Nel **nuovo oggetto Criteri di gruppo** la finestra di dialogo, digitare <GPO Name>e fare clic su **OK** (in cui <GPO Name> è il nome di questo oggetto Criteri di gruppo).  

    ![proteggere enterprise admin gruppi](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_47.gif)  

5.  Nel riquadro dei dettagli fare doppio clic su <GPO Name>e fare clic su **modifica**.  

6.  Passare a **Computer Configurazione computer\Criteri\Impostazioni di Windows\Impostazioni Protezione\criteri**e fare clic su **Assegnazione diritti utente**.  

    ![proteggere enterprise admin gruppi](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_48.gif)  

7.  Configurare i diritti utente per impedire ai membri del gruppo Enterprise Admins di accedere ai server membri e workstation sulla rete eseguendo le operazioni seguenti:  

    1.  Fare doppio clic su **negare l'accesso al computer dalla rete** e seleziona **definire queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

    3.  Tipo **Enterprise Admins**, fare clic su **Controlla nomi**e fare clic su **OK**.  

        ![proteggere enterprise admin gruppi](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_49.gif)  

    4.  Fare clic su **OK**, e **OK** nuovamente.  

8.  Configurare i diritti utente per impedire ai membri del gruppo Enterprise Admins l'accesso come processo batch effettuando le operazioni seguenti:  

    1.  Fare doppio clic su **Nega accesso come processo batch** e seleziona **definire queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

        > [!NOTE]  
        > In una foresta che contiene più domini, fare clic su **percorsi** e selezionare il dominio radice della foresta.  

    3.  Tipo **Enterprise Admins**, fare clic su **Controlla nomi**e fare clic su **OK**.  

        ![proteggere enterprise admin gruppi](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_50.gif)  

    4.  Fare clic su **OK**, e **OK** nuovamente.  

9. Configurare i diritti utente per impedire ai membri del gruppo EA di accesso come servizio effettuando le operazioni seguenti:  

    1.  Fare doppio clic su **all'opzione Nega accesso come servizio** e seleziona **definire queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e quindi fare clic su **Sfoglia**.  

        > [!NOTE]  
        > In una foresta che contiene più domini, fare clic su **percorsi** e selezionare il dominio radice della foresta.  

    3.  Tipo **Enterprise Admins**, fare clic su **Controlla nomi**e fare clic su **OK**.  

        ![proteggere enterprise admin gruppi](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_51.gif)  

    4.  Fare clic su **OK**, e **OK** nuovamente.  

10. Configurare i diritti utente a impedire ai membri del gruppo Enterprise Admins di accedere localmente al server e workstation membri effettuando le operazioni seguenti:  

    1.  Fare doppio clic su **Nega accesso locale** e seleziona **definire queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e quindi fare clic su **Sfoglia**.  

        > [!NOTE]  
        > In una foresta che contiene più domini, fare clic su **percorsi** e selezionare il dominio radice della foresta.  

    3.  Tipo **Enterprise Admins**, fare clic su **Controlla nomi**e fare clic su **OK**.  

        ![proteggere enterprise admin gruppi](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_52.gif)  

    4.  Fare clic su **OK**, e **OK** nuovamente.  

11. Configurare i diritti utente per impedire ai membri del gruppo Enterprise Admins di accedere ai server membri e workstation tramite Servizi Desktop remoto eseguendo le operazioni seguenti:  

    1.  Fare doppio clic su **Nega accesso tramite Servizi Desktop remoto** e seleziona **definire queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e quindi fare clic su **Sfoglia**.  

        > [!NOTE]  
        > In una foresta che contiene più domini, fare clic su **percorsi** e selezionare il dominio radice della foresta.  

    3.  Tipo **Enterprise Admins**, fare clic su **Controlla nomi**e fare clic su **OK**.  

        ![proteggere enterprise admin gruppi](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_53.gif)  

    4.  Fare clic su **OK**, e **OK** nuovamente.  

12. Per uscire da **Editor Gestione criteri di gruppo**, fare clic su **File**e fare clic su **uscire**.  

13. In **Gestione criteri di gruppo**, collegare l'oggetto Criteri di gruppo per il server membro e workstation unità organizzative effettuando le operazioni seguenti:  

    1.  Passare al <Forest>\Domains\\<Domain> (in cui <Forest> è il nome dell'insieme di strutture e <Domain> è il nome del dominio in cui si desidera impostare i criteri di gruppo).  

    2.  Fare doppio clic su oggetto Criteri di gruppo verranno applicate a e fare clic su, l'unità Organizzativa **collegare un oggetto Criteri di gruppo esistente**.  

        ![proteggere enterprise admin gruppi](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_54.gif)  

    3.  Selezionare l'oggetto Criteri di gruppo appena creato e fare clic su **OK**.  

        ![proteggere enterprise admin gruppi](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_55.gif)  

    4.  Creare collegamenti a tutte le altre unità organizzative contenenti le workstation.  

    5.  Creare collegamenti a tutte le altre unità organizzative contenenti i server membri.  

    6.  In una foresta che contiene più domini, un oggetto Criteri di gruppo simili deve essere creato in ogni dominio che richiede che il gruppo Enterprise Admins essere protetti.  

> [!IMPORTANT]  
> Se jump server vengono utilizzati per amministrare i controller di dominio e Active Directory, verificare che jump server si trovano in un'unità Organizzativa a cui non è ancora collegato oggetti Criteri di gruppo.  

### <a name="verification-steps"></a>Passaggi di verifica  

#### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Verificare le impostazioni oggetto Criteri di gruppo "Nega accesso al computer dalla rete"  
Da qualsiasi server membro o una workstation in cui non è interessata dall'oggetto Criteri di gruppo viene modificato (ad esempio, "jump server"), tentare di accedere a un server membro o una workstation in rete che è interessata dall'oggetto Criteri di gruppo viene modificato. Per verificare le impostazioni oggetto Criteri di gruppo, tentare di eseguire il mapping di unità di sistema utilizzando il **NET USE** comando eseguendo i passaggi seguenti:  

1.  Accedere localmente utilizzando un account che sia membro del gruppo EA.  

2.  Con il mouse, sposta il puntatore nell'angolo superiore destro o in basso a destra dello schermo. Quando il **accessi** barra viene visualizzata, fare clic su **ricerca**.  

3.  Nel **ricerca** digitare **prompt dei comandi**, fare doppio clic su **prompt dei comandi**, quindi fare clic su **Esegui come amministratore** per aprire un prompt dei comandi con privilegi elevati.  

4.  Quando viene richiesto di approvare l'elevazione dei privilegi, fare clic su **Sì**.  

    ![proteggere enterprise admin gruppi](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_56.gif)  

5.  Nel **prompt dei comandi** finestra, digitare **net uso \ \ \<Server Name>\c$**, dove <Server Name> è il nome del server membri o workstation che si sta tentando di accedere attraverso la rete.  

6.  Lo screenshot seguente mostra il messaggio di errore che dovrebbe essere visualizzato.  

    ![proteggere enterprise admin gruppi](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_57.gif)  

#### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Verificare le impostazioni oggetto Criteri di gruppo "Nega accesso come processo batch"  

Da qualsiasi server membro o una workstation interessati dalle modifiche dell'oggetto Criteri di gruppo, accedere localmente.  

##### <a name="create-a-batch-file"></a>Creare un File Batch  

1.  Con il mouse, sposta il puntatore nell'angolo superiore destro o in basso a destra dello schermo. Quando il **accessi** barra viene visualizzata, fare clic su **ricerca**.  

2.  Nel **ricerca** digitare **blocco note**e fare clic su **blocco note**.  

3.  In **blocco note**, tipo **c: dir**.  

4.  Fare clic su **File**e fare clic su **Salva con nome**.  

5.  Nel **File** nella casella nome ** <Filename>. bat** (in cui <Filename> è il nome del nuovo file batch).  

##### <a name="schedule-a-task"></a>Pianificare un'attività  

1.  Con il mouse, sposta il puntatore nell'angolo superiore destro o in basso a destra dello schermo. Quando il **accessi** barra viene visualizzata, fare clic su **ricerca**.  

2.  Nel **ricerca** digitare **utilità di pianificazione**e fare clic su **utilità di pianificazione**.  

    > [!NOTE]  
    > Nei computer che eseguono Windows 8, nel **ricerca** digitare **Pianifica attività**e fare clic su **Pianifica attività**.  

3.  Fare clic su **azione**e fare clic su **Crea attività**.  

4.  Nel **Crea attività** la finestra di dialogo, digitare ** <Task Name> ** (in cui <Task Name> è il nome della nuova attività).  

5.  Fare clic su di **azioni** scheda e fare clic su **New**.  

6.  Nel **azione** selezionare **avviare un programma**.  

7.  In **programma o script**, fare clic su **Sfoglia**, individuare e selezionare il file batch creato il **creare un File Batch** sezione e fare clic su **aprire**.  

8.  Fare clic su **OK**.  

9. Fare clic su di **generale** scheda.  

10. Nel **opzioni di protezione** campo, fare clic su **Cambia utente o gruppo**.  

11. Digitare il nome di un account che sia membro del gruppo EA, fare clic su **Controlla nomi**e fare clic su **OK**.  

12. Selezionare **eseguire se l'utente è connesso o non** e seleziona **non archiviare password**. L'attività sarà solo possibile accedere alle risorse del computer locale.  

13. Fare clic su **OK**.  

14. Dovrebbe essere visualizzata una finestra di dialogo, account utente che richiede le credenziali necessarie per eseguire l'attività.  

15. Dopo aver immesso le credenziali, fare clic su **OK**.  

16. Dovrebbe essere visualizzata una finestra di dialogo simile al seguente.  

    ![proteggere enterprise admin gruppi](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_58.gif)  

#### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Verificare le impostazioni oggetto Criteri di gruppo "Nega accesso come servizio"  

1.  Da qualsiasi server membro o una workstation interessati dalle modifiche dell'oggetto Criteri di gruppo, accedere localmente.  

2.  Con il mouse, sposta il puntatore nell'angolo superiore destro o in basso a destra dello schermo. Quando il **accessi** barra viene visualizzata, fare clic su **ricerca**.  

3.  Nel **ricerca** digitare **servizi**e fare clic su **servizi**.  

4.  Individuare e fare doppio clic su **Spooler di stampa**.  

5.  Fare clic su di **accesso** scheda.  

6.  In **accedere come**selezionare **questo account**.  

7.  Fare clic su **Sfoglia**, digitare il nome di un account che sia membro del gruppo EA, fare clic su **Controlla nomi**e fare clic su **OK**.  

8.  In **Password:** e **Conferma password**, digita la password dell'account selezionato e scegliere **OK**.  

9. Fare clic su **OK** tre volte.  

10. Fare doppio clic su di **Spooler di stampa** service e seleziona **riavviare**.  

11. Quando viene riavviato il servizio, verrà visualizzata una finestra di dialogo simile al seguente.  

    ![proteggere enterprise admin gruppi](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_59.gif)  

#### <a name="revert-changes-to-the-printer-spooler-service"></a>Ripristinare le modifiche per il servizio Spooler di stampa  

1.  Da qualsiasi server membro o una workstation interessati dalle modifiche dell'oggetto Criteri di gruppo, accedere localmente.  

2.  Con il mouse, sposta il puntatore nell'angolo superiore destro o in basso a destra dello schermo. Quando il **accessi** barra viene visualizzata, fare clic su **ricerca**.  

3.  Nel **ricerca** digitare **servizi**e fare clic su **servizi**.  

4.  Individuare e fare doppio clic su **Spooler di stampa**.  

5.  Fare clic su di **accesso** scheda.  

6.  In **accedere come**, selezionare il **sistema locale** account, quindi scegliere **OK**.  

#### <a name="verify-deny-log-on-locally-gpo-settings"></a>Verificare le impostazioni oggetto Criteri di gruppo "Nega accesso locale"  

1.  Da qualsiasi server membro o una workstation interessati dall'oggetto Criteri di gruppo viene modificato, tentare di accedere localmente utilizzando un account che sia membro del gruppo EA. Dovrebbe essere visualizzata una finestra di dialogo simile al seguente.  

    ![proteggere enterprise admin gruppi](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_60.gif)  

#### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>Verificare le impostazioni oggetto Criteri di gruppo "Nega accesso tramite Servizi Desktop remoto"  

1.  Con il mouse, sposta il puntatore nell'angolo superiore destro o in basso a destra dello schermo. Quando il **accessi** barra viene visualizzata, fare clic su **ricerca**.  

2.  Nel **ricerca** digitare **connessione desktop remoto**, quindi fare clic su **connessione Desktop remoto**.  

3.  Nel **Computer** , digitare il nome del computer che si desidera connettersi e quindi fare clic su **Connetti**. (È anche possibile digitare l'indirizzo IP anziché il nome del computer.)  

4.  Quando richiesto, fornire le credenziali per un account che sia membro del gruppo EA.  

5.  Dovrebbe essere visualizzata una finestra di dialogo simile al seguente.  

    ![proteggere enterprise admin gruppi](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_61.gif)  
