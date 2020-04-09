---
ms.assetid: 13fe87d9-75cf-45bc-a954-ef75d4423839
title: 'Appendice I: creazione di account di gestione per account e gruppi protetti in Active Directory'
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: c2141e4fad564579fd687b2dfc7e4a12e1634acb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80823484"
---
# <a name="appendix-i-creating-management-accounts-for-protected-accounts-and-groups-in-active-directory"></a>Appendice I: Creazione di account di gestione per gli account protetti e i gruppi in Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Uno dei problemi relativi all'implementazione di un modello di Active Directory che non si basa sull'appartenenza permanente a gruppi con privilegi elevati è che è necessario disporre di un meccanismo per popolare questi gruppi quando è necessaria l'appartenenza temporanea ai gruppi. Per alcune soluzioni Privileged Identity Management è necessario che agli account del servizio del software venga concessa l'appartenenza permanente a gruppi come DA o Administrators in ogni dominio della foresta. Tuttavia, tecnicamente non è necessario per le soluzioni Privileged Identity Management (PIM) per eseguire i propri servizi in contesti con privilegi elevati.  
  
Questa appendice fornisce informazioni che è possibile usare per le soluzioni PIM implementate in modo nativo o di terze parti per creare account con privilegi limitati e che possono essere controllati in modo rigoroso, ma possono essere usati per popolare i gruppi con privilegi in Active Directory quando è richiesta l'elevazione temporanea. Se si implementa PIM come soluzione nativa, questi account possono essere utilizzati dal personale amministrativo per eseguire il popolamento temporaneo del gruppo e, se si sta implementando PIM tramite software di terze parti, è possibile adattare questi account per funzionare come account del servizio.  
  
> [!NOTE]  
> Le procedure descritte in questa appendice forniscono un approccio alla gestione dei gruppi con privilegi elevati in Active Directory. È possibile adattare queste procedure in base alle esigenze, aggiungere altre restrizioni o omettere alcune delle restrizioni descritte qui.  
  
## <a name="creating-management-accounts-for-protected-accounts-and-groups-in-active-directory"></a>Creazione di account di gestione per account e gruppi protetti in Active Directory

La creazione di account che possono essere utilizzati per gestire l'appartenenza di gruppi con privilegi senza richiedere l'autorizzazione di diritti eccessivi per gli account di gestione è costituita da quattro attività generali descritte nelle istruzioni dettagliate che seguono:  
  
1.  In primo luogo, è necessario creare un gruppo che gestirà gli account, perché questi account devono essere gestiti da un set limitato di utenti attendibili. Se non si dispone già di una struttura di unità organizzative che supporta la separazione di account e sistemi protetti e con privilegi dal popolamento generale del dominio, è necessario crearne uno. Sebbene in questa appendice non vengano fornite istruzioni specifiche, le schermate mostrano un esempio di tale gerarchia di unità organizzative.  
  
2.  Creare gli account di gestione. Questi account devono essere creati come account utente "normali" e non dispongono di diritti utente oltre a quelli che sono già stati concessi agli utenti per impostazione predefinita.  
  
3.  Implementare le restrizioni per gli account di gestione che le rendono utilizzabili solo per lo scopo specializzato per cui sono state create, oltre a controllare gli utenti che possono abilitare e utilizzare gli account, ovvero il gruppo creato nel primo passaggio.  
  
4.  Configurare le autorizzazioni per l'oggetto AdminSDHolder in ogni dominio per consentire agli account di gestione di modificare l'appartenenza dei gruppi con privilegi nel dominio.  
  
È necessario testare accuratamente tutte queste procedure e modificarle in base alle esigenze dell'ambiente prima di implementarle in un ambiente di produzione. È inoltre necessario verificare che tutte le impostazioni funzionino come previsto (alcune procedure di test sono fornite in questa appendice) ed è necessario testare uno scenario di ripristino di emergenza in cui gli account di gestione non sono disponibili per l'uso per popolare gruppi protetti a scopo di ripristino. Per ulteriori informazioni sul backup e il ripristino di Active Directory, vedere la [Guida dettagliata al backup e al ripristino di servizi di dominio Active Directory](https://technet.microsoft.com/library/cc771290(v=ws.10).aspx).  
  
> [!NOTE]  
> Implementando i passaggi descritti in questa appendice, verranno creati account che saranno in grado di gestire l'appartenenza di tutti i gruppi protetti in ogni dominio, non solo i gruppi di Active Directory con privilegi più elevati, ad esempio EAs, DAs e BAs. Per ulteriori informazioni sui gruppi protetti in Active Directory, vedere [Appendice C: account protetti e gruppi in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
### <a name="step-by-step-instructions-for-creating-management-accounts-for-protected-groups"></a>Istruzioni dettagliate per la creazione di account di gestione per i gruppi protetti  
  
#### <a name="creating-a-group-to-enable-and-disable-management-accounts"></a>Creazione di un gruppo per abilitare e disabilitare gli account di gestione

Per gli account di gestione è necessario reimpostare le password a ogni utilizzo e deve essere disabilitato quando le attività che lo richiedono sono state completate. Sebbene sia possibile considerare anche l'implementazione dei requisiti di accesso alle smart card per questi account, si tratta di una configurazione facoltativa. queste istruzioni presuppongono che gli account di gestione vengano configurati con un nome utente e una password complessa e lungo come controlli minimi. In questo passaggio verrà creato un gruppo che dispone delle autorizzazioni per reimpostare la password negli account di gestione e per abilitare e disabilitare gli account.  
  
Per creare un gruppo per abilitare e disabilitare gli account di gestione, seguire questa procedura:  
  
1.  Nella struttura dell'unità organizzativa in cui verranno ospitati gli account di gestione, fare clic con il pulsante destro del mouse sull'unità organizzativa in cui si desidera creare il gruppo, scegliere **nuovo** e fare clic su **gruppo**.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_115.png)  
  
2.  Nella finestra di dialogo **nuovo oggetto-gruppo** immettere un nome per il gruppo. Se si prevede di usare questo gruppo per "attivare" tutti gli account di gestione nella foresta, impostarlo come gruppo di sicurezza universale. Se si dispone di una foresta a dominio singolo o se si prevede di creare un gruppo in ogni dominio, è possibile creare un gruppo di sicurezza globale. Fare clic su **OK** per creare il gruppo.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_116.png)  
  
3.  Fare clic con il pulsante destro del mouse sul gruppo appena creato, scegliere **Proprietà**e fare clic sulla scheda **oggetto** . Nella finestra di dialogo **Proprietà oggetto** del gruppo selezionare **Proteggi oggetto da eliminazioni accidentali**, che non solo impedirà agli utenti autorizzati diversamente di eliminare il gruppo, ma anche di trasferirlo in un'altra ou a meno che l'attributo non venga prima deselezionato.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_117.png)  
  
    > [!NOTE]  
    > Se sono già state configurate le autorizzazioni per le unità organizzative padre del gruppo per limitare l'amministrazione a un set limitato di utenti, potrebbe non essere necessario eseguire i passaggi seguenti. Vengono fornite in questo modo, anche se non è ancora stato implementato un controllo amministrativo limitato sulla struttura dell'unità organizzativa in cui è stato creato il gruppo, è possibile proteggere il gruppo dalla modifica da parte di utenti non autorizzati.  
  
4.  Fare clic sulla scheda **membri** e aggiungere gli account per i membri del team che saranno responsabili dell'abilitazione degli account di gestione o del popolamento dei gruppi protetti quando necessario.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_118.png)  
  
5.  Se non è ancora stato fatto, nella console **Active Directory utenti e computer** fare clic su **Visualizza** e selezionare **funzionalità avanzate**. Fare clic con il pulsante destro del mouse sul gruppo appena creato, scegliere **Proprietà**e quindi fare clic sulla scheda **sicurezza** . Nella scheda **sicurezza** fare clic su **Avanzate**.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_119.png)  
  
6.  Nella finestra di dialogo **impostazioni di sicurezza avanzate per [gruppo]** fare clic su **Disabilita ereditarietà**. Quando richiesto, fare clic su **Converti autorizzazioni ereditate in autorizzazioni esplicite per questo oggetto**, quindi fare clic su **OK** per tornare alla finestra di dialogo **sicurezza** del gruppo.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_120.png)  
  
7.  Nella scheda **sicurezza** rimuovere i gruppi a cui non è consentito accedere a questo gruppo. Se, ad esempio, non si vuole che gli utenti autenticati possano leggere il nome e le proprietà generali del gruppo, è possibile rimuovere tale voce ACE. È anche possibile rimuovere le voci ACE, ad esempio per gli operatori account e per l'accesso compatibile con il server precedente a Windows 2000. Tuttavia, è consigliabile lasciare il set minimo di autorizzazioni per gli oggetti. Lasciare intatte le voci ACE seguenti:  
  
    -   AUTO  
  
    -   SISTEMA  
  
    -   Domain Admins  
  
    -   Enterprise Admins  
  
    -   Administrators  
  
    -   Gruppo accesso autorizzazione Windows (se applicabile)  
  
    -   CONTROLLER DI DOMINIO AZIENDALI  
  
    Sebbene possa sembrare illogico consentire ai gruppi con privilegi più elevati in Active Directory di gestire questo gruppo, l'obiettivo dell'implementazione di queste impostazioni non è impedire ai membri di tali gruppi di apportare modifiche autorizzate. L'obiettivo è quello di garantire che, quando si ha l'opportunità di richiedere livelli molto elevati di privilegi, le modifiche autorizzate avranno esito positivo. Per questo motivo, la modifica dell'annidamento, dei diritti e delle autorizzazioni predefinite del gruppo con privilegi è sconsigliata in questo documento. Lasciando intatte le strutture predefinite e svuotando l'appartenenza dei gruppi di privilegi più elevati nella directory, è possibile creare un ambiente più sicuro che funzioni ancora come previsto.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_121.png)  
  
    > [!NOTE]  
    > Se non sono già stati configurati criteri di controllo per gli oggetti nella struttura dell'unità organizzativa in cui è stato creato il gruppo, è necessario configurare il controllo per registrare le modifiche del gruppo.  
  
8.  È stata completata la configurazione del gruppo che verrà usato per "estrarre" gli account di gestione quando sono necessari e per "archiviare" gli account quando le attività sono state completate.  
  
#### <a name="creating-the-management-accounts"></a>Creazione degli account di gestione

È necessario creare almeno un account che verrà usato per gestire l'appartenenza dei gruppi con privilegi nell'installazione di Active Directory e preferibilmente un secondo account che funge da backup. Se si sceglie di creare gli account di gestione in un singolo dominio nella foresta e di concedere loro le funzionalità di gestione per tutti i gruppi protetti di domini o se si sceglie di implementare gli account di gestione in ogni dominio della foresta, le procedure sono in effetti identiche.  
  
> [!NOTE]  
> I passaggi descritti in questo documento presuppongono che non siano ancora stati implementati i controlli degli accessi in base al ruolo e Privileged Identity Management per Active Directory. Pertanto, alcune procedure devono essere eseguite da un utente il cui account è membro del gruppo Domain Admins per il dominio in questione.  
>   
> Quando si utilizza un account con privilegi DA, è possibile accedere a un controller di dominio per eseguire le attività di configurazione. I passaggi che non richiedono privilegi DA possono essere eseguiti da account con privilegi meno elevati che sono connessi alle workstation amministrative. Schermate che mostrano finestre di dialogo associate al colore blu più chiaro rappresentano le attività che possono essere eseguite in un controller di dominio. Schermate che mostrano finestre di dialogo con il colore blu più scuro rappresentano le attività che possono essere eseguite su workstation amministrative con account con privilegi limitati.  
  
Per creare gli account di gestione, seguire questa procedura:  
  
1. Accedere a un controller di dominio con un account membro del gruppo DA del dominio.  

2. Avviare **Active Directory utenti e computer** e passare all'unità organizzativa in cui verrà creato l'account di gestione.  

3. Fare clic con il pulsante destro del mouse sull'unità organizzativa e scegliere **nuovo** e quindi **utente**.  

4. Nella finestra di dialogo **nuovo oggetto-utente** immettere le informazioni di denominazione desiderate per l'account e fare clic su **Avanti**.  

   ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_122.png)  
  
5. Specificare una password iniziale per l'account utente, deselezionare l' **utente deve modificare la password all'accesso successivo**, selezionare l' **utente non può modificare la password** e l' **account è disabilitato**e fare clic su **Avanti**.  

   ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_123.png)  

6. Verificare che i dettagli dell'account siano corretti e fare clic su **fine**.  

7. Fare clic con il pulsante destro del mouse sull'oggetto utente appena creato e scegliere **Proprietà**.  

8. Fare clic sulla scheda **account** .  

9. Nel campo **Opzioni account** selezionare l' **account è sensibile e non può essere delegata** flag, selezionare il **questo account supporta la crittografia AES 128 bit Kerberos** e/o l'account supporta il flag di **crittografia AES 256 Kerberos** e fare clic su **OK**.  

   ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_124.png)  

   > [!NOTE]  
   > Poiché questo account, analogamente ad altri account, avrà una funzione limitata ma potente, l'account deve essere utilizzato solo in host amministrativi protetti. Per tutti gli host amministrativi protetti nell'ambiente in uso, è consigliabile implementare l'impostazione Criteri di gruppo **sicurezza di rete: configurare i tipi di crittografia consentiti per Kerberos** per consentire solo i tipi di crittografia più sicuri che è possibile implementare per gli host protetti.  
   >
   > Sebbene l'implementazione di tipi di crittografia più sicuri per gli host non rilevi gli attacchi con furto di credenziali, l'utilizzo e la configurazione appropriati degli host protetti. L'impostazione di tipi di crittografia più avanzati per gli host utilizzati solo da account con privilegi riduce semplicemente la superficie di attacco complessiva dei computer.  
   >
   > Per ulteriori informazioni sulla configurazione dei tipi di crittografia nei sistemi e negli account, vedere [configurazioni di Windows per il tipo di crittografia supportato da Kerberos](https://blogs.msdn.com/b/openspecification/archive/2011/05/31/windows-configurations-for-kerberos-supported-encryption-type.aspx).  
   >
   > Queste impostazioni sono supportate solo nei computer che eseguono Windows Server 2012, Windows Server 2008 R2, Windows 8 o Windows 7.  
  
10. Nella scheda **oggetto** selezionare **Proteggi oggetto da eliminazioni accidentali**. In questo modo l'oggetto non solo verrà eliminato (anche dagli utenti autorizzati), ma ne impedirà lo spostamento in un'unità organizzativa diversa nella gerarchia di servizi di dominio Active Directory, a meno che la casella di controllo non venga prima cancellata da un utente con l'autorizzazione per la modifica dell'attributo.  

    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_125.png)  

11. Fare clic sulla scheda **controllo remoto** .  

12. Deselezionare il flag **Abilita controllo remoto** . Non dovrebbe mai essere necessario che il personale di supporto si connetta alle sessioni di questo account per implementare le correzioni.  

    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_126.png)  

    > [!NOTE]  
    > Ogni oggetto in Active Directory deve avere un proprietario IT designato e un proprietario aziendale designato, come descritto in [pianificazione della compromissione](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md). Se si tiene traccia della proprietà degli oggetti di servizi di dominio Active Directory in Active Directory, anziché in un database esterno, è necessario immettere le informazioni di proprietà appropriate nelle proprietà dell'oggetto.  
    >
    > In questo caso, il proprietario di un'azienda è probabilmente una divisione IT, alberoe c'non è un divieto per i proprietari aziendali, anche i proprietari IT. Il punto di stabilire la proprietà degli oggetti consiste nel consentire all'utente di identificare i contatti quando è necessario apportare modifiche agli oggetti, ad esempio anni dalla creazione iniziale.  

13. Fare clic sulla scheda **organizzazione** .  

14. Immettere le informazioni necessarie negli standard degli oggetti di servizi di dominio Active Directory.  

    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_127.png)  

15. Fare clic sulla scheda **connessione remota** .  

16. Nel campo **autorizzazione accesso alla rete** selezionare **Nega accesso**. Questo account non deve mai dover connettersi tramite una connessione remota.  

    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_128.png)  

    > [!NOTE]  
    > È improbabile che questo account verrà usato per accedere ai controller di dominio di sola lettura (RODC) nell'ambiente in uso. Tuttavia, in caso di circostanza è necessario che l'account acceda a un RODC, è necessario aggiungere questo account al gruppo di replica password di RODC negato, in modo che la password non venga memorizzata nella cache nel RODC.  
    >
    > Anche se la password dell'account deve essere reimpostata dopo ogni utilizzo e l'account deve essere disabilitato, l'implementazione di questa impostazione non ha un effetto deleterio sull'account e potrebbe essere utile in situazioni in cui un amministratore dimentica di reimpostare la password dell'account e di disabilitarla.  

17. Fare clic sulla scheda **Membro di**.  

18. Fare clic su **Add**.  

19. Digitare **negato controller** di sola lettura Password gruppo di replica nella finestra di dialogo **Seleziona utenti, contatti, computer** e fare clic su **Controlla nomi**. Quando il nome del gruppo è sottolineato nel selettore oggetti, fare clic su **OK** e verificare che l'account sia ora un membro dei due gruppi visualizzati nello screenshot seguente. Non aggiungere l'account a gruppi protetti.  

20. Fare clic su **OK**.  

    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_129.png)  

21. Fare clic sulla scheda **sicurezza** e fare clic su **Avanzate**.  

22. Nella finestra di dialogo **impostazioni di sicurezza avanzate** fare clic su **Disabilita ereditarietà** e copiare le autorizzazioni ereditate come autorizzazioni esplicite e fare clic su **Aggiungi**.  

    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_130.png)  

23. Nella finestra di dialogo **voce autorizzazione per [account]** fare clic su **Seleziona un'entità** e aggiungere il gruppo creato nella procedura precedente. Scorrere fino alla parte inferiore della finestra di dialogo e fare clic su **Cancella tutto** per rimuovere tutte le autorizzazioni predefinite.  

    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_131.png)  

24. Scorrere fino alla parte superiore della finestra di dialogo **voce autorizzazione** . Verificare che l'elenco a discesa **tipo** sia impostato su **Consenti**e nell'elenco a discesa **applica a** selezionare **solo questo oggetto**.  

25. Nel campo **autorizzazioni** selezionare **Leggi tutte le proprietà**, **Leggi autorizzazioni**e **Reimposta password**.  

    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_132.png)  

26. Nel campo **Proprietà** selezionare **Leggi userAccountControl** e **scrittura userAccountControl**.  

27. Fare **di nuovo clic** su **OK**nella finestra di dialogo **impostazioni di sicurezza avanzate** .  

    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_133.png)  

    > [!NOTE]  
    > L'attributo **userAccountControl** controlla più opzioni di configurazione dell'account. Non è possibile concedere l'autorizzazione per modificare solo alcune opzioni di configurazione quando si concede l'autorizzazione di scrittura all'attributo.  

28. Nel campo **nome gruppo o utente** della scheda **sicurezza** rimuovere tutti i gruppi a cui non è consentito accedere o gestire l'account. Non rimuovere i gruppi che sono stati configurati con le voci ACE di negazione, ad esempio il gruppo Everyone e l'account autocalcolato (la voce ACE è stata impostata quando l' **utente non può modificare** il flag password è stato abilitato durante la creazione dell'account. Non rimuovere inoltre il gruppo appena aggiunto, l'account di sistema o i gruppi come EA, DA, BA o il gruppo di accesso di autorizzazione Windows.  

    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_134.png)  

29. Fare clic su **Avanzate** e verificare che la finestra di dialogo Impostazioni di sicurezza avanzate sia simile alla schermata seguente.  

30. Fare clic su **OK**e di nuovo su **OK** per chiudere la finestra di dialogo delle proprietà dell'account.  

    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_135.png)  

31. Il programma di installazione del primo account di gestione è ora completo. L'account viene testato in una procedura successiva.  

##### <a name="creating-additional-management-accounts"></a>Creazione di account di gestione aggiuntivi

È possibile creare altri account di gestione ripetendo i passaggi precedenti, copiando l'account appena creato oppure creando uno script per creare account con le impostazioni di configurazione desiderate. Si noti, tuttavia, che se si copia l'account appena creato, molte impostazioni e ACL personalizzati non verranno copiati nel nuovo account e sarà necessario ripetere la maggior parte dei passaggi di configurazione.  
  
È invece possibile creare un gruppo al quale si delegano i diritti per popolare e non popolare i gruppi protetti, ma sarà necessario proteggere il gruppo e gli account inseriti. Poiché nella directory sono presenti pochissimi account a cui viene concessa la possibilità di gestire l'appartenenza dei gruppi protetti, la creazione di singoli account potrebbe essere l'approccio più semplice.  
  
Indipendentemente dal modo in cui si sceglie di creare un gruppo in cui si inseriscono gli account di gestione, è necessario assicurarsi che ogni account sia protetto come descritto in precedenza. È inoltre consigliabile implementare restrizioni degli oggetti Criteri di gruppo simili a quelle descritte in [Appendice D: protezione degli account amministratore predefiniti in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
  
##### <a name="auditing-management-accounts"></a>Controllo degli account di gestione

È necessario configurare il controllo per l'account per la registrazione, come minimo, tutte le Scritture nell'account. Ciò consentirà di non solo identificare correttamente l'abilitazione dell'account e la reimpostazione della password durante gli usi autorizzati, ma anche di identificare i tentativi da parte di utenti non autorizzati di modificare l'account. Le Scritture non riuscite per l'account devono essere acquisite nel sistema SIEM (Security Information and Event Monitoring), se applicabile, e devono attivare avvisi che forniscono notifiche al personale responsabile dell'analisi dei potenziali compromessi.  
  
Le soluzioni SIEM accettano informazioni sugli eventi da origini di sicurezza interattive (ad esempio, registri eventi, dati dell'applicazione, flussi di rete, prodotti antimalware e origini di rilevamento delle intrusioni), fascicolano i dati e tentano di creare visualizzazioni intelligenti e azioni proattive. Sono disponibili molte soluzioni SIEM commerciali e molte aziende creano implementazioni private. Un SIEM ben progettato e implementato in modo appropriato può migliorare significativamente le funzionalità di monitoraggio della sicurezza e di risposta agli eventi imprevisti. Tuttavia, le funzionalità e l'accuratezza variano notevolmente tra le soluzioni. I SIEM non rientrano nell'ambito di questo documento, ma le raccomandazioni specifiche per gli eventi devono essere prese in considerazione da qualsiasi implementatore di SIEM.  
  
Per ulteriori informazioni sulle impostazioni di configurazione di controllo consigliate per i controller di dominio, vedere [Active Directory di monitoraggio per i segnali di compromissione](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md). Le impostazioni di configurazione specifiche del controller di dominio sono disponibili nel [monitoraggio Active Directory per i segnali di compromissione](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md).  
  
#### <a name="enabling-management-accounts-to-modify-the-membership-of-protected-groups"></a>Abilitazione degli account di gestione per modificare l'appartenenza dei gruppi protetti

In questa procedura vengono configurate le autorizzazioni per l'oggetto AdminSDHolder del dominio per consentire agli account di gestione appena creati di modificare l'appartenenza dei gruppi protetti nel dominio. Questa procedura non può essere eseguita tramite un'interfaccia utente grafica (GUI).  
  
Come illustrato nell' [Appendice C: account e gruppi protetti in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md), l'ACL nell'oggetto AdminSDHolder di un dominio viene effettivamente "copiato" negli oggetti protetti quando viene eseguita l'attività SDPROP. I gruppi e gli account protetti non ereditano le autorizzazioni dall'oggetto AdminSDHolder; le autorizzazioni vengono impostate in modo esplicito in modo che corrispondano a quelle nell'oggetto AdminSDHolder. Pertanto, quando si modificano le autorizzazioni per l'oggetto AdminSDHolder, è necessario modificarle per gli attributi appropriati per il tipo di oggetto protetto di destinazione.  
  
In questo caso, verranno concessi gli account di gestione appena creati per consentire la lettura e la scrittura dell'attributo members sugli oggetti Group. Tuttavia, l'oggetto AdminSDHolder non è un oggetto gruppo e gli attributi di gruppo non vengono esposti nell'editor ACL grafico. Per questo motivo, le modifiche alle autorizzazioni vengono implementate tramite l'utilità della riga di comando DSACLS. Per concedere le autorizzazioni per gli account di gestione (disabilitati) per modificare l'appartenenza dei gruppi protetti, seguire questa procedura:  
  
1. Accedere a un controller di dominio, preferibilmente il controller di dominio che contiene il ruolo emulatore PDC (emulatore PDC), con le credenziali di un account utente che è stato reso membro del gruppo DA nel dominio.  
  
   ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_136.png)  
  
2. Aprire un prompt dei comandi con privilegi elevati facendo clic con il pulsante destro del mouse su **prompt dei comandi** e scegliere **Esegui come amministratore**.  
  
   ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_137.gif)  
  
3. Quando viene richiesto di approvare l'elevazione, fare clic su **Sì**.  
  
   ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_138.gif)  
  
   > [!NOTE]  
   > Per ulteriori informazioni sull'elevazione e sul controllo dell'account utente (UAC) in Windows, vedere [processi UAC e interazioni](https://technet.microsoft.com/library/dd835561(v=WS.10).aspx) nel sito Web TechNet.  
  
4. Al prompt dei comandi digitare (sostituendo le informazioni specifiche del dominio) **dsacls [nome distinto dell'oggetto AdminSDHolder nel dominio]/g [nome UPN dell'account di gestione]: RPWP; membro**.  
  
   ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_139.gif)  
  
   Il comando precedente, che non fa distinzione tra maiuscole e minuscole, funziona nel modo seguente:  
  
   - DSACLS imposta o Visualizza le voci ACE negli oggetti directory  
  
   - CN = AdminSDHolder, CN = System, DC = TailSpinToys, DC = MSFT identifica l'oggetto da modificare  
  
   - /G indica che è in corso la configurazione di una concessione ACE  
  
   - PIM001@tailspintoys.msft è il nome dell'entità utente (UPN) dell'entità di sicurezza a cui verranno concesse le voci ACE  
  
   - RPWP concede le autorizzazioni di proprietà di lettura e scrittura  
  
   - Member è il nome della proprietà (Attribute) in cui verranno impostate le autorizzazioni  
  
   Per ulteriori informazioni sull'utilizzo di **dsacls**, digitare dsacls senza parametri al prompt dei comandi.  
  
   Se sono stati creati più account di gestione per il dominio, è necessario eseguire il comando DSACLS per ogni account. Una volta completata la configurazione dell'ACL nell'oggetto AdminSDHolder, è necessario forzare l'esecuzione di SDProp o attendere il completamento dell'esecuzione pianificata. Per informazioni su come forzare l'esecuzione di SDProp, vedere "esecuzione manuale di SDProp" nell' [Appendice C: account protetti e gruppi in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
   Quando SDProp è stato eseguito, è possibile verificare che le modifiche apportate all'oggetto AdminSDHolder siano state applicate ai gruppi protetti nel dominio. Non è possibile verificare questa impostazione visualizzando l'ACL nell'oggetto AdminSDHolder per i motivi descritti in precedenza, ma è possibile verificare che le autorizzazioni siano state applicate visualizzando gli ACL nei gruppi protetti.  
  
5. In **Active Directory utenti e computer**verificare che siano state abilitate le **funzionalità avanzate**. A tale scopo, fare clic su **Visualizza**, individuare il gruppo **Domain Admins** , fare clic con il pulsante destro del mouse sul gruppo e scegliere **proprietà**.  
  
6. Fare clic sulla scheda **sicurezza** e fare clic su **Avanzate** per aprire la finestra di dialogo **impostazioni di sicurezza avanzate per Domain Admins** .  
  
   ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_140.gif)  
  
7. Selezionare **Consenti ACE per l'account di gestione** e fare clic su **modifica**. Verificare che all'account siano state concesse solo le autorizzazioni **Leggi membri** e **Scrivi membri** per il gruppo da, quindi fare clic su **OK**.  
  
8. Fare clic su **OK** nella finestra di dialogo **impostazioni di sicurezza avanzate** , quindi fare di nuovo clic su **OK** per chiudere la finestra di dialogo proprietà per il gruppo da.  
  
   ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_141.gif)  
  
9. È possibile ripetere i passaggi precedenti per altri gruppi protetti nel dominio; le autorizzazioni devono essere le stesse per tutti i gruppi protetti. A questo punto è stata completata la creazione e la configurazione degli account di gestione per i gruppi protetti in questo dominio.  
  
    > [!NOTE]  
    > Qualsiasi account che disponga dell'autorizzazione per scrivere l'appartenenza di un gruppo in Active Directory può anche aggiungere se stesso al gruppo. Questo comportamento è da progettazione e non può essere disabilitato. Per questo motivo, è consigliabile tenere sempre gli account di gestione disabilitati quando non sono in uso e monitorare attentamente gli account quando vengono disabilitati e quando sono in uso.  
  
#### <a name="verifying-group-and-account-configuration-settings"></a>Verifica delle impostazioni di configurazione di gruppi e account

Ora che sono stati creati e configurati gli account di gestione che possono modificare l'appartenenza dei gruppi protetti nel dominio (che include i gruppi EA, DA e BA con privilegi elevati), è necessario verificare che gli account e il relativo gruppo di gestione siano stati creati correttamente. La verifica è costituita dalle attività generali seguenti:  
  
1.  Testare il gruppo in grado di abilitare e disabilitare gli account di gestione per verificare che i membri del gruppo possano abilitare e disabilitare gli account e reimpostare le proprie password, ma non possono eseguire altre attività amministrative sugli account di gestione.  
  
2.  Testare gli account di gestione per verificare che possano aggiungere e rimuovere membri dai gruppi protetti nel dominio, ma non possono modificare altre proprietà di account e gruppi protetti.  
  
##### <a name="test-the-group-that-will-enable-and-disable-management-accounts"></a>Testare il gruppo che consentirà di abilitare e disabilitare gli account di gestione
  
1.  Per testare l'abilitazione di un account di gestione e la reimpostazione della password, accedere a una workstation amministrativa protetta con un account membro del gruppo creato nell' [appendice I: creazione di account di gestione per gli account protetti e i gruppi in Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_142.gif)  
  
2.  Aprire **Active Directory utenti e computer**, fare clic con il pulsante destro del mouse sull'account di gestione e scegliere **Abilita account**.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_143.gif)  
  
3.  Verrà visualizzata una finestra di dialogo che conferma che l'account è stato abilitato.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_144.gif)  
  
4.  Reimpostare quindi la password per l'account di gestione. A tale scopo, fare di nuovo clic con il pulsante destro del mouse sull'account e scegliere **Reimposta password**.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_145.gif)  
  
5.  Digitare una nuova password per l'account nei campi **nuova password** e **Conferma password** , quindi fare clic su **OK**.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_146.gif)  
  
6.  Verrà visualizzata una finestra di dialogo che conferma che la password per l'account è stata reimpostata.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_147.gif)  
  
7.  Provare ora a modificare le proprietà aggiuntive dell'account di gestione. Fare clic con il pulsante destro del mouse sull'account e scegliere **Proprietà**, quindi fare clic sulla scheda **controllo remoto** .  
  
8.  Selezionare **Abilita controllo remoto** e fare clic su **applica**. L'operazione avrà esito negativo e verrà visualizzato un messaggio di errore di **accesso negato** .  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_148.gif)  
  
9. Fare clic sulla scheda **account** per l'account e tentare di modificare il nome dell'account, l'orario di accesso o le workstation di accesso. Tutti dovrebbero avere esito negativo e le opzioni dell'account che non sono controllate dall'attributo **userAccountControl** devono essere disattivate e non disponibili per la modifica.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_149.gif)  
  
10. Tentativo di aggiungere il gruppo di gestione a un gruppo protetto, ad esempio il gruppo DA. Quando si fa clic su **OK**, viene visualizzato un messaggio che informa che non si dispone delle autorizzazioni necessarie per modificare il gruppo.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_150.gif)  
  
11. Eseguire test aggiuntivi in base alle esigenze per verificare che non sia possibile configurare nulla nell'account di gestione, ad eccezione delle impostazioni **userAccountControl** e delle reimpostazioni della password.  
  
    > [!NOTE]  
    > L'attributo **userAccountControl** controlla più opzioni di configurazione dell'account. Non è possibile concedere l'autorizzazione per modificare solo alcune opzioni di configurazione quando si concede l'autorizzazione di scrittura all'attributo.  
  
##### <a name="test-the-management-accounts"></a>Testare gli account di gestione

Ora che sono stati abilitati uno o più account che possono modificare l'appartenenza dei gruppi protetti, è possibile testare gli account per assicurarsi che possano modificare l'appartenenza al gruppo protetta, ma non possono eseguire altre modifiche per gli account e i gruppi protetti.  
  
1.  Accedere a un host amministrativo sicuro come primo account di gestione.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_151.gif)  
  
2.  Avviare **Active Directory utenti e computer** e individuare il **gruppo Domain Admins**.  
  
3.  Fare clic con il pulsante destro del mouse sul gruppo **Domain Admins** e scegliere **Proprietà**.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_152.gif)  
  
4.  Nelle **Proprietà Domain Admins**fare clic sulla scheda **membri** e **quindi su** Aggiungi. Immettere il nome di un account a cui verranno assegnati i privilegi di Domain Admins temporanei e fare clic su **Controlla nomi**. Quando il nome dell'account è sottolineato, fare clic su **OK** per tornare alla scheda **membri** .  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_153.gif)  
  
5.  Nella scheda **membri** della finestra di dialogo **Proprietà Domain Admins** fare clic su **applica**. Dopo aver fatto clic su **applica**, l'account deve rimanere membro del gruppo da e non dovrebbe essere visualizzato alcun messaggio di errore.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_154.gif)  
  
6.  Fare clic sulla scheda **gestito da** nella finestra di dialogo **Proprietà Domain Admins** e verificare che non sia possibile immettere testo in alcun campo e che tutti i pulsanti siano disabilitati.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_155.gif)  
  
7.  Fare clic sulla scheda **generale** nella finestra di dialogo **Proprietà Domain Admins** e verificare che non sia possibile modificare le informazioni relative a tale scheda.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_156.gif)  
  
8.  Ripetere questi passaggi per gruppi protetti aggiuntivi in base alle esigenze. Al termine, accedere a un host amministrativo sicuro con un account membro del gruppo creato per abilitare e disabilitare gli account di gestione. Reimpostare quindi la password per l'account di gestione appena testato e disabilitare l'account. È stata completata l'installazione degli account di gestione e del gruppo che sarà responsabile per l'abilitazione e la disabilitazione degli account.  
