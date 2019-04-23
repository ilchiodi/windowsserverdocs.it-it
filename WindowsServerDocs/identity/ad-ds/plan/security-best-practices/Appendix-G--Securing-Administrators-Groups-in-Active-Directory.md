---
ms.assetid: 4baefbd3-038f-44c0-85ba-f24e9722b757
title: 'Appendice G: protezione dei gruppi Administrators in Active Directory'
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 2912dfc534d751d4aa121d238dffc36c07562d76
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882712"
---
# <a name="appendix-g-securing-administrators-groups-in-active-directory"></a>Appendice g: Protezione dei gruppi di amministratori in Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-g-securing-administrators-groups-in-active-directory"></a>Appendice g: Protezione dei gruppi di amministratori in Active Directory  
Come avviene con i gruppi Enterprise Admins (EA) e Domain Admins (DA), l'appartenenza al gruppo Administrators (BA) incorporato deve solo in scenari di ripristino di emergenza o di compilazione. Non dovrebbe esserci alcun account di utenti del gruppo Administrators, fatta eccezione per l'account amministratore predefinito per il dominio, se è stato protetto come descritto in [appendice d: Protezione predefiniti Administrator account in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

Gli amministratori sono, per impostazione predefinita, i proprietari della maggior parte degli oggetti di dominio Active Directory nei rispettivi domini. L'appartenenza al gruppo potrebbe essere necessario negli scenari di ripristino di emergenza o compilazione in cui è richiesta la proprietà o la possibilità di assumere la proprietà degli oggetti. Inoltre, DAs ed EAs ereditare un numero di propri diritti e autorizzazioni per l'appartenenza predefinita nel gruppo Administrators. Gruppo predefinito di annidamento per i gruppi con privilegi in Active Directory non deve essere modificato e il gruppo di amministratori di ciascun dominio deve essere protetto come descritto nelle istruzioni dettagliate che seguono.  

Per il gruppo di amministratori in ogni dominio nella foresta:  

1.  Rimuovere tutti i membri dal gruppo di amministratori, con la possibile eccezione dell'account amministratore predefinito per il dominio, purché sia stata impostata come descritto in [appendice d: Protezione predefiniti Administrator account in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

2.  Nel GPO collegati alle unità organizzative che contiene i server membri e workstation in ogni dominio, il gruppo DA deve essere aggiunte ai diritti utente seguenti in **diritti utente di Computer Configurazione computer\Criteri\Impostazioni computer\Impostazioni di Windows\Impostazioni protezione\Criteri Policies\ Assegnazione**:  

    -   Nega accesso al computer dalla rete  

    -   Nega accesso come processo batch  

    -   Nega accesso come servizio  

3.  Sui controller di dominio unità Organizzativa in ogni dominio nella foresta, il gruppo di amministratori deve essere concesso diritti utente seguenti:  

    -   Accedi al computer dalla rete  

    -   Consenti accesso locale  

    -   Consenti accesso tramite Servizi Desktop remoto  

4.  Il controllo deve essere configurato per l'invio di avvisi se vengono apportate modifiche per le proprietà o l'appartenenza del gruppo Administrators.  

#### <a name="step-by-step-instructions-for-removing-all-members-from-the-administrators-group"></a>Istruzioni dettagliate per la rimozione di tutti i membri dal gruppo di amministratori  

1.  Nelle **Server Manager**, fare clic su **Tools**, fare clic su **Active Directory Users and Computers**.  

2.  Per rimuovere tutti i membri dal gruppo Administrators, procedere come segue:  

    1.  Fare doppio clic sui **gli amministratori** di gruppo e fare clic sui **membri** scheda.  

        ![gruppi di amministrazione sicure](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_79.gif)  

    2.  Selezionare un membro del gruppo, fare clic su **rimuovere**, fare clic su **Yes**, fare clic su **OK**.  

3.  Ripetere il passaggio 2 fino a quando non sono stati rimossi tutti i membri del gruppo Administrators.  

#### <a name="step-by-step-instructions-to-secure-administrators-groups-in-active-directory"></a>Istruzioni dettagliate per la protezione di gruppi Administrators in Active Directory  

1.  Nelle **Server Manager**, fare clic su **Tools**, fare clic su **Gestione criteri di gruppo**.  

2.  Nell'albero della console, espandere &lt;foresta&gt;\Domains\\&lt;dominio&gt;e quindi **oggetti Criteri di gruppo** (dove &lt;foresta&gt; è la nome della foresta e &lt;dominio&gt; è il nome del dominio in cui si desidera impostare i criteri di gruppo).  

3.  Nell'albero della console, fare doppio clic su **oggetti Criteri di gruppo**, fare clic su **New**.  

    ![gruppi di amministrazione sicure](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_80.gif)  

4.  Nel **nuovo GPO** finestra di dialogo, digitare <GPO Name>e fare clic su **OK** (dove *nome oggetto Criteri di gruppo* è il nome di questo oggetto Criteri di gruppo).  

    ![gruppi di amministrazione sicure](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_81.gif)  

5.  Nel riquadro dei dettagli, fare doppio clic su **<GPO Name>**, fare clic su **modifica**.  

6.  Passare a **Computer Configurazione computer\Criteri\Impostazioni di Windows\Impostazioni Protezione\criteri**, fare clic su **Assegnazione diritti utente**.  

    ![gruppi di amministrazione sicure](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_82.gif)  

7.  Configurare i diritti utente per impedire che i membri del gruppo di amministratori l'accesso a server membri e workstation in rete eseguendo queste operazioni:  

    1.  Fare doppio clic su **Nega l'accesso al computer dalla rete** e selezionare **Definisci queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

    3.  Tipo di **Administrators**, fare clic su **Controlla nomi**, fare clic su **OK**.  

        ![gruppi di amministrazione sicure](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_83.gif)  

    4.  Fare clic su **OK**, e **OK** nuovamente.  

8.  Configurare i diritti utente per impedire ai membri del gruppo Administrators in grado di accedere come processo batch nel modo seguente:  

    1.  Fare doppio clic su **Nega accesso come processo batch** e selezionare **Definisci queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

    3.  Tipo di **Administrators**, fare clic su **Controlla nomi**, fare clic su **OK**.  

        ![gruppi di amministrazione sicure](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_84.gif)  

    4.  Fare clic su **OK**, e **OK** nuovamente.  

9. Configurare i diritti utente per impedire ai membri del gruppo Administrators in grado di accedere come servizio effettuando le operazioni seguenti:  

    1.  Fare doppio clic su **Nega accesso come servizio** e selezionare **Definisci queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

    3.  Tipo di **Administrators**, fare clic su **Controlla nomi**, fare clic su **OK**.  

        ![gruppi di amministrazione sicure](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_85.gif)  

    4.  Fare clic su **OK**, e **OK** nuovamente.  

10. Per uscire **Editor Gestione criteri di gruppo**, fare clic su **File**, fare clic su **uscire**.  

11. Nelle **Gestione criteri di gruppo**, collegare l'oggetto Criteri di gruppo per il server membro e workstation unità organizzative eseguendo le operazioni seguenti:  

    1.  Passare al &lt;foresta&gt;> \Domains\\&lt;dominio&gt; (dove &lt;foresta&gt; è il nome della foresta e &lt;dominio&gt; corrisponde al nome del dominio in cui si desidera impostare i criteri di gruppo).  

    2.  Fare doppio clic su unità Organizzativa che oggetto Criteri di gruppo verranno applicati a e fare clic su **collegarne un esistente**.  

        ![gruppi di amministrazione sicure](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_86.gif)  

    3.  Selezionare l'oggetto Criteri di gruppo appena creato e fare clic su **OK**.  

        ![gruppi di amministrazione sicure](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_87.gif)  

    4.  Creare collegamenti a tutte le altre unità organizzative che includono le workstation.  

    5.  Creare collegamenti a tutte le altre unità organizzative che includono server membri.  

        > [!IMPORTANT]  
        > Se jump server vengono utilizzati per amministrare i controller di dominio e Active Directory, verificare che jump server si trovano in un'unità Organizzativa a cui non è collegato oggetti Criteri di gruppo.  

        > [!NOTE]  
        > Quando si implementano restrizioni nel gruppo di amministratori in oggetti Criteri di gruppo, Windows applica le impostazioni per i membri del gruppo Administrators locale del computer oltre a gruppo di amministratori del dominio. Pertanto, è necessario prestare attenzione quando si implementa restrizioni nel gruppo Administrators. Sebbene divieto di rete, batch e gli accessi di servizio per i membri del gruppo Administrators è consigliabile ovunque sia possibile implementare, non si limitano gli accessi locali o gli accessi tramite Servizi Desktop remoto. Il blocco di questi tipi di accesso può bloccare l'amministrazione legittimo di un computer da membri del gruppo Administrators locale.  
        >   
        > Lo screenshot seguente mostra le impostazioni di configurazione che bloccano l'utilizzo improprio incorporati locale e amministratore account di dominio, oltre a un utilizzo improprio del gruppo incorporato Administrators del dominio o locale. Si noti che il **Nega accesso tramite Servizi Desktop remoto** diritto utente non include il gruppo Administrators, perché incluso in questa impostazione blocca anche questi accessi per gli account che sono membri del gruppo Administrators del computer locale. Se servizi nei computer configurati per l'esecuzione nel contesto di uno qualsiasi dei gruppi con privilegi descritti in questa sezione, implementare queste impostazioni possono causare alle applicazioni e servizi. Pertanto, come con tutti i consigli forniti in questa sezione, è consigliabile verificare attentamente le impostazioni per l'applicabilità nell'ambiente in uso.  

        ![gruppi di amministrazione sicure](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_88.gif)  

#### <a name="step-by-step-instructions-to-grant-user-rights-to-the-administrators-group"></a>Istruzioni dettagliate per concedere all'utente i diritti al gruppo Administrators  

1.  Nelle **Server Manager**, fare clic su **Tools**, fare clic su **Gestione criteri di gruppo**.  

2.  Nell'albero della console, espandere <Forest>\Domains\\<Domain>, quindi **oggetti Criteri di gruppo** (dove <Forest> è il nome della foresta e <Domain> è il nome del dominio in cui si desidera impostare i criteri di gruppo).  

3.  Nell'albero della console, fare doppio clic su **oggetti Criteri di gruppo**, fare clic su **New**.  

    ![gruppi di amministrazione sicure](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_89.gif)  

4.  Nel **nuovo GPO** finestra di dialogo, digitare <GPO Name>, fare clic su **OK** (in cui <GPO Name> è il nome di questo oggetto Criteri di gruppo).  

    ![gruppi di amministrazione sicure](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_90.gif)  

5.  Nel riquadro dei dettagli, fare doppio clic su **<GPO Name>**, fare clic su **modifica**.  

6.  Passare a **Computer Configurazione computer\Criteri\Impostazioni di Windows\Impostazioni Protezione\criteri**, fare clic su **Assegnazione diritti utente**.  

    ![gruppi di amministrazione sicure](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_91.gif)  

7.  Configurare i diritti utente per consentire ai membri del gruppo Administrators per il controller di dominio di accesso in rete eseguendo queste operazioni:  

    1.  Fare doppio clic su **accesso al computer dalla rete** e selezionare **Definisci queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

    3.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

        ![gruppi di amministrazione sicure](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_92.gif)  

    4.  Fare clic su **OK**, e **OK** nuovamente.  

8.  Configurare i diritti utente per consentire ai membri del gruppo Administrators accedere in locale eseguendo le operazioni seguenti:  

    1.  Fare doppio clic su **Consenti accesso locale** e selezionare **Definisci queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

    3.  Tipo di **gli amministratori**, fare clic sul segno **nomi**, fare clic su **OK**.  

        ![gruppi di amministrazione sicure](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_93.gif)  

    4.  Fare clic su **OK**, e **OK** nuovamente.  

9. Configurare i diritti utente per consentire ai membri del gruppo Administrators di accesso tramite Servizi Desktop remoto eseguendo le operazioni seguenti:  

    1.  Fare doppio clic su **Consenti accesso tramite Servizi Desktop remoto** e selezionare **Definisci queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

    3.  Tipo di **Administrators**, fare clic su **Controlla nomi**, fare clic su **OK**.  

        ![gruppi di amministrazione sicure](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_94.gif)  

    4.  Fare clic su **OK**, e **OK** nuovamente.  

10. Per uscire **Editor Gestione criteri di gruppo**, fare clic su **File**, fare clic su **uscire**.  

11. Nelle **Gestione criteri di gruppo**, collegare l'oggetto Criteri di gruppo ai controller di dominio unità Organizzativa procedendo come segue:  

    1.  Passare il <Forest>\Domains\\ <Domain> (dove <Forest> è il nome della foresta e <Domain> è il nome del dominio in cui si desidera impostare i criteri di gruppo).  

    2.  Fare doppio clic su controller di dominio unità Organizzativa e fare clic su **collegarne un esistente**.  

        ![gruppi di amministrazione sicure](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_95.gif)  

    3.  Selezionare l'oggetto Criteri di gruppo appena creato e fare clic su **OK**.  

        ![gruppi di amministrazione sicure](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_96.gif)  

#### <a name="verification-steps"></a>Passaggi di verifica  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Verificare le impostazioni oggetto Criteri di gruppo "Nega accesso al computer dalla rete"  
Da qualsiasi server membro o una workstation che non è interessato dalle modifiche dell'oggetto Criteri di gruppo (ad esempio, "jump server"), tentare di accedere a un server membro o una workstation in rete che è interessata dalle modifiche dell'oggetto Criteri di gruppo. Per verificare le impostazioni di GPO, provare a eseguire il mapping di unità di sistema usando il **NET USE** comando.  

1.  Accedere in locale usando un account membro del gruppo Administrators.  

2.  Con il mouse, spostare il puntatore del mouse nell'angolo superiore destro o inferiore destro dello schermo. Quando la **accessi** barra viene visualizzata, fare clic su **ricerca**.  

3.  Nel **ricerca** , digitare **prompt dei comandi**, fare doppio clic su **prompt dei comandi**, quindi fare clic su **Esegui come amministratore** aprire con privilegi elevati prompt dei comandi.  

4.  Quando viene richiesto di approvare l'elevazione dei privilegi, fare clic su **Sì**.  

    ![gruppi di amministrazione sicure](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_97.gif)  

5.  Nel **prompt dei comandi** finestra, digitare **net utilizzare \\ \\ \<nome del Server\>\c$**, dove \<nome Server\> è la nome del server membri o workstation che si sta provando ad accedere attraverso la rete.  

6.  Lo screenshot seguente mostra il messaggio di errore che deve essere visualizzato.  

    ![gruppi di amministrazione sicure](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_98.gif)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Verificare le impostazioni oggetto Criteri di gruppo "Nega accesso come processo batch"  
Da qualsiasi server membro o una workstation interessate dalle modifiche dell'oggetto Criteri di gruppo, accedere localmente.  

###### <a name="create-a-batch-file"></a>Creare un File Batch  

1.  Con il mouse, spostare il puntatore del mouse nell'angolo superiore destro o inferiore destro dello schermo. Quando la **accessi** barra viene visualizzata, fare clic su **ricerca**.  

2.  Nel **ricerca** , digitare **blocco note**, fare clic su **Notepad**.  

3.  Nelle **Notepad**, digitare **dir c:**.  

4.  Fare clic su **File**, fare clic su **Salva con nome**.  

5.  Nel **nome File** digitare  **<Filename>bat** (dove <Filename> è il nome del nuovo file batch).  

###### <a name="schedule-a-task"></a>Pianificare un'attività  

1.  Con il mouse, spostare il puntatore del mouse nell'angolo superiore destro o inferiore destro dello schermo. Quando la **accessi** barra viene visualizzata, fare clic su **ricerca**.  

2.  Nel **ricerca** , digitare **utilità di pianificazione**, fare clic su **utilità di pianificazione**.  

    > [!NOTE]  
    > Nei computer che eseguono Windows 8, nella casella di ricerca, digitare le attività di pianificazione e fare clic su Pianifica attività.  

3.  Fare clic su **azione**, fare clic su **Crea attività**.  

4.  Nel **attività di creazione** finestra di dialogo, digitare **<Task Name>** (dove <Task Name> è il nome della nuova attività).  

5.  Scegliere il **azioni** scheda e fare clic su **New**.  

6.  Nel **azione** campi, selezionare **avviare un programma**.  

7.  Nel **programma/script** campo, fare clic su **Sfoglia**individuare e selezionare il file batch creato nel **creare un File Batch** sezione e fare clic su **aprire**.  

8.  Fare clic su **OK**.  

9. Fare clic sulla scheda **Generale**.  

10. Nel **opzioni di sicurezza** campo, fare clic su **Cambia utente o gruppo**.  

11. Digitare il nome di un account membro del gruppo Administrators, fare clic su **Controlla nomi**, fare clic su **OK**.  

12. Selezionare **eseguire se l'utente non è connesso onor** e **non archiviare password**. L'attività sarà solo possibile accedere alle risorse del computer locale.  

13. Fare clic su **OK**.  

14. Verrà visualizzata una finestra di dialogo, account utente che richiede le credenziali per eseguire l'attività.  

15. Dopo aver immesso la password, fare clic su **OK**.  

16. Verrà visualizzata una finestra di dialogo simile alla seguente.  

    ![gruppi di amministrazione sicure](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_99.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Verificare le impostazioni oggetto Criteri di gruppo "Nega accesso come servizio"  

1.  Da qualsiasi server membro o una workstation interessate dalle modifiche dell'oggetto Criteri di gruppo, accedere localmente.  

2.  Con il mouse, spostare il puntatore del mouse nell'angolo superiore destro o inferiore destro dello schermo. Quando la **accessi** barra viene visualizzata, fare clic su **ricerca**.  

3.  Nel **ricerca** , digitare **services**, fare clic su **Services**.  

4.  Individuare e fare doppio clic su **Spooler di stampa**.  

5.  Fare clic sulla scheda **Connessione**.  

6.  Nel **accedere come** campi, selezionare **questo account**.  

7.  Fare clic su **esplorare**, digitare il nome di un account membro del gruppo Administrators, fare clic su **Controlla nomi**, fare clic su **OK**.  

8.  Nel **Password** e **Conferma password** campi, digitare la password dell'account selezionato, quindi scegliere **OK**.  

9. Fare clic su **OK** altre tre volte.  

10. Fare doppio clic su **Spooler di stampa** e fare clic su **riavviare**.  

11. Quando il servizio viene riavviato, verrà visualizzata una finestra di dialogo simile alla seguente.  

    ![gruppi di amministrazione sicure](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_100.png)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>Annullare le modifiche per il servizio Spooler di stampa  

1.  Da qualsiasi server membro o una workstation interessate dalle modifiche dell'oggetto Criteri di gruppo, accedere localmente.  

2.  Con il mouse, spostare il puntatore del mouse nell'angolo superiore destro o inferiore destro dello schermo. Quando la **accessi** barra viene visualizzata, fare clic su **ricerca**.  

3.  Nel **ricerca** , digitare **services**, fare clic su **Services**.  

4.  Individuare e fare doppio clic su **Spooler di stampa**.  

5.  Fare clic sulla scheda **Connessione**.  

6.  Nel **accedere come** campo, fare clic su **LocalSystem** dell'account e fare clic su **OK**.  
