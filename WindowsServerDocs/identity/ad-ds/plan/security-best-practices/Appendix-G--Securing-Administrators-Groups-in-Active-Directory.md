---
ms.assetid: 4baefbd3-038f-44c0-85ba-f24e9722b757
title: Appendice C - protezione dei gruppi Administrators in Active Directory
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4cf89f7bf31ce848de384778b0213d0ddc822158
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="appendix-g-securing-administrators-groups-in-active-directory"></a>Appendice g: protezione dei gruppi Administrators in Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-g-securing-administrators-groups-in-active-directory"></a>Appendice g: protezione dei gruppi Administrators in Active Directory  
Come avviene con i gruppi Enterprise Admins (EA) e Domain Admins (DA), l'appartenenza al gruppo incorporato Administrators (BA) dovrebbe essere necessaria solo in scenari di ripristino di emergenza o di compilazione. Non dovrebbe esserci alcun account di utenti del gruppo Administrators, fatta eccezione per l'account amministratore predefinito per il dominio, se è stato protetto come descritto in [appendice d: protezione predefiniti Administrator account in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

Gli amministratori sono, per impostazione predefinita, i proprietari della maggior parte degli oggetti di dominio Active Directory nei rispettivi domini. L'appartenenza a questo gruppo potrebbe essere necessario negli scenari di ripristino di compilazione o emergenza in cui è necessaria la proprietà o la possibilità di assumere la proprietà di oggetti. Inoltre, DAs ed EAs ereditare un numero di propri diritti e autorizzazioni per l'appartenenza predefinita del gruppo Administrators. Gruppo predefinito, la nidificazione di gruppi con privilegi in Active Directory non deve essere modificato e gruppo di amministratori di ciascun dominio deve essere protetto come descritto nelle istruzioni dettagliate che seguono.  

Per il gruppo Administrators in ogni dominio nella foresta:  

1.  Rimuovere tutti i membri dal gruppo Administrators, con la possibile eccezione dell'account amministratore predefinito per il dominio, purché sia stata impostata come descritto in [appendice d: protezione predefiniti Administrator account in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

2.  Oggetti Criteri di gruppo collegati alle unità organizzative che contiene i server membri e workstation in ogni dominio, il gruppo DA deve essere aggiunto ai diritti utente seguenti in **Computer Configurazione computer\Criteri\Impostazioni computer\Impostazioni di Windows\Impostazioni protezione\Criteri Policies\ assegnazione dei diritti utente**:  

    -   Nega accesso al computer dalla rete  

    -   Nega accesso come processo batch  

    -   Nega accesso come servizio  

3.  Al controller di dominio, unità Organizzativa in ogni dominio nella foresta, il gruppo di amministratori deve essere concesso diritti utente seguenti:  

    -   Accesso al computer dalla rete  

    -   Consenti accesso locale  

    -   Consenti accesso tramite Servizi Desktop remoto  

4.  Il controllo deve essere configurato per l'invio di avvisi se vengono apportate modifiche per le proprietà o l'appartenenza del gruppo Administrators.  

#### <a name="step-by-step-instructions-for-removing-all-members-from-the-administrators-group"></a>Istruzioni dettagliate per la rimozione di tutti i membri dal gruppo Administrators  

1.  In **Server Manager**, fare clic su **strumenti**e fare clic su **Active Directory Users and Computers**.  

2.  Per rimuovere tutti i membri dal gruppo Administrators, eseguire i passaggi seguenti:  

    1.  Fare doppio clic il **amministratori** di gruppo e fare clic sul **membri** scheda.  

        ![gruppi di amministratori sicuro](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_79.gif)  

    2.  Selezionare un membro del gruppo, fare clic su **rimuovere**, fare clic su **Sì**e fare clic su **OK**.  

3.  Ripetere il passaggio 2 fino a quando non sono stati rimossi tutti i membri del gruppo Administrators.  

#### <a name="step-by-step-instructions-to-secure-administrators-groups-in-active-directory"></a>Istruzioni dettagliate per la protezione dei gruppi Administrators in Active Directory  

1.  In **Server Manager**, fare clic su **strumenti**e fare clic su **Gestione criteri di gruppo**.  

2.  Nell'albero della console, espandere <Forest>\Domains\\<Domain>e quindi **oggetti Criteri di gruppo** (in cui <Forest> è il nome dell'insieme di strutture e <Domain> è il nome del dominio in cui si desidera impostare i criteri di gruppo).  

3.  Nell'albero della console fare doppio clic su **oggetti Criteri di gruppo**e fare clic su **New**.  

    ![gruppi di amministratori sicuro](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_80.gif)  

4.  Nel **nuovo oggetto Criteri di gruppo** la finestra di dialogo, digitare <GPO Name>e fare clic su **OK** (in cui *nome oggetto Criteri di gruppo* è il nome di questo oggetto Criteri di gruppo).  

    ![gruppi di amministratori sicuro](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_81.gif)  

5.  Nel riquadro dei dettagli fare doppio clic su ** <GPO Name> **e fare clic su **modifica**.  

6.  Passare a **Computer Configurazione computer\Criteri\Impostazioni di Windows\Impostazioni Protezione\criteri**e fare clic su **Assegnazione diritti utente**.  

    ![gruppi di amministratori sicuro](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_82.gif)  

7.  Configurare i diritti utente per impedire ai membri del gruppo Administrators di accedere ai server membri e workstation sulla rete eseguendo le operazioni seguenti:  

    1.  Fare doppio clic su **negare l'accesso al computer dalla rete** e seleziona **definire queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

    3.  Tipo **amministratori**, fare clic su **Controlla nomi**e fare clic su **OK**.  

        ![gruppi di amministratori sicuro](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_83.gif)  

    4.  Fare clic su **OK**, e **OK** nuovamente.  

8.  Configurare i diritti utente per impedire ai membri del gruppo Administrators di accesso come processo batch effettuando le operazioni seguenti:  

    1.  Fare doppio clic su **Nega accesso come processo batch** e seleziona **definire queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

    3.  Tipo **amministratori**, fare clic su **Controlla nomi**e fare clic su **OK**.  

        ![gruppi di amministratori sicuro](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_84.gif)  

    4.  Fare clic su **OK**, e **OK** nuovamente.  

9. Configurare i diritti utente per impedire ai membri del gruppo Administrators di accesso come servizio effettuando le operazioni seguenti:  

    1.  Fare doppio clic su **Nega accesso come servizio** e seleziona **definire queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

    3.  Tipo **amministratori**, fare clic su **Controlla nomi**e fare clic su **OK**.  

        ![gruppi di amministratori sicuro](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_85.gif)  

    4.  Fare clic su **OK**, e **OK** nuovamente.  

10. Per uscire da **Editor Gestione criteri di gruppo**, fare clic su **File**e fare clic su **uscire**.  

11. In **Gestione criteri di gruppo**, collegare l'oggetto Criteri di gruppo per il server membro e workstation unità organizzative effettuando le operazioni seguenti:  

    1.  Passare al <Forest>\Domains\\<Domain> (in cui <Forest> è il nome dell'insieme di strutture e <Domain> è il nome del dominio in cui si desidera impostare i criteri di gruppo).  

    2.  Fare doppio clic su oggetto Criteri di gruppo verranno applicate a e fare clic su, l'unità Organizzativa **collegare un oggetto Criteri di gruppo esistente**.  

        ![gruppi di amministratori sicuro](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_86.gif)  

    3.  Selezionare l'oggetto Criteri di gruppo appena creato e fare clic su **OK**.  

        ![gruppi di amministratori sicuro](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_87.gif)  

    4.  Creare collegamenti a tutte le altre unità organizzative contenenti le workstation.  

    5.  Creare collegamenti a tutte le altre unità organizzative contenenti i server membri.  

        > [!IMPORTANT]  
        > Se jump server vengono utilizzati per amministrare i controller di dominio e Active Directory, verificare che jump server si trovano in un'unità Organizzativa a cui non è ancora collegato oggetti Criteri di gruppo.  

        > [!NOTE]  
        > Quando si implementano restrizioni nel gruppo di amministratori in oggetti Criteri di gruppo, Windows applica le impostazioni per i membri del gruppo Administrators locale del computer oltre a gruppo di amministratori del dominio. Pertanto, è necessario prestare attenzione quando si implementa restrizioni nel gruppo Administrators. Sebbene divieto di rete, batch e gli accessi al servizio per i membri del gruppo Administrators è consigliabile nel punto in cui è possibile implementare, non si limitano gli accessi locali o gli accessi tramite Servizi Desktop remoto. Il blocco di questi tipi di accesso può bloccare l'amministrazione legittimo di un computer da membri del gruppo Administrators locale.  
        >   
        > Lo screenshot seguente mostra le impostazioni di configurazione che bloccano l'utilizzo improprio incorporati locale e amministratore account di dominio, oltre a uso improprio di gruppi di amministratori locale o di dominio predefiniti. Si noti che il **Nega accesso tramite Servizi Desktop remoto** diritto utente non include il gruppo Administrators, perché incluso in questa impostazione blocca anche questi accessi per gli account che sono membri del gruppo Administrators del computer locale. Se servizi nel computer sono configurati per l'esecuzione nel contesto di uno qualsiasi dei gruppi con privilegi descritti in questa sezione, implementare queste impostazioni può causare servizi e delle applicazioni. Pertanto, come con tutti i consigli forniti in questa sezione, è necessario testarne le impostazioni per l'applicabilità nell'ambiente in uso.  

        ![gruppi di amministratori sicuro](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_88.gif)  

#### <a name="step-by-step-instructions-to-grant-user-rights-to-the-administrators-group"></a>Istruzioni dettagliate per concedere diritti al gruppo Administrators  

1.  In **Server Manager**, fare clic su **strumenti**e fare clic su **Gestione criteri di gruppo**.  

2.  Nell'albero della console, espandere <Forest>\Domains\\<Domain>e quindi **oggetti Criteri di gruppo** (in cui <Forest> è il nome dell'insieme di strutture e <Domain> è il nome del dominio in cui si desidera impostare i criteri di gruppo).  

3.  Nell'albero della console fare doppio clic su **oggetti Criteri di gruppo**e fare clic su **New**.  

    ![gruppi di amministratori sicuro](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_89.gif)  

4.  Nel **nuovo oggetto Criteri di gruppo** la finestra di dialogo, digitare <GPO Name>e fare clic su **OK** (in cui <GPO Name> è il nome di questo oggetto Criteri di gruppo).  

    ![gruppi di amministratori sicuro](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_90.gif)  

5.  Nel riquadro dei dettagli fare doppio clic su ** <GPO Name> **e fare clic su **modifica**.  

6.  Passare a **Computer Configurazione computer\Criteri\Impostazioni di Windows\Impostazioni Protezione\criteri**e fare clic su **Assegnazione diritti utente**.  

    ![gruppi di amministratori sicuro](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_91.gif)  

7.  Configurare i diritti utente per consentire ai membri del gruppo Administrators per i controller di dominio di accesso in rete effettuando le operazioni seguenti:  

    1.  Fare doppio clic su **accesso al computer dalla rete** e seleziona **definire queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

    3.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

        ![gruppi di amministratori sicuro](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_92.gif)  

    4.  Fare clic su **OK**, e **OK** nuovamente.  

8.  Configurare i diritti utente per consentire ai membri del gruppo Administrators per l'accesso in locale effettuando le operazioni seguenti:  

    1.  Fare doppio clic su **Consenti accesso locale** e seleziona **definire queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

    3.  Tipo **amministratori**, fai clic su verifica **nomi**e fare clic su **OK**.  

        ![gruppi di amministratori sicuro](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_93.gif)  

    4.  Fare clic su **OK**, e **OK** nuovamente.  

9. Configurare i diritti utente per consentire ai membri del gruppo Administrators per l'accesso tramite Servizi Desktop remoto eseguendo le operazioni seguenti:  

    1.  Fare doppio clic su **Consenti accesso tramite Servizi Desktop remoto** e seleziona **definire queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

    3.  Tipo **amministratori**, fare clic su **Controlla nomi**e fare clic su **OK**.  

        ![gruppi di amministratori sicuro](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_94.gif)  

    4.  Fare clic su **OK**, e **OK** nuovamente.  

10. Per uscire da **Editor Gestione criteri di gruppo**, fare clic su **File**e fare clic su **uscire**.  

11. In **Gestione criteri di gruppo**, collegare l'oggetto Criteri di gruppo ai controller di dominio, unità Organizzativa effettuando le operazioni seguenti:  

    1.  Passare al <Forest>\Domains\\<Domain> (in cui <Forest> è il nome dell'insieme di strutture e <Domain> è il nome del dominio in cui si desidera impostare i criteri di gruppo).  

    2.  Fare doppio clic su controller di dominio, unità Organizzativa e fare clic su **collegare un oggetto Criteri di gruppo esistente**.  

        ![gruppi di amministratori sicuro](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_95.gif)  

    3.  Selezionare l'oggetto Criteri di gruppo appena creato e fare clic su **OK**.  

        ![gruppi di amministratori sicuro](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_96.gif)  

#### <a name="verification-steps"></a>Passaggi di verifica  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Verificare le impostazioni oggetto Criteri di gruppo "Nega accesso al computer dalla rete"  
Da qualsiasi server membro o una workstation in cui non è interessata dall'oggetto Criteri di gruppo viene modificato (ad esempio, "jump server"), tentare di accedere a un server membro o una workstation in rete che è interessata dall'oggetto Criteri di gruppo viene modificato. Per verificare le impostazioni oggetto Criteri di gruppo, tentare di eseguire il mapping di unità di sistema utilizzando il **NET USE** comando.  

1.  Accedere localmente utilizzando un account che sia membro del gruppo Administrators.  

2.  Con il mouse, sposta il puntatore nell'angolo superiore destro o in basso a destra dello schermo. Quando il **accessi** barra viene visualizzata, fare clic su **ricerca**.  

3.  Nel **ricerca** digitare **prompt dei comandi**, fare doppio clic su **prompt dei comandi**, quindi fare clic su **Esegui come amministratore** per aprire un prompt dei comandi con privilegi elevati.  

4.  Quando viene richiesto di approvare l'elevazione dei privilegi, fare clic su **Sì**.  

    ![gruppi di amministratori sicuro](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_97.gif)  

5.  Nel **prompt dei comandi** finestra, digitare **net uso \ \ \<Server Name>\c$**, dove <Server Name> è il nome del server membri o workstation che si sta tentando di accedere attraverso la rete.  

6.  Lo screenshot seguente mostra il messaggio di errore che dovrebbe essere visualizzato.  

    ![gruppi di amministratori sicuro](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_98.gif)  

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

2.  Nel **ricerca** digitare **utilità di pianificazione**e fare clic su **utilità di pianificazione**.  

    > [!NOTE]  
    > Nei computer che eseguono Windows 8, nella casella di ricerca, digitare le attività di pianificazione e fare clic su attività di pianificazione.  

3.  Fare clic su **azione**e fare clic su **Crea attività**.  

4.  Nel **Crea attività** la finestra di dialogo, digitare ** <Task Name> ** (in cui <Task Name> è il nome della nuova attività).  

5.  Fare clic su di **azioni** scheda e fare clic su **New**.  

6.  Nel **azione** selezionare **avviare un programma**.  

7.  Nel **programma o script** campo, fare clic su **Sfoglia**, individuare e selezionare il file batch creato nel **creare un File Batch** sezione e fare clic su **aprire**.  

8.  Fare clic su **OK**.  

9. Fare clic su di **generale** scheda.  

10. Nel **opzioni di protezione** campo, fare clic su **Cambia utente o gruppo**.  

11. Digitare il nome di un account che sia membro del gruppo Administrators, fare clic su **Controlla nomi**e fare clic su **OK**.  

12. Selezionare **eseguire se l'utente non è connesso onor** e **non archiviare password**. L'attività sarà solo possibile accedere alle risorse del computer locale.  

13. Fare clic su **OK**.  

14. Dovrebbe essere visualizzata una finestra di dialogo, account utente che richiede le credenziali necessarie per eseguire l'attività.  

15. Dopo aver immesso la password, fare clic su **OK**.  

16. Dovrebbe essere visualizzata una finestra di dialogo simile al seguente.  

    ![gruppi di amministratori sicuro](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_99.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Verificare le impostazioni oggetto Criteri di gruppo "Nega accesso come servizio"  

1.  Da qualsiasi server membro o una workstation interessati dalle modifiche dell'oggetto Criteri di gruppo, accedere localmente.  

2.  Con il mouse, sposta il puntatore nell'angolo superiore destro o in basso a destra dello schermo. Quando il **accessi** barra viene visualizzata, fare clic su **ricerca**.  

3.  Nel **ricerca** digitare **servizi**e fare clic su **servizi**.  

4.  Individuare e fare doppio clic su **Spooler di stampa**.  

5.  Fare clic su di **accesso** scheda.  

6.  Nel **accedere come** selezionare **questo account**.  

7.  Fare clic su **Sfoglia**, digitare il nome di un account che sia membro del gruppo Administrators, fare clic su **Controlla nomi**e fare clic su **OK**.  

8.  Nel **Password** e **Conferma password** campi, digitare la password dell'account selezionato e scegliere **OK**.  

9. Fare clic su **OK** tre volte.  

10. Fare doppio clic su **Spooler di stampa** e fare clic su **riavviare**.  

11. Quando viene riavviato il servizio, verrà visualizzata una finestra di dialogo simile al seguente.  

    ![gruppi di amministratori sicuro](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_100.png)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>Ripristinare le modifiche per il servizio Spooler di stampa  

1.  Da qualsiasi server membro o una workstation interessati dalle modifiche dell'oggetto Criteri di gruppo, accedere localmente.  

2.  Con il mouse, sposta il puntatore nell'angolo superiore destro o in basso a destra dello schermo. Quando il **accessi** barra viene visualizzata, fare clic su **ricerca**.  

3.  Nel **ricerca** digitare **servizi**e fare clic su **servizi**.  

4.  Individuare e fare doppio clic su **Spooler di stampa**.  

5.  Fare clic su di **accesso** scheda.  

6.  Nel **accedere come** campo, fare clic su **sistema locale** account, quindi scegliere **OK**.  
