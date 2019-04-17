---
ms.assetid: 13fe87d9-75cf-45bc-a954-ef75d4423839
title: Appendice I - creazione di account di gestione per gli account protetti e gruppi in Active Directory
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 666180adca691d6c9783a43063df76877115fc40
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="appendix-i-creating-management-accounts-for-protected-accounts-and-groups-in-active-directory"></a>Appendice i: creazione di gestione degli account per gli account protetti e gruppi in Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

  
## <a name="appendix-i-creating-management-accounts-for-protected-accounts-and-groups-in-active-directory"></a>Appendice i: creazione di gestione degli account per gli account protetti e gruppi in Active Directory  

Una delle sfide nell'implementazione di un modello di Active Directory che non si basa l'appartenenza permanente a gruppi con privilegi elevati è che deve essere presente un meccanismo per popolare questi gruppi quando è richiesta l'appartenenza a gruppi. Alcune soluzioni di gestione di identità con privilegi richiedono che gli account del servizio del software vengono concesse l'appartenenza permanente a gruppi, ad esempio DA o gli amministratori in ogni dominio nella foresta. Tuttavia, è tecnicamente non necessario per le soluzioni di gestione di identità con privilegi (PIM) per l'esecuzione dei servizi in tali contesti con privilegi elevati.  
  
Questa appendice contiene informazioni che è possibile utilizzare per le soluzioni PIM in modo nativo implementate o di terze parti per creare gli account che dispone di privilegi limitati e possono essere controllati rigorosamente, ma possono essere utilizzati per popolare i gruppi con privilegi in Active Directory quando è richiesta l'elevazione dei privilegi temporaneo. Se stai implementando PIM come soluzione nativa, questi account possono essere utilizzati dal personale amministrativo per eseguire il popolamento del gruppo temporaneo e se stai implementando un PIM tramite software di terzi, potrebbe essere in grado di adattarsi a questi account per funzionare come account del servizio.  
  
> [!NOTE]  
> Le procedure descritte in questa appendice forniscono un approccio per la gestione di gruppi con privilegi elevati in Active Directory. È possibile adattare queste procedure in base alle proprie esigenze, aggiungere altre restrizioni oppure omettere alcune limitazioni che sono descritti di seguito.  
  
### <a name="creating-management-accounts-for-protected-accounts-and-groups-in-active-directory"></a>Creazione di gestione degli account per gli account protetti e gruppi in Active Directory  
Creazione di account che può essere utilizzata per gestire l'appartenenza di gruppi con privilegi senza richiedere gli account di gestione per concedere diritti eccessivo e delle autorizzazioni è costituito da quattro attività generali descritte nelle istruzioni che seguono:  
  
1.  Prima di tutto, è necessario creare un gruppo che gestirà gli account, perché questi account devono essere gestiti da un set limitato di utenti attendibili. Se non si dispone già di una struttura di unità Organizzativa che supporta limitando gli account con privilegi e protetti e i sistemi al pubblico generale nel dominio, è necessario crearne uno. Sebbene in questa appendice non vengono fornite istruzioni specifiche, screenshot mostrano un esempio di tale gerarchia di unità Organizzative.  
  
2.  Creare gli account di gestione. Questi account devono essere creati come account utente "normale" e senza diritti utente oltre a quelli che sono già concesse agli utenti per impostazione predefinita.  
  
3.  Implementare restrizioni degli account di gestione che li utilizzabile solo per lo scopo specifico per cui sono state create, oltre a controllare chi può attivare e utilizzare gli account (il gruppo appena creato nel primo passaggio).  
  
4.  Configurare le autorizzazioni sull'oggetto AdminSDHolder in ogni dominio per consentire gli account di gestione modificare l'appartenenza dei gruppi con privilegi nel dominio.  
  
Si deve accuratamente tutte queste procedure di test e modificarle in base alle esigenze dell'ambiente del prima di implementarli in un ambiente di produzione. È inoltre necessario verificare che tutte le impostazioni di funzionano come previsto (alcune procedure di test sono disponibili in questa appendice), ed è necessario testare uno scenario di ripristino di emergenza in cui gli account di gestione non sono disponibili per essere utilizzato per popolare i gruppi protetti per scopi di ripristino. For more information about backing up and restoring Active Directory, see the [AD DS Backup and Recovery Step-by-Step Guide](https://technet.microsoft.com/library/cc771290(v=ws.10).aspx).  
  
> [!NOTE]  
> Implementando i passaggi descritti in questa appendice, si creerà gli account che saranno in grado di gestire l'appartenenza di tutti i gruppi protetti in ogni dominio, non solo i gruppi di Active Directory con privilegi più elevato come EAs, DAs e BAs. Per ulteriori informazioni sui gruppi protetti in Active Directory, vedere [gli account protetti appendice c: e gruppi in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
#### <a name="step-by-step-instructions-for-creating-management-accounts-for-protected-groups"></a>Istruzioni dettagliate per la creazione di account di gestione per gruppi protetti  
  
##### <a name="creating-a-group-to-enable-and-disable-management-accounts"></a>Creazione di un gruppo per abilitare e disabilitare l'account di gestione  
Account di gestione deve avere le proprie password reimpostata in ogni uso e deve essere disabilitato una volta completate le attività che li richiedono. Anche se è anche consigliabile implementare i requisiti di accesso con smart card per questi account, è una configurazione facoltativa e queste istruzioni si presuppone che l'account di gestione verranno configurati con un nome utente e password lunghe e complesse come controlli minimi. In questo passaggio si creerà un gruppo che dispone delle autorizzazioni per reimpostare la password degli account di gestione e di abilitare e disabilitare l'account.  
  
Per creare un gruppo per abilitare e disabilitare l'account di gestione, eseguire i passaggi seguenti:  
  
1.  Nella struttura di unità Organizzativa in cui si verranno alloggiamento gli account di gestione, fare doppio clic su unità Organizzativa in cui si desidera creare il gruppo, fare clic su **New** e fare clic su **gruppo**.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_115.png)  
  
2.  Nel **nuovo oggetto - gruppo** finestra di dialogo immettere un nome per il gruppo. Se si prevede di utilizzare questo gruppo per "attivare" tutti gli account di gestione nell'insieme di strutture, crea un gruppo di sicurezza universale. Se si dispone di una foresta con dominio singolo o se si prevede di creare un gruppo in ogni dominio, è possibile creare un gruppo di sicurezza globale. Fare clic su **OK** per creare il gruppo.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_116.png)  
  
3.  Right-click the group you just created, click **Properties**, and click the **Object** tab. In the group's **Object property** dialog box, select **Protect object from accidental deletion**, which will not only prevent otherwise-authorized users from deleting the group, but also from moving it to another OU unless the attribute is first deselected.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_117.png)  
  
    > [!NOTE]  

    > Se si sono già configurato autorizzazioni padre del gruppo unità organizzative per limitare l'amministrazione a un set limitato di utenti, potrebbe non devi eseguire i passaggi seguenti. Vengono forniti qui in modo che anche se non è stato ancora implementato il controllo amministrativo limitato tramite la struttura di unità Organizzativa in cui si crea questo gruppo, è possibile proteggere il gruppo contro le modifiche da utenti non autorizzati.  
  
4.  Fare clic su di **membri** scheda e aggiungere gli account per i membri del team responsabile per l'attivazione di account di gestione o popolamento protetti gruppi quando necessario.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_118.png)  
  
5.  Se non si è già fatto, nel **Active Directory Users and Computers** console, fare clic su **visualizzazione** e seleziona **funzionalità avanzate**. Right-click the group you just created, click **Properties**, and click the **Security** tab. On the **Security** tab, click **Advanced**.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_119.png)  
  
6.  Nel **impostazioni di sicurezza avanzate per [gruppo]** la finestra di dialogo, fare clic su **Disabilita ereditarietà**. Quando richiesto, fare clic su **Converti autorizzazioni ereditate in autorizzazioni esplicite per questo oggetto**e fare clic su **OK** per tornare al gruppo **sicurezza** la finestra di dialogo.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_120.png)  
  
7.  Nel **sicurezza** scheda, rimuovere i gruppi che non dovrebbero poter accedere a questo gruppo. Se non vuoi Authenticated Users per essere in grado di leggere il nome e le proprietà generali, ad esempio, è possibile rimuovere tale voce. È inoltre possibile rimuovere le voci ACE, ad esempio quelli relativi account operators e precedente a Windows 2000 Server accesso compatibile. Tuttavia, lasciare un set minimo di autorizzazioni per gli oggetti in posizione. Lascia intatti seguenti voci:  
  
    -   AUTOMATICO  
  
    -   SISTEMA  
  
    -   Domain Admins  
  
    -   Enterprise Admins  
  
    -   Amministratori  
  
    -   Gruppo di accesso autorizzazione Windows (se applicabile)  
  
    -   CONTROLLER DI DOMINIO ORGANIZZAZIONE  
  
    Anche se potrebbe sembrare illogico per consentire i gruppi con privilegi più elevati in Active Directory per gestire questo gruppo, l'obiettivo dell'implementazione di queste impostazioni è non per impedire ai membri dei gruppi di apportare modifiche autorizzate. L'obiettivo è piuttosto, assicurarsi che dopo aver occasione per richiedere livelli molto alti dei privilegi, le modifiche autorizzate avranno esito positivo. È per questo motivo che modifica l'impostazione predefinita la nidificazione dei gruppi, diritti con privilegi e le autorizzazioni sono sconsigliate in questo documento. Lasciando intatti strutture predefinito e svuotino l'appartenenza dei gruppi con privilegi di livello più alti nella directory, è possibile creare un ambiente più sicuro che funzioni come previsto.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_121.png)  
  
    > [!NOTE]  
    > Se non si sono già configurato Criteri di controllo per gli oggetti nella struttura di unità Organizzativa in cui è stato creato il gruppo, è necessario configurare il controllo per registrare le modifiche in questo gruppo.  
  
8.  Configurazione del gruppo che verrà utilizzato per "estrarre" è stata completata di gestione degli account quando sono necessari e "controllare" gli account quando sono state completate le loro attività.  
  
##### <a name="creating-the-management-accounts"></a>Creare gli account di gestione  
È necessario creare almeno un account che verrà utilizzato per gestire l'appartenenza di gruppi con privilegi nell'installazione di Active Directory e preferibilmente un secondo account da utilizzare come una copia di backup. Se si sceglie di creare gli account di gestione in un singolo dominio nella foresta e concedere loro funzionalità di gestione per tutti i domini protected gruppi o se si sceglie di implementare gli account di gestione in ogni dominio nella foresta, le procedure descritte in modo efficace sono uguali.  
  
> [!NOTE]  
> I passaggi descritti in questo documento si presuppongono che non è stato ancora implementato i controlli di accesso in base al ruolo e la gestione delle identità con privilegi per Active Directory. Pertanto, alcune procedure devono essere eseguite da un utente il cui account è un membro del gruppo Domain Admins per il dominio in questione.  
>   
> Quando si utilizza un account con privilegi DA, è possibile accedere a un controller di dominio per eseguire le attività di configurazione. I passaggi che non richiedono privilegi DA possono essere eseguiti da account con meno privilegi che sono connessi a workstation amministrative. Screenshot che mostra le finestre di dialogo bordo di colore azzurro rappresentano le attività che possono essere eseguite in un controller di dominio. Screenshot che mostra le finestre di dialogo di colore blu rappresentano le attività che possono essere eseguite sulle workstation amministrative con gli account che dispone di privilegi limitati.  
  
Per creare gli account di gestione, eseguire i passaggi seguenti:  
  
1.  Accedere a un controller di dominio con un account che sia un membro del gruppo DA del dominio.  
  
2.  Avviare **Active Directory Users and Computers** e passare all'unità Organizzativa in cui verrà creato l'account di gestione.  
  
3.  Fare doppio clic su unità Organizzativa e fare clic su **New** e fare clic su **utente**.  
  
4.  Nel **nuovo oggetto - utente** finestra di dialogo casella, immettere le informazioni di denominazione desiderate per l'account e fare clic su **Avanti**.  
  
5.  Accedere a un controller di dominio con un account che sia un membro del gruppo DA del dominio.  
  
6.  Avviare **Active Directory Users and Computers** e passare all'unità Organizzativa in cui verrà creato l'account di gestione.  
  
7.  Fare doppio clic su unità Organizzativa e fare clic su **New** e fare clic su **utente**.  
  
8.  Nel **nuovo oggetto - utente** finestra di dialogo casella, immettere le informazioni di denominazione desiderate per l'account e fare clic su **Avanti**.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_122.png)  
  
9. Fornire una password iniziale per l'account utente, deselezionare **cambiamento obbligatorio password all'accesso successivo**selezionare **cambiamento password non** e **Account è disabilitato**e fare clic su **Avanti**.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_123.png)  
  
10. Verificare che i dettagli dell'account siano corretti e fare clic su **fine**.  
  
11. Fare doppio clic su oggetto utente appena creato e fare clic su **proprietà**.  
  
12. Fare clic su di **Account** scheda.  
  
13. Nel **opzioni Account** campi, selezionare il **Account è sensibile e non può essere delegato** flag, selezionare il **questo account supporta la crittografia Kerberos AES 128 bit** e/o **questo account supporta la crittografia AES Kerberos 256** flag e fare clic su **OK**.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_124.png)  
  
    > [!NOTE]  
    > Poiché questo account, ad esempio altri account dispone di una funzione limitata, ma potente, l'account deve essere utilizzato solo su host amministrativi protetti. Per host amministrativi sicuro tutti nell'ambiente in uso, è consigliabile implementare l'impostazione di criteri di gruppo **sicurezza di rete: tipi di configurazione di crittografia consentiti per Kerberos** per consentire solo i tipi di crittografia più sicuri, è possibile implementare per gli host protetti.  
    >   
    > A differenza che implementa i tipi di crittografia più sicuri per gli host non attenuare gli attacchi contro il furto di credenziali, l'uso appropriato e la configurazione degli host protetto non. L'impostazione di tipi di crittografia più avanzati per gli host che vengono utilizzati solo per gli account con privilegi semplicemente riduce la superficie di attacco complessiva dei computer.  
    >   
    > Per ulteriori informazioni sulla configurazione dei tipi di crittografia nei sistemi e gli account, vedere [le configurazioni di Windows per tipo di crittografia supportati Kerberos](http://blogs.msdn.com/b/openspecification/archive/2011/05/31/windows-configurations-for-kerberos-supported-encryption-type.aspx).  
    >   
    > **Nota** queste impostazioni sono supportate solo nei computer che eseguono Windows Server 2012, Windows Server 2008 R2, Windows 8 o Windows 7.  
  
14. Nel **oggetto** selezionare **Proteggi oggetto da eliminazioni accidentali**. Non solo impedirà l'oggetto viene eliminata (anche dagli utenti autorizzati), ma ne impedirà da spostare in un'altra unità Organizzativa nella gerarchia di dominio Active Directory, a meno che la casella di controllo è deselezionata prima da un utente con autorizzazione per modificare l'attributo.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_125.png)  
  
15. Fare clic su di **telecomando** scheda.  
  
16. Cancella il **abilitare il controllo remoto** flag. Non devono essere mai necessario per il personale di supporto per la connessione alle sessioni dell'account per implementare le correzioni.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_126.png)  
  
    > [!NOTE]  
    > Ogni oggetto in Active Directory deve avere un proprietario designato IT e un proprietario designato business, come descritto in [pianificazione del compromesso](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md). Se si verifica la proprietà di oggetti di dominio Active Directory in Active Directory (invece di un database esterno), è necessario immettere informazioni sulla proprietà appropriate nelle proprietà dell'oggetto.  
    >   
    > In questo caso, il titolare dell'azienda è molto probabile che una divisione IT, andthere non è divieto imprenditori anche essere proprietari IT. Il punto di definizione di proprietà degli oggetti è consentono di identificare i contatti quando le modifiche devono essere apportate agli oggetti, ad esempio anni dalla loro creazione iniziale.  
  
17. Fare clic su di **organizzazione** scheda.  
  
18. Immettere le informazioni necessarie in standard dell'oggetto di dominio Active Directory.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_127.png)  
  
19. Fare clic su di **Dial-in** scheda.  
  
20. Nel **autorizzazione di accesso alla rete** selezionare **negare l'accesso**. Questo account mai necessario connettersi tramite una connessione remota.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_128.png)  
  
    > [!NOTE]  
    > È improbabile che questo account verrà utilizzato per accedere al controller di dominio di sola lettura (RODC) nell'ambiente in uso. Tuttavia, circostanza mai richiede l'account per accedere a un controller di dominio, è necessario aggiungere questo account per il gruppo di replica Password RODC negato in modo che la password non viene memorizzato nella cache nel RODC.  
    >   
    > Anche se la password dell'account deve essere reimpostata dopo ogni uso e l'account deve essere disabilitato, implementare questa impostazione non ha un effetto induce l'account e può essere utile nelle situazioni in cui un amministratore si dimentica di reimpostare la password dell'account e disabilitarlo.  
  
21. Fare clic su di **membro di** scheda.  
  
22. Fare clic su **aggiungere**.  
  
23. Tipo **negato replica passw** nel **Seleziona utenti, contatti, computer** la finestra di dialogo e fare clic su **Controlla nomi**. Quando il nome del gruppo viene sottolineato nella selezione oggetto, fare clic su **OK** e verificare che l'account è un membro dei gruppi di due visualizzato nella schermata seguente. Non aggiungere l'account a tutti i gruppi protetti.  
  
24. Fare clic su **OK**.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_129.png)  
  
25. Fare clic su di **sicurezza** scheda e fare clic su **avanzate**.  
  
26. Nel **impostazioni di sicurezza avanzate** la finestra di dialogo, fare clic su **Disabilita ereditarietà** e copiare le autorizzazioni ereditate le autorizzazioni esplicite e fare clic su **Aggiungi**.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_130.png)  
  
27. Nel **voce autorizzazione per [Account]** la finestra di dialogo, fare clic su **seleziona un'entità** e aggiungere il gruppo creato nella procedura precedente. Scorrere fino alla parte inferiore della finestra di dialogo e fare clic su **Cancella tutto** per rimuovere tutte le autorizzazioni predefinite.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_131.png)  
  
28. Scorri fino alla parte superiore del **voce autorizzazione** la finestra di dialogo. Assicurarsi che il **tipo** elenco a discesa è impostato su **Consenti**e il **si applica a** elenco a discesa, selezionare **solo questo oggetto**.  
  
29. Nel **autorizzazioni** selezionare **Leggi tutte le proprietà**, **delle autorizzazioni di lettura**, e **reimpostazione password**.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_132.png)  
  
30. Nel **proprietà** selezionare **leggere userAccountControl** e **scrivere userAccountControl**.  
  
31. Fare clic su **OK**, **OK** nuovamente il **impostazioni di sicurezza avanzate** la finestra di dialogo.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_133.png)  
  
    > [!NOTE]  
    > Il **userAccountControl** attributo controlla più opzioni di configurazione di account. È possibile concedere l'autorizzazione per modificare solo alcune delle opzioni di configurazione quando si concede l'autorizzazione di scrittura all'attributo.  
  
32. Nel **utenti e gruppi** campo del **sicurezza** scheda, rimuovere tutti i gruppi che devono essere consentiti di accedere o gestire l'account. Non rimuovere tutti i gruppi che sono stati configurati con le ACE di negazione, ad esempio il gruppo Everyone e SELF calcolato account (ne è stato impostato quando la **cambiamento password non** flag è stato abilitato durante la creazione dell'account. Non rimuovere anche il gruppo che appena aggiunto, l'account di sistema o gruppi di EA, DA, BA o gruppo di accesso autorizzazione Windows.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_134.png)  
  
33. Fare clic su **avanzate** e verificare che la finestra di dialogo Impostazioni avanzate di sicurezza è simile allo screenshot seguente.  
  
34. Fare clic su **OK**, e **OK** per chiudere la finestra di dialogo di proprietà dell'account.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_135.png)  
  
35. Configurazione dell'account di gestione prima è stata completata. Si eseguirà il test dell'account in una procedura successiva.  
  
###### <a name="creating-additional-management-accounts"></a>Creazione di account di gestione aggiuntiva  
È possibile creare gli account di gestione aggiuntivo ripetendo i passaggi precedenti, copiando l'account che appena creato o creando uno script per creare gli account con le impostazioni di configurazione desiderata. Si noti tuttavia che se si copia l'account che appena creato, molte delle impostazioni personalizzate e gli ACL non verranno copiati nel nuovo account e sarà necessario ripetere la maggior parte dei passaggi di configurazione.  
  
In alternativa, è possibile creare un gruppo a cui si delegare i diritti per compilare e unpopulate gruppi protetti, ma è necessario proteggere il gruppo e gli account inserite in essa contenuti. Poiché devono essere pochissimi account nella directory che sono concesse la possibilità di gestire l'appartenenza di gruppi protetti, la creazione di singoli account potrebbe essere l'approccio più semplice.  
  
Indipendentemente dalla modalità si sceglie di creare un gruppo in cui inserire gli account di gestione, è necessario assicurarsi che ogni account è stato protetto come descritto in precedenza. È inoltre consigliabile implementare restrizioni oggetto Criteri di gruppo simili a quelli descritti [appendice d: protezione predefiniti Administrator account in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
  
###### <a name="auditing-management-accounts"></a>Controllo degli account di gestione  
È necessario configurare il controllo dell'account per accedere, come minimo, tutte le operazioni di scrittura per l'account. In questo modo che è non solo per identificare corretta abilitazione dell'account e la reimpostazione della password del proprio durante usi autorizzati, ma anche identificare i tentativi da utenti non autorizzati di modificare l'account. Operazioni di scrittura non riuscite per l'account deve essere acquisite nel sistema informazioni sulla protezione e il monitoraggio di eventi (SIEM) (se applicabile) e devono attivare avvisi che forniscono notifiche per il personale IT responsabile dell'analisi potenziali compromissioni.  
  
Soluzioni SIEM ricevono informazioni sull'evento da origini di sicurezza necessari (ad esempio, i registri eventi, dati dell'applicazione, flussi di rete, prodotti antimalware e origini di rilevamento delle intrusioni), collate i dati e tentano di eseguire intelligente visualizzazioni e azioni proattive. Sono disponibili molte soluzioni SIEM commerciali e molte aziende creare implementazioni private. Monitoraggio di sicurezza e capacità di risposta agli eventi imprevisti, può migliorare notevolmente un SIEM ben progettata e implementata in modo appropriato. Tuttavia, funzionalità e l'accuratezza variare notevolmente tra soluzioni. Soluzioni Siem non rientrano nell'ambito di questo documento, ma i suggerimenti di eventi specifici contenuti devono essere considerati da qualsiasi implementatore SIEM.  
  
Per ulteriori informazioni sulle impostazioni di configurazione di controllo consigliate per i controller di dominio, vedere [monitoraggio di Active Directory per i segni di compromissione](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md). Le impostazioni di configurazione specifici del controller di dominio sono incluse nei [monitoraggio di Active Directory per i segni di compromissione](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md).  
  
##### <a name="enabling-management-accounts-to-modify-the-membership-of-protected-groups"></a>Abilitazione di account di gestione modificare l'appartenenza di gruppi protetti  
In questa procedura, si configurerà le autorizzazioni sull'oggetto AdminSDHolder del dominio per consentire gli account di gestione appena creato per la modifica dell'appartenenza di gruppi protetti nel dominio. Questa procedura non può essere eseguita tramite un'interfaccia utente grafica (GUI).  
  
Come descritto in [gli account protetti appendice c: e gruppi in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md), l'ACL AdminSDHolder del dominio oggetto viene effettivamente "copiato" oggetti protetti quando viene eseguita l'attività SDProp. Gli account e gruppi protetti non ereditano le autorizzazioni dall'oggetto AdminSDHolder; le autorizzazioni vengono impostate in modo esplicito affinché corrispondano a quelle dell'oggetto AdminSDHolder. Pertanto, quando si modificano le autorizzazioni per l'oggetto AdminSDHolder, è necessario modificarli per gli attributi che sono appropriati per il tipo dell'oggetto protetto di destinazione.  
  
In questo caso, si concedono gli account di gestione appena creato per consentire loro di lettura e scrittura dell'attributo i membri per gli oggetti gruppo. Tuttavia, l'oggetto AdminSDHolder non è un oggetto gruppo e gli attributi di gruppo non sono esposte nell'editor ACL di grafica. È per questo motivo verrà implementare le modifiche delle autorizzazioni tramite l'utilità della riga di comando Dsacls. Per concedere le autorizzazioni di account per modificare l'appartenenza di gruppi protetti di gestione (disabilitata), eseguire i passaggi seguenti:  
  
1.  Accedere a un controller di dominio, preferibilmente il controller di dominio che detiene il ruolo emulatore PDC (emulatore PDC), con le credenziali di un account utente che è stata effettuata un membro del gruppo DA nel dominio.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_136.png)  
  
2.  Aprire un prompt dei comandi con privilegi elevati facendo **prompt dei comandi** e fare clic su **Esegui come amministratore**.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_137.gif)  
  
3.  Quando viene richiesto di approvare l'elevazione dei privilegi, fare clic su **Sì**.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_138.gif)  
  
    > [!NOTE]  
    > For more information about elevation and user account control (UAC) in Windows, see [UAC Processes and Interactions](https://technet.microsoft.com/library/dd835561(v=WS.10).aspx) on the TechNet website.  
  
4.  Al Prompt dei comandi, digitare (sostituendo le informazioni specifiche di dominio) **Dsacls [nome distinto dell'oggetto AdminSDHolder nel dominio] /G [account di gestione UPN]: RPWP; membro**.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_139.gif)  
  
    Il comando precedente (che non è tra maiuscole e minuscole) funziona come segue:  
  
    -   DSACLS imposta o Visualizza le voci ACE per gli oggetti directory  
  
    -   CN = AdminSDHolder, CN = System, DC = TailSpinToys, DC = msft identifica l'oggetto da modificare  
  
    -   /G indica che è stata configurata una voce ACE di concedere  
  
    -   PIM001@tailspintoys.msftè il nome dell'entità di utente (UPN) dell'entità di sicurezza a cui verranno concesso le voci ACE  
  
    -   Concede RPWP proprietà autorizzazioni lettura e scrittura proprietà  
  
    -   Membro è il nome della proprietà (attributo) in cui verranno impostate le autorizzazioni  
  
    Per ulteriori informazioni sull'utilizzo di **Dsacls**, digitare Dsacls senza parametri in un prompt dei comandi.  
  
    Se hai creato più account di gestione per il dominio, è necessario eseguire il comando Dsacls per ogni account. Dopo aver completato la configurazione di ACL nell'oggetto AdminSDHolder, è necessario forzare SDProp se eseguire o attendere l'esecuzione pianificata. Per informazioni sull'avvio forzato SDProp eseguire, vedere "Esecuzione SDProp manualmente" nella [gli account protetti appendice c: e gruppi in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
    Quando si è eseguito SDProp, è possibile verificare che le modifiche apportate all'oggetto AdminSDHolder siano state applicate ai gruppi protetti nel dominio. È possibile eseguire questa verifica, visualizzando l'ACL per l'oggetto AdminSDHolder per i motivi descritti in precedenza, ma è possibile verificare che le autorizzazioni siano state applicate visualizzando gli ACL nei gruppi protetti.  
  
5.  In **Active Directory Users and Computers**, verificare che sia stata abilitata **funzionalità avanzate**. A tale scopo, fare clic su **visualizzazione**, individuare il **Domain Admins** gruppo, fare doppio clic il gruppo e fare clic su **proprietà**.  
  
6.  Fare clic su di **sicurezza** scheda e fare clic su **avanzate** per aprire il **impostazioni di sicurezza avanzate per Domain Admins** la finestra di dialogo.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_140.gif)  
  
7.  Selezionare **ACE consentire per l'account di gestione** e fare clic su **modifica**. Verificare che l'account sia stata concessa solo **membri di lettura** e **membri di scrittura** le autorizzazioni per il gruppo DA e fare clic su **OK**.  
  
8.  Fare clic su **OK** nel **impostazioni di sicurezza avanzate** la finestra di dialogo, fare clic su **OK** per chiudere la finestra di dialogo proprietà per il gruppo DA.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_141.gif)  
  
9. È possibile ripetere i passaggi precedenti per altri gruppi protetti nel dominio. le autorizzazioni devono essere lo stesso per tutti i gruppi protetti. Creazione e la configurazione degli account di gestione per i gruppi protetti in questo dominio è stata completata.  
  
    > [!NOTE]  
    > Qualsiasi account che dispone dell'autorizzazione di scrittura appartenenza di un gruppo in Active Directory possono anche aggiungere al gruppo. Questo comportamento è previsto e non può essere disabilitato. Per questo motivo, puoi tenere sempre disabilitato quando non è in uso di account di gestione e gli account devono monitorare attentamente quando che stai disabilitati e quando sono in uso.  
  
##### <a name="verifying-group-and-account-configuration-settings"></a>Verifica impostazioni di configurazione di Account e gruppo  
Ora che è creato e configurato l'account di gestione che è possibile modificare l'appartenenza di gruppi protetti nel dominio (che include i gruppi con privilegi più elevati di EA, DA e BA), è necessario verificare che gli account e il proprio gruppo di gestione siano stati creati correttamente. Verifica è costituito da queste attività generali:  
  
1.  Testare il gruppo che è possibile abilitare e disabilitare l'account di gestione per verificare che i membri del gruppo possono abilitano e disabilitare gli account e reimpostare le password, ma non possono eseguire altre attività di amministrazione degli account di gestione.  
  
2.  L'account di gestione per verificare che possono aggiungere e rimuovere membri protetti gruppi nel dominio, ma non le altre proprietà di account protetti e gruppi di test.  
  
###### <a name="test-the-group-that-will-enable-and-disable-management-accounts"></a>Il gruppo che verrà abilitare e disabilitare la gestione account di test  
  
1.  Per testare l'attivazione di un account di gestione e reimpostare la password, accedere a una workstation amministrativa protetta con un account che sia membro del gruppo creato in [appendice i: creazione di account di gestione per gli account protetti e gruppi in Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_142.gif)  
  
2.  Apri **Active Directory Users and Computers**, fare doppio clic sull'account di gestione e fare clic su **Abilita Account**.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_143.gif)  
  
3.  Dovrebbe essere visualizzata una finestra di dialogo, confermare che l'account sia stato abilitato.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_144.gif)  
  
4.  Successivamente, reimpostare la password dell'account di gestione. A tale scopo, fare doppio clic su account di nuovo e quindi fare clic su **Reimposta Password**.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_145.gif)  
  
5.  Digitare una nuova password per l'account nel **nuova password** e **Conferma password** campi e fare clic su **OK**.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_146.gif)  
  
6.  Una finestra di dialogo dovrebbe essere visualizzato, confermare che è stata reimpostata la password per l'account.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_147.gif)  
  
7.  Ora tentare di modificare le proprietà aggiuntive dell'account di gestione. Fare doppio clic su account e fare clic su **proprietà**, scegliere il **controllo remoto** scheda.  
  
8.  Selezionare **abilitare il controllo remoto** e fare clic su **applica**. Operazione non riuscirà e un **accesso negato** deve essere visualizzato il messaggio di errore.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_148.gif)  
  
9. Fare clic su di **Account** scheda per l'account e tentare di modificare l'account, orario di accesso o accesso alle workstation. Tutti devono avere esito negativo e opzioni che non sono controllate da account di **userAccountControl** attributo deve essere disabilitata e disponibile per la modifica.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_149.gif)  
  
10. Tentare di aggiungere il gruppo di gestione a un gruppo protetto, ad esempio il gruppo DA. Quando si fa clic **OK**, un messaggio dovrebbe essere visualizzato, che informa che si dispone delle autorizzazioni per modificare il gruppo.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_150.gif)  
  
11. Eseguire test aggiuntivi, come richiesto per verificare che non è possibile configurare nulla per l'account di gestione, ad eccezione **userAccountControl** impostazioni e la reimpostazione della password.  
  
    > [!NOTE]  
    > Il **userAccountControl** attributo controlla più opzioni di configurazione di account. È possibile concedere l'autorizzazione per modificare solo alcune delle opzioni di configurazione quando si concede l'autorizzazione di scrittura all'attributo.  
  
###### <a name="test-the-management-accounts"></a>Testare l'account di gestione  
Ora che è stata abilitata uno o più account che è possibile modificare l'appartenenza di gruppi protetti, è possibile testare gli account per garantire che è possibile modificare l'appartenenza al gruppo protetto, ma non può eseguire altre modifiche su account protetti e gruppi.  
  
1.  Accedere a un host amministrativo sicuro come il primo account di gestione.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_151.gif)  
  
2.  Avviare **Active Directory Users and Computers** e individuare il **gruppo Domain Admins**.  
  
3.  Fare doppio clic su di **Domain Admins** di gruppo e fare clic su **proprietà**.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_152.gif)  
  
4.  Nel **Domain Admins proprietà**, fare clic su di **membri** scheda e **fare clic su** Aggiungi. Immettere il nome di un account che verrà assegnato temporanei privilegi Domain Admins e fare clic su **Controlla nomi**. Quando il nome dell'account viene sottolineato, fare clic su **OK** per tornare al **membri** scheda.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_153.gif)  
  
5.  Nel **membri** scheda per il **Domain Admins proprietà** la finestra di dialogo, fare clic su **applica**. Dopo aver fatto clic **applica**, l'account deve essere un membro del gruppo DA e dovrebbe essere visualizzato alcun messaggio di errore.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_154.gif)  
  
6.  Fare clic su di **gestito da** nella scheda il **Domain Admins proprietà** finestra di dialogo casella e verificare che non è possibile immettere testo in tutti i campi e tutti i pulsanti vengono disattivati.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_155.gif)  
  
7.  Fare clic su di **generale** nella scheda il **Domain Admins proprietà** finestra di dialogo casella e verificare che non è possibile modificare le informazioni sulla scheda.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_156.gif)  
  
8.  Ripetere questi passaggi per altri gruppi protetti in base alle esigenze. Al termine, accedere a un host amministrativo sicuro con un account che sia membro del gruppo creato per abilitare e disabilitare l'account di gestione. Quindi reimpostare la password dell'account di gestione appena testati e disabilitare l'account. È stata completata la configurazione di account di gestione e il gruppo che sarà responsabile per abilitare e disabilitare l'account.  
  


