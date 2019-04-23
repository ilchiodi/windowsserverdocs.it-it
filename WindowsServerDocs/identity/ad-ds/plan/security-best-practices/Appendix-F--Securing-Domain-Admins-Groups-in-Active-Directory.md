---
ms.assetid: 017b88a6-f29b-4787-99b6-b5c8eaf8c3df
title: 'Appendice F: protezione Domain Admins gruppi in Active Directory'
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 1f35503d1f02d616255c067fbc1750a0cab974cc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880142"
---
# <a name="appendix-f-securing-domain-admins-groups-in-active-directory"></a>Appendice F: Protezione Domain Admins gruppi in Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-f-securing-domain-admins-groups-in-active-directory"></a>Appendice F: Protezione Domain Admins gruppi in Active Directory  
Come avviene con il gruppo Enterprise Admins (EA), l'appartenenza al gruppo Domain Admins (DA) dovrebbe essere necessaria solo in scenari di ripristino di emergenza o di compilazione. Non dovrebbe esserci alcun account di utenti del gruppo DA fatta eccezione per l'account amministratore predefinito per il dominio, se è stato protetto come descritto in [appendice d: Protezione predefiniti Administrator account in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

Domain Admins sono, per impostazione predefinita, i membri del gruppo Administrators locale su tutti i server membri e le workstation nei rispettivi domini. La nidificazione predefinito non deve essere modificata per supportabilità e ripristino di emergenza. Se Domain Admins sono state rimosse dal gruppo Administrators locale nei server membri, il gruppo deve essere aggiunto al gruppo Administrators in ogni server membro e workstation nel dominio. Gruppo Domain Admins di ciascun dominio deve essere protetto come descritto nelle istruzioni dettagliate che seguono.  

Per il gruppo Domain Admins in ogni dominio nella foresta:  

1.  Rimuovere tutti i membri dal gruppo, con la possibile eccezione dell'account amministratore predefinito per il dominio, purché sia stata impostata come descritto in [appendice d: Protezione predefiniti Administrator account in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

2.  Nel GPO collegati alle unità organizzative che contiene i server membri e workstation in ogni dominio, il gruppo DA deve essere aggiunte ai diritti utente seguenti in **Computer Configurazione computer\Criteri\Impostazioni computer\Impostazioni di Windows\Impostazioni sicurezza\Criteri locali\Assegnazione diritti Le assegnazioni**:  

    -   Nega accesso al computer dalla rete  

    -   Nega accesso come processo batch  

    -   Nega accesso come servizio  

    -   Nega accesso locale  

    -   Nega accesso tramite diritti utente di Servizi Desktop remoto  

3.  Il controllo deve essere configurato per l'invio di avvisi se vengono apportate modifiche per le proprietà o l'appartenenza al gruppo Domain Admins.  

#### <a name="step-by-step-instructions-for-removing-all-members-from-the-domain-admins-group"></a>Istruzioni dettagliate per la rimozione di tutti i membri dal gruppo Domain Admins  

1.  Nelle **Server Manager**, fare clic su **Tools**, fare clic su **Active Directory Users and Computers**.  

2.  Per rimuovere tutti i membri dal gruppo DA, procedere come segue:  

    1.  Fare doppio clic il **Domain Admins** di gruppo e fare clic sui **membri** scheda.  

        ![gruppi di amministratore di dominio protetto](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_62.gif)  

    2.  Selezionare un membro del gruppo, fare clic su **rimuovere**, fare clic su **Yes**, fare clic su **OK**.  

3.  Ripetere il passaggio 2 fino a quando non sono stati rimossi tutti i membri del gruppo.  

#### <a name="step-by-step-instructions-to-secure-domain-admins-in-active-directory"></a>Istruzioni dettagliate per la protezione Domain Admins in Active Directory  

1.  Nelle **Server Manager**, fare clic su **Tools**, fare clic su **Gestione criteri di gruppo**.  

2.  Nell'albero della console, espandere \<foresta\>\\domini\\\<dominio\>e quindi **oggetti Criteri di gruppo** (dove \<foresta\> è il nome della foresta e \<dominio\> è il nome del dominio in cui si desidera impostare i criteri di gruppo).  

3.  Nell'albero della console, fare doppio clic su **oggetti Criteri di gruppo**, fare clic su **New**.  

    ![gruppi di amministratore di dominio protetto](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_63.gif)  

4.  Nel **nuovo GPO** finestra di dialogo, digitare \<nome oggetto Criteri di gruppo\>, fare clic su **OK** (in cui \<nome oggetto Criteri di gruppo\> è il nome di questo oggetto Criteri di gruppo).  

    ![gruppi di amministratore di dominio protetto](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_64.gif)  

5.  Nel riquadro dei dettagli, fare doppio clic su \<nome oggetto Criteri di gruppo\>, fare clic su **modificare**.  

6.  Passare a **Computer Configurazione computer\Criteri\Impostazioni di Windows\Impostazioni Protezione\criteri**, fare clic su **Assegnazione diritti utente**.  

    ![gruppi di amministratore di dominio protetto](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_65.gif)  

7.  Configurare i diritti utente per impedire ai membri del gruppo Domain Admins di accedere ai server membri e workstation in rete eseguendo queste operazioni:  

    1.  Fare doppio clic su **Nega l'accesso al computer dalla rete** e selezionare **Definisci queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

    3.  Tipo di **Domain Admins**, fare clic su **Controlla nomi**, fare clic su **OK**.  

        ![gruppi di amministratore di dominio protetto](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_66.gif)  

    4.  Fare clic su **OK**, e **OK** nuovamente.  

8.  Configurare i diritti utente per impedire ai membri del gruppo in grado di accedere come processo batch nel modo seguente:  

    1.  Fare doppio clic su **Nega accesso come processo batch** e selezionare **Definisci queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

    3.  Tipo di **Domain Admins**, fare clic su **Controlla nomi**, fare clic su **OK**.  

        ![gruppi di amministratore di dominio protetto](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_67.gif)  

    4.  Fare clic su **OK**, e **OK** nuovamente.  

9. Configurare i diritti utente per impedire ai membri del gruppo in grado di accedere come servizio effettuando le operazioni seguenti:  

    1.  Fare doppio clic su **Nega accesso come servizio** e selezionare **Definisci queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

    3.  Tipo di **Domain Admins**, fare clic su **Controlla nomi**, fare clic su **OK**.  

        ![gruppi di amministratore di dominio protetto](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_68.gif)  

    4.  Fare clic su **OK**, e **OK** nuovamente.  

10. Configurare i diritti utente per impedire ai membri del gruppo Domain Admins in grado di accedere localmente ai server membri e workstation nel modo seguente:  

    1.  Fare doppio clic su **Nega accesso locale** e selezionare **Definisci queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

    3.  Tipo di **Domain Admins**, fare clic su **Controlla nomi**, fare clic su **OK**.  

        ![gruppi di amministratore di dominio protetto](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_69.gif)  

    4.  Fare clic su **OK**, e **OK** nuovamente.  

11. Configurare i diritti utente per impedire che i membri del gruppo Domain Admins l'accesso a server membri e workstation tramite Servizi Desktop remoto eseguendo le operazioni seguenti:  

    1.  Fare doppio clic su **Nega accesso tramite Servizi Desktop remoto** e selezionare **Definisci queste impostazioni dei criteri**.  

    2.  Fare clic su **Aggiungi utente o gruppo** e fare clic su **Sfoglia**.  

    3.  Tipo di **Domain Admins**, fare clic su **Controlla nomi**, fare clic su **OK**.  

        ![gruppi di amministratore di dominio protetto](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_70.gif)  

    4.  Fare clic su **OK**, e **OK** nuovamente.  

12. Per uscire **Editor Gestione criteri di gruppo**, fare clic su **File**, fare clic su **uscire**.  

13. In Gestione criteri di gruppo collegato oggetto Criteri di gruppo per il server membro e workstation unità organizzative eseguendo queste operazioni:  

    1.  Passare al \<foresta\>\Domains\\\<dominio\> (dove \<foresta\> è il nome della foresta e \<dominio\> è il nome del il dominio in cui si desidera imposta i criteri di gruppo).  

    2.  Fare doppio clic su unità Organizzativa che oggetto Criteri di gruppo verranno applicati a e fare clic su **collegarne un esistente**.  

        ![gruppi di amministratore di dominio protetto](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_71.gif)  

    3.  Selezionare l'oggetto Criteri di gruppo appena creato e fare clic su **OK**.  

        ![gruppi di amministratore di dominio protetto](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_72.gif)  

    4.  Creare collegamenti a tutte le altre unità organizzative che includono le workstation.  

    5.  Creare collegamenti a tutte le altre unità organizzative che includono server membri.  

        > [!IMPORTANT]  
        > Se jump server vengono utilizzati per amministrare i controller di dominio e Active Directory, verificare che jump server si trovano in un'unità Organizzativa a cui non è collegato oggetti Criteri di gruppo.  

#### <a name="verification-steps"></a>Passaggi di verifica  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Verificare le impostazioni oggetto Criteri di gruppo "Nega accesso al computer dalla rete"  
Da qualsiasi server membro o una workstation che non è interessato dalle modifiche dell'oggetto Criteri di gruppo (ad esempio, "jump server"), tentare di accedere a un server membro o una workstation in rete che è interessata dalle modifiche dell'oggetto Criteri di gruppo. Per verificare le impostazioni di GPO, provare a eseguire il mapping di unità di sistema usando il **NET USE** comando.  

1.  Accedere in locale usando un account membro del gruppo Domain Admins.  

2.  Con il mouse, spostare il puntatore del mouse nell'angolo superiore destro o inferiore destro dello schermo. Quando la **accessi** barra viene visualizzata, fare clic su **ricerca**.  

3.  Nel **ricerca** , digitare **prompt dei comandi**, fare doppio clic su **prompt dei comandi**, quindi fare clic su **Esegui come amministratore** aprire con privilegi elevati prompt dei comandi.  

4.  Quando viene richiesto di approvare l'elevazione dei privilegi, fare clic su **Sì**.  

    ![gruppi di amministratore di dominio protetto](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_73.gif)  

5.  Nel **prompt dei comandi** finestra, digitare **net utilizzare \\ \\ \<nome del Server\>\c$**, dove \<nome Server\> è la nome del server membri o workstation che si sta provando ad accedere attraverso la rete.  

6.  Lo screenshot seguente mostra il messaggio di errore che deve essere visualizzato.  

    ![gruppi di amministratore di dominio protetto](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_74.gif)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Verificare le impostazioni oggetto Criteri di gruppo "Nega accesso come processo batch"  

Da qualsiasi server membro o una workstation interessate dalle modifiche dell'oggetto Criteri di gruppo, accedere localmente.  

###### <a name="create-a-batch-file"></a>Creare un File Batch  

1.  Con il mouse, spostare il puntatore del mouse nell'angolo superiore destro o inferiore destro dello schermo. Quando la **accessi** barra viene visualizzata, fare clic su **ricerca**.  

2.  Nel **ricerca** , digitare **blocco note**, fare clic su **Notepad**.  

3.  Nelle **Notepad**, digitare **dir c:**.  

4.  Fare clic su **File**, fare clic su **Salva con nome**.  

5.  Nel **File** campo nome, tipo  **\<Filename\>bat** (in cui \<Filename\> è il nome del nuovo file batch).  

###### <a name="schedule-a-task"></a>Pianificare un'attività  

1.  Con il mouse, spostare il puntatore del mouse nell'angolo superiore destro o inferiore destro dello schermo. Quando la **accessi** barra viene visualizzata, fare clic su **ricerca**.  

2.  Nel **ricerca** , digitare **utilità di pianificazione**, fare clic su **utilità di pianificazione**.  

    > [!NOTE]  
    > Nei computer che eseguono Windows 8, nelle **ricerca** , digitare **pianificare le attività**, fare clic su **pianificare attività**.  

3.  Nel **utilità di pianificazione** barra dei menu, fare clic su **azione**, fare clic su **Crea attività**.  

4.  Nel **attività di creazione** nella finestra di dialogo, digitare **\<Task Name\>** (in cui \<nome attività\> è il nome della nuova attività).  

5.  Scegliere il **azioni** scheda e fare clic su **New**.  

6.  Nel **azione** campi, selezionare **avviare un programma**.  

7.  Sotto **programma/script**, fare clic su **Sfoglia**individuare e selezionare il file batch creato nel **creare un File Batch** sezione e fare clic su **aprire**.  

8.  Fare clic su **OK**.  

9. Fare clic sulla scheda **Generale**.  

10. Sotto **sicurezza** opzioni, fare clic su **Cambia utente o gruppo**.  

11. Digitare il nome di un account membro del gruppo Domain Admins, fare clic su **Controlla nomi**, fare clic su **OK**.  

12. Selezionare **eseguire anche se l'utente è connesso o non** e selezionare **non archiviare password**. L'attività sarà solo possibile accedere alle risorse del computer locale.  

13. Fare clic su **OK**.  

14. Verrà visualizzata una finestra di dialogo, account utente che richiede le credenziali per eseguire l'attività.  

15. Dopo aver immesso le credenziali, fare clic su **OK**.  

16. Verrà visualizzata una finestra di dialogo simile alla seguente.  

    ![gruppi di amministratore di dominio protetto](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_75.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Verificare le impostazioni oggetto Criteri di gruppo "Nega accesso come servizio"  

1.  Da qualsiasi server membro o una workstation interessate dalle modifiche dell'oggetto Criteri di gruppo, accedere localmente.  

2.  Con il mouse, spostare il puntatore del mouse nell'angolo superiore destro o inferiore destro dello schermo. Quando la **accessi** barra viene visualizzata, fare clic su **ricerca**.  

3.  Nel **ricerca** , digitare **services**, fare clic su **Services**.  

4.  Individuare e fare doppio clic su **Spooler di stampa**.  

5.  Fare clic sulla scheda **Connessione**.  

6.  Sotto **accedere come**, selezionare la **questo account** opzione.  

7.  Fare clic su **esplorare**, digitare il nome di un account membro del gruppo Domain Admins, fare clic su **Controlla nomi**, fare clic su **OK**.  

8.  Sotto **Password** e **Conferma password**, digitare la password dell'account selezionato e fare clic su **OK**.  

9. Fare clic su **OK** altre tre volte.  

10. Fare doppio clic su **Spooler di stampa** e fare clic su **riavviare**.  

11. Quando il servizio viene riavviato, verrà visualizzata una finestra di dialogo simile alla seguente.  

    ![gruppi di amministratore di dominio protetto](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_76.gif)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>Annullare le modifiche per il servizio Spooler di stampa  

1.  Da qualsiasi server membro o una workstation interessate dalle modifiche dell'oggetto Criteri di gruppo, accedere localmente.  

2.  Con il mouse, spostare il puntatore del mouse nell'angolo superiore destro o inferiore destro dello schermo. Quando la **accessi** barra viene visualizzata, fare clic su **ricerca**.  

3.  Nel **ricerca** , digitare **services**, fare clic su **Services**.  

4.  Individuare e fare doppio clic su **Spooler di stampa**.  

5.  Fare clic sulla scheda **Connessione**.  

6.  Sotto **accedere come**, selezionare la **LocalSystem** dell'account e fare clic su **OK**.  

##### <a name="verify-deny-log-on-locally-gpo-settings"></a>Verificare le impostazioni oggetto Criteri di gruppo "Nega accesso locale"  

1.  Da qualsiasi server membro o una workstation interessate dalle modifiche dell'oggetto Criteri di gruppo, tenta di accedere in locale usando un account membro del gruppo Domain Admins. Verrà visualizzata una finestra di dialogo simile alla seguente.  

    ![gruppi di amministratore di dominio protetto](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_77.gif)  

##### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>Verificare le impostazioni oggetto Criteri di gruppo "Nega accesso tramite Servizi Desktop remoto"    
1.  Con il mouse, spostare il puntatore del mouse nell'angolo superiore destro o inferiore destro dello schermo. Quando la **accessi** barra viene visualizzata, fare clic su **ricerca**.  

2.  Nel **ricerca** , digitare **connessione desktop remoto**, fare clic su **connessione Desktop remoto**.  

3.  Nel **Computer** digitare il nome del computer in cui si desidera connettersi a e fare clic su **Connect**. (È anche possibile digitare l'indirizzo IP anziché il nome del computer.)  

4.  Quando richiesto, fornire le credenziali per un account membro del gruppo Domain Admins.  

5.  Verrà visualizzata una finestra di dialogo simile alla seguente.  

    ![gruppi di amministratore di dominio protetto](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_78.gif)  
