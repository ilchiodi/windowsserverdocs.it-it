---
ms.assetid: f643099e-f9c6-476f-9378-5a9228c39b33
title: Appendice E-protezione dei gruppi Enterprise Admins in Active Directory
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 5294be945ce4a93ffeb1c27cffa8a82470920e7b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80821634"
---
# <a name="appendix-e-securing-enterprise-admins-groups-in-active-directory"></a>Appendice E: Protezione dei gruppi Enterprise Admins in Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-e-securing-enterprise-admins-groups-in-active-directory"></a>Appendice E: Protezione dei gruppi Enterprise Admins in Active Directory  
Il gruppo Enterprise Admins (EA), che si trova nel dominio radice della foresta, non deve contenere alcun utente su base giornaliera, con la possibile eccezione dell'account amministratore del dominio radice, purché sia protetto come descritto in [Appendice D: protezione degli account amministratore predefiniti in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

Enterprise Admins sono, per impostazione predefinita, i membri del gruppo Administrators in ogni dominio della foresta. Non è consigliabile rimuovere il gruppo EA dai gruppi Administrators in ogni dominio perché, in caso di scenario di ripristino di emergenza di una foresta, i diritti EA saranno probabilmente necessari. Il gruppo Enterprise Admins della foresta deve essere protetto come descritto in dettaglio nelle istruzioni dettagliate riportate di seguito.  

Per il gruppo Enterprise Admins nella foresta:  

1.  Negli oggetti Criteri di gruppo collegati alle unità organizzative che contengono server membri e workstation in ogni dominio, il gruppo Enterprise Admins deve essere aggiunto ai seguenti diritti utente in **computer Computer\criteri\impostazioni Windows\Impostazioni protezione\Criteri locali\Assegnazione diritti assegnati**:  

    -   Nega accesso al computer dalla rete  

    -   Nega accesso come processo batch  

    -   Nega accesso come servizio  

    -   Nega accesso locale  

    -   Nega accesso tramite Servizi Desktop remoto  

2.  Configurare il controllo per l'invio di avvisi se vengono apportate modifiche alle proprietà o all'appartenenza del gruppo Enterprise Admins.  

### <a name="step-by-step-instructions-for-removing-all-members-from-the-enterprise-admins-group"></a>Istruzioni dettagliate per la rimozione di tutti i membri dal gruppo Enterprise Admins  

1.  In **Server Manager**fare clic su **strumenti**e quindi su **Active Directory utenti e computer**.  

2.  Se non si gestisce il dominio radice per la foresta, nell'albero della console fare clic con il pulsante destro del mouse su <Domain>, quindi scegliere **Cambia dominio** (dove <Domain> è il nome del dominio che si sta amministrando).  

    ![proteggere i gruppi di amministratori dell'organizzazione](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_43.gif)  

3.  Nella finestra di dialogo **Cambia dominio** fare clic su **Sfoglia**, selezionare il dominio radice per la foresta e quindi fare clic su **OK**.  

    ![proteggere i gruppi di amministratori dell'organizzazione](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_44.gif)  

4.  Per rimuovere tutti i membri dal gruppo EA:  

    1.  Fare doppio clic sul gruppo **Enterprise Admins** , quindi fare clic sulla scheda **membri** .  

        ![proteggere i gruppi di amministratori dell'organizzazione](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_45.gif)  

    2.  Selezionare un membro del gruppo, fare clic su **Rimuovi**, fare clic su **Sì**e quindi su **OK**.  

5.  Ripetere il passaggio 2 fino a quando non sono stati rimossi tutti i membri del gruppo EA.  

### <a name="step-by-step-instructions-to-secure-enterprise-admins-in-active-directory"></a>Istruzioni dettagliate per proteggere gli amministratori aziendali in Active Directory  

1.  In **Server Manager**fare clic su **strumenti**e quindi su **Gestione criteri di gruppo**.  

2.  Nell'albero della console espandere <Forest>\Domains\\<Domain>, quindi **criteri di gruppo oggetti** (dove <Forest> è il nome della foresta e <Domain> è il nome del dominio in cui si desidera impostare il criteri di gruppo).  

    > [!NOTE]  
    > In una foresta che contiene più domini, è necessario creare un oggetto Criteri di gruppo simile in ogni dominio che richiede la sicurezza del gruppo Enterprise Admins.  

3.  Nell'albero della console fare clic con il pulsante destro del mouse su **criteri di gruppo oggetti**, quindi scegliere **nuovo**.  

    ![proteggere i gruppi di amministratori dell'organizzazione](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_46.gif)  

4.  Nella finestra di dialogo **nuovo oggetto Criteri** di gruppo digitare <GPO Name>e fare clic su **OK** (dove <GPO Name> è il nome di questo GPO).  

    ![proteggere i gruppi di amministratori dell'organizzazione](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_47.gif)  

5.  Nel riquadro dei dettagli fare clic con il pulsante destro del mouse su <GPO Name>, quindi scegliere **modifica**.  

6.  Passare a **computer Computer\criteri\impostazioni Windows\Impostazioni protezione\Criteri criteri**e fare clic su **assegnazione diritti utente**.  

    ![proteggere i gruppi di amministratori dell'organizzazione](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_48.gif)  

7.  Configurare i diritti utente per impedire ai membri del gruppo Enterprise Admins di accedere ai server membri e alle workstation in rete eseguendo le operazioni seguenti:  

    1.  Fare doppio clic su **Nega accesso al computer dalla rete** e selezionare **Definisci le impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

    3.  Digitare **Enterprise Admins**, fare clic su **Controlla nomi**e quindi su **OK**.  

        ![proteggere i gruppi di amministratori dell'organizzazione](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_49.gif)  

    4.  Fare clic su **OK**e di nuovo su **OK** .  

8.  Configurare i diritti utente per impedire ai membri del gruppo Enterprise Admins di accedere come processo batch eseguendo le operazioni seguenti:  

    1.  Fare doppio clic su **Nega accesso come processo batch** e selezionare **Definisci queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

        > [!NOTE]  
        > In una foresta che contiene più domini, fare clic su **percorsi** e selezionare il dominio radice della foresta.  

    3.  Digitare **Enterprise Admins**, fare clic su **Controlla nomi**e quindi su **OK**.  

        ![proteggere i gruppi di amministratori dell'organizzazione](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_50.gif)  

    4.  Fare clic su **OK**e di nuovo su **OK** .  

9. Configurare i diritti utente per impedire ai membri del gruppo EA di accedere come servizio effettuando le operazioni seguenti:  

    1.  Fare doppio clic su **Nega log come servizio** e selezionare **Definisci queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** , quindi fare clic su **Sfoglia**.  

        > [!NOTE]  
        > In una foresta che contiene più domini, fare clic su **percorsi** e selezionare il dominio radice della foresta.  

    3.  Digitare **Enterprise Admins**, fare clic su **Controlla nomi**e quindi su **OK**.  

        ![proteggere i gruppi di amministratori dell'organizzazione](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_51.gif)  

    4.  Fare clic su **OK**e di nuovo su **OK** .  

10. Configurare i diritti utente per impedire ai membri del gruppo Enterprise Admins di accedere localmente ai server e alle workstation membri seguendo questa procedura:  

    1.  Fare doppio clic **su Nega accesso locale** e selezionare **Definisci queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** , quindi fare clic su **Sfoglia**.  

        > [!NOTE]  
        > In una foresta che contiene più domini, fare clic su **percorsi** e selezionare il dominio radice della foresta.  

    3.  Digitare **Enterprise Admins**, fare clic su **Controlla nomi**e quindi su **OK**.  

        ![proteggere i gruppi di amministratori dell'organizzazione](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_52.gif)  

    4.  Fare clic su **OK**e di nuovo su **OK** .  

11. Configurare i diritti utente per impedire ai membri del gruppo Enterprise Admins di accedere ai server e alle workstation membri tramite Servizi Desktop remoto eseguendo le operazioni seguenti:  

    1.  Fare doppio clic su **Nega accesso tramite Servizi Desktop remoto** e selezionare **Definisci queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** , quindi fare clic su **Sfoglia**.  

        > [!NOTE]  
        > In una foresta che contiene più domini, fare clic su **percorsi** e selezionare il dominio radice della foresta.  

    3.  Digitare **Enterprise Admins**, fare clic su **Controlla nomi**e quindi su **OK**.  

        ![proteggere i gruppi di amministratori dell'organizzazione](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_53.gif)  

    4.  Fare clic su **OK**e di nuovo su **OK** .  

12. Per uscire da **Editor gestione criteri di gruppo**, fare clic su **file**e quindi su **Esci**.  

13. In **gestione criteri di gruppo**collegare l'oggetto Criteri di gruppo al server membro e alle unità organizzative della workstation effettuando le operazioni seguenti:  

    1.  Passare al <Forest>\Domains\\<Domain> (dove <Forest> è il nome della foresta e <Domain> è il nome del dominio in cui si desidera impostare il Criteri di gruppo).  

    2.  Fare clic con il pulsante destro del mouse sull'unità organizzativa a cui verrà applicato l'oggetto Criteri di gruppo e scegliere **collega un oggetto Criteri**di gruppo  

        ![proteggere i gruppi di amministratori dell'organizzazione](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_54.gif)  

    3.  Selezionare l'oggetto Criteri di gruppo appena creato e fare clic su **OK**.  

        ![proteggere i gruppi di amministratori dell'organizzazione](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_55.gif)  

    4.  Creare collegamenti a tutte le altre ou che contengono workstation.  

    5.  Creare collegamenti a tutte le altre ou che contengono server membro.  

    6.  In una foresta che contiene più domini, è necessario creare un oggetto Criteri di gruppo simile in ogni dominio che richiede la sicurezza del gruppo Enterprise Admins.  

> [!IMPORTANT]  
> Se i server di salto vengono utilizzati per amministrare i controller di dominio e Active Directory, assicurarsi che i server Jump si trovino in un'unità organizzativa a cui questi oggetti Criteri di gruppo non sono collegati.  

### <a name="verification-steps"></a>Passaggi di verifica  

#### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Verificare le impostazioni dell'oggetto Criteri di gruppo "Nega accesso a questo computer dalla rete"  
Da qualsiasi server membro o workstation che non è influenzato dall'oggetto Criteri di gruppo, ad esempio un "Jump server", tenta di accedere a un server membro o a una workstation sulla rete interessata dall'oggetto Criteri di gruppo modificato. Per verificare le impostazioni dell'oggetto Criteri di gruppo, provare a eseguire il mapping dell'unità di sistema utilizzando il comando **net use** attenendosi alla procedura seguente:  

1.  Accedere localmente utilizzando un account membro del gruppo EA.  

2.  Con il mouse, spostare il puntatore nell'angolo superiore destro o inferiore destro dello schermo. Quando viene visualizzata la barra degli **accessi** , fare clic su **Cerca**.  

3.  Nella casella di **ricerca** Digitare **prompt**dei comandi, fare clic con il pulsante destro del mouse su **prompt dei comandi**e quindi scegliere **Esegui come amministratore** per aprire un prompt dei comandi con privilegi elevati.  

4.  Quando viene richiesto di approvare l'elevazione, fare clic su **Sì**.  

    ![proteggere i gruppi di amministratori dell'organizzazione](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_56.gif)  

5.  Nella finestra del **prompt dei comandi** Digitare **net use \\\\nome server \<\>\c $** , dove \<nome server\> è il nome del server membro o della workstation a cui si sta tentando di accedere attraverso la rete.  

6.  Lo screenshot seguente mostra il messaggio di errore che dovrebbe essere visualizzato.  

    ![proteggere i gruppi di amministratori dell'organizzazione](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_57.gif)  

#### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Verificare le impostazioni dell'oggetto Criteri di gruppo "Nega accesso come processo batch"  

Accedere localmente da qualsiasi server membro o workstation interessato dall'oggetto Criteri di gruppo.  

##### <a name="create-a-batch-file"></a>Creazione di un file batch  

1.  Con il mouse, spostare il puntatore nell'angolo superiore destro o inferiore destro dello schermo. Quando viene visualizzata la barra degli **accessi** , fare clic su **Cerca**.  

2.  Nella casella di **ricerca** Digitare **blocco note**e fare clic su **blocco note**.  

3.  In **blocco note**Digitare **dir c:** .  

4.  Fare clic su **file**e quindi su **Salva con nome**.  

5.  Nella casella nome **file** Digitare **<Filename>. bat** , dove <Filename> è il nome del nuovo file batch.  

##### <a name="schedule-a-task"></a>Pianificare un'attività  

1.  Con il mouse, spostare il puntatore nell'angolo superiore destro o inferiore destro dello schermo. Quando viene visualizzata la barra degli **accessi** , fare clic su **Cerca**.  

2.  Nella casella di **ricerca** Digitare **utilità di pianificazione**e fare clic su **utilità di pianificazione**.  

    > [!NOTE]  
    > Nei computer che eseguono Windows 8, nella casella di **ricerca** Digitare **Pianifica attività**, quindi fare clic su **Pianifica attività**.  

3.  Fare clic su **azione**e quindi su **Crea attività**.  

4.  Nella finestra di dialogo **Crea attività** Digitare **<Task Name>** (dove <Task Name> è il nome della nuova attività).  

5.  Fare clic sulla scheda **azioni** e quindi su **nuovo**.  

6.  Nel campo **azione** selezionare **avvia un programma**.  

7.  In **programma/script**fare clic su **Sfoglia**, individuare e selezionare il file batch creato nella sezione **creare un file batch** e fare clic su **Apri**.  

8.  Fare clic su **OK**.  

9. Fare clic sulla scheda **Generale**.  

10. Nel campo **Opzioni di sicurezza** fare clic su **modifica utente o gruppo**.  

11. Digitare il nome di un account membro del gruppo EAs, fare clic su **Controlla nomi**e quindi su **OK**.  

12. Selezionare **Esegui se l'utente è connesso o meno** e selezionare **non archiviare la password**. L'attività avrà accesso solo alle risorse del computer locale.  

13. Fare clic su **OK**.  

14. Verrà visualizzata una finestra di dialogo in cui vengono richieste le credenziali dell'account utente per l'esecuzione dell'attività.  

15. Dopo aver immesso le credenziali, fare clic su **OK**.  

16. Verrà visualizzata una finestra di dialogo simile alla seguente.  

    ![proteggere i gruppi di amministratori dell'organizzazione](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_58.gif)  

#### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Verificare le impostazioni dell'oggetto Criteri di gruppo "Nega accesso come servizio"  

1.  Accedere localmente da qualsiasi server membro o workstation interessato dall'oggetto Criteri di gruppo.  

2.  Con il mouse, spostare il puntatore nell'angolo superiore destro o inferiore destro dello schermo. Quando viene visualizzata la barra degli **accessi** , fare clic su **Cerca**.  

3.  Nella casella di **ricerca** Digitare **Services**, quindi fare clic su **Servizi**.  

4.  Individuare e fare doppio clic su **spooler di stampa**.  

5.  Fare clic sulla scheda **Connessione**.  

6.  In **Accedi come**selezionare **l'account**.  

7.  Fare clic su **Sfoglia**, digitare il nome di un account membro del gruppo EAS, fare clic su **Controlla nomi**e quindi su **OK**.  

8.  In **password:** e **Conferma password**digitare la password dell'account selezionato, quindi fare clic su **OK**.  

9. Fare clic su **OK** altre tre volte.  

10. Fare clic con il pulsante destro del mouse sul servizio **spooler di stampa** e scegliere **Riavvia**.  

11. Quando il servizio viene riavviato, viene visualizzata una finestra di dialogo simile alla seguente.  

    ![proteggere i gruppi di amministratori dell'organizzazione](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_59.gif)  

#### <a name="revert-changes-to-the-printer-spooler-service"></a>Ripristinare le modifiche al servizio spooler di stampa  

1.  Accedere localmente da qualsiasi server membro o workstation interessato dall'oggetto Criteri di gruppo.  

2.  Con il mouse, spostare il puntatore nell'angolo superiore destro o inferiore destro dello schermo. Quando viene visualizzata la barra degli **accessi** , fare clic su **Cerca**.  

3.  Nella casella di **ricerca** Digitare **Services**, quindi fare clic su **Servizi**.  

4.  Individuare e fare doppio clic su **spooler di stampa**.  

5.  Fare clic sulla scheda **Connessione**.  

6.  In **Accedi come**selezionare l'account di **sistema locale** e fare clic su **OK**.  

#### <a name="verify-deny-log-on-locally-gpo-settings"></a>Verificare le impostazioni dell'oggetto Criteri di gruppo "Nega accesso locale"  

1.  Da qualsiasi server membro o workstation interessato dalle modifiche dell'oggetto Criteri di gruppo, provare ad accedere localmente utilizzando un account membro del gruppo EA. Verrà visualizzata una finestra di dialogo simile alla seguente.  

    ![proteggere i gruppi di amministratori dell'organizzazione](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_60.gif)  

#### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>Verificare le impostazioni dell'oggetto Criteri di gruppo "Nega accesso tramite Servizi Desktop remoto"  

1.  Con il mouse, spostare il puntatore nell'angolo superiore destro o inferiore destro dello schermo. Quando viene visualizzata la barra degli **accessi** , fare clic su **Cerca**.  

2.  Nella casella di **ricerca** Digitare **Connessione desktop remoto**, quindi fare clic su **Connessione desktop remoto**.  

3.  Nel campo **computer** Digitare il nome del computer a cui si desidera connettersi, quindi fare clic su **Connetti**. È anche possibile digitare l'indirizzo IP anziché il nome del computer.  

4.  Quando richiesto, fornire le credenziali di un account membro del gruppo EA.  

5.  Verrà visualizzata una finestra di dialogo simile alla seguente.  

    ![proteggere i gruppi di amministratori dell'organizzazione](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_61.gif)  
