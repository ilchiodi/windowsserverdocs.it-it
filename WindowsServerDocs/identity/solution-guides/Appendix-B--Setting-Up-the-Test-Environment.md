---
ms.assetid: 82918181-525d-4e93-af96-957dac6aedb6
title: Appendice B configurazione dell'ambiente di Test
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: deb08b0663e5f349df7cce51ddabd4aae7f624c5
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="appendix-b-setting-up-the-test-environment"></a>Appendice b: configurazione dell'ambiente di Test

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento illustra i passaggi per configurare un laboratorio pratico per testare il controllo dinamico degli accessi. Le istruzioni devono essere seguite in sequenza perché esistono numerose dipendenze tra componenti.  
  
## <a name="prerequisites"></a>Prerequisiti  
**Requisiti hardware e software**  
  
Requisiti per la configurazione dell'ambiente di testing:  
  
-   Un server host che eseguono Windows Server 2008 R2 con SP1 e Hyper-V  
  
-   Una copia dell'immagine ISO Windows Server 2012  
  
-   Una copia dell'immagine ISO Windows 8  
  
-   Microsoft Office 2010  
  
-   Un server che esegue Microsoft Exchange Server 2003 o versione successiva  
  
È necessario creare le macchine virtuali seguenti per testare gli scenari di controllo dinamico degli accessi:  
  
-   DC1 (controller di dominio)  
  
-   DC2 (controller di dominio)  
  
-   File1 (file server e Active Directory Rights Management Services)  
  
-   SRV1 (server POP3 e SMTP)  
  
-   CLIENT1 (computer client con Microsoft Outlook)  
  
Le password per le macchine virtuali devono essere come segue:  
  
-   BUILTIN\Administrator:pass@word1  
  
-   Contoso\Administrator:pass@word1  
  
-   Tutti gli altri account:pass@word1  
  
## <a name="build-the-test-lab-virtual-machines"></a>Creare macchine virtuali del laboratorio di test  
  
### <a name="install-the-hyper-v-role"></a>Installare il ruolo Hyper-V  
È necessario installare il ruolo Hyper-V in un computer che esegue Windows Server 2008 R2 con SP1.  
  
##### <a name="to-install-the-hyper-v-role"></a>Per installare il ruolo Hyper-V  
  
1.  Fare clic su **Start**, quindi fare clic su Server Manager.  
  
2.  Nell'area Riepilogo ruoli della finestra principale di Server Manager, fare clic su **Aggiungi ruoli**.  
  
3.  Nel **Selezione ruoli Server** pagina, fare clic su **Hyper-V**.  
  
4.  Nel **creare reti virtuali** pagina, fare clic su uno o più schede di rete se si desidera effettuare la connessione di rete disponibile per macchine virtuali.  
  
5.  Nel **Conferma selezioni per l'installazione** pagina, fare clic su **installare**.  
  
6.  Per completare l'installazione, è necessario riavviare il computer. Fare clic su **Chiudi** per completare la procedura guidata, quindi fare clic su **Sì** per riavviare il computer.  
  
7.  Dopo aver riavviato il computer, accedi con lo stesso account utilizzato per installare il ruolo. Dopo la ripresa guidata configurazione completamento dell'installazione, fare clic su **Chiudi** per completare la procedura guidata.  
  
### <a name="create-an-internal-virtual-network"></a>Creare una rete virtuale interna  
Ora si creerà una rete virtuale interna denominata ID_AD_Network.  
  
##### <a name="to-create-a-virtual-network"></a>Per creare una rete virtuale  
  
1.  Aprire Gestione Hyper-V.  
  
2.  Dal **azioni** menu, fare clic su **Gestione reti virtuali**.  
  
3.  In **crea rete virtuale**, selezionare il **interna**.  
  
4.  Fare clic su **aggiungere**. Il **nuova rete virtuale** verrà visualizzata la pagina.  
  
5.  Tipo **ID_AD_Network** come nome della nuova rete. Esaminare le altre proprietà e modificarle se necessario.  
  
6.  Fare clic su **OK** per creare la rete virtuale e chiudere Gestione reti virtuali oppure fare clic su **applica** per creare la rete virtuale e continuare a usare Gestione reti virtuali.  
  
### <a name="BKMK_Build"></a>Creare il controller di dominio  
Creare una macchina virtuale da utilizzare come controller di dominio (DC1). Installare la macchina virtuale usando l'ISO di Windows Server 2012 e assegnarle il nome DC1.  
  
##### <a name="to-install-active-directory-domain-services"></a>Per installare servizi di dominio Active Directory  
  
1.  Connettere la macchina virtuale alla rete ID_AD_Network. Accedere a DC1 come Administrator con la password ** pass@word1 **.  
  
2.  In Server Manager, fare clic su **Gestisci**, quindi fare clic su **Aggiungi ruoli e funzionalità**.  
  
3.  Nel **prima di iniziare** pagina, fare clic su **Avanti**.  
  
4.  Nel **Selezione tipo di installazione** pagina, fare clic su **installazione basata su ruoli o basata su funzionalità**, quindi fare clic su **Avanti**.  
  
5.  Nel **server di destinazione** pagina, fare clic su **Avanti**.  
  
6.  Nel **Selezione ruoli server** pagina, fare clic su **servizi di dominio Active Directory**. Nel **Aggiunta guidata ruoli e funzionalità** la finestra di dialogo, fare clic su **Aggiungi funzionalità**e quindi fare clic su **Avanti**.  
  
7.  Nel **selezionare le funzionalità** pagina, fare clic su **Avanti**.  
  
8.  Nel **servizi di dominio Active Directory** pagina, esaminare le informazioni e quindi fare clic su **Avanti**.  
  
9. Nel **Conferma selezioni per l'installazione** pagina, fare clic su **installare**. L'indicatore di stato di installazione funzionalità nella pagina risultati indica che il ruolo viene installato.  
  
10. Nel **risultati** pagina, verificare che l'installazione ha avuto esito positivo, fare clic su **Chiudi**. In Server Manager fare clic sull'icona di avviso con un punto esclamativo nell'angolo superiore destro dello schermo, accanto a **Gestisci**. Nell'elenco attività, fare clic su di **promuovere il server a un controller di dominio** collegamento.  
  
11. Nel **configurazione della distribuzione** pagina, fare clic su **aggiungere una nuova foresta**, digitare il nome del dominio radice della **contoso.com**, quindi fare clic su **Avanti**.  
  
12. Nel **opzioni Controller di dominio** pagina, selezionare i livelli di funzionalità foresta e del dominio di Windows Server 2012, specificare la password DSRM ** pass@word1 **, quindi fare clic su **Avanti**.  
  
13. Nel **opzioni DNS** pagina, fare clic su **Avanti**.  
  
14. Nel **opzioni aggiuntive** pagina, fare clic su **Avanti**.  
  
15. Nel **percorsi** pagina, digitare i percorsi per il database di Active Directory, file di log e della cartella SYSVOL (o accettare i percorsi predefiniti), quindi fare clic su **Avanti**.  
  
16. Nel **verifica opzioni** pagina, confermare le selezioni e quindi fare clic su **Avanti**.  
  
17. Nel **controllo dei prerequisiti** pagina, verificare che la convalida dei prerequisiti viene completata e quindi fare clic su **installare**.  
  
18. Nel **risultati** pagina, verificare che il server è stato configurato correttamente come controller di dominio e quindi fare clic su **Chiudi**.  
  
19. Riavviare il server per completare l'installazione di servizi di dominio Active Directory. (Per impostazione predefinita, questo avviene automaticamente.)  
  
Creare gli utenti seguenti mediante Centro di amministrazione di Active Directory.  
  
##### <a name="create-users-and-groups-on-dc1"></a>Creare utenti e gruppi in DC1  
  
1.  Accedere a contoso.com come amministratore. Avviare Centro di amministrazione di Active Directory.  
  
2.  Creare gruppi di sicurezza seguenti:  
  
    |Nome del gruppo|Indirizzo di posta elettronica|  
    |--------------|-----------------|  
    |FinanceAdmin|financeadmin@contoso.com|  
    |FinanceException|financeexception@contoso.com|  
  
3.  Creare una unità organizzativa (OU) seguenti:  
  
    |Nome di unità Organizzativa|Computer|  
    |-----------|-------------|  
    |FileServerOU|FILE1|  
  
4.  Creare gli utenti seguenti con gli attributi indicati:  
  
    |Utente|Nome utente|Indirizzo di posta elettronica|Reparto|Gruppo|Paese/area geografica|  
    |--------|------------|-----------------|--------------|---------|-------------------|  
    |Myriam Delesalle|MDelesalle|MDelesalle@contoso.com|Finanza||CI|  
    |Miles Reid|MReid|MReid@contoso.com|Finanza|FinanceAdmin|CI|  
    |Esther Valle|EValle|EValle@contoso.com|Operazioni|FinanceException|CI|  
    |Maira Wenzel|MWenzel|MWenzel@contoso.com|RISORSE UMANE||CI|  
    |Jeff Low|JLow|JLow@contoso.com|RISORSE UMANE||CI|  
    |Server RMS|RMS|rms@contoso.com||||  
  
    For more information about creating security groups, see [Create a New Group](https://technet.microsoft.com/library/dd861305.aspx) on the Windows Server website.  
  
##### <a name="to-create-a-group-policy-object"></a>Per creare un oggetto Criteri di gruppo  
  
1.  Passare il cursore sull'angolo superiore destro dello schermo e fare clic sull'icona di ricerca. Nella casella di ricerca, digitare **Gestione criteri di gruppo**e fare clic su **Gestione criteri di gruppo**.  
  
2.  Espandere **foresta: contoso.com**, quindi espandere **domini**, passare a **contoso.com**, espandere **(contoso.com)**e quindi seleziona **FileServerOU**. Fare doppio clic su **crea un oggetto Criteri di gruppo in questo dominio e collegarlo qui**
  
3.  Digitare un nome descrittivo per l'oggetto Criteri di gruppo, ad esempio **FlexibleAccessGPO**, quindi fare clic su **OK**.  
  
##### <a name="to-enable-dynamic-access-control-for-contosocom"></a>Per abilitare il controllo dinamico degli accessi per contoso.com  
  
1.  Aprire la Console Gestione criteri di gruppo, fare clic su **contoso.com**e quindi fare doppio clic su **i controller di dominio**.  
  
2.  Fare doppio clic su **criterio controller di dominio predefiniti**e seleziona **modifica**.  
  
3.  Nella finestra Editor Gestione criteri di gruppo, fare doppio clic su **configurazione Computer**, fare doppio clic su **criteri**, fare doppio clic su **modelli amministrativi**, fare doppio clic su **sistema**e quindi fare doppio clic su **KDC**.  
  
4.  Fare doppio clic su **supporto KDC per attestazioni, autenticazione composta e blindatura Kerberos** e selezionare l'opzione accanto a **abilitato**. È necessario abilitare questa impostazione per utilizzare criteri di accesso centrale.  
  
5.  Aprire un prompt dei comandi con privilegi elevati ed eseguire il comando seguente:  
  
    ```  
    gpupdate /force  
    ```  
  
### <a name="BKMK_FS1"></a>Creare il file server e il server AD RMS (FILE1)  
  
1.  Creare una macchina virtuale con nome FILE1 dall'immagine ISO Windows Server 2012.  
  
2.  Connettere la macchina virtuale alla rete ID_AD_Network.  
  
3.  Aggiungere la macchina virtuale al dominio contoso.com e quindi accedere a FILE1 come contoso\administrator specificando la password ** pass@word1 **.  
  
#### <a name="install-file-services-resource-manager"></a>Installare Gestione risorse File server  
  
###### <a name="to-install-the-file-services-role-and-the-file-server-resource-manager"></a>Per installare il ruolo Servizi File e Gestione risorse File Server  
  
1.  In Server Manager, fare clic su **Aggiungi ruoli e funzionalità**.  
  
2.  Nel **prima di iniziare** pagina, fare clic su **Avanti**.  
  
3.  Nel **Selezione tipo di installazione** pagina, fare clic su **Avanti**.  
  
4.  Nel **server di destinazione** pagina, fare clic su **Avanti**.  
  
5.  Nel **Selezione ruoli Server** espandere **servizi File e archiviazione**, selezionare la casella di controllo accanto a **File e iSCSI servizi**, espandere e selezionare **Gestione risorse File Server**.  
  
    Nell'Aggiunta guidata ruoli e funzionalità, fare clic su **Aggiungi funzionalità**, quindi fare clic su **Avanti**.  
  
6.  Nel **selezionare le funzionalità** pagina, fare clic su **Avanti**.  
  
7.  Nel **Conferma selezioni per l'installazione** pagina, fare clic su **installare**.  
  
8.  Nel **lo stato dell'installazione** pagina, fare clic su **Chiudi**.  
  
#### <a name="install-the-microsoft-office-filter-packs-on-the-file-server"></a>Installare Microsoft Office Filter Pack nel file server  
È necessario installare Microsoft Office Filter Pack in Windows Server 2012 per abilitare gli IFilter per una gamma più ampia di file di Office rispetto a quelli forniti per impostazione predefinita.  Windows Server 2012 non dispone degli IFilter per i file di Microsoft Office installato per impostazione predefinita e Usa l'infrastruttura di classificazione file per eseguire l'analisi dei contenuti.  
  
To download and install the IFilters, see [Microsoft Office 2010 Filter Packs](https://go.microsoft.com/fwlink/?LinkID=234122).  
  
#### <a name="configure-email-notifications-on-file1"></a>Configurare le notifiche di posta elettronica in FILE1  
Quando si creano quote e screening dei file, hai la possibilità di inviare notifiche tramite posta elettronica agli utenti quando sta per raggiungere il limite di quota o quando tentano di salvare i file che sono stati bloccati. Se si vuole avvisare regolarmente alcuni amministratori di quota e gli eventi di screening dei file, è possibile configurare uno o più destinatari predefiniti. Per inviare queste notifiche, è necessario specificare il server SMTP da utilizzare per l'inoltro di messaggi di posta elettronica.  
  
###### <a name="to-configure-email-options-in-file-server-resource-manager"></a>Per configurare le opzioni di posta elettronica in Gestione risorse File Server  
  
1.  Aprire Gestione risorse File Server. Per aprire Gestione risorse File Server, fare clic su **Start**, tipo **Gestione risorse file server**, quindi fare clic su **Gestione risorse File Server**.  
  
2.  Nell'interfaccia di gestione risorse File Server, fare doppio clic su **Gestione risorse File Server**, quindi fare clic su **configurare le opzioni**. Il **opzioni di gestione risorse File Server** apre la finestra di dialogo.  
  
3.  Nel **notifiche tramite posta elettronica** scheda, in nome server SMTP o indirizzo IP, digitare il nome host o l'indirizzo IP del server SMTP che inoltrerà le notifiche di posta elettronica.  
  
4.  If you want to routinely notify certain administrators of quota or file screening events, under **Default administrator recipients**, type each email address such as fileadmin@contoso.com. Use the format account@domain, and use semicolons to separate multiple accounts.  
  
#### <a name="create-groups-on-file1"></a>Creare gruppi in FILE1  
  
###### <a name="to-create-security-groups-on-file1"></a>Per creare gruppi di sicurezza in FILE1  
  
1.  Accedere a FILE1 come contoso\administrator, con la password: ** pass@word1 **.  
  
2.  Aggiungere NT AUTHORITY\Authenticated Users al **winrmremotewmiusers _** gruppo.  
  
#### <a name="create-files-and-folders-on-file1"></a>Creare file e cartelle in FILE1  
  
1.  Creare un nuovo volume NTFS in FILE1 e quindi creare la cartella seguente: D:\Finance Documents.  
  
2.  Creare i file seguenti con i dettagli specificati:  
  
    -   **Finance Memo.docx**: aggiungere testo nel documento. Ad esempio, ' le regole aziendali sulle chi può accedere a documenti finanziari sono stati modificati. I documenti finanziari sono ora accessibili solo ai membri del gruppo FinanceExpert. Nessun altro reparto o gruppi hanno accesso. " È necessario valutare l'impatto della modifica prima di implementarli nell'ambiente. Assicurarsi che in questo documento è CONTOSO CONFIDENTIAL come piè di pagina in ogni pagina.  
  
    -   **Request for Approval to Hire.docx**: creare un modulo in questo documento che raccoglie le informazioni sui candidati. È necessario disporre i campi seguenti del documento: **Applicant Name, Social Security number, posizione professionale, Proposed Salary, Starting Date, Supervisor name, Department**. Aggiungere un'altra sezione del documento che dispone di un modulo per **Supervisor Signature, Approved Salary, Conformation of Offer**, e **Status of Offer**.   
        Rendere il documento abilitare rights management.  
  
    -   **Word Document1.docx**: aggiungere del contenuto di prova al documento.  
  
    -   **Word Document2.docx**: aggiungere contenuto di prova al documento.  
  
    -   **Workbook1**  
  
    -   **Workbook2.xlsx**  
  
    -   Creare una cartella sul desktop denominato Regular Expressions. Creare un documento di testo sotto la cartella **RegEx-SSN**. Digitare il seguente contenuto nel file, quindi salvare e chiudere il file:   
        ^(?! 000)([0-7]\d{2}|7([0-7]\d|7[012])) ([-]?) (?! 00) \d\d\3 (?! 0000) \d {4}$  
  
3.  Condividere la cartella D:\Finance Documents come Finance Documents e concedere a tutti in lettura e l'accesso in scrittura alla condivisione.  
  
> [!NOTE]  
> Criteri di accesso centrale non sono abilitati per impostazione predefinita nel sistema o il volume c: di avvio.  
  
#### <a name="BKMK_CS1"></a>Installare Active Directory Rights Management Services  
Aggiungere Active Directory Rights Management Services (AD RMS) e tutte le funzionalità necessarie mediante Server Manager. Scegliere tutte le impostazioni predefinite.  
  
###### <a name="to-install-active-directory-rights-management-services"></a>Per installare Active Directory Rights Management Services  
  
1.  Accedere a FILE1 come CONTOSO\Administrator o come membro del gruppo Domain Admins.  
  
    > [!IMPORTANT]  
    > Per installare il ruolo server AD RMS il programma di installazione account (in questo caso CONTOSO\Administrator) deve essere membro sia il gruppo Administrators locale sul computer del server AD RMS, in cui viene installato l'appartenenza al gruppo Enterprise Admins in Active Directory.  
  
2.  In Server Manager, fare clic su **Aggiungi ruoli e funzionalità**. Viene visualizzata l'aggiunta guidata ruoli e funzionalità.  
  
3.  Nel **prima di iniziare** schermata, fare clic su **Avanti**.  
  
4.  Nel **Selezione tipo di installazione** schermata, fare clic su **ruolo o funzionalità in base installare**e quindi fare clic su **Avanti**.  
  
5.  Nel **selezione Server di destinazione** schermata, fare clic su **Avanti**.  
  
6.  Nel **Selezione ruoli Server** schermata, selezionare la casella accanto a **Active Directory Rights Management Services**, quindi fare clic su **Avanti**.  
  
7.  Nel **Aggiungi funzionalità necessarie per Active Directory Rights Management Services? ** la finestra di dialogo, fare clic su **Aggiungi funzionalità**.  
  
8.  Nel **Selezione ruoli Server** schermata, fare clic su **Avanti**.  
  
9. Nel **selezionare le funzionalità da installare** schermata, fare clic su **Avanti**.  
  
10. Nel **Active Directory Rights Management Services** schermata, fare clic su Avanti.  
  
11. Nel **Selezione servizi ruolo** schermata, fare clic su **Avanti**.  
  
12. Nel **ruolo Server Web (IIS)** schermata, fare clic su **Avanti**.  
  
13. Nel **Selezione servizi ruolo** schermata, fare clic su **Avanti**.  
  
14. Nel **Conferma selezioni per l'installazione** schermata, fare clic su **installare**.  
  
15. Dopo che è stata completata l'installazione, scegliere il **lo stato dell'installazione** schermata, fare clic su **eseguire la configurazione aggiuntiva**. Verrà visualizzata la configurazione guidata di AD RMS.  
  
16. Nel **AD RMS** schermata, fare clic su **Avanti**.  
  
17. Nel **Cluster AD RMS** schermo, seleziona **creare un nuovo cluster radice AD RMS** e quindi fare clic su **Avanti**.  
  
18. Nel **Database di configurazione** schermata, fare clic su **Usa Database interno di Windows in questo server**e quindi fare clic su **Avanti**.  
  
    > [!NOTE]  
    > È consigliabile utilizzare Database interno di Windows per gli ambienti di test solo perché non supporta più di un server del cluster AD RMS. Le distribuzioni di produzione è necessario utilizzare un server di database diverso.  
  
19. Nel **Account del servizio** schermata **Account utente di dominio**, fare clic su **specificare** e quindi specificare il nome utente (**contoso\rms**) e la Password (**pass@word1**) e fare clic su **OK**e quindi fare clic su **Avanti**.  
  
20. Nel **modalità crittografia** schermata, fare clic su **modalità crittografia 2**.  
  
21. Nel **archivio chiavi Cluster** schermata, fare clic su **Avanti**.  
  
22. Nel **Password chiave Cluster** schermata il **Password** e **Conferma password** caselle digitare ** pass@word1 **e quindi fare clic su **Avanti**.  
  
23. Nel **sito Web Cluster** schermata, assicurarsi che **sito Web predefinito** sia selezionata e quindi fare clic su **Avanti**.  
  
24. Nel **indirizzo Cluster** schermo, seleziona il **utilizzare una connessione non crittografata** opzione, nel **nome di dominio completo** digitare **FILE1.contoso.com**e quindi fare clic su **Avanti**.  
  
25. Nel **nome certificato concessore di licenze** schermata, accettare il nome predefinito (**FILE1**) nella casella di testo e fare clic su **Avanti**.  
  
26. Nel **registrazione SCP** schermo, seleziona **registra SCP**, quindi fare clic su **Avanti**.  
  
27. Nel **conferma** schermata, fare clic su **installare**.  
  
28. Nel **risultati** schermata, fare clic su **Chiudi**e quindi fare clic su **Chiudi** in **lo stato dell'installazione** dello schermo. Al termine, disconnettersi e accedere come contoso\rms specificando la password fornita (**pass@word1**).  
  
29. Avviare la console AD RMS e passare a **modelli di criteri per i diritti**.  
  
    Per aprire la console AD RMS, in Server Manager, fare clic su **Server locale** nell'albero della console, quindi fare clic su **strumenti**, quindi fare clic su **Active Directory Rights Management Services**.  
  
30. Fare clic su di **Crea criterio diritti Distributed** modello si trova nel riquadro destro fare clic su **Aggiungi**e selezionare le seguenti informazioni:  
  
    -   Language: Inglese  
  
    -   Nome: Contoso Finance Admin Only  
  
    -   Descrizione: Contoso Finance Admin Only  
  
    Fare clic su **Aggiungi**, quindi fare clic su **Avanti**.  
  
31. Nella sezione utenti e diritti fare clic su **utenti e diritti**, fare clic su **Aggiungi**, tipo ** financeadmin@contoso.com **e fare clic su **OK**.  
  
32. Selezionare **controllo completo**, lasciando **Grant proprietario (autore) diritto di controllo completo senza scadenza** selezionato.  
  
33. Fare clic sulle schede rimanenti senza apportare modifiche, quindi fare clic su **fine**. Accedere come CONTOSO\Administrator.  
  
34. Sfoglia per selezionare la cartella, C:\inetpub\wwwroot\\_wmcs\certification, il file ServerCertification.asmx e aggiungervi gli utenti autenticati per avere autorizzazioni lettura e scrittura al file.  
  
35. Aprire Windows PowerShell ed eseguire `Get-FsrmRmsTemplate`. Verificare che sia in grado di vedere il modello RMS creato nei passaggi precedenti in questa procedura con questo comando.  
  
> [!IMPORTANT]  
> Se si desidera modificare immediatamente in modo da poterli testare i file server, è necessario eseguire le operazioni seguenti:  
>   
> 1.  Nel file server, FILE1, aprire un prompt dei comandi con privilegi elevati ed eseguire i comandi seguenti:  
>   
>     -   gpupdate /force.  
>     -   NLTEST /SC_RESET:contoso.com  
> 2.  Nel controller di dominio (DC1) replicare Active Directory.  
>   
>     For more information about steps to force the replication of Active Directory, see [Active Directory Replication](https://technet.microsoft.com/library/cc794809(WS.10).aspx)  
  
Facoltativamente, invece di usare l'aggiunta guidata ruoli e funzionalità in Server Manager, è possibile utilizzare Windows PowerShell per installare e configurare il ruolo server AD RMS come illustrato nella procedura seguente.  
  
###### <a name="to-install-and-configure-an-ad-rms-cluster-in-windows-server-2012-using-windows-powershell"></a>Per installare e configurare un cluster AD RMS in Windows Server 2012 tramite Windows PowerShell  
  
1.  Accedere a CONTOSO\Administrator con la password: ** pass@word1 **.  
  
    > [!IMPORTANT]  
    > Per installare il ruolo server AD RMS il programma di installazione account (in questo caso CONTOSO\Administrator) deve essere membro sia il gruppo Administrators locale sul computer del server AD RMS, in cui viene installato l'appartenenza al gruppo Enterprise Admins in Active Directory.  
  
2.  Sul desktop del Server, fare doppio clic sull'icona di Windows PowerShell sulla barra delle applicazioni e seleziona **Esegui come amministratore** per aprire un prompt di Windows PowerShell con privilegi amministrativi.  
  
3.  Per usare i cmdlet di Server Manager per installare il ruolo server AD RMS, digitare:  
  
    ```  
    Add-WindowsFeature ADRMS '"IncludeAllSubFeature '"IncludeManagementTools  
    ```  
  
4.  Creare l'unità di Windows PowerShell che rappresenti il server AD RMS che si sta installando.  
  
    Ad esempio, per creare un'unità di Windows PowerShell denominata RC per installare e configurare il primo server in un cluster radice AD RMS, digitare:  
  
    ```  
    Import-Module ADRMS  
    New-PSDrive -PSProvider ADRMSInstall -Name RC -Root RootCluster  
    ```  
  
5.  Impostare le proprietà sugli oggetti nello spazio dei nomi di unità che rappresentano le impostazioni di configurazione necessarie.  
  
    Ad esempio, per impostare l'account del servizio AD RMS, al prompt dei comandi Windows PowerShell, digitare:  
  
    ```  
    $svcacct = Get-Credential  
    ```  
  
    Quando viene visualizzata la finestra di dialogo sicurezza di Windows, digitare il nome utente dominio account del servizio AD RMS CONTOSO\RMS e la password assegnata.  
  
    Successivamente, per assegnare l'account del servizio AD RMS alle impostazioni del cluster AD RMS, digitare quanto segue:  
  
    ```  
    Set-ItemProperty -Path RC:\ -Name ServiceAccount -Value $svcacct  
    ```  
  
    Successivamente, per impostare il server AD RMS per utilizzare Database interno di Windows, al prompt dei comandi Windows PowerShell, digitare:  
  
    ```  
    Set-ItemProperty -Path RC:\ClusterDatabase -Name UseWindowsInternalDatabase -Value $true  
    ```  
  
    Successivamente, per archiviare in modo sicuro la password della chiave cluster in una variabile, al prompt dei comandi Windows PowerShell, digitare:  
  
    ```  
    $password = Read-Host -AsSecureString -Prompt "Password:"  
    ```  
  
    Digitare la password della chiave cluster e quindi premere INVIO.  
  
    Successivamente, per assegnare la password per l'installazione di AD RMS, al prompt dei comandi di Windows PowerShell, digitare:  
  
    ```  
    Set-ItemProperty -Path RC:\ClusterKey -Name CentrallyManagedPassword -Value $password  
    ```  
  
    Successivamente, per impostare l'indirizzo del cluster AD RMS, al prompt dei comandi Windows PowerShell, digitare:  
  
    ```  
    Set-ItemProperty -Path RC:\ -Name ClusterURL -Value "http://file1.contoso.com:80"  
    ```  
  
    Successivamente, per assegnare il nome certificato concessore di licenze per l'installazione di AD RMS, al prompt dei comandi Windows PowerShell, digitare:  
  
    ```  
    Set-ItemProperty -Path RC:\ -Name SLCName -Value "FILE1"  
    ```  
  
    Successivamente, per impostare il punto di connessione del servizio (SCP) per il cluster AD RMS, al prompt dei comandi Windows PowerShell, digitare:  
  
    ```  
    Set-ItemProperty -Path RC:\ -Name RegisterSCP -Value $true  
    ```  
  
6.  Eseguire il **Install-ADRMS** cmdlet. Oltre a installare il ruolo server AD RMS e la configurazione del server, questo cmdlet installa anche altre funzionalità richieste da AD RMS, se necessario.  
  
    Ad esempio, per passare all'unità di Windows PowerShell denominata RC e installare e configurare AD RMS, digitare:  
  
    ```  
    Set-Location RC:\  
    Install-ADRMS -Path.  
    ```  
  
    Digitare "Y" quando il cmdlet richiede di confermare Vuoi avviare l'installazione.  
  
7.  Disconnettersi come CONTOSO\Administrator e accedere come CONTOSO\RMS specificando la password fornita ("pass@word1").  
  
    > [!IMPORTANT]  
    > Per gestire il server AD RMS l'account l'accesso e per gestire il server (in questo caso CONTOSO\RMS) deve essere membro sia del gruppo Administrators locale nel computer server AD RMS, nonché l'appartenenza al gruppo Enterprise Admins in Active Directory.  
  
8.  Sul desktop del Server, fare doppio clic sull'icona di Windows PowerShell sulla barra delle applicazioni e seleziona **Esegui come amministratore** per aprire un prompt di Windows PowerShell con privilegi amministrativi.  
  
9. Creare l'unità di Windows PowerShell che rappresenti il server AD RMS che si sta configurando.  
  
    Ad esempio, per creare un'unità di Windows PowerShell denominata RC per configurare il cluster radice AD RMS, digitare:  
  
    ```  
    Import-Module ADRMSAdmin `  
    New-PSDrive -PSProvider ADRMSAdmin -Name RC -Root http://localhost -Force -Scope Global  
    ```  
  
10. Per creare nuovo modello dei diritti per l'amministratore di Contoso finance e assegnargli diritti utente con controllo completo nell'installazione di AD RMS, al prompt dei comandi di Windows PowerShell, digitare:  
  
    ```  
    New-Item -Path RC:\RightsPolicyTemplate '"LocaleName en-us -DisplayName "Contoso Finance Admin Only" -Description "Contoso Finance Admin Only" -UserGroup financeadmin@contoso.com  -Right ('FullControl')  
    ```  
  
11. Per verificare che è possibile visualizzare il nuovo modello dei diritti per l'amministratore di Contoso finance, al prompt dei comandi Windows PowerShell:  
  
    ```  
    Get-FsrmRmsTemplate  
    ```  
  
    Esaminare l'output del cmdlet per verificare che il modello RMS creato nel passaggio precedente è presente.  
  
### <a name="build-the-mail-server-srv1"></a>Generare il server di posta (SRV1)  
SRV1 è il server di posta SMTP/POP3. È necessario configurarlo in modo che è possibile inviare notifiche tramite posta elettronica come parte dello scenario di assistenza per accesso negato.  
  
Configurare Microsoft Exchange Server nel computer. For more information, see [How to Install Exchange Server](https://go.microsoft.com/fwlink/?prd=12364).  
  
### <a name="build-the-client-virtual-machine-client1"></a>Creare la macchina virtuale client (CLIENT1)  
  
##### <a name="to-build-the-client-virtual-machine"></a>Per creare la macchina virtuale client  
  
1.  Connettere CLIENT1 alla rete ID_AD_Network.  
  
2.  Installare Microsoft Office 2010.  
  
3.  Accedere come Contoso\Administrator e usare le informazioni seguenti per configurare Microsoft Outlook.  
  
    -   Il nome: File Administrator  
  
    -   Indirizzo di posta elettronica:fileadmin@contoso.com  
  
    -   Tipo di account: POP3  
  
    -   Server della posta in arrivo: indirizzo IP statico di of SRV1  
  
    -   Server della posta in uscita: indirizzo IP statico di of SRV1  
  
    -   Nome utente:fileadmin@contoso.com  
  
    -   Ricorda password: selezionare  
  
4.  Creare un collegamento ad Outlook sul desktop di Contoso\administrator..  
  
5.  Aprire Outlook e risolvere tutti i messaggi 'primo avvio del programma'.  
  
6.  Eliminare i messaggi di prova generati.  
  
7.  Creare un nuovo collegamento sul desktop per tutti gli utenti nella macchina virtuale client che punti ai documenti \\\FILE1\Finance.  
  
8.  Riavviare se necessario.  
  
##### <a name="enable-access-denied-assistance-on-the-client-virtual-machine"></a>Abilita l'assistenza per accesso negato nella macchina virtuale client  
  
1.  Aprire l'Editor del Registro di sistema e passare a **HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Explorer**.  
  
    -   Impostare **EnableShellExecuteFileStreamCheck** a **1**.  
  
    -   Valore: DWORD  
  
## <a name="BKMK_CF"></a>Configurazione del laboratorio per distribuzione di attestazioni tra uno scenario di insiemi di strutture  
  
### <a name="BKMK_2.1"></a>Creare una macchina virtuale per DC2  
  
-   Creare una macchina virtuale utilizzando il file ISO di Windows Server 2012.  
  
-   Creare il nome della macchina virtuale come DC2.  
  
-   Connettere la macchina virtuale alla rete ID_AD_Network.  
  
> [!IMPORTANT]  
> Aggiunta di macchine virtuali a un dominio e la distribuzione di tipi di attestazioni tra foreste è necessario che le macchine virtuali siano in grado di risolvere i nomi di dominio completi dei domini pertinenti. Si potrebbe essere necessario configurare manualmente le impostazioni DNS nelle macchine virtuali a questo scopo. For more information, see [Configuring a virtual network](https://technet.microsoft.com/library/cc732470%28v=ws.10%29.aspx).  
>   
> Tutte le immagini di macchina virtuale (server e client) devono essere riconfigurate per l'utilizzo di un indirizzo IP statico versione 4 (IPv4) indirizzo e le impostazioni del client di sistema DNS (Domain Name). For more information, see [Configure a DNS Client for Static IP Address](https://go.microsoft.com/fwlink/?LinkId=150952).  
  
### <a name="BKMK_2.2"></a>Impostare una nuova foresta denominata adatum.com  
  
##### <a name="to-install-active-directory-domain-services"></a>Per installare servizi di dominio Active Directory  
  
1.  Connettere la macchina virtuale alla rete ID_AD_Network. Accedere a DC2 come Administrator con la password ** Pass@word1 **.  
  
2.  In Server Manager, fare clic su **Gestisci**, quindi fare clic su **Aggiungi ruoli e funzionalità**.  
  
3.  Nel **prima di iniziare** pagina, fare clic su **Avanti**.  
  
4.  Nel **Selezione tipo di installazione** pagina, fare clic su **installazione basata su ruoli o basata su funzionalità**, quindi fare clic su **Avanti**.  
  
5.  Nel **server di destinazione** pagina, fare clic su **selezionare un server dal pool di server**, fare clic sul nome del server in cui si desidera installare servizi di dominio Active Directory (AD DS), quindi fare clic su **Avanti**.  
  
6.  Nel **Selezione ruoli Server** pagina, fare clic su **servizi di dominio Active Directory**. Nel **Aggiunta guidata ruoli e funzionalità** la finestra di dialogo, fare clic su **Aggiungi funzionalità**e quindi fare clic su **Avanti**.  
  
7.  Nel **Selezione funzionalità** pagina, fare clic su **Avanti**.  
  
8.  Nel **di dominio Active Directory** pagina, esaminare le informazioni e quindi fare clic su **Avanti**.  
  
9. Nel **conferma** pagina, fare clic su **installare**. L'indicatore di stato di installazione funzionalità nella pagina risultati indica che il ruolo viene installato.  
  
10. Nel **risultati** verificare che l'installazione ha avuto esito positivo e fare clic sull'icona di avviso con un punto esclamativo nell'angolo superiore destro dello schermo, accanto a **Gestisci**. Nell'elenco attività, fare clic su di **promuovere il server a un controller di dominio** collegamento.  
  
    > [!IMPORTANT]  
    > Se si chiude l'installazione guidata a questo punto invece di fare clic su **promuovere il server a un controller di dominio**, è possibile continuare l'installazione di servizi di dominio Active Directory facendo **attività** in Server Manager.  
  
11. Nel **configurazione della distribuzione** pagina, fare clic su **aggiungere una nuova foresta**, digitare il nome del dominio radice della **adatum.com**, quindi fare clic su **Avanti**.  
  
12. Nel **opzioni Controller di dominio** pagina, selezionare i livelli di funzionalità foresta e del dominio di Windows Server 2012, specificare la password DSRM ** pass@word1 **, quindi fare clic su **Avanti**.  
  
13. Nel **opzioni DNS** pagina, fare clic su **Avanti**.  
  
14. Nel **opzioni aggiuntive** pagina, fare clic su **Avanti**.  
  
15. Nel **percorsi** pagina, digitare i percorsi per il database di Active Directory, file di log e della cartella SYSVOL (o accettare i percorsi predefiniti), quindi fare clic su **Avanti**.  
  
16. Nel **verifica opzioni** pagina, confermare le selezioni e quindi fare clic su **Avanti**.  
  
17. Nel **controllo dei prerequisiti** pagina, verificare che la convalida dei prerequisiti viene completata e quindi fare clic su **installare**.  
  
18. Nel **risultati** pagina, verificare che il server è stato configurato correttamente come controller di dominio e quindi fare clic su **Chiudi**.  
  
19. Riavviare il server per completare l'installazione di servizi di dominio Active Directory. (Per impostazione predefinita, questo avviene automaticamente.)  
  
> [!IMPORTANT]  
> Per garantire che la rete è configurata correttamente, dopo aver configurato entrambe le foreste, è necessario eseguire le operazioni seguenti:  
>   
> -   Accedere ad adatum.com come adatum\administrator.. Aprire una finestra del prompt dei comandi, digitare **nslookup contoso.com**, quindi premere INVIO.  
> -   Accedere a contoso.com come Contoso\administrator.. Aprire una finestra del prompt dei comandi, digitare **nslookup adatum.com**, quindi premere INVIO.  
>   
> Se questi comandi vengono eseguiti senza errori, le foreste possono comunicare tra loro. For more information on nslookup errors, see the troubleshooting section in the topic [Using NSlookup.exe](https://support.microsoft.com/kb/200525)  
  
### <a name="BKMK_2.22"></a>Impostare contoso.com come foresta trusting per adatum.com  
In this step, you create a trust relationship between the Adatum Corporation site and the Contoso, Ltd. site.  
  
##### <a name="to-set-contoso-as-a-trusting-forest-to-adatum"></a>Per impostare contoso.com come foresta trusting per adatum.com  
  
1.  Accedere a DC2 come administrator. Nel **Start** digitare domain.msc.  
  
2.  Nell'albero della console, fare doppio clic su adatum.com e quindi fare clic su proprietà.  
  
3.  Nel **trust** scheda, fare clic su **nuova relazione di Trust**, quindi fare clic su **Avanti**.  
  
4.  Nel **nome Trust** digitare **contoso.com**nel sistema DNS (Domain Name) nome campo e quindi fare clic su **Avanti**.  
  
5.  Nel **tipo di Trust** pagina, fare clic su **trust tra foreste**, quindi fare clic su **Avanti**.  
  
6.  Nel **direzione del Trust** pagina, fare clic su **bidirezionale**.  
  
7.  Nel **parti del Trust** pagina, fare clic su **questo dominio e il dominio specificato**, quindi fare clic su **Avanti**.  
  
8.  Continuare a seguire le istruzioni della procedura guidata.  
  
### <a name="BKMK_2.4"></a>Creare altri utenti nella foresta Adatum  
Creare l'utente Jeff Low con la password ** pass@word1 **e assegnargli l'attributo company con il valore **Adatum**.  
  
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
  
### <a name="BKMK_2.55"></a>Abilitare le proprietà della risorsa Company in contoso.com  
  
##### <a name="to-enable-the-company-resource-property-on-contosocom"></a>Per abilitare le proprietà della risorsa Company in contoso.com  
  
1.  Accedere a contoso.com come amministratore.  
  
2.  In Server Manager, fare clic su **strumenti**, quindi fare clic su **centro di amministrazione di Active Directory**.  
  
3.  Nel riquadro sinistro del centro di amministrazione di Active Directory, fare clic su **visualizzazione albero**. Nel riquadro a sinistra, fare clic su **controllo dinamico degli accessi**e quindi fare doppio clic su **le proprietà delle risorse**.  
  
4.  Selezionare **società** dal **le proprietà delle risorse** elenco del mouse e scegliere **proprietà**. Nel **valori suggeriti** fare clic su **Aggiungi** per aggiungere i valori suggeriti: Contoso e Adatum, quindi fare clic su **OK** due volte.  
  
5.  Selezionare **società** dal **le proprietà delle risorse** elenco del mouse e scegliere **abilitare**.  
  
### <a name="BKMK_2.6"></a>Abilitare controllo dinamico degli accessi per adatum.com  
  
##### <a name="to-enable-dynamic-access-control-for-adatumcom"></a>Per abilitare il controllo dinamico degli accessi per adatum.com  
  
1.  Accedere ad adatum.com come amministratore.  
  
2.  Aprire la Console Gestione criteri di gruppo, fare clic su **adatum.com**e quindi fare doppio clic su **i controller di dominio**.  
  
3.  Fare doppio clic su **criterio controller di dominio predefiniti**e seleziona **modifica**.  
  
4.  Nella finestra Editor Gestione criteri di gruppo, fare doppio clic su **configurazione Computer**, fare doppio clic su **criteri**, fare doppio clic su **modelli amministrativi**, fare doppio clic su **sistema**e quindi fare doppio clic su **KDC**.  
  
5.  Fare doppio clic su **supporto KDC per attestazioni, autenticazione composta e blindatura Kerberos** e selezionare l'opzione accanto a **abilitato**. È necessario abilitare questa impostazione per utilizzare criteri di accesso centrale.  
  
6.  Aprire un prompt dei comandi con privilegi elevati ed eseguire il comando seguente:  
  
    ```  
    gpupdate /force  
    ```  
  
### <a name="BKMK_2.8"></a>Creare il tipo di attestazione Company in contoso.com  
  
##### <a name="to-create-a-claim-type-by-using-windows-powershell"></a>Per creare un tipo di attestazione mediante Windows PowerShell  
  
1.  Accedere a contoso.com come amministratore.  
  
2.  Aprire un prompt dei comandi con privilegi elevati in Windows PowerShell, quindi digitare il codice seguente:  
  
    ```  
    New-ADClaimType '"SourceTransformPolicy `  
    '"DisplayName 'Company' `  
    '"ID 'ad://ext/Company:ContosoAdatum' `  
    '"IsSingleValued $true `  
    '"ValueType 'string' `  
  
    ```  
  
### <a name="BKMK_2.9"></a>Creare la regola di accesso centrale  
  
##### <a name="to-create-a-central-access-rule"></a>Per creare una regola di accesso centrale  
  
1.  Nel riquadro sinistro del centro di amministrazione di Active Directory, fare clic su **visualizzazione albero**. Nel riquadro a sinistra, fare clic su **controllo dinamico degli accessi**, quindi fare clic su **regole di accesso centrale**.  
  
2.  Fare doppio clic su **regole di accesso centrale**, fare clic su **New**e quindi **regola di accesso centrale**.  
  
3.  Nel **nome** digitare **AdatumEmployeeAccessRule**.  
  
4.  Nel **autorizzazioni** selezionare il **utilizzare le seguenti autorizzazioni come correnti** opzione, fare clic su **modifica**e quindi fare clic su **Aggiungi**. Fare clic su di **seleziona un'entità** collegamento, digitare **Authenticated Users**, quindi fare clic su **OK**.  
  
5.  Nel **voce autorizzazione per autorizzazioni** la finestra di dialogo, fare clic su **aggiungere una condizione**e immettere le condizioni seguenti: [**utente**] [**società**] [**è uguale a**] [**valore**] [**Adatum**]. Le autorizzazioni devono essere **modifica, lettura ed esecuzione, lettura, scrittura**.  
  
6.  Fare clic su **OK**.  
  
7.  Fare clic su **OK** tre volte per completare e tornare al centro di amministrazione di Active Directory.  
  
    ![Guide alle soluzioni](media/Appendix-B--Setting-Up-the-Test-Environment/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *  
  
    Il file o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet su una singola riga, anche se può sembrare che siano divisi su più righe a causa dei limiti di formattazione.  
  
    ```  
    New-ADCentralAccessRule `  
    -CurrentAcl:"O:SYG:SYD:AR(A;;FA;;;OW)(A;;FA;;;BA)(A;;FA;;;SY)(XA;;0x1301bf;;;AU;(@USER.ad://ext/Company:ContosoAdatum == `"Adatum`"))" `  
    -Name:"AdatumEmployeeAccessRule" `  
    -ProposedAcl:$null `  
    -ProtectedFromAccidentalDeletion:$true `  
    -Server:"contoso.com" `  
    ```  
  
### <a name="BKMK_2.10"></a>Creare il criterio di accesso centrale  
  
##### <a name="to-create-a-central-access-policy"></a>Per creare un criterio di accesso centrale  
  
1.  Accedere a contoso.com come amministratore.  
  
2.  Aprire un prompt dei comandi con privilegi elevati in Windows PowerShell e quindi incollare il codice seguente:  
  
    ```  
    New-ADCentralAccessPolicy "Adatum Only Access Policy"   
    Add-ADCentralAccessPolicyMember "Adatum Only Access Policy" `  
    -Member "AdatumEmployeeAccessRule" `  
    ```  
  
### <a name="BKMK_2.11"></a>Pubblicare i nuovi criteri tramite criteri di gruppo  
  
##### <a name="to-apply-the-central-access-policy-across-file-servers-through-group-policy"></a>Per applicare il criterio di accesso centrale nei file server mediante criteri di gruppo  
  
1.  Nel **Start** digitare **strumenti di amministrazione**e il **ricerca** barra, fare clic su **impostazioni**. Nel **impostazioni** risultati, fare clic su **strumenti di amministrazione**. Aprire Console Gestione criteri di gruppo dal **strumenti di amministrazione** cartella.  
  
    > [!TIP]  
    > Se il **Mostra strumenti di amministrazione** è disabilitata, la cartella Strumenti di amministrazione e il relativo contenuto non verrà visualizzata nel **impostazioni** risultati.  
  
2.  Fare clic su dominio contoso.com, fare clic su **crea un oggetto Criteri di gruppo in questo dominio e collegarlo qui**  
  
3.  Digitare un nome descrittivo per l'oggetto Criteri di gruppo, ad esempio **AdatumAccessGPO**, quindi fare clic su **OK**.  
  
##### <a name="to-apply-the-central-access-policy-to-the-file-server-through-group-policy"></a>Per applicare il criterio di accesso centrale al file server mediante criteri di gruppo  
  
1.  Nel **Start** digitare **Gestione criteri di gruppo**, nel **ricerca** casella. Apri **Gestione criteri di gruppo** dalla cartella Strumenti di amministrazione.  
  
    > [!TIP]  
    > Se il **Mostra strumenti di amministrazione** è disabilitata, la cartella Strumenti di amministrazione e il relativo contenuto non verrà visualizzati nei risultati di impostazioni.  
  
2.  Individuare e selezionare Contoso nel modo seguente: Management\Forest di criteri di gruppo: contoso.com\Domains\contoso.com.  
  
3.  Fare doppio clic su di **AdatumAccessGPO** criteri e selezionare **modifica**.  
  
4.  In Editor Gestione criteri di gruppo, fare clic su **configurazione Computer**, espandere **criteri**, espandere **le impostazioni di Windows**, quindi fare clic su **le impostazioni di sicurezza**.  
  
5.  Espandere **File System**, fare doppio clic su **criteri di accesso centrale**, quindi fare clic su **criteri di accesso centrale gestire**.  
  
6.  Nel **configurazione di criteri di accesso centrale** la finestra di dialogo, fare clic su **Aggiungi**selezionare **Adatum Only Access Policy**e quindi fare clic su **OK**.  
  
7.  Chiudere Editor Gestione criteri di gruppo. È stato aggiunto il criterio di accesso centrale di criteri di gruppo.  
  
### <a name="BKMK_2.12"></a>Creare la cartella Earnings sul file server  
Creare un nuovo volume NTFS in FILE1 e creare la cartella seguente: d:\earnings..  
  
> [!NOTE]  
> Criteri di accesso centrale non sono abilitati per impostazione predefinita nel sistema o il volume c: di avvio.  
  
### <a name="BKMK_2.13"></a>Impostare la classificazione e applicare i criteri di accesso centrale alla cartella Earnings  
  
##### <a name="to-assign-the-central-access-policy-on-the-file-server"></a>Per assegnare il criterio di accesso centrale nel file server  
  
1.  Nella console di gestione Hyper-V connettersi al server FILE1. Accedere al server usando Contoso\Administrator con la password ** pass@word1 **.  
  
2.  Aprire un prompt dei comandi con privilegi elevati e digitare: **gpupdate /force**. Ciò garantisce che le modifiche di criteri di gruppo saranno applicate al server.  
  
3.  È anche necessario aggiornare le proprietà delle risorse globali da Active Directory. Aprire Windows PowerShell, digitare `Update-FSRMClassificationpropertyDefinition`, quindi premere INVIO. Chiudere Windows PowerShell.  
  
4.  Aprire Esplora risorse e passare a d:\earnings.. Fare doppio clic su di **Earnings** cartella e scegliere **proprietà**.  
  
5.  Click the **Classification** tab. Select **Company**, and then select **Adatum** in the **Value** field.  
  
6.  Fare clic su **modifica**selezionare **Adatum Only Access Policy** dal menu a discesa e quindi fare clic su **applica**.  
  
7.  Click the **Security** tab, click **Advanced**, and then click the **Central Policy** tab. You should see the **AdatumEmployeeAccessRule** listed. È possibile espandere l'elemento per visualizzare tutte le autorizzazioni impostate durante la creazione della regola in Active Directory.  
  
8.  Fare clic su **OK** per tornare a Windows Explorer.  
  


