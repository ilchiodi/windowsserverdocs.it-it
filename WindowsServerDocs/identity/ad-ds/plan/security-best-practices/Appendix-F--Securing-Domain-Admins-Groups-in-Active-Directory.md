---
ms.assetid: 017b88a6-f29b-4787-99b6-b5c8eaf8c3df
title: Appendice F - protezione Domain Admins gruppi in Active Directory
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 3535714e0df43cb94c7e89c503bc5f875cd49e28
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="appendix-f-securing-domain-admins-groups-in-active-directory"></a>Appendice f: protezione Domain Admins gruppi in Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-f-securing-domain-admins-groups-in-active-directory"></a>Appendice f: protezione Domain Admins gruppi in Active Directory  
Come avviene con il gruppo Enterprise Admins (EA), l'appartenenza al gruppo Domain Admins (DA) dovrebbe essere necessaria solo in scenari di ripristino di emergenza o di compilazione. Non dovrebbe esserci alcun account di utenti di gruppo DA fatta eccezione per l'account Administrator predefinito per il dominio, se è stato protetto come descritto in [appendice d: protezione predefiniti Administrator account in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

Domain Admins sono, per impostazione predefinita, i membri del gruppo Administrators locale in tutti i server membri e workstation nei rispettivi domini. La nidificazione predefinito non deve essere modificata per supporto e ripristino di emergenza. Se Domain Admins sono stati rimossi dal gruppo Administrators locale nei server membri, il gruppo deve essere aggiunto al gruppo Administrators in ogni server membro e workstation nel dominio. Gruppo Domain Admins del dominio ogni deve essere protetto come descritto nelle istruzioni dettagliate che seguono.  

Per il gruppo Domain Admins in ogni dominio nella foresta:  

1.  Rimuovere tutti i membri dal gruppo, con la possibile eccezione dell'account amministratore predefinito per il dominio, purché sia stata impostata come descritto in [appendice d: protezione predefiniti Administrator account in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

2.  Oggetti Criteri di gruppo collegati alle unità organizzative che contiene i server membri e workstation in ogni dominio, il gruppo DA deve essere aggiunto ai diritti utente seguenti in **Configurazione computer\Criteri\Impostazioni di Windows\Impostazioni protezione\Criteri Settings\nome diritti**:  

    -   Nega accesso al computer dalla rete  

    -   Nega accesso come processo batch  

    -   Nega accesso come servizio  

    -   Nega accesso locale  

    -   Nega accesso tramite diritti utente di Servizi Desktop remoto  

3.  Il controllo deve essere configurato per l'invio di avvisi se vengono apportate modifiche per le proprietà o l'appartenenza al gruppo Domain Admins.  

#### <a name="step-by-step-instructions-for-removing-all-members-from-the-domain-admins-group"></a>Istruzioni dettagliate per la rimozione di tutti i membri dal gruppo Domain Admins  

1.  In **Server Manager**, fare clic su **strumenti**e fare clic su **Active Directory Users and Computers**.  

2.  Per rimuovere tutti i membri dal gruppo DA, eseguire i passaggi seguenti:  

    1.  Fare doppio clic il **Domain Admins** di gruppo e fare clic su di **membri** scheda.  

        ![gruppi di amministratori di dominio protetto](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_62.gif)  

    2.  Selezionare un membro del gruppo, fare clic su **rimuovere**, fare clic su **Sì**e fare clic su **OK**.  

3.  Ripetere il passaggio 2 fino a quando non sono stati rimossi tutti i membri del gruppo DA.  

#### <a name="step-by-step-instructions-to-secure-domain-admins-in-active-directory"></a>Istruzioni dettagliate per la protezione Domain Admins in Active Directory  

1.  In **Server Manager**, fare clic su **strumenti**e fare clic su **Gestione criteri di gruppo**.  

2.  Nell'albero della console, espandere \ < Forest\ > \\Domains\\\ < domain > \,. e quindi **oggetti Criteri di gruppo** (dove \ < Forest\ > è il nome dell'insieme di strutture e \ < domain \ > è il nome del dominio in cui si desidera impostare i criteri di gruppo).  

3.  Nell'albero della console fare doppio clic su **oggetti Criteri di gruppo**e fare clic su **New**.  

    ![gruppi di amministratori di dominio protetto](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_63.gif)  

4.  Nel **nuovo oggetto Criteri di gruppo** la finestra di dialogo, digitare \ < Name\ oggetto Criteri di gruppo >, fare clic su **OK** (dove \ < Name\ oggetto Criteri di gruppo > è il nome di questo oggetto Criteri di gruppo).  

    ![gruppi di amministratori di dominio protetto](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_64.gif)  

5.  Nel riquadro dei dettagli fare doppio clic su \ < Name\ oggetto Criteri di gruppo >, fare clic su **modifica**.  

6.  Passare a **Computer Configurazione computer\Criteri\Impostazioni di Windows\Impostazioni Protezione\criteri**e fare clic su **Assegnazione diritti utente**.  

    ![gruppi di amministratori di dominio protetto](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_65.gif)  

7.  Configurare i diritti utente per impedire ai membri del gruppo Domain Admins di accedere ai server membri e workstation sulla rete eseguendo le operazioni seguenti:  

    1.  Fare doppio clic su **negare l'accesso al computer dalla rete** e seleziona **definire queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

    3.  Tipo **Domain Admins**, fare clic su **Controlla nomi**e fare clic su **OK**.  

        ![gruppi di amministratori di dominio protetto](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_66.gif)  

    4.  Fare clic su **OK**, e **OK** nuovamente.  

8.  Configurare i diritti utente per impedire ai membri del gruppo dall'accesso come processo batch effettuando le operazioni seguenti:  

    1.  Fare doppio clic su **Nega accesso come processo batch** e seleziona **definire queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

    3.  Tipo **Domain Admins**, fare clic su **Controlla nomi**e fare clic su **OK**.  

        ![gruppi di amministratori di dominio protetto](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_67.gif)  

    4.  Fare clic su **OK**, e **OK** nuovamente.  

9. Configurare i diritti utente per impedire che i membri del gruppo DA accesso come servizio effettuando le operazioni seguenti:  

    1.  Fare doppio clic su **Nega accesso come servizio** e seleziona **definire queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

    3.  Tipo **Domain Admins**, fare clic su **Controlla nomi**e fare clic su **OK**.  

        ![gruppi di amministratori di dominio protetto](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_68.gif)  

    4.  Fare clic su **OK**, e **OK** nuovamente.  

10. Configurare i diritti utente per impedire ai membri del gruppo Domain Admins di accedere localmente al server e workstation membri effettuando le operazioni seguenti:  

    1.  Fare doppio clic su **Nega accesso locale** e seleziona **definire queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

    3.  Tipo **Domain Admins**, fare clic su **Controlla nomi**e fare clic su **OK**.  

        ![gruppi di amministratori di dominio protetto](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_69.gif)  

    4.  Fare clic su **OK**, e **OK** nuovamente.  

11. Configurare i diritti utente per impedire ai membri del gruppo Domain Admins di accedere ai server membri e workstation tramite Servizi Desktop remoto eseguendo le operazioni seguenti:  

    1.  Fare doppio clic su **Nega accesso tramite Servizi Desktop remoto** e seleziona **definire queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

    3.  Tipo **Domain Admins**, fare clic su **Controlla nomi**e fare clic su **OK**.  

        ![gruppi di amministratori di dominio protetto](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_70.gif)  

    4.  Fare clic su **OK**, e **OK** nuovamente.  

12. Per uscire da **Editor Gestione criteri di gruppo**, fare clic su **File**e fare clic su **uscire**.  

13. In Gestione criteri di gruppo, collegare l'oggetto Criteri di gruppo per il server membro e workstation unità organizzative effettuando le operazioni seguenti:  

    1.  Passare al \ < Forest\ > \Domains\\\ < domain \ > (dove \ < Forest\ > è il nome dell'insieme di strutture e \ < domain \ > è il nome del dominio in cui si desidera impostare i criteri di gruppo).  

    2.  Fare doppio clic su oggetto Criteri di gruppo verranno applicate a e fare clic su, l'unità Organizzativa **collegare un oggetto Criteri di gruppo esistente**.  

        ![gruppi di amministratori di dominio protetto](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_71.gif)  

    3.  Selezionare l'oggetto Criteri di gruppo appena creato e fare clic su **OK**.  

        ![gruppi di amministratori di dominio protetto](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_72.gif)  

    4.  Creare collegamenti a tutte le altre unità organizzative contenenti le workstation.  

    5.  Creare collegamenti a tutte le altre unità organizzative contenenti i server membri.  

        > [!IMPORTANT]  
        > Se jump server vengono utilizzati per amministrare i controller di dominio e Active Directory, verificare che jump server si trovano in un'unità Organizzativa a cui non è ancora collegato oggetti Criteri di gruppo.  

#### <a name="verification-steps"></a>Passaggi di verifica  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Verificare le impostazioni oggetto Criteri di gruppo "Nega accesso al computer dalla rete"  
Da qualsiasi server membro o una workstation in cui non è interessata dall'oggetto Criteri di gruppo viene modificato (ad esempio, "jump server"), tentare di accedere a un server membro o una workstation in rete che è interessata dall'oggetto Criteri di gruppo viene modificato. Per verificare le impostazioni oggetto Criteri di gruppo, tentare di eseguire il mapping di unità di sistema utilizzando il **NET USE** comando.  

1.  Accedere localmente utilizzando un account che sia un membro del gruppo Domain Admins.  

2.  Con il mouse, sposta il puntatore nell'angolo superiore destro o in basso a destra dello schermo. Quando il **accessi** barra viene visualizzata, fare clic su **ricerca**.  

3.  Nel **ricerca** digitare **prompt dei comandi**, fare doppio clic su **prompt dei comandi**, quindi fare clic su **Esegui come amministratore** per aprire un prompt dei comandi con privilegi elevati.  

4.  Quando viene richiesto di approvare l'elevazione dei privilegi, fare clic su **Sì**.  

    ![gruppi di amministratori di dominio protetto](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_73.gif)  

5.  Nel **prompt dei comandi** finestra, digitare **net uso \ < Server le > \c$**, dove \ < Server le > è il nome del server membri o workstation che si sta tentando di accedere attraverso la rete.  

6.  Lo screenshot seguente mostra il messaggio di errore che dovrebbe essere visualizzato.  

    ![gruppi di amministratori di dominio protetto](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_74.gif)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Verificare le impostazioni oggetto Criteri di gruppo "Nega accesso come processo batch"  

Da qualsiasi server membro o una workstation interessati dalle modifiche dell'oggetto Criteri di gruppo, accedere localmente.  

###### <a name="create-a-batch-file"></a>Creare un File Batch  

1.  Con il mouse, sposta il puntatore nell'angolo superiore destro o in basso a destra dello schermo. Quando il **accessi** barra viene visualizzata, fare clic su **ricerca**.  

2.  Nel **ricerca** digitare **blocco note**e fare clic su **blocco note**.  

3.  In **blocco note**, tipo **c: dir**.  

4.  Fare clic su **File**e fare clic su **Salva con nome**.  

5.  Nel **File** campo del nome, tipo **\ < Filename\ > bat** (dove \ < Filename\ > è il nome del nuovo file batch).  

###### <a name="schedule-a-task"></a>Pianificare un'attività  

1.  Con il mouse, sposta il puntatore nell'angolo superiore destro o in basso a destra dello schermo. Quando il **accessi** barra viene visualizzata, fare clic su **ricerca**.  

2.  Nel **ricerca** digitare **utilità di pianificazione**e fare clic su **utilità di pianificazione**.  

    > [!NOTE]  
    > Nei computer che eseguono Windows 8, nel **ricerca** digitare **Pianifica attività**e fare clic su **Pianifica attività**.  

3.  Nel **utilità di pianificazione** barra dei menu, fare clic su **azione**e fare clic su **Crea attività**.  

4.  Nel **Crea attività** la finestra di dialogo, digitare **\ < le attività >** (dove \ < le attività > è il nome della nuova attività).  

5.  Fare clic su di **azioni** scheda e fare clic su **New**.  

6.  Nel **azione** selezionare **avviare un programma**.  

7.  In **programma o script**, fare clic su **Sfoglia**, individuare e selezionare il file batch creato il **creare un File Batch** sezione e fare clic su **aprire**.  

8.  Fare clic su **OK**.  

9. Fare clic su di **generale** scheda.  

10. In **sicurezza** opzioni, fare clic su **Cambia utente o gruppo**.  

11. Digitare il nome di un account che sia membro del gruppo Domain Admins, fare clic su **Controlla nomi**e fare clic su **OK**.  

12. Selezionare **eseguire se l'utente è connesso o non** e seleziona **non archiviare password**. L'attività sarà solo possibile accedere alle risorse del computer locale.  

13. Fare clic su **OK**.  

14. Dovrebbe essere visualizzata una finestra di dialogo, account utente che richiede le credenziali necessarie per eseguire l'attività.  

15. Dopo aver immesso le credenziali, fare clic su **OK**.  

16. Dovrebbe essere visualizzata una finestra di dialogo simile al seguente.  

    ![gruppi di amministratori di dominio protetto](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_75.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Verificare le impostazioni oggetto Criteri di gruppo "Nega accesso come servizio"  

1.  Da qualsiasi server membro o una workstation interessati dalle modifiche dell'oggetto Criteri di gruppo, accedere localmente.  

2.  Con il mouse, sposta il puntatore nell'angolo superiore destro o in basso a destra dello schermo. Quando il **accessi** barra viene visualizzata, fare clic su **ricerca**.  

3.  Nel **ricerca** digitare **servizi**e fare clic su **servizi**.  

4.  Individuare e fare doppio clic su **Spooler di stampa**.  

5.  Fare clic su di **accesso** scheda.  

6.  In **accedere come**, selezionare il **questo account** opzione.  

7.  Fare clic su **Sfoglia**, digitare il nome di un account che sia membro del gruppo Domain Admins, fare clic su **Controlla nomi**e fare clic su **OK**.  

8.  In **Password** e **Conferma password**, digita la password dell'account selezionato e scegliere **OK**.  

9. Fare clic su **OK** tre volte.  

10. Fare doppio clic su **Spooler di stampa** e fare clic su **riavviare**.  

11. Quando viene riavviato il servizio, verrà visualizzata una finestra di dialogo simile al seguente.  

    ![gruppi di amministratori di dominio protetto](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_76.gif)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>Ripristinare le modifiche per il servizio Spooler di stampa  

1.  Da qualsiasi server membro o una workstation interessati dalle modifiche dell'oggetto Criteri di gruppo, accedere localmente.  

2.  Con il mouse, sposta il puntatore nell'angolo superiore destro o in basso a destra dello schermo. Quando il **accessi** barra viene visualizzata, fare clic su **ricerca**.  

3.  Nel **ricerca** digitare **servizi**e fare clic su **servizi**.  

4.  Individuare e fare doppio clic su **Spooler di stampa**.  

5.  Fare clic su di **accesso** scheda.  

6.  In **accedere come**, selezionare il **sistema locale** account, quindi scegliere **OK**.  

##### <a name="verify-deny-log-on-locally-gpo-settings"></a>Verificare le impostazioni oggetto Criteri di gruppo "Nega accesso locale"  

1.  Da qualsiasi server membro o una workstation interessati dall'oggetto Criteri di gruppo viene modificato, tentare di accedere localmente utilizzando un account che sia un membro del gruppo Domain Admins. Dovrebbe essere visualizzata una finestra di dialogo simile al seguente.  

    ![gruppi di amministratori di dominio protetto](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_77.gif)  

##### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>Verificare le impostazioni oggetto Criteri di gruppo "Nega accesso tramite Servizi Desktop remoto"    
1.  Con il mouse, sposta il puntatore nell'angolo superiore destro o in basso a destra dello schermo. Quando il **accessi** barra viene visualizzata, fare clic su **ricerca**.  

2.  Nel **ricerca** digitare **connessione desktop remoto**e fare clic su **connessione Desktop remoto**.  

3.  Nel **Computer** , digitare il nome del computer che si desidera connettersi e fare clic su **Connetti**. (È anche possibile digitare l'indirizzo IP anziché il nome del computer.)  

4.  Quando richiesto, fornire le credenziali per un account che sia un membro del gruppo Domain Admins.  

5.  Dovrebbe essere visualizzata una finestra di dialogo simile al seguente.  

    ![gruppi di amministratori di dominio protetto](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_78.gif)  
