---
ms.assetid: 4baefbd3-038f-44c0-85ba-f24e9722b757
title: Appendice G-protezione dei gruppi di amministratori in Active Directory
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: f113dc7fc5b131a2c0ef10433125ef12a775707c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80821524"
---
# <a name="appendix-g-securing-administrators-groups-in-active-directory"></a>Appendice G: Protezione dei gruppi Administrators in Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-g-securing-administrators-groups-in-active-directory"></a>Appendice G: Protezione dei gruppi Administrators in Active Directory  
Come nel caso dei gruppi Enterprise Admins (EA) e Domain Admins (DA), l'appartenenza al gruppo Administrators predefinito (BA) dovrebbe essere obbligatoria solo negli scenari di compilazione o di ripristino di emergenza. Non devono essere presenti account utente giornalieri nel gruppo Administrators, ad eccezione dell'account Administrator predefinito per il dominio, se è stato protetto come descritto in [Appendice D: protezione degli account amministratore predefiniti in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

Gli amministratori sono, per impostazione predefinita, i proprietari della maggior parte degli oggetti di dominio Active Directory nei rispettivi domini. L'appartenenza a questo gruppo può essere obbligatoria negli scenari di compilazione o ripristino di emergenza in cui è richiesta la proprietà o la possibilità di assumere la proprietà degli oggetti. Inoltre, DAs ed EAs ereditare un numero di propri diritti e autorizzazioni per l'appartenenza predefinita nel gruppo Administrators. La nidificazione di gruppi predefinita per i gruppi con privilegi in Active Directory non deve essere modificata e il gruppo Administrators di ogni dominio deve essere protetto come descritto nelle istruzioni dettagliate seguenti.  

Per il gruppo Administrators in ogni dominio nella foresta:  

1.  Rimuovere tutti i membri dal gruppo Administrators, con la possibile eccezione dell'account Administrator predefinito per il dominio, purché sia stato protetto come descritto in [Appendice D: protezione degli account amministratore predefiniti in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

2.  Negli oggetti Criteri di gruppo collegati alle unità organizzative che contengono server membri e workstation in ogni dominio, il gruppo BA deve essere aggiunto ai seguenti diritti utente in **computer Computer\criteri\impostazioni Windows\Impostazioni protezione\Criteri Policies \ User Rights Assignment**:  

    -   Nega accesso al computer dalla rete  

    -   Nega accesso come processo batch  

    -   Nega accesso come servizio  

3.  Nell'unità organizzativa Domain Controllers in ogni dominio della foresta, al gruppo Administrators devono essere concessi i diritti utente seguenti:  

    -   Accedi al computer dalla rete  

    -   Consenti accesso locale  

    -   Consenti accesso tramite Servizi Desktop remoto  

4.  Il controllo deve essere configurato per l'invio di avvisi se vengono apportate modifiche per le proprietà o l'appartenenza del gruppo Administrators.  

#### <a name="step-by-step-instructions-for-removing-all-members-from-the-administrators-group"></a>Istruzioni dettagliate per la rimozione di tutti i membri dal gruppo Administrators  

1.  In **Server Manager**fare clic su **strumenti**e quindi su **Active Directory utenti e computer**.  

2.  Per rimuovere tutti i membri dal gruppo Administrators, seguire questa procedura:  

    1.  Fare doppio clic sul gruppo **Administrators** e scegliere la scheda **membri** .  

        ![proteggere i gruppi di amministratori](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_79.gif)  

    2.  Selezionare un membro del gruppo, fare clic su **Rimuovi**, fare clic su **Sì**e quindi su **OK**.  

3.  Ripetere il passaggio 2 fino a quando non sono stati rimossi tutti i membri del gruppo Administrators.  

#### <a name="step-by-step-instructions-to-secure-administrators-groups-in-active-directory"></a>Istruzioni dettagliate per la protezione dei gruppi di amministratori in Active Directory  

1.  In **Server Manager**fare clic su **strumenti**e quindi su **Gestione criteri di gruppo**.  

2.  Nell'albero della console espandere &lt;foresta&gt;\Domains\\&lt;dominio&gt;, quindi **criteri di gruppo oggetti** (dove &lt;foresta&gt; è il nome della foresta e &lt;dominio&gt; è il nome del dominio in cui si desidera impostare il criteri di gruppo).  

3.  Nell'albero della console fare clic con il pulsante destro del mouse su **criteri di gruppo oggetti**, quindi scegliere **nuovo**.  

    ![proteggere i gruppi di amministratori](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_80.gif)  

4.  Nella finestra di dialogo **nuovo oggetto Criteri** di gruppo digitare <GPO Name>, quindi fare clic su **OK** (dove *nome oggetto Criteri* di gruppo è il nome dell'oggetto Criteri di gruppo).  

    ![proteggere i gruppi di amministratori](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_81.gif)  

5.  Nel riquadro dei dettagli fare clic con il pulsante destro del mouse su **<GPO Name>** , quindi scegliere **modifica**.  

6.  Passare a **computer Computer\criteri\impostazioni Windows\Impostazioni protezione\Criteri criteri**e fare clic su **assegnazione diritti utente**.  

    ![proteggere i gruppi di amministratori](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_82.gif)  

7.  Configurare i diritti utente per impedire ai membri del gruppo Administrators di accedere ai server e alle workstation membri tramite la rete effettuando le operazioni seguenti:  

    1.  Fare doppio clic su **Nega accesso al computer dalla rete** e selezionare **Definisci le impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

    3.  Digitare **Administrators**, fare clic su **Controlla nomi**, quindi fare clic su **OK**.  

        ![proteggere i gruppi di amministratori](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_83.gif)  

    4.  Fare clic su **OK**e di nuovo su **OK** .  

8.  Configurare i diritti utente per impedire ai membri del gruppo Administrators di accedere come processo batch eseguendo le operazioni seguenti:  

    1.  Fare doppio clic su **Nega accesso come processo batch** e selezionare **Definisci queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

    3.  Digitare **Administrators**, fare clic su **Controlla nomi**, quindi fare clic su **OK**.  

        ![proteggere i gruppi di amministratori](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_84.gif)  

    4.  Fare clic su **OK**e di nuovo su **OK** .  

9. Configurare i diritti utente per impedire ai membri del gruppo Administrators di accedere come servizio effettuando le operazioni seguenti:  

    1.  Fare doppio clic su **Nega accesso come servizio** e selezionare **Definisci queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

    3.  Digitare **Administrators**, fare clic su **Controlla nomi**, quindi fare clic su **OK**.  

        ![proteggere i gruppi di amministratori](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_85.gif)  

    4.  Fare clic su **OK**e di nuovo su **OK** .  

10. Per uscire da **Editor gestione criteri di gruppo**, fare clic su **file**e quindi su **Esci**.  

11. In **gestione criteri di gruppo**collegare l'oggetto Criteri di gruppo al server membro e alle unità organizzative della workstation effettuando le operazioni seguenti:  

    1.  Passare alla foresta &lt;&gt;> \Domains\\&lt;dominio&gt; (dove &lt;foresta&gt; è il nome della foresta e &lt;dominio&gt; è il nome del dominio in cui si desidera impostare il Criteri di gruppo).  

    2.  Fare clic con il pulsante destro del mouse sull'unità organizzativa a cui verrà applicato l'oggetto Criteri di gruppo e scegliere **collega un oggetto Criteri**di gruppo  

        ![proteggere i gruppi di amministratori](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_86.gif)  

    3.  Selezionare l'oggetto Criteri di gruppo appena creato e fare clic su **OK**.  

        ![proteggere i gruppi di amministratori](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_87.gif)  

    4.  Creare collegamenti a tutte le altre ou che contengono workstation.  

    5.  Creare collegamenti a tutte le altre ou che contengono server membro.  

        > [!IMPORTANT]  
        > Se i server di salto vengono utilizzati per amministrare i controller di dominio e Active Directory, assicurarsi che i server Jump si trovino in un'unità organizzativa a cui questi oggetti Criteri di gruppo non sono collegati.  

        > [!NOTE]  
        > Quando si implementano restrizioni nel gruppo di amministratori in oggetti Criteri di gruppo, Windows applica le impostazioni per i membri del gruppo Administrators locale del computer oltre a gruppo di amministratori del dominio. Pertanto, è necessario prestare attenzione quando si implementano le restrizioni nel gruppo Administrators. Sebbene il divieto di accesso alla rete, al batch e al servizio per i membri del gruppo Administrators venga consigliato ovunque sia possibile implementare, non limitare gli accessi locali o gli accessi tramite Servizi Desktop remoto. Il blocco di questi tipi di accesso può bloccare l'amministrazione legittimo di un computer da membri del gruppo Administrators locale.  
        >   
        > Lo screenshot seguente mostra le impostazioni di configurazione che bloccano l'utilizzo improprio degli account predefiniti di amministratore locale e di dominio, oltre a un utilizzo improprio dei gruppi di amministratori locali o di dominio predefiniti. Si noti che il **Nega accesso tramite Servizi Desktop remoto** diritto utente non include il gruppo Administrators, perché incluso in questa impostazione blocca anche questi accessi per gli account che sono membri del gruppo Administrators del computer locale. Se i servizi nei computer sono configurati per essere eseguiti nel contesto di uno dei gruppi privilegiati descritti in questa sezione, l'implementazione di queste impostazioni può causare errori nei servizi e nelle applicazioni. Pertanto, come con tutti i consigli forniti in questa sezione, è consigliabile verificare attentamente le impostazioni per l'applicabilità nell'ambiente in uso.  

        ![proteggere i gruppi di amministratori](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_88.gif)  

#### <a name="step-by-step-instructions-to-grant-user-rights-to-the-administrators-group"></a>Istruzioni dettagliate per concedere diritti utente al gruppo Administrators  

1.  In **Server Manager**fare clic su **strumenti**e quindi su **Gestione criteri di gruppo**.  

2.  Nell'albero della console espandere <Forest>\Domains\\<Domain>, quindi **criteri di gruppo oggetti** (dove <Forest> è il nome della foresta e <Domain> è il nome del dominio in cui si desidera impostare il criteri di gruppo).  

3.  Nell'albero della console fare clic con il pulsante destro del mouse su **criteri di gruppo oggetti**, quindi scegliere **nuovo**.  

    ![proteggere i gruppi di amministratori](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_89.gif)  

4.  Nella finestra di dialogo **nuovo oggetto Criteri** di gruppo digitare <GPO Name>e fare clic su **OK** (dove <GPO Name> è il nome di questo GPO).  

    ![proteggere i gruppi di amministratori](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_90.gif)  

5.  Nel riquadro dei dettagli fare clic con il pulsante destro del mouse su **<GPO Name>** , quindi scegliere **modifica**.  

6.  Passare a **computer Computer\criteri\impostazioni Windows\Impostazioni protezione\Criteri criteri**e fare clic su **assegnazione diritti utente**.  

    ![proteggere i gruppi di amministratori](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_91.gif)  

7.  Configurare i diritti utente per consentire ai membri del gruppo Administrators di accedere ai controller di dominio tramite la rete effettuando le operazioni seguenti:  

    1.  Fare doppio clic su **accesso a questo computer dalla rete** e selezionare **Definisci queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

    3.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

        ![proteggere i gruppi di amministratori](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_92.gif)  

    4.  Fare clic su **OK**e di nuovo su **OK** .  

8.  Configurare i diritti utente per consentire ai membri del gruppo Administrators di accedere localmente eseguendo le operazioni seguenti:  

    1.  Fare doppio clic su **Consenti accesso locale** e selezionare **Definisci queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

    3.  Digitare **Administrators**, fare clic su Controlla **nomi**, quindi fare clic su **OK**.  

        ![proteggere i gruppi di amministratori](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_93.gif)  

    4.  Fare clic su **OK**e di nuovo su **OK** .  

9. Configurare i diritti utente per consentire ai membri del gruppo Administrators di accedere tramite Servizi Desktop remoto eseguendo le operazioni seguenti:  

    1.  Fare doppio clic su **Consenti accesso tramite Servizi Desktop remoto** e selezionare **Definisci queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

    3.  Digitare **Administrators**, fare clic su **Controlla nomi**, quindi fare clic su **OK**.  

        ![proteggere i gruppi di amministratori](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_94.gif)  

    4.  Fare clic su **OK**e di nuovo su **OK** .  

10. Per uscire da **Editor gestione criteri di gruppo**, fare clic su **file**e quindi su **Esci**.  

11. In **gestione criteri di gruppo**collegare l'oggetto Criteri di gruppo all'unità organizzativa controller di dominio effettuando le operazioni seguenti:  

    1.  Passare al <Forest>\Domains\\<Domain> (dove <Forest> è il nome della foresta e <Domain> è il nome del dominio in cui si desidera impostare il Criteri di gruppo).  

    2.  Fare clic con il pulsante destro del mouse sull'unità organizzativa Domain Controllers e scegliere **collega un oggetto Criteri**di gruppo  

        ![proteggere i gruppi di amministratori](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_95.gif)  

    3.  Selezionare l'oggetto Criteri di gruppo appena creato e fare clic su **OK**.  

        ![proteggere i gruppi di amministratori](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_96.gif)  

#### <a name="verification-steps"></a>Passaggi di verifica  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Verificare le impostazioni dell'oggetto Criteri di gruppo "Nega accesso a questo computer dalla rete"  
Da qualsiasi server membro o workstation che non è influenzato dall'oggetto Criteri di gruppo, ad esempio un "Jump server", tenta di accedere a un server membro o a una workstation sulla rete interessata dall'oggetto Criteri di gruppo modificato. Per verificare le impostazioni dell'oggetto Criteri di gruppo, provare a eseguire il mapping dell'unità di sistema utilizzando il comando **net use** .  

1.  Accedere localmente utilizzando un account membro del gruppo Administrators.  

2.  Con il mouse, spostare il puntatore nell'angolo superiore destro o inferiore destro dello schermo. Quando viene visualizzata la barra degli **accessi** , fare clic su **Cerca**.  

3.  Nella casella di **ricerca** Digitare **prompt**dei comandi, fare clic con il pulsante destro del mouse su **prompt dei comandi**e quindi scegliere **Esegui come amministratore** per aprire un prompt dei comandi con privilegi elevati.  

4.  Quando viene richiesto di approvare l'elevazione, fare clic su **Sì**.  

    ![proteggere i gruppi di amministratori](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_97.gif)  

5.  Nella finestra del **prompt dei comandi** Digitare **net use \\\\nome server \<\>\c $** , dove \<nome server\> è il nome del server membro o della workstation a cui si sta tentando di accedere attraverso la rete.  

6.  Lo screenshot seguente mostra il messaggio di errore che dovrebbe essere visualizzato.  

    ![proteggere i gruppi di amministratori](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_98.gif)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Verificare le impostazioni dell'oggetto Criteri di gruppo "Nega accesso come processo batch"  
Accedere localmente da qualsiasi server membro o workstation interessato dall'oggetto Criteri di gruppo.  

###### <a name="create-a-batch-file"></a>Creazione di un file batch  

1.  Con il mouse, spostare il puntatore nell'angolo superiore destro o inferiore destro dello schermo. Quando viene visualizzata la barra degli **accessi** , fare clic su **Cerca**.  

2.  Nella casella di **ricerca** Digitare **blocco note**e fare clic su **blocco note**.  

3.  In **blocco note**Digitare **dir c:** .  

4.  Fare clic su **file**e quindi su **Salva con nome**.  

5.  Nel campo **nome file** Digitare **<Filename>. bat** , dove <Filename> è il nome del nuovo file batch.  

###### <a name="schedule-a-task"></a>Pianificare un'attività  

1.  Con il mouse, spostare il puntatore nell'angolo superiore destro o inferiore destro dello schermo. Quando viene visualizzata la barra degli **accessi** , fare clic su **Cerca**.  

2.  Nella casella di **ricerca** Digitare **utilità di pianificazione**e fare clic su **utilità di pianificazione**.  

    > [!NOTE]  
    > Nei computer che eseguono Windows 8, nella casella di ricerca digitare Pianifica attività, quindi fare clic su Pianifica attività.  

3.  Fare clic su **azione**e quindi su **Crea attività**.  

4.  Nella finestra di dialogo **Crea attività** Digitare **<Task Name>** (dove <Task Name> è il nome della nuova attività).  

5.  Fare clic sulla scheda **azioni** e quindi su **nuovo**.  

6.  Nel campo **azione** selezionare **avvia un programma**.  

7.  Nel campo **programma/script** fare clic su **Sfoglia**, individuare e selezionare il file batch creato nella sezione **creare un file batch** e fare clic su **Apri**.  

8.  Fare clic su **OK**.  

9. Fare clic sulla scheda **Generale**.  

10. Nel campo **Opzioni di sicurezza** fare clic su **modifica utente o gruppo**.  

11. Digitare il nome di un account membro del gruppo Administrators, fare clic su **Controlla nomi**e quindi su **OK**.  

12. Selezionare **Esegui se l'utente è registrato onor not** e non **archiviare la password**. L'attività avrà accesso solo alle risorse del computer locale.  

13. Fare clic su **OK**.  

14. Verrà visualizzata una finestra di dialogo in cui vengono richieste le credenziali dell'account utente per l'esecuzione dell'attività.  

15. Dopo aver immesso la password, fare clic su **OK**.  

16. Verrà visualizzata una finestra di dialogo simile alla seguente.  

    ![proteggere i gruppi di amministratori](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_99.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Verificare le impostazioni dell'oggetto Criteri di gruppo "Nega accesso come servizio"  

1.  Accedere localmente da qualsiasi server membro o workstation interessato dall'oggetto Criteri di gruppo.  

2.  Con il mouse, spostare il puntatore nell'angolo superiore destro o inferiore destro dello schermo. Quando viene visualizzata la barra degli **accessi** , fare clic su **Cerca**.  

3.  Nella casella di **ricerca** Digitare **Services**, quindi fare clic su **Servizi**.  

4.  Individuare e fare doppio clic su **spooler di stampa**.  

5.  Fare clic sulla scheda **Connessione**.  

6.  Nel campo **Accedi come** selezionare **l'account**.  

7.  Fare clic su **Sfoglia**, digitare il nome di un account membro del gruppo Administrators, fare clic su **Controlla nomi**e quindi su **OK**.  

8.  Nei campi **password** e **Conferma password** digitare la password dell'account selezionato, quindi fare clic su **OK**.  

9. Fare clic su **OK** altre tre volte.  

10. Fare clic con il pulsante destro del mouse su **spooler di stampa** e scegliere **Riavvia**.  

11. Quando il servizio viene riavviato, viene visualizzata una finestra di dialogo simile alla seguente.  

    ![proteggere i gruppi di amministratori](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_100.png)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>Ripristinare le modifiche al servizio spooler di stampa  

1.  Accedere localmente da qualsiasi server membro o workstation interessato dall'oggetto Criteri di gruppo.  

2.  Con il mouse, spostare il puntatore nell'angolo superiore destro o inferiore destro dello schermo. Quando viene visualizzata la barra degli **accessi** , fare clic su **Cerca**.  

3.  Nella casella di **ricerca** Digitare **Services**, quindi fare clic su **Servizi**.  

4.  Individuare e fare doppio clic su **spooler di stampa**.  

5.  Fare clic sulla scheda **Connessione**.  

6.  Nel campo **Accedi come** fare clic su account di **sistema locale** e fare clic su **OK**.  
