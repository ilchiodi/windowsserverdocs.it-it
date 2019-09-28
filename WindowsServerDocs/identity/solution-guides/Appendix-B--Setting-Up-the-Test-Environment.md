---
ms.assetid: 82918181-525d-4e93-af96-957dac6aedb6
title: Appendice B configurazione dell'ambiente di test
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: af045545826269630af9327480cda59093d219df
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407148"
---
# <a name="appendix-b-setting-up-the-test-environment"></a>Appendice B: Configurazione dell'ambiente di testing

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento illustra la procedura da eseguire per configurare un laboratorio pratico in cui testare la funzionalità Controllo dinamico degli accessi. Le istruzioni devono essere seguite in sequenza perché esistono numerose dipendenze tra componenti.  

## <a name="prerequisites"></a>Prerequisiti  
**Requisiti hardware e software**  

Requisiti per la configurazione del laboratorio di test:  

-   Un server host che esegue Windows Server 2008 R2 con SP1 e Hyper-V  

-   Una copia dell'ISO di Windows Server 2012  

-   Una copia dell'immagine ISO di Windows 8  

-   Microsoft Office 2010  

-   Un server che esegue Microsoft Exchange Server 2003 o versione successiva  

Per testare gli scenari di Controllo dinamico degli accessi, è necessario configurare le macchine virtuali seguenti:  

-   DC1 (controller di dominio)  

-   DC2 (controller di dominio)  

-   FILE1 (file server e Active Directory Rights Management Services)  

-   SRV1 (server POP3 e SMTP)  

-   CLIENT1 (computer client con Microsoft Outlook)  

Le password per le macchine virtuali devono essere le seguenti:  

-   BUILTIN\Administrator: pass@word1  

-   Contoso\Administrator: pass@word1  

-   Tutti gli altri account: pass@word1  

## <a name="build-the-test-lab-virtual-machines"></a>Configurare le macchine virtuali del laboratorio di test  

### <a name="install-the-hyper-v-role"></a>Installare il ruolo Hyper-V  
È necessario installare il ruolo Hyper-V in un computer che esegue Windows Server 2008 R2 con SP1.  

##### <a name="to-install-the-hyper-v-role"></a>Per installare il ruolo Hyper-V  

1.  Fare clic sul pulsante **Start**e quindi scegliere Server Manager.  

2.  Nell'area Riepilogo ruoli della finestra principale di Server Manager fare clic su **Aggiungi ruoli**.  

3.  Nella pagina **Selezione ruoli server** fare clic su **Hyper-V**.  

4.  Nella pagina **Crea reti virtuali** fare clic su una o più schede di rete per rendere la connessione di rete corrispondente disponibile per le macchine virtuali.  

5.  Nella pagina **Conferma selezioni per la rimozione** fare clic su **Rimuovi**.  

6.  Per completare il processo di installazione è necessario riavviare il computer. Fare clic su **Chiudi** per terminare la procedura guidata e quindi su **Sì** per riavviare il computer.  

7.  Dopo avere ravviato il computer, accedere con lo stesso account usato per installare il ruolo. Dopo il completamento dell'installazione, fare clic su **Chiudi** per terminare la Ripresa guidata configurazione.  

### <a name="create-an-internal-virtual-network"></a>Creare una rete virtuale interna  
A questo punto si creerà una rete virtuale interna denominata ID_AD_Network.  

##### <a name="to-create-a-virtual-network"></a>Per creare una rete virtuale  

1.  Aprire la console di gestione di Hyper-V.  

2.  Scegliere **Gestione reti virtuali** dal menu **Azioni**.  

3.  In **Crea rete virtuale**selezionare **Interna**.  

4.  Fai clic su **Aggiungi**. Verrà visualizzata la pagina **Nuova rete virtuale** .  

5.  Digitare **ID_AD_Network** come nome della nuova rete. Verificare le altre proprietà e modificarle se necessario.  

6.  Fare clic su **OK** per creare la rete virtuale e chiudere Gestione reti virtuali oppure fare clic su **Applica** per creare la rete virtuale e continuare a usare Gestione reti virtuali.  

### <a name="BKMK_Build"></a>Compilazione del controller di dominio  
Creare una macchina virtuale da usare come controller di dominio (DC1). Installare la macchina virtuale usando l'ISO di Windows Server 2012 e denominarla DC1.  

##### <a name="to-install-active-directory-domain-services"></a>Per installare Servizi di dominio Active Directory  

1. Connettere la macchina virtuale alla rete ID_AD_Network. Accedere a DC1 come amministratore con la password <strong>pass@word1</strong>.  

2. In Server Manager fare clic su **Gestione**e quindi su **Aggiungi ruoli e funzionalità**.  

3. Nella pagina **Prima di iniziare**, fare clic su **Avanti**.  

4. Nella pagina **Selezione tipo di installazione** selezionare **Installazione basata su ruoli o basata su funzionalità**e quindi fare clic su **Avanti**.  

5. Nella pagina **Selezione server di destinazione** fare clic su **Avanti**.  

6. Nella pagina **Selezione ruoli server** fare clic su **Servizi di dominio Active Directory**. Nella finestra di dialogo **Aggiunta guidata ruoli e funzionalità** fare clic su **Aggiungi funzionalità** e quindi su **Avanti**.  

7. Nella pagina **Selezione funzionalità** fare clic sul pulsante **Avanti**.  

8. Nella pagina **Servizi di dominio Active Directory** verificare le informazioni e fare clic su **Avanti**.  

9. Nella pagina **Conferma selezioni per l'installazione** fare clic su **Installa**. La barra di stato di Installazione funzionalità nella pagina Risultati indica che il ruolo è in fase di installazione.  

10. Nella pagina **Risultati** verificare che l'installazione sia riuscita e fare clic su **Chiudi**. In Server Manager fare clic sull'icona di avviso con un punto esclamativo nell'angolo superiore destro dello schermo, accanto a **Gestisci**. Nell'elenco Attività fare clic sul collegamento **Alza di livello il server a controller di dominio**.  

11. Nella pagina **Configurazione distribuzione** fare clic su **Aggiungi una nuova foresta**, digitare il nome del dominio radice, **contoso.com**, e quindi fare clic su **Avanti**.  

12. Nella pagina **Opzioni controller di dominio** selezionare i livelli di funzionalità dominio e foresta come Windows Server 2012, specificare la password della modalità ripristino servizi directory <strong>pass@word1</strong>, quindi fare clic su **Avanti**.  

13. Nella pagina **Opzioni DNS** fare clic su **Avanti**.  

14. Nella pagina **Opzioni aggiuntive** fare clic su **Avanti**.  

15. Nella pagina **Percorsi** digitare i percorsi del database di Active Directory, dei file di log e della cartella SYSVOL (o accettare i percorsi predefiniti), quindi fare clic su **Avanti**.  

16. Nella pagina **Verifica opzioni** verificare le selezioni e fare clic su **Avanti**.  

17. Nella pagina **Controllo dei prerequisiti** verificare che la convalida dei prerequisiti sia stata completata, quindi fare clic su **Installa**.  

18. Nella pagina **Risultati** verificare che il server sia stato configurato correttamente come controller di dominio e quindi fare clic su **Chiudi**.  

19. Riavviare il server per completare l'installazione di Servizi di dominio Active Directory. Per impostazione predefinita, il server viene riavviato automaticamente.  

Creare gli utenti seguenti mediante Centro di amministrazione di Active Directory.  

##### <a name="create-users-and-groups-on-dc1"></a>Creare utenti e gruppi in DC1  

1. Accedere a contoso.com come Administrator. Avviare Centro di amministrazione di Active Directory.  

2. Creare i gruppi di sicurezza seguenti:  


   |    Nome gruppo    |        Indirizzo di posta elettronica         |
   |------------------|------------------------------|
   |   FinanceAdmin   |   financeadmin@contoso.com   |
   | FinanceException | financeexception@contoso.com |


3. Creare l'unità organizzativa seguente:  


   |   Nome unità organizzativa    | Computer |
   |--------------|-----------|
   | FileServerOU |   FILE1   |


4. Creare gli utenti seguenti con gli attributi indicati:  


   |       Utente       |  Nome utente  |     Indirizzo di posta elettronica      | department |      Group       | Paese/area geografica |
   |------------------|------------|------------------------|------------|------------------|----------------|
   | Myriam Delesalle | MDelesalle | MDelesalle@contoso.com |  Finanza   |                  |       US       |
   |    Miles Reid    |   MReid    |   MReid@contoso.com    |  Finanza   |   FinanceAdmin   |       US       |
   |   Esther Valle   |   EValle   |   EValle@contoso.com   | Operazioni | FinanceException |       US       |
   |   Maira Wenzel   |  MWenzel   |  MWenzel@contoso.com   |     RU     |                  |       US       |
   |     Jeff Low     |    JLow    |    JLow@contoso.com    |     RU     |                  |       US       |
   |    RMS Server    |    rms     |    rms@contoso.com     |            |                  |                |

   Per altre informazioni sulla creazione di gruppi di sicurezza, vedere [Creare un nuovo gruppo](https://technet.microsoft.com/library/dd861305.aspx) sul sito Web di Windows Server.  

##### <a name="to-create-a-group-policy-object"></a>Per creare un oggetto Criteri di gruppo  

1.  Passare il cursore sull'angolo superiore destro dello schermo e fare clic sull'icona di ricerca. Nella casella di ricerca digitare **gestione criteri di gruppo** e fare clic su **Gestione criteri di gruppo**.  

2.  Espandere **Foresta: contoso.com**e quindi espandere **Domini**, passare a **contoso.com**, espandere **(contoso.com)** e quindi selezionare **FileServerOU**. Fare clic con il pulsante destro del mouse su **Crea un oggetto Criteri di gruppo nel dominio e collegarlo qui**

3.  Digitare un nome descrittivo per l'oggetto Criteri di gruppo, ad esempio **FlexibleAccessGPO**, e quindi fare clic su **OK**.  

##### <a name="to-enable-dynamic-access-control-for-contosocom"></a>Per abilitare il controllo dinamico degli accessi per contoso.com  

1.  Aprire la Console Gestione Criteri di gruppo, fare clic su **contoso.com** e quindi fare doppio clic su **Controller di dominio**.  

2.  Fare clic con il pulsante destro del mouse su **Criterio Controller di domini predefiniti**e scegliere **Modifica**.  

3.  Nella finestra Editor Gestione Criteri di gruppo fare doppio clic su **Configurazione computer**, quindi su **Criteri**, **Modelli amministrativi**, **Sistema** e infine su **KDC**.  

4.  Fare doppio clic su **Supporto KDC di attestazioni, autenticazione composta e blindatura Kerberos** e selezionare l'opzione accanto a **Abilitato**. Questa impostazione deve essere abilitata per poter usare i criteri di accesso centrale.  

5.  Aprire un prompt dei comandi con privilegi elevati ed eseguire il comando seguente:  

    ```  
    gpupdate /force  
    ```  

### <a name="BKMK_FS1"></a>Compilare il file server e il server di AD RMS (FILE1)  

1. Creare una macchina virtuale con nome FILE1 dall'ISO di Windows Server 2012.  

2. Connettere la macchina virtuale alla rete ID_AD_Network.  

3. Aggiungere la macchina virtuale al dominio contoso.com, quindi accedere a FILE1 come CONTOSO\Administrator usando la password <strong>pass@word1</strong>.  

#### <a name="install-file-services-resource-manager"></a>Installare Gestione risorse file server  

###### <a name="to-install-the-file-services-role-and-the-file-server-resource-manager"></a>Per installare il ruolo Servizi file e Gestione risorse file server  

1.  In Server Manager fare clic su **Aggiungi ruoli e funzionalità**.  

2.  Nella pagina **Prima di iniziare**, fare clic su **Avanti**.  

3.  Nella pagina **Selezione tipo di installazione** fare clic su **Avanti**.  

4.  Nella pagina **Selezione server di destinazione** fare clic su **Avanti**.  

5.  Nella pagina **Selezione ruoli server** espandere **Servizi file e archiviazione**, selezionare la casella di controllo accanto a **Servizi file e iSCSI**, espandere e infine selezionare **Gestione risorse file server**.  

    Nell'Aggiunta guidata ruoli e funzionalità fare clic su **Aggiungi funzionalità**e quindi su **Avanti**.  

6.  Nella pagina **Selezione funzionalità** fare clic sul pulsante **Avanti**.  

7.  Nella pagina **Conferma selezioni per l'installazione** fare clic su **Installa**.  

8.  Nella pagina **Stato dell'installazione** fare clic su **Chiudi**.  

#### <a name="install-the-microsoft-office-filter-packs-on-the-file-server"></a>Installare i Microsoft Office Filter Pack nel file server  
È necessario installare i pacchetti di filtri Microsoft Office in Windows Server 2012 per abilitare gli IFilter per una matrice più ampia di file di Office rispetto a quelli forniti per impostazione predefinita.  Windows Server 2012 non dispone di filtri IFilter per i file di Microsoft Office installati per impostazione predefinita e l'infrastruttura di classificazione file USA IFilters per eseguire l'analisi del contenuto.  

Per scaricare e installare gli IFilter, vedere [Microsoft Office 2010 Filter Packs](https://go.microsoft.com/fwlink/?LinkID=234122).  

#### <a name="configure-email-notifications-on-file1"></a>Configurare le notifiche tramite posta elettronica in FILE1  
Quando si creano quote e screening dei file, è possibile inviare notifiche tramite posta elettronica agli utenti quando stanno per raggiungere il limite di quota o quando tentano di salvare un file bloccato. Se si vuole avvisare regolarmente alcuni amministratori in merito a eventi di quota e screening dei file, è possibile configurare uno o più destinatari predefiniti. Per inviare queste notifiche, è necessario specificare il server SMTP da usare per inoltrare i messaggi di posta elettronica.  

###### <a name="to-configure-email-options-in-file-server-resource-manager"></a>Per configurare le opzioni di posta elettronica in Gestione risorse file server  

1. Aprire Gestione risorse file server. Per aprire Gestione risorse file server, fare clic su **Start**, digitare **gestione risorse file server**e quindi fare clic su **Gestione risorse file server**.  

2. Nell'interfaccia di Gestione risorse file server fare clic con il pulsante destro del mouse su **Gestione risorse file server** e quindi scegliere **Configura opzioni**. Verrà visualizzata la finestra di dialogo **Gestione risorse file server**.  

3. Nella scheda **Notifiche posta elettronica** , in Nome server SMTP o indirizzo IP, digitare il nome host o l'indirizzo IP del server SMTP che inoltrerà le notifiche tramite posta elettronica.  

4. Se si desidera notificare periodicamente a determinati amministratori gli eventi di quota o di screening dei file, in **destinatari amministratori predefiniti**Digitare l'indirizzo di posta elettronica, ad esempio fileadmin@contoso.com. Usare il formato account@domain e usare un punto e virgola per separare più account.  

#### <a name="create-groups-on-file1"></a>Creare gruppi in FILE1  

###### <a name="to-create-security-groups-on-file1"></a>Per creare gruppi di sicurezza in FILE1  

1. Accedere a FILE1 come CONTOSO\Administrator, con la password: <strong>pass@word1</strong>.  

2. Aggiungere NT AUTHORITY\Authenticated Users al gruppo **WinRMRemoteWMIUsers__** .  

#### <a name="create-files-and-folders-on-file1"></a>Creare file e cartelle in FILE1  

1.  Creare un nuovo volume NTFS in FILE1 e quindi creare la cartella seguente: D:\Finance Documents.  

2.  Creare i file seguenti con i dettagli specificati:  

    -   **Finance Memo.docx**: aggiungere del testo di argomento finanziario nel documento. Ad esempio, le regole business relative agli utenti che possono accedere ai documenti finanziari sono cambiate. Da ora i documenti finanziari sono accessibili solo ai membri del gruppo FinanceExpert. Nessun altro reparto o gruppo ha accesso. Prima di implementare questo cambiamento nell'ambiente operativo, è necessario valutarne l'impatto. Verificare che il piè di pagina di ogni pagina del documento riporti il testo CONTOSO CONFIDENTIAL.  

    -   **Request for Approval to Hire.docx**: creare nel documento un modulo che raccoglie le informazioni sui candidati. Il documento deve contenere i campi seguenti: **Applicant Name, Social Security number, Job Title, Proposed Salary, Starting Date, Supervisor name, Department**. Aggiungere al documento un'altra sezione con un modulo per **Supervisor Signature, Approved Salary, Conformation of Offer** e **Status of Offer**.   
        Abilitare Rights Management per il documento.  

    -   **Word Document1.docx**: aggiungere del contenuto di prova al documento.  

    -   **Word Document2.docx**: aggiungere del contenuto di prova al documento.  

    -   **WorkBook1. xlsx**  

    -   **Workbook2. xlsx**  

    -   Sul desktop creare una cartella denominata Regular Expressions. Nella cartella creare un documento di testo denominato **RegEx-SSN**. Digitare il contenuto seguente nel file, quindi salvare e chiudere il file:   
        ^(?!000)([0-7]\d{2}|7([0-7]\d|7[012]))([ -]?)(?!00)\d\d\3(?!0000)\d{4}$  

3.  Condividere la cartella D:\Finance Documents come Finance Documents e concedere a tutti l'accesso in lettura e scrittura alla condivisione.  

> [!NOTE]  
> Per impostazione predefinita, i criteri di accesso centrale non sono abilitati nel volume di sistema o di avvio C:.  

#### <a name="BKMK_CS1"></a>Installa Active Directory Rights Management Services  
Aggiungere Active Directory Rights Management Services (AD RMS) e tutte le funzionalità necessarie mediante Server Manager. Scegliere tutte le impostazioni predefinite.  

###### <a name="to-install-active-directory-rights-management-services"></a>Per installare Active Directory Rights Management Services  

1. Accedere a FILE1 come CONTOSO\Administrator o come membro del gruppo Domain Admins.  

   > [!IMPORTANT]  
   > Per installare il ruolo del server AD RMS, l'account con cui si esegue l'installazione (in questo caso CONTOSO\Administrator) deve essere membro sia del gruppo Administrators locale nel computer server in cui deve essere installato AD RMS, sia del gruppo Enterprise Admins in Active Directory.  

2. In Server Manager fare clic su **Aggiungi ruoli e funzionalità**. Verrà visualizzata l'Aggiunta guidata ruoli e funzionalità.  

3. Nella schermata **Prima di iniziare** fare clic su **Avanti**.  

4. Nella schermata **Selezione tipo di installazione** fare clic su **Installazione basata su ruoli o basata su funzionalità**e quindi su **Avanti**.  

5. Nella schermata **Selezione server di destinazione** fare clic su **Avanti**.  

6. Nella schermata **Selezione ruoli server** selezionare la casella accanto a **Active Directory Rights Management Services** e quindi fare clic su **Avanti**.  

7. Nella finestra di dialogo **Aggiungere le funzionalità necessarie per Active Directory Rights Management Services** fare clic su **Aggiungi funzionalità**.  

8. Nella schermata **Selezione ruoli server** fare clic su **Avanti**.  

9. Nella schermata **Selezionare le funzionalità da installare** fare clic su **Avanti**.  

10. Nella schermata **Active Directory Rights Management Services** fare clic su Avanti.  

11. Nella schermata **Selezione servizi ruolo** fare clic su **Avanti**.  

12. Nella schermata del ruolo **Server Web (IIS)** fare clic su **Avanti**.  

13. Nella schermata **Selezione servizi ruolo** fare clic su **Avanti**.  

14. Nella schermata **Conferma selezioni per l'installazione** fare clic su **Installa**.  

15. Dopo il completamento dell'installazione, nella schermata **Stato dell'installazione** fare clic su **Esegui attività di configurazione aggiuntive**. Verrà visualizzata la Configurazione guidata AD RMS.  

16. Nella schermata **AD RMS** fare clic su **Avanti**.  

17. Nella schermata **Cluster AD RMS** selezionare **Crea un nuovo cluster AD RMS** e fare clic su **Avanti**.  

18. Nella schermata **Database di configurazione** fare clic su **Usa Database interno di Windows sul server** e quindi su **Avanti**.  

    > [!NOTE]  
    > L'uso di Database interno di Windows è consigliato solo per gli ambienti di test perché supporta un solo server nel cluster AD RMS. Negli ambienti di produzione occorre usare un server di database server separato.  

19. Nella schermata **account del servizio** , in **account utente di dominio**, fare clic su **specifica** e quindi specificare il nome utente (**contoso\rms**) e la password (<strong>pass@word1</strong>), quindi fare clic su **OK**, quindi su **Avanti**.  

20. Nella schermata **Modalità crittografia** fare clic su **Modalità crittografia 2**.  

21. Nella schermata **Archivio chiavi cluster** fare clic su **Avanti**.  

22. Nelle caselle **password e** **Conferma password** della schermata della password della **chiave del cluster** Digitare <strong>pass@word1</strong>, quindi fare clic su **Avanti**.  

23. Nella schermata **Sito Web cluster** verificare che l'opzione **Sito Web predefinito** sia selezionata e quindi fare clic su **Avanti**.  

24. Nella schermata **Indirizzo del cluster** selezionare l'opzione **Usa una connessione non crittografata**, nella casella **Nome di dominio completo** digitare **FILE1.contoso.com** e quindi fare clic su **Avanti**.  

25. Nella schermata **Nome certificato concessore di licenze** accettare il nome predefinito (**FILE1**) nella casella di testo e fare clic su **Avanti**.  

26. Nella schermata **Registrazione SCP** selezionare **Registra SCP**e quindi fare clic su **Avanti**.  

27. Nella schermata **Conferma** fare clic su **Installa**.  

28. Nella schermata **Risultati** fare clic su **Chiudi** e quindi ancora su **Chiudi** nella schermata **Stato dell'installazione**. Al termine, disconnettersi e accedere come contoso\rms usando la password specificata (<strong>pass@word1</strong>).  

29. Avviare la console AD RMS e passare a **Modelli di criteri per i diritti di utilizzo**.  

    Per aprire la console AD RMS, in Server Manager fare clic su **Server locale** nell'albero della console, quindi fare clic su **Strumenti**e infine su **Active Directory Rights Management Services**.  

30. Fare clic su **Crea modello di criteri per i diritti di utilizzo distribuito** nel riquadro destro, fare clic su **Aggiungi**e selezionare le informazioni seguenti:  

    -   Language: Inglese (Stati Uniti)  

    -   Nome: Contoso Finance Admin Only  

    -   Descrizione: Contoso Finance Admin Only  

    Fare clic su **Aggiungi** e quindi su **Avanti**.  

31. Nella sezione utenti e diritti fare clic su **utenti e diritti**, fare clic su **aggiungi**, digitare <strong>financeadmin@contoso.com</strong>e fare clic su **OK**.  

32. Selezionare **Controllo completo**e lasciare l'opzione **Concedi al proprietario (autore) il diritto di controllo completo senza scadenza** selezionata.  

33. Fare clic sulle schede rimanenti senza apportare modifiche e quindi fare clic su **Fine**. Eseguire l'accesso come CONTOSO\Administrator.  

34. Passare alla cartella C:\Inetpub\Wwwroot @ no__t-0_wmcs\certification, selezionare il file ServerCertification. asmx e aggiungere gli utenti autenticati per avere le autorizzazioni di lettura e scrittura per il file.  

35. Aprire Windows PowerShell ed eseguire `Get-FsrmRmsTemplate`. Verificare di riuscire a vedere il modello RMS creato nei passaggi precedenti di questa procedura con questo comando.  

> [!IMPORTANT]  
> Se si vuole modificare i file server immediatamente in modo da poterli testare, eseguire le operazioni seguenti:  
>   
> 1.  Nel file server, FILE1, aprire un prompt dei comandi con privilegi elevati ed eseguire i comandi seguenti:  
>   
>     -   gpupdate /force.  
>     -   NLTEST /SC_RESET:contoso.com  
> 2.  Nel controller di dominio (DC1) replicare Active Directory.  
>   
>     Per altre informazioni sui passaggi da eseguire per forzare la replica di Active Directory, vedere [Replica di Active Directory](https://technet.microsoft.com/library/cc794809(WS.10).aspx)  

Se si preferisce, invece di usare l'Aggiunta guidata ruoli e funzionalità in Server Manager, è possibile usare Windows PowerShell per installare e configurare il ruolo del server AD RMS come illustrato nella procedura seguente.  

###### <a name="to-install-and-configure-an-ad-rms-cluster-in-windows-server-2012-using-windows-powershell"></a>Per installare e configurare un cluster AD RMS in Windows Server 2012 mediante Windows PowerShell  

1. Accedere come CONTOSO\Administrator con la password: <strong>pass@word1</strong>.  

   > [!IMPORTANT]  
   > Per installare il ruolo del server AD RMS, l'account con cui si esegue l'installazione (in questo caso CONTOSO\Administrator) deve essere membro sia del gruppo Administrators locale nel computer server in cui deve essere installato AD RMS, sia del gruppo Enterprise Admins in Active Directory.  

2. Nel desktop del server fare clic con il pulsante destro del mouse sull'icona di Windows PowerShell sulla barra delle applicazioni e scegliere **Esegui come amministratore** per aprire un prompt di Windows PowerShell con privilegi amministrativi.  

3. Per usare i cmdlet di Server Manager per installare il ruolo del server AD RMS, digitare:  

   ```  
   Add-WindowsFeature ADRMS '"IncludeAllSubFeature '"IncludeManagementTools  
   ```  

4. Creare l'unità di Windows PowerShell che rappresenti il server AD RMS da installare.  

   Ad esempio, per creare un'unità di Windows PowerShell denominata RC per installare e configurare il primo server in un cluster radice AD RMS, digitare:  

   ```  
   Import-Module ADRMS  
   New-PSDrive -PSProvider ADRMSInstall -Name RC -Root RootCluster  
   ```  

5. Impostare le proprietà sugli oggetti nello spazio dei nomi dell'unità che rappresentano le impostazioni di configurazione necessarie.  

   Ad esempio, per impostare l'account del servizio AD RMS, al prompt dei comandi di Windows PowerShell digitare:  

   ```  
   $svcacct = Get-Credential  
   ```  

   Quando viene visualizzata la finestra di dialogo Sicurezza di Windows, digitare il nome utente di dominio dell'account del servizio AD RMS, CONTOSO\RMS, e la password assegnata.  

   Per assegnare l'account del servizio AD RMS alle impostazioni del cluster AD RMS, digitare:  

   ```  
   Set-ItemProperty -Path RC:\ -Name ServiceAccount -Value $svcacct  
   ```  

   Per impostare il server AD RMS in modo che usi Database interno di Windows, al prompt dei comandi di Windows PowerShell digitare:  

   ```  
   Set-ItemProperty -Path RC:\ClusterDatabase -Name UseWindowsInternalDatabase -Value $true  
   ```  

   Per archiviare in modo sicuro la password della chiave cluster in una variabile, al prompt dei comandi di Windows PowerShell digitare:  

   ```  
   $password = Read-Host -AsSecureString -Prompt "Password:"  
   ```  

   Digitare la password della chiave cluster e premere INVIO.  

   Per assegnare la password all'installazione di AD RMS, al prompt dei comandi di Windows PowerShell digitare:  

   ```  
   Set-ItemProperty -Path RC:\ClusterKey -Name CentrallyManagedPassword -Value $password  
   ```  

   Per impostare l'indirizzo del cluster di AD RMS, al prompt dei comandi di Windows PowerShell digitare:  

   ```  
   Set-ItemProperty -Path RC:\ -Name ClusterURL -Value "http://file1.contoso.com:80"  
   ```  

   Per assegnare il nome SLC all'installazione di AD RMS, al prompt dei comandi di Windows PowerShell digitare:  

   ```  
   Set-ItemProperty -Path RC:\ -Name SLCName -Value "FILE1"  
   ```  

   Per impostare il punto di connessione del servizio (SCP) per il cluster AD RMS, al prompt dei comandi di Windows PowerShell digitare:  

   ```  
   Set-ItemProperty -Path RC:\ -Name RegisterSCP -Value $true  
   ```  

6. Eseguire il cmdlet **Install-ADRMS**. Oltre a installare il ruolo del server AD RMS e a configurare il server, questo cmdlet installa anche altre funzionalità per AD RMS, se necessarie.  

   Ad esempio, per passare all'unità di Windows PowerShell denominata RC e installare e configurare AD RMS, digitare:  

   ```  
   Set-Location RC:\  
   Install-ADRMS -Path.  
   ```  

   Digitare "Y" quando il cmdlet chiede di confermare l'avvio dell'installazione.  

7. Disconnettersi come CONTOSO\Administrator e accedere come CONTOSO\RMS usando la password specificata ("pass@word1").  

   > [!IMPORTANT]  
   > Per gestire il server AD RMS, l'account usato per effettuare l'accesso e per gestire il server (in questo caso CONTOSO\RMS) deve essere membro sia del gruppo Administrators locale nel computer server AD RMS, sia del gruppo Enterprise Admins in Active Directory.  

8. Nel desktop del server fare clic con il pulsante destro del mouse sull'icona di Windows PowerShell sulla barra delle applicazioni e scegliere **Esegui come amministratore** per aprire un prompt di Windows PowerShell con privilegi amministrativi.  

9. Creare l'unità di Windows PowerShell che rappresenti il server AD RMS da configurare.  

    Ad esempio, per creare un'unità di Windows PowerShell denominata RC per configurare il cluster radice AD RMS, digitare:  

    ```  
    Import-Module ADRMSAdmin `  
    New-PSDrive -PSProvider ADRMSAdmin -Name RC -Root http://localhost -Force -Scope Global  
    ```  

10. Per creare un nuovo modello dei diritti per l'amministratore di Contoso Finance e assegnargli diritti utente con controllo completo nell'installazione di AD RMS, al prompt dei comandi di Windows PowerShell digitare:  

    ```  
    New-Item -Path RC:\RightsPolicyTemplate '"LocaleName en-us -DisplayName "Contoso Finance Admin Only" -Description "Contoso Finance Admin Only" -UserGroup financeadmin@contoso.com  -Right ('FullControl')  
    ```  

11. Per verificare di riuscire a vedere il nuovo modello dei diritti per l'amministratore di Contoso Finance, al prompt dei comandi di Windows PowerShell digitare:  

    ```  
    Get-FsrmRmsTemplate  
    ```  

    Esaminare l'output del cmdlet per verificare che il modello RMS creato al passaggio precedente sia presente.  

### <a name="build-the-mail-server-srv1"></a>Creare il server della posta (SRV1)  
SRV1 è il server della posta SMTP/POP3. È necessario installarlo per poter inviare notifiche tramite posta elettronica in caso sia necessaria assistenza in situazioni di accesso negato.  

Configurare Microsoft Exchange Server in questo computer. Per altre informazioni, vedere [Come installare Exchange Server](https://go.microsoft.com/fwlink/?prd=12364).  

### <a name="build-the-client-virtual-machine-client1"></a>Creare la macchina virtuale client (CLIENT1)  

##### <a name="to-build-the-client-virtual-machine"></a>Per creare la macchina virtuale client  

1. Connettere CLIENT1 alla rete ID_AD_Network.  

2. Installare Microsoft Office 2010.  

3. Accedere come Contoso\Administrator e usare le informazioni seguenti per configurare Microsoft Outlook.  

   - Nome: File Administrator  

   - Indirizzo di posta elettronica: fileadmin@contoso.com  

   - Tipo di account: POP3  

   - Server della posta in arrivo: indirizzo IP statico di of SRV1  

   - Server della posta in uscita: indirizzo IP statico di of SRV1  

   - Nome utente:fileadmin@contoso.com  

   - Ricorda password: Selezione  

4. Creare un collegamento ad Outlook sul desktop di contoso\administrator.  

5. Aprire Outlook e indirizzare tutti i messaggi "primo avvio".  

6. Eliminare gli eventuali messaggi di prova generati.  

7. Creare una nuova scorciatoia sul desktop per tutti gli utenti della macchina virtuale client che punta ai documenti \\ \ FILE1\Finance.  

8. Riavviare se necessario.  

##### <a name="enable-access-denied-assistance-on-the-client-virtual-machine"></a>Abilitare l'assistenza per accesso negato nella macchina virtuale client  

1.  Aprire l'editor del Registro di sistema e passare a **HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Explorer**.  

    -   Impostare **EnableShellExecuteFileStreamCheck** su **1**.  

    -   Valore: DWORD  

## <a name="BKMK_CF"></a>Scenario di installazione Lab per la distribuzione di attestazioni tra foreste  

### <a name="BKMK_2.1"></a>Creare una macchina virtuale per DC2  

-   Creare una macchina virtuale dall'ISO di Windows Server 2012.  

-   Assegnare alla macchina virtuale il nome DC2.  

-   Connettere la macchina virtuale alla rete ID_AD_Network.  

> [!IMPORTANT]  
> Per aggiungere macchine virtuali a un dominio e distribuire tipi di attestazione tra foreste, è necessario che le macchine virtuali siano in grado di risolvere i nomi di dominio completi (FQDN) dei domini pertinenti. A questo scopo potrebbe essere necessario configurare manualmente le impostazioni del DNS nelle macchine virtuali. Per altre informazioni, vedere [Configurazione di una rete virtuale](https://technet.microsoft.com/library/cc732470%28v=ws.10%29.aspx).  
>   
> Tutte le immagini macchina virtuale (server e client) devono essere riconfigurate in modo da usare un indirizzo IP statico versione 4 (IPv4) e le impostazioni client DNS (Domain Name System). Per altre informazioni, vedere [Configurare un client DNS con indirizzo IP statico](https://go.microsoft.com/fwlink/?LinkId=150952).  

### <a name="BKMK_2.2"></a>Configurare una nuova foresta denominata adatum.com  

##### <a name="to-install-active-directory-domain-services"></a>Per installare Servizi di dominio Active Directory  

1. Connettere la macchina virtuale alla rete ID_AD_Network. Accedere a DC2 come amministratore con la password <strong>Pass@word1</strong>.  

2. In Server Manager fare clic su **Gestione**e quindi su **Aggiungi ruoli e funzionalità**.  

3. Nella pagina **Prima di iniziare**, fare clic su **Avanti**.  

4. Nella pagina **Selezione tipo di installazione** selezionare **Installazione basata su ruoli o basata su funzionalità**e quindi fare clic su **Avanti**.  

5. Nella pagina **Selezione server di destinazione** fare clic su **Selezionare un server dal pool di server**, fare clic sul nome del server in cui si vuole installare Servizi di dominio Active Directory e quindi fare clic su **Avanti**.  

6. Nella pagina **Selezione ruoli server** fare clic su **Servizi di dominio Active Directory**. Nella finestra di dialogo **Aggiunta guidata ruoli e funzionalità** fare clic su **Aggiungi funzionalità** e quindi su **Avanti**.  

7. Nella pagina **Selezione funzionalità** fare clic su **Avanti**.  

8. Nella pagina **Servizi di dominio Active Directory** esaminare le informazioni e quindi fare clic su **Avanti**.  

9. Nella pagina **Conferma** fare clic su **Installa**. La barra di stato di Installazione funzionalità nella pagina Risultati indica che il ruolo è in fase di installazione.  

10. Nella pagina **Risultati** verificare che l'installazione sia riuscita, quindi fare clic sull'icona di avviso con un punto esclamativo nell'angolo superiore destro dello schermo, accanto a **Gestisci**. Nell'elenco Attività fare clic sul collegamento **Alza di livello il server a controller di dominio**.  

    > [!IMPORTANT]  
    > Se si chiude l'installazione guidata a questo punto invece di fare clic su **Alza di livello il server a controller di dominio**, sarà possibile continuare l'installazione di Servizi di dominio Active Directory facendo clic su **Attività** in Server Manager.  

11. Nella pagina **Configurazione distribuzione** fare clic su **Aggiungi una nuova foresta**, digitare il nome del dominio radice, **adatum.com**, e quindi fare clic su **Avanti**.  

12. Nella pagina **Opzioni controller di dominio** selezionare i livelli di funzionalità dominio e foresta come Windows Server 2012, specificare la password della modalità ripristino servizi directory <strong>pass@word1</strong>, quindi fare clic su **Avanti**.  

13. Nella pagina **Opzioni DNS** fare clic su **Avanti**.  

14. Nella pagina **Opzioni aggiuntive** fare clic su **Avanti**.  

15. Nella pagina **Percorsi** digitare i percorsi del database di Active Directory, dei file di log e della cartella SYSVOL (o accettare i percorsi predefiniti), quindi fare clic su **Avanti**.  

16. Nella pagina **Verifica opzioni** verificare le selezioni e fare clic su **Avanti**.  

17. Nella pagina **Controllo dei prerequisiti** verificare che la convalida dei prerequisiti sia stata completata, quindi fare clic su **Installa**.  

18. Nella pagina **Risultati** verificare che il server sia stato configurato correttamente come controller di dominio e quindi fare clic su **Chiudi**.  

19. Riavviare il server per completare l'installazione di Servizi di dominio Active Directory. Per impostazione predefinita, il server viene riavviato automaticamente.  

> [!IMPORTANT]  
> Per verificare che la rete sia configurata correttamente, dopo aver configurato entrambe le foreste eseguire le operazioni seguenti:  
>   
> -   Accedere ad adatum.com come adatum\administrator. Aprire una finestra del prompt dei comandi, digitare **nslookup contoso.com** e premere INVIO.  
> -   Accedere a contoso.com come contoso\administrator. Aprire una finestra del prompt dei comandi, digitare **nslookup adatum.com**e premere INVIO.  
>   
> Se questi comandi vengono eseguiti senza errori, significa che le foreste possono comunicare tra loro. Per altre informazioni sugli errori nslookup, vedere la sezione relativa alla risoluzione dei problemi nell'argomento [Uso di NSlookup.exe](https://support.microsoft.com/kb/200525)  

### <a name="BKMK_2.22"></a>Impostare contoso.com come foresta trusting su adatum.com  
In questo passaggio si creerà una relazione di trust tra il sito Adatum Corporation e il sito Contoso, Ltd.  

##### <a name="to-set-contoso-as-a-trusting-forest-to-adatum"></a>Per impostare contoso.com come foresta trusting per adatum.com  

1.  Accedere a DC2 come amministratore. Nella schermata **Start** digitare domain.msc.  

2.  Nell'albero della console fare clic con il pulsante destro del mouse su adatum.com e scegliere Proprietà.  

3.  Nella scheda **Trust** fare clic su **Nuova relazione di trust**e quindi su **Avanti**.  

4.  Nella pagina **Nome trust** digitare **contoso.com**nel campo del nome DNS (Domain Name System) e quindi fare clic su **Avanti**.  

5.  Nella pagina **Tipo di trust** fare clic su **Trust tra foreste** e quindi su **Avanti**.  

6.  Nella pagina **Direzione del trust** fare clic su **Bidirezionale**.  

7.  Nella pagina **Parti del trust** fare clic su **Questo dominio e il dominio specificato**e quindi su **Avanti**.  

8.  Continuare a seguire le istruzioni della procedura guidata.  

### <a name="BKMK_2.4"></a>Creare altri utenti nella foresta adatum  
Creare l'utente Jeff low con la password <strong>pass@word1</strong>e assegnare l'attributo Company con il valore **adatum**.  

##### <a name="to-create-a-user-with-the-company-attribute"></a>Per creare un utente con l'attributo Company  

1.  Aprire un prompt dei comandi con privilegi elevati in Windows PowerShell e incollare il codice seguente:  

    ```  
    New-ADUser `  
    -SamAccountName jlow `  
    -Name "Jeff Low" `  
    -UserPrincipalName jlow@adatum.com `  
    -AccountPassword (ConvertTo-SecureString `  
    -AsPlainText "pass@word1" -Force) `  
    -Enabled $true `  
    -PasswordNeverExpires $true `  
    -Path 'CN=Users,DC=adatum,DC=com' `  
    -Company Adatum`  

    ```  

### <a name="BKMK_2.5"></a>Creare il tipo di attestazione Company in adataum.com  

##### <a name="to-create-a-claim-type-by-using-windows-powershell"></a>Per creare un tipo di attestazione mediante Windows PowerShell  

1.  Accedere ad adatum.com come amministratore.  

2.  Aprire un prompt dei comandi con privilegi elevati in Windows PowerShell e digitare il codice seguente:  

    ```  
    New-ADClaimType `  
    -AppliesToClasses:@('user') `  
    -Description:"Company" `  
    -DisplayName:"Company" `  
    -ID:"ad://ext/Company:ContosoAdatum" `  
    -IsSingleValued:$true `  
    -Server:"adatum.com" `  
    -SourceAttribute:Company `  
    -SuggestedValues:@((New-Object Microsoft.ActiveDirectory.Management.ADSuggestedValueEntry("Contoso", "Contoso", "")), (New-Object Microsoft.ActiveDirectory.Management.ADSuggestedValueEntry("Adatum", "Adatum", ""))) `  

    ```  

### <a name="BKMK_2.55"></a>Abilitare la proprietà della risorsa Company in contoso.com  

##### <a name="to-enable-the-company-resource-property-on-contosocom"></a>Per abilitare la proprietà della risorsa Company in contoso.com  

1.  Accedere a contoso.com come amministratore.  

2.  In Server Manager fare clic su **Strumenti** e quindi su **Centro di amministrazione di Active Directory**.  

3.  Nel riquadro sinistro del Centro di amministrazione di Active Directory fare clic su **Visualizzazione albero**. Nel riquadro sinistro fare clic su **Controllo di accesso dinamico** e quindi fare doppio clic su **Proprietà risorse**.  

4.  Selezionare **Company** nell'elenco **Proprietà risorse** , fare clic con il pulsante destro del mouse e scegliere **Proprietà**. Nella sezione **Valori suggeriti** fare clic su **Aggiungi** per aggiungere i valori suggeriti: Contoso e Adatum, quindi fare clic su **OK** due volte.  

5.  Selezionare **Company** nell'elenco **Proprietà risorse**, fare clic con il pulsante destro del mouse e scegliere **Abilita**.  

### <a name="BKMK_2.6"></a>Abilitare il controllo dinamico degli accessi in adatum.com  

##### <a name="to-enable-dynamic-access-control-for-adatumcom"></a>Per abilitare il controllo dinamico degli accessi per adatum.com  

1.  Accedere ad adatum.com come amministratore.  

2.  Aprire la Console Gestione Criteri di gruppo, fare clic su **adatum.com** e quindi fare doppio clic su **Controller di dominio**.  

3.  Fare clic con il pulsante destro del mouse su **Criterio Controller di domini predefiniti**e scegliere **Modifica**.  

4.  Nella finestra Editor Gestione Criteri di gruppo fare doppio clic su **Configurazione computer**, quindi su **Criteri**, **Modelli amministrativi**, **Sistema** e infine su **KDC**.  

5.  Fare doppio clic su **Supporto KDC di attestazioni, autenticazione composta e blindatura Kerberos** e selezionare l'opzione accanto a **Abilitato**. Questa impostazione deve essere abilitata per poter usare i criteri di accesso centrale.  

6.  Aprire un prompt dei comandi con privilegi elevati ed eseguire il comando seguente:  

    ```  
    gpupdate /force  
    ```  

### <a name="BKMK_2.8"></a>Creare il tipo di attestazione Company in contoso.com  

##### <a name="to-create-a-claim-type-by-using-windows-powershell"></a>Per creare un tipo di attestazione mediante Windows PowerShell  

1.  Accedere a contoso.com come amministratore.  

2.  Aprire un prompt dei comandi con privilegi elevati in Windows PowerShell e digitare il codice seguente:  

    ```  
    New-ADClaimType '"SourceTransformPolicy `  
    '"DisplayName 'Company' `  
    '"ID 'ad://ext/Company:ContosoAdatum' `  
    '"IsSingleValued $true `  
    '"ValueType 'string' `  

    ```  

### <a name="BKMK_2.9"></a>Creare la regola di accesso centrale  

##### <a name="to-create-a-central-access-rule"></a>Per creare una regola di accesso centrale  

1. Nel riquadro sinistro del Centro di amministrazione di Active Directory fare clic su **Visualizzazione albero**. Nel riquadro sinistro fare clic su **Controllo di accesso dinamico** e quindi su **Regole di accesso centrale**.  

2. Fare clic con il pulsante destro del mouse su **Regole di accesso centrale**, scegliere **Nuova**e quindi **Regola di accesso centrale**.  

3. Nel campo **Nome** digitare **AdatumEmployeeAccessRule**.  

4. Nella sezione **Autorizzazioni** selezionare l'opzione **Usa queste autorizzazioni come correnti**, fare clic su **Modifica** e quindi su **Aggiungi**. Fare clic sul collegamento **Seleziona un'entità** , digitare **Authenticated Users**e fare clic su **OK**.  

5. Nella finestra di dialogo **Voce autorizzazione per Autorizzazioni** fare clic su **Aggiungi condizione**, quindi immettere le condizioni seguenti: [**User**] [**Company**] [**Equals**] [**Value**] [**Adatum**]. Le autorizzazioni devono essere **Modifica, Lettura/esecuzione, Lettura, Scrittura**.  

6. Fare clic su **OK**.  

7. Fare tre volte clic su **OK** per completare la procedura, quindi tornare al Centro di amministrazione di Active Directory.  

   ![solution guide i](media/Appendix-B--Setting-Up-the-Test-Environment/PowerShellLogoSmall.gif)***<em>comandi equivalenti di Windows PowerShell</em>***  

   Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.  

   ```  
   New-ADCentralAccessRule `  
   -CurrentAcl:"O:SYG:SYD:AR(A;;FA;;;OW)(A;;FA;;;BA)(A;;FA;;;SY)(XA;;0x1301bf;;;AU;(@USER.ad://ext/Company:ContosoAdatum == `"Adatum`"))" `  
   -Name:"AdatumEmployeeAccessRule" `  
   -ProposedAcl:$null `  
   -ProtectedFromAccidentalDeletion:$true `  
   -Server:"contoso.com" `  
   ```  

### <a name="BKMK_2.10"></a>Creare i criteri di accesso centrale  

##### <a name="to-create-a-central-access-policy"></a>Per creare i criteri di accesso centrale  

1.  Accedere a contoso.com come amministratore.  

2.  Aprire un prompt dei comandi con privilegi elevati in Windows PowerShell e incollare il codice seguente:  

    ```  
    New-ADCentralAccessPolicy "Adatum Only Access Policy"   
    Add-ADCentralAccessPolicyMember "Adatum Only Access Policy" `  
    -Member "AdatumEmployeeAccessRule" `  
    ```  

### <a name="BKMK_2.11"></a>Pubblicare i nuovi criteri tramite Criteri di gruppo  

##### <a name="to-apply-the-central-access-policy-across-file-servers-through-group-policy"></a>Per applicare i criteri di accesso centrale nei file server mediante Criteri di gruppo  

1.  Nella schermata **Start** digitare **Strumenti di amministrazione**e nella barra **Cerca** fare clic su **Impostazioni**. Nei risultati di **Impostazioni** fare clic su **Strumenti di amministrazione**. Aprire la Console Gestione Criteri di gruppo dalla cartella **Strumenti di amministrazione** .  

    > [!TIP]  
    > Se l'impostazione **Mostra strumenti di amministrazione** è disabilitata, la cartella Strumenti di amministrazione e il relativo contenuto non verranno visualizzati nei risultati di **Impostazioni**.  

2.  Fare clic con il pulsante destro del mouse sul dominio contoso.com, scegliere **Crea un oggetto Criteri di gruppo in questo dominio e collegarlo qui** .  

3.  Digitare un nome descrittivo per l'oggetto Criteri di gruppo, ad esempio **AdatumAccessGPO**, e quindi fare clic su **OK**.  

##### <a name="to-apply-the-central-access-policy-to-the-file-server-through-group-policy"></a>Per applicare i criteri di accesso centrale al file server mediante Criteri di gruppo  

1.  Nella schermata **Start** digitare **Gestione criteri di gruppo** nella casella **Cerca**. Aprire **Gestione criteri di gruppo** dalla cartella Strumenti di amministrazione.  

    > [!TIP]  
    > Se l'impostazione **Mostra strumenti di amministrazione** è disabilitata, la cartella Strumenti di amministrazione e il relativo contenuto non verranno visualizzati nei risultati di Impostazioni.  

2.  Individuare e selezionare Contoso nel modo seguente: Gestione Criteri di gruppo\Foresta: contoso.com\Domains\contoso.com.  

3.  Fare clic con il pulsante destro del mouse sul criterio **AdatumAccessGPO** e scegliere **Modifica**.  

4.  Nell'Editor Gestione Criteri di gruppo fare clic su **Configurazione computer**, espandere **Criteri**, espandere **Impostazioni di Windows**e quindi fare clic su **Impostazioni sicurezza**.  

5.  Espandere **File system**, fare clic con il pulsante destro del mouse su **Central Access Policy** e quindi scegliere **Gestisci criteri di accesso centrale**.  

6.  Nella finestra di dialogo **Configurazione criteri di accesso centrale** fare clic su **Aggiungi**, selezionare **Adatum Only Access Policy**e quindi fare clic su **OK**.  

7.  Chiudi l'Editor Gestione Criteri di gruppo. I criteri di accesso centrale sono stati aggiunti in Criteri di gruppo.  

### <a name="BKMK_2.12"></a>Creare la cartella guadagni nel file server  
Creare un nuovo volume NTFS in FILE1 e quindi creare la cartella seguente: D:\Earnings.  

> [!NOTE]  
> Per impostazione predefinita, i criteri di accesso centrale non sono abilitati nel volume di sistema o di avvio C:.  

### <a name="BKMK_2.13"></a>Impostare la classificazione e applicare i criteri di accesso centrale nella cartella guadagni  

##### <a name="to-assign-the-central-access-policy-on-the-file-server"></a>Per assegnare i criteri di accesso centrale nel file server  

1. Nella Console di gestione di Hyper-V connettersi al server FILE1. Accedere al server usando Contoso\Administrator con la password <strong>pass@word1</strong>.  

2. Aprire un prompt dei comandi con privilegi elevati e digitare: **gpupdate /force**. In questo modo le modifiche a Criteri di gruppo saranno applicate al server.  

3. È anche necessario aggiornare le proprietà delle risorse globali da Active Directory. Aprire Windows PowerShell, digitare `Update-FSRMClassificationpropertyDefinition`e quindi premere INVIO. Chiudere Windows PowerShell.  

4. Aprire Esplora risorse e passare a D:\EARNINGS. Fare clic con il pulsante destro del mouse sulla cartella **Earnings** e quindi scegliere **Proprietà**.  

5. Fare clic sulla scheda **Classificazione**. Selezionare Companye quindi selezionare **Adatum** nel campo **Valore** .  

6. Fare clic su **Modifica**, selezionare **Adatum Only Access Policy** nel menu a discesa e quindi fare clic su **Applica**.  

7. Fare clic sulla scheda **Sicurezza**, quindi su **Avanzate** e infine fare clic sulla scheda **Criteri centrali**. La voce **AdatumEmployeeAccessRule** dovrebbe essere elencata. È possibile espanderla per visualizzate tutte le autorizzazioni impostate durante la creazione della regola in Active Directory.  

8. Fare clic su **OK** per tornare a Esplora risorse.  



