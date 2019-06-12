---
ms.assetid: 13fe87d9-75cf-45bc-a954-ef75d4423839
title: 'Appendice I: creazione di account di gestione per gli account protetti e gruppi in Active Directory'
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e90c2c075ba2dc2b63e9a18c9eba192116265b90
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66443519"
---
# <a name="appendix-i-creating-management-accounts-for-protected-accounts-and-groups-in-active-directory"></a>Appendice i: Creazione di gestione di account per gli account protetti e gruppi in Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Una delle difficoltà nell'implementazione di un modello di Active Directory che non richiede l'appartenenza permanente a gruppi con privilegi elevati è che deve essere presente un meccanismo per popolare questi gruppi quando è richiesta l'appartenenza a gruppi. Alcune soluzioni di gestione identità con privilegi richiede che gli account del servizio del software vengono concesse l'appartenenza permanente a gruppi come amministratori in ogni dominio nella foresta o DA. Tuttavia, tecnicamente non è necessario per le soluzioni di Privileged Identity Management (PIM) eseguire i propri servizi in tali contesti con privilegi elevati.  
  
Questa appendice vengono fornite informazioni che è possibile usare per le soluzioni PIM in modo nativo implementate o di terze parti per creare gli account che dispongono di privilegi limitati e possono essere rigorosamente controllati, ma possono essere usati per popolare i gruppi con privilegi in Active Directory quando è richiesta l'elevazione temporanea. Se si sta implementando PIM come soluzione nativa, questi account potrebbero essere usati dal personale amministrativo per eseguire il popolamento del gruppo temporaneo e se si implementa PIM tramite software di terze parti, è possibile adattare questi account per il funzionamento come servizio account.  
  
> [!NOTE]  
> Le procedure descritte in questa appendice forniscono un approccio per la gestione dei gruppi con privilegi elevati in Active Directory. È possibile adattare queste procedure di base alle proprie esigenze, aggiungere ulteriori restrizioni o omettere alcune limitazioni che sono descritte di seguito.  
  
## <a name="creating-management-accounts-for-protected-accounts-and-groups-in-active-directory"></a>Creazione di gestione di account per gli account protetti e gruppi in Active Directory

Creazione di account che può essere utilizzata per gestire l'appartenenza di gruppi con privilegi, senza che richiede gli account di gestione a cui concedere le autorizzazioni e diritti significativamente è costituito da quattro attività generali che sono descritte nelle istruzioni dettagliate che eseguire la seguente:  
  
1.  In primo luogo, è necessario creare un gruppo che verrà gestiti gli account, poiché questi account devono essere gestiti da un set limitato di utenti attendibili. Se si dispone già di una struttura OU appropriata ai memorizzando gli account con privilegi e protetti e i sistemi dal popolamento generale nel dominio, è necessario crearne uno. Sebbene in questa appendice non vengono fornite istruzioni specifiche, screenshot mostrano un esempio di tale gerarchia di unità Organizzative.  
  
2.  Creare gli account di gestione. Questi account devono essere creati come "normali" account utente e non concesso alcun diritto utente oltre a quelli che sono già concesso agli utenti per impostazione predefinita.  
  
3.  Implementare le restrizioni per gli account di gestione che li rendono utilizzabile solo per lo scopo specializzato per il quale sono stati creati, oltre a controllare chi può abilitare e usare gli account (il gruppo creato nel primo passaggio).  
  
4.  Configurare le autorizzazioni per l'oggetto AdminSDHolder in ogni dominio per consentire gli account di gestione modificare l'appartenenza dei gruppi con privilegi nel dominio.  
  
Si deve accuratamente tutte queste procedure di test e modificarle secondo necessità per l'ambiente prima di implementarli in un ambiente di produzione. È inoltre necessario verificare che tutte le impostazioni di funzionano come previsto (alcune procedure di test vengono forniti in questa appendice), e testare uno scenario di ripristino di emergenza in cui gli account di gestione non sono disponibili da utilizzare per popolare i gruppi protetti per il ripristino a scopo. Per altre informazioni sul backup e ripristino di Active Directory, vedere la [AD DS Guida al Backup e ripristino dettagliata](https://technet.microsoft.com/library/cc771290(v=ws.10).aspx).  
  
> [!NOTE]  
> Implementando i passaggi descritti in questa appendice, verranno creati account che sarà in grado di gestire l'appartenenza di tutti i gruppi protetti in ogni dominio, non solo i gruppi di Active Directory di privilegi più elevati, ad esempio EAs, DAs ed BAs. Per altre informazioni sui gruppi protetti in Active Directory, vedere [appendice c: Account protetti e gruppi in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
### <a name="step-by-step-instructions-for-creating-management-accounts-for-protected-groups"></a>Istruzioni dettagliate per la creazione di account di gestione di gruppi protetti  
  
#### <a name="creating-a-group-to-enable-and-disable-management-accounts"></a>Creazione di un gruppo per abilitare e disabilitare gli account di gestione

Gli account di gestione devono avere le proprie password reimpostata in ogni uso e devono essere disabilitati una volta completate le attività che li richiedono. Anche se è anche possibile implementare i requisiti di accesso della smart card per questi account, è una configurazione facoltativa e queste istruzioni si presuppone che gli account di gestione verranno configurati con un nome utente e una password lunga e complessa come minimo controlli. In questo passaggio si creerà un gruppo che dispone delle autorizzazioni per reimpostare la password negli account di gestione e per abilitare e disabilitare gli account.  
  
Per creare un gruppo per abilitare e disabilitare gli account di gestione, procedere come segue:  
  
1.  Nella struttura dell'unità Organizzativa in cui si verranno alloggiamento gli account di gestione, fare doppio clic su unità Organizzativa in cui si desidera creare il gruppo, fare clic su **New** e fare clic su **gruppo**.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_115.png)  
  
2.  Nel **nuovo oggetto - gruppo** finestra di dialogo immettere un nome per il gruppo. Se si prevede di usare questo gruppo per tutti gli account di gestione dell'insieme di strutture "attiva", renderlo un gruppo di protezione universale. Se si dispone di una foresta con dominio singolo o se si prevede di creare un gruppo in ogni dominio, è possibile creare un gruppo di sicurezza globale. Fare clic su **OK** per creare il gruppo.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_116.png)  
  
3.  Fare clic con il pulsante destro del mouse sul gruppo appena creato, fare clic su **Proprietà**e selezionare la scheda **Oggetto** . Del gruppo **proprietà dell'oggetto** finestra di dialogo **Proteggi oggetto da eliminazioni accidentali**, che non solo impedirà a utenti con autorizzazioni di eliminare il gruppo, ma anche dal spostandolo un'altra OU a meno che l'attributo è il primo deselezionati.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_117.png)  
  
    > [!NOTE]  
    > Se è già stato configurato le autorizzazioni su unità organizzative per limitare l'amministrazione a un set limitato di utenti padre del gruppo, potrebbe non devi eseguire la procedura seguente. Vengono forniti di seguito in modo che anche se non è stato ancora implementato un controllo amministrativo limitato sulla struttura dell'unità Organizzativa in cui è stato creato questo gruppo, è possibile proteggere il gruppo modificato dagli utenti non autorizzati.  
  
4.  Scegliere il **membri** scheda e aggiungere gli account per i membri del team che saranno responsabili per l'abilitazione di account di gestione o popolamento protette gruppi quando necessario.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_118.png)  
  
5.  Se non si già fatto, nel **Active Directory Users and Computers** console, fare clic su **View** e selezionare **Advanced Features**. Pulsante destro del mouse il gruppo appena creato, fare clic su **delle proprietà**, fare clic sui **sicurezza** scheda. Nella scheda **Sicurezza** fare clic su **Avanzate**.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_119.png)  
  
6.  Nel **impostazioni di sicurezza avanzate per [gruppo]** finestra di dialogo, fare clic su **Disabilita ereditarietà**. Quando richiesto, fare clic su **Converti autorizzazioni ereditate in autorizzazioni esplicite tohoto objektu**, fare clic su **OK** per tornare al gruppo **sicurezza** nella finestra di dialogo.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_120.png)  
  
7.  Nel **sicurezza** scheda, rimuovere i gruppi che non sono autorizzati per accedere a questo gruppo. Ad esempio, se si preferisce che non autenticato agli utenti di essere in grado di leggere il gruppo nome e delle proprietà generale, è possibile rimuovere questa voce ACE. È anche possibile rimuovere le voci ACE, ad esempio quelli relativi account operatori e precedente a Windows 2000 Server compatibile con l'accesso. È consigliabile, tuttavia, lasciare un set minimo di autorizzazioni per oggetti posto. Lasciare intatto seguenti voci:  
  
    -   SELF  
  
    -   SISTEMA  
  
    -   Domain Admins  
  
    -   Enterprise Admins  
  
    -   Administrators  
  
    -   Gruppo di accesso autorizzazione Windows (se applicabile)  
  
    -   CONTROLLER DI DOMINIO ORGANIZZAZIONE  
  
    Anche se potrebbe sembrare illogico consentire o meno i gruppi con privilegi più elevati in Active Directory per gestire questo gruppo, l'obiettivo dell'implementazione di queste impostazioni non è impedire ai membri dei gruppi di apportare modifiche autorizzate. Piuttosto, l'obiettivo è garantire che dopo aver occasione per richiedere livelli di privilegio molto elevati, autorizzate modifiche avrà esito positivo. È per questo motivo che modifica l'impostazione predefinita la nidificazione dei gruppi, i diritti, con privilegi e autorizzazioni non sono consigliate in questo documento. Intatti strutture predefinite e svuotamento l'appartenenza dei gruppi con privilegi più elevati nella directory, è possibile creare un ambiente più sicuro che funzioni come previsto.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_121.png)  
  
    > [!NOTE]  
    > Se non si sono già configurato i criteri di controllo per gli oggetti nella struttura dell'unità Organizzativa in cui è stato creato questo gruppo, è necessario configurare il controllo per registrare le modifiche di questo gruppo.  
  
8.  Aver completato la configurazione del gruppo che verrà usato per "check out" di gestione degli account sono necessarie e "check in" gli account quando sono state completate le attività.  
  
#### <a name="creating-the-management-accounts"></a>Creare gli account di gestione

È necessario creare almeno un account che verrà usato per gestire l'appartenenza di gruppi con privilegi nell'installazione di Active Directory e, preferibilmente un secondo account come una copia di backup. Se si sceglie di creare gli account di gestione in un singolo dominio nella foresta e concedere loro le funzionalità di gestione per tutti i domini di gruppi protetti, oppure se si sceglie di implementare gli account di gestione in ogni dominio nella foresta, le procedure sono in modo efficace lo stesso.  
  
> [!NOTE]  
> Le procedure descritte in questo documento presuppongono che non è stato ancora implementato i controlli di accesso basato sui ruoli e privileged identity management per Active Directory. Di conseguenza, alcune procedure devono essere eseguite da un utente il cui account è un membro del gruppo Domain Admins per il dominio in questione.  
>   
> Quando si usa un account con privilegi DA, è possibile accedere a un controller di dominio per eseguire le attività di configurazione. I passaggi che non richiedono privilegi DA possono essere eseguiti dagli account con meno privilegi che vengono registrati workstation amministrative. Screenshot che mostra le finestre di dialogo bordo di colore azzurro rappresentano attività che possono essere eseguite in un controller di dominio. Screenshot che mostra le finestre di dialogo colore blu rappresentano le attività che possono essere eseguite su workstation amministrative con gli account che dispongono di privilegi limitati.  
  
Per creare gli account di gestione, procedere come segue:  
  
1. Accedere al controller di dominio con un account che sia un membro del gruppo DA del dominio.  

2. Avvio veloce **Active Directory Users and Computers** e passare all'unità Organizzativa in cui si creerà l'account di gestione.  

3. Fare doppio clic su unità Organizzativa e fare clic su **New** e fare clic su **utente**.  

4. Nel **nuovo oggetto - utente** finestra di dialogo casella, immettere le informazioni di denominazione desiderate per l'account e fare clic su **successivo**.  

   ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_122.png)  
  
5. Fornire una password iniziale per l'account utente, deselezionare **cambiamento obbligatorio password all'accesso successivo**, selezionare **cambiamento password non consentito** e **Account disabilitato**, e Fare clic su **successivo**.  

   ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_123.png)  

6. Verificare che i dettagli dell'account siano corretti e fare clic su **fine**.  

7. Fare doppio clic sull'oggetto utente appena creato e fare clic su **proprietà**.  

8. Scegliere il **Account** scheda.  

9. Nel **opzioni di Account** campi, selezionare la **Account è sensibile e non può essere delegato** flag, selezionare il **questo account supporta la crittografia Kerberos AES 128 bit** e/o il **questo account supporta la crittografia Kerberos AES 256** flag e fare clic su **OK**.  

   ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_124.png)  

   > [!NOTE]  
   > Poiché questo account, come altri account, avrà una funzione limitata, ma potente, l'account deve essere utilizzato solo su host amministrativi protetti. Per tutti i host amministrativi protetti nell'ambiente in uso, è consigliabile implementare l'impostazione di criteri di gruppo **sicurezza di rete: Configurare i tipi di crittografia consentiti per Kerberos** per consentire solo i tipi di crittografia più sicuri è possibile implementare per host protetti.  
   >
   > Sebbene l'implementazione di tipi di crittografia più sicuri per gli host non consenta di limitare gli attacchi contro il furto di credenziali, l'utilizzo appropriato e la configurazione degli host protetto esegue. L'impostazione di tipi di crittografia più avanzati per gli host che vengono usati solo per gli account con privilegi semplicemente riduce la superficie di attacco complessiva dei computer.  
   >
   > Per altre informazioni sulla configurazione dei tipi di crittografia nei sistemi e gli account, vedere [configurazioni di Windows per il tipo di crittografia supportati Kerberos](http://blogs.msdn.com/b/openspecification/archive/2011/05/31/windows-configurations-for-kerberos-supported-encryption-type.aspx).  
   >
   > Queste impostazioni sono supportate solo nei computer che eseguono Windows Server 2012, Windows Server 2008 R2, Windows 8 o Windows 7.  
  
10. Nel **oggetti** scheda, seleziona **Proteggi oggetto da eliminazioni accidentali**. Ciò non solo impedirà l'oggetto in corso l'eliminazione (anche dagli utenti autorizzati), ma ne impediscono viene spostato in un'altra unità Organizzativa nella gerarchia di Active Directory Domain Services, a meno che la casella di controllo viene innanzitutto cancellata da un utente con autorizzazione per modificare l'attributo.  

    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_125.png)  

11. Scegliere il **controllo remoto** scheda.  

12. Cancella il **abilitare il controllo remoto** flag. Non deve mai essere necessaria per il personale di supporto per la connessione alle sessioni dell'account per implementare le correzioni.  

    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_126.png)  

    > [!NOTE]  
    > Ogni oggetto in Active Directory deve avere un proprietario designato IT e imprenditori designato, come descritto in [pianificazione del compromesso](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md). Se si stanno registrando la proprietà degli oggetti di Active Directory Domain Services in Active Directory (invece di un database esterno), è necessario immettere informazioni sulla proprietà appropriata nelle proprietà dell'oggetto.  
    >
    > In questo caso, il titolare dell'organizzazione è probabile che una divisione IT, andthere non è nessuna divieto di proprietari di business in corso anche i proprietari IT. Il punto di stabilire la proprietà degli oggetti è consentire di identificare i contatti quando le modifiche devono essere apportate agli oggetti, ad esempio anni dalla data di creazione iniziale.  

13. Fare clic sui **organizzazione** scheda.  

14. Immettere le informazioni necessarie nella standard dell'oggetto Active Directory Domain Services.  

    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_127.png)  

15. Fare clic sui **Dial-in** scheda.  

16. Nel **l'autorizzazione di accesso di rete** campi, selezionare **negare l'accesso**. Questo account non deve mai essere necessario connettersi tramite una connessione remota.  

    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_128.png)  

    > [!NOTE]  
    > È improbabile che questo account verrà utilizzato per accedere al controller di dominio di sola lettura (RODC) nell'ambiente in uso. Tuttavia, debba circostanza mai richiedono che l'account per accedere a un RODC, è necessario aggiungere questo account al gruppo di replica Password RODC negata in modo che la password non viene memorizzato nella cache nel RODC.  
    >
    > Anche se la password dell'account deve essere reimpostata dopo ogni uso e l'account deve essere disabilitata, l'implementazione di questa impostazione non ha un effetto deleterio sull'account e può essere utile nelle situazioni in cui si dimentica un amministratore di reimpostare l'account la password e disabilitare la funzionalità.  

17. Fare clic sulla scheda **Membro di**.  

18. Fai clic su **Aggiungi**.  

19. Tipo di **negato replica passw** nel **Seleziona utenti, contatti, computer** la finestra di dialogo e fare clic su **Controlla nomi**. Quando il nome del gruppo è sottolineato nel selettore oggetti, fare clic su **OK** e verificare che l'account è ora un membro dei due gruppi visualizzato nello screenshot seguente. Non aggiungere l'account per tutti i gruppi protetti.  

20. Fare clic su **OK**.  

    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_129.png)  

21. Scegliere il **sicurezza** scheda e fare clic su **avanzate**.  

22. Nel **impostazioni di sicurezza avanzate** finestra di dialogo, fare clic su **Disabilita ereditarietà** e copiare le autorizzazioni ereditate le autorizzazioni esplicite e fare clic su **Aggiungi**.  

    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_130.png)  

23. Nel **voci di autorizzazione per [Account]** finestra di dialogo, fare clic su **seleziona un'entità** e aggiungere il gruppo appena creato nella procedura precedente. Scorrere fino alla fine della finestra di dialogo e fare clic su **Cancella tutto** per rimuovere tutte le autorizzazioni predefinite.  

    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_131.png)  

24. Scorrere fino alla parte superiore della **voce di autorizzazione** nella finestra di dialogo. Verificare che il **tipo** elenco a discesa è impostata su **Consenti**e nel **si applica a** elenco a discesa, seleziona **solo questo oggetto**.  

25. Nel **le autorizzazioni** campi, selezionare **Leggi tutte le proprietà**, **autorizzazioni di lettura**, e **Reimposta password**.  

    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_132.png)  

26. Nel **delle proprietà** campi, selezionare **leggere userAccountControl** e **scrivere userAccountControl**.  

27. Fare clic su **OK**, **OK** nuovamente il **impostazioni di sicurezza avanzate** nella finestra di dialogo.  

    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_133.png)  

    > [!NOTE]  
    > Il **userAccountControl** attributo controlla le opzioni di configurazione di più account. Non è possibile concedere l'autorizzazione per modificare solo alcune delle opzioni di configurazione quando si concede l'autorizzazione di scrittura per l'attributo.  

28. Nel **nomi utente o gruppo** campo le **sicurezza** scheda, rimuovere tutti i gruppi che devono essere consentiti accedere o gestire l'account. Non rimuovere tutti i gruppi che sono stati configurati con le voci ACE di negazione, ad esempio il gruppo Everyone e SELF calcolate account (questa voce ACE impostata durante la **cambiamento password non** flag è stato abilitato durante la creazione dell'account. Non rimuovere anche il gruppo che appena aggiunto, l'account di sistema o i gruppi, ad esempio contratto Enterprise, DA, BA o il gruppo di accesso autorizzazione Windows.  

    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_134.png)  

29. Fare clic su **avanzate** e verificare che la finestra di dialogo Impostazioni avanzate di sicurezza è simile allo screenshot seguente.  

30. Fare clic su **OK**, e **OK** per chiudere la finestra di dialogo proprietà dell'account.  

    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_135.png)  

31. Il programma di installazione del primo account di gestione è stata completata. Si testerà l'account in una procedura successiva.  

##### <a name="creating-additional-management-accounts"></a>Creazione di account di gestione aggiuntivo

È possibile creare gli account di gestione aggiuntivo, ripetere i passaggi precedenti, copiando l'account che appena creato o creando uno script per creare un account con le impostazioni di configurazione desiderato. Si noti tuttavia che se si copia l'account che appena creato, molte delle impostazioni personalizzate e gli ACL non verranno copiati nel nuovo account e sarà necessario ripetere la maggior parte dei passaggi di configurazione.  
  
In alternativa, è possibile creare un gruppo a cui si delegare i diritti per popolare e unpopulate gruppi protetti, ma sarà necessario proteggere il gruppo e l'account che è inserire in esso. Poiché non vi sarà un numero molto ridotto gli account nella directory che hanno la possibilità di gestire l'appartenenza di gruppi protetti, la creazione di singoli account potrebbe essere l'approccio più semplice.  
  
Indipendentemente dal modo in cui si sceglie di creare un gruppo in cui inserire gli account di gestione, è necessario assicurarsi che ogni account è protetto come descritto in precedenza. È anche consigliabile implementare le restrizioni di oggetto Criteri di gruppo simili a quelli descritti [appendice d: Protezione predefiniti Administrator account in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
  
##### <a name="auditing-management-accounts"></a>Controllo degli account di gestione

È necessario configurare il controllo dell'account di accesso, come minimo, tutte le operazioni di scrittura all'account. In questo modo che non solo identifica corretta abilitazione dell'account e la reimpostazione della password del proprio durante usi autorizzati, ma anche identificare i tentativi da parte degli utenti non autorizzati di modificare l'account. Operazioni di scrittura non riusciti per l'account devono essere acquisiti nel sistema di monitoraggio di eventi (SIEM) e le informazioni di sicurezza (se applicabile) e devono attivare gli avvisi che forniscono una notifica per il personale responsabile per l'analisi dei potenziali danneggiamenti.  
  
Soluzioni SIEM accettano informazioni sugli eventi da origini di sicurezza coinvolti (ad esempio, i registri eventi, i dati dell'applicazione, i flussi di rete, i prodotti antimalware e origini di rilevamento delle intrusioni), regole di confronto dati e tentano di eseguire azioni proattive e le viste intelligenti . Sono disponibili molte soluzioni SIEM commerciale e molte aziende creare implementazioni private. Un sistema SIEM ben progettato e implementato in modo appropriato può migliorare significativamente il monitoraggio della protezione e le funzionalità di risposta agli eventi imprevisti. Tuttavia, funzionalità e sull'accuratezza variare notevolmente tra le soluzioni. Siem esulano dall'ambito di questo articolo, ma devono essere considerati i suggerimenti di evento specifiche contenuti da qualsiasi responsabile dell'implementazione SIEM.  
  
Per altre informazioni sulle impostazioni di configurazione di controllo consigliate per i controller di dominio, vedere [monitoraggio di Active Directory per i segni di compromissione](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md). Le impostazioni di configurazione specifici dei controller di dominio sono disponibili in [monitoraggio di Active Directory per i segni di compromissione](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md).  
  
#### <a name="enabling-management-accounts-to-modify-the-membership-of-protected-groups"></a>Abilitazione della gestione account modificare l'appartenenza di gruppi protetti

In questa procedura si configurerà le autorizzazioni sull'oggetto AdminSDHolder del dominio per consentire gli account di gestione appena creato modificare l'appartenenza di gruppi protetti nel dominio. Questa procedura non può essere eseguita tramite un'interfaccia utente grafica (GUI).  
  
Come illustrato in [appendice c: Account protetti e gruppi in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md), l'ACL in AdminSDHolder del dominio, un oggetto viene effettivamente "copiato" oggetti protetti quando viene eseguita l'attività SDProp. Gli account e gruppi protetti non ereditano le relative autorizzazioni dall'oggetto AdminSDHolder. le autorizzazioni vengono impostate in modo esplicito affinché corrispondano a quelle per l'oggetto AdminSDHolder. Pertanto, quando si modificano le autorizzazioni per l'oggetto AdminSDHolder, è necessario modificarle per gli attributi appropriati per il tipo dell'oggetto protetto di destinazione.  
  
In questo caso, si concedono gli account di gestione appena creato per consentire loro di lettura e scrittura i membri dell'attributo per gli oggetti gruppo. Tuttavia, l'oggetto AdminSDHolder non è un oggetto gruppo e raggruppare gli attributi non sono esposte nell'editor di ACL con interfaccia grafica. È per questo motivo che si implementerà le modifiche delle autorizzazioni tramite l'utilità della riga di comando Dsacls. Per concedere le autorizzazioni degli account per modificare l'appartenenza di gruppi protetti di gestione (disabilitata), procedere come segue:  
  
1. Accedere a un controller di dominio, preferibilmente il controller di dominio che detiene il ruolo emulatore PDC (emulatore PDC), con le credenziali di un account utente che è diventato un membro del gruppo nel dominio.  
  
   ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_136.png)  
  
2. Aprire un prompt dei comandi con privilegi elevati facendo **prompt dei comandi** e fare clic su **Esegui come amministratore**.  
  
   ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_137.gif)  
  
3. Quando viene richiesto di approvare l'elevazione dei privilegi, fare clic su **Sì**.  
  
   ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_138.gif)  
  
   > [!NOTE]  
   > Per altre informazioni sull'elevazione dei privilegi e account il controllo utente (UAC) in Windows, vedere [UAC processi e interazioni](https://technet.microsoft.com/library/dd835561(v=WS.10).aspx) sul sito Web TechNet.  
  
4. Al Prompt dei comandi, digitare (sostituendo le informazioni specifiche del dominio) **Dsacls [nome distinto dell'oggetto AdminSDHolder nel dominio] [UPN dell'account di gestione] /G: RPWP; membro**.  
  
   ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_139.gif)  
  
   Il comando precedente (ovvero non distinzione maiuscole/minuscole) funziona nel modo seguente:  
  
   - DSACLS imposta o Visualizza le voci ACE per oggetti directory  
  
   - CN = AdminSDHolder, CN = System, DC = TailSpinToys, DC = msft identifica l'oggetto da modificare  
  
   - /G indica che è stata configurata una voce ACE di concessione  
  
   - PIM001@tailspintoys.msft è il nome dell'entità di utente (UPN) dell'entità di sicurezza a cui verranno concesse le voci ACE  
  
   - Concede RPWP proprietà autorizzazioni lettura e scrittura proprietà  
  
   - Membro è il nome della proprietà (attributo) in cui verranno impostate le autorizzazioni  
  
   Per altre informazioni sull'uso di **Dsacls**, digitare Dsacls senza parametri a un prompt dei comandi.  
  
   Se è stato creato più account di gestione per il dominio, è necessario eseguire il comando Dsacls per ogni account. Dopo aver completato la configurazione di ACL per l'oggetto AdminSDHolder, è necessario forzare SDProp per eseguire o attendere fino a quando non viene completata l'esecuzione pianificata. Per informazioni sulla forzatura SDProp per l'esecuzione, vedere "Esecuzione SDProp manualmente" nella [appendice c: Account protetti e gruppi in Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
   Quando si è eseguito SDProp, è possibile verificare che siano state applicate le modifiche apportate all'oggetto AdminSDHolder a gruppi protetti nel dominio. Non è possibile verificare questo visualizzando l'ACL per l'oggetto AdminSDHolder per i motivi descritti in precedenza, ma è possibile verificare che le autorizzazioni siano state applicate visualizzando gli ACL nei gruppi protetti.  
  
5. Nelle **Active Directory Users and Computers**, verificare che sia stata attivata **funzionalità avanzate**. A tale scopo, fare clic su **View**, individuare il **Domain Admins** raggruppare, fare clic sul gruppo e fare clic su **proprietà**.  
  
6. Fare clic sui **sicurezza** scheda e fare clic su **avanzate** per aprire il **Advanced Security Settings for Domain Admins** nella finestra di dialogo.  
  
   ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_140.gif)  
  
7. Selezionare **ACE consentire per l'account di gestione** e fare clic su **modificare**. Verificare che l'account è stato concesso solo **membri lettura** e **membri di scrittura** le autorizzazioni per il gruppo DA e fare clic su **OK**.  
  
8. Fare clic su **OK** nel **impostazioni di sicurezza avanzate** la finestra di dialogo e fare clic su **OK** per chiudere la finestra di dialogo proprietà per il gruppo DA.  
  
   ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_141.gif)  
  
9. È possibile ripetere i passaggi precedenti per altri gruppi protetti nel dominio. le autorizzazioni devono essere gli stessi per tutti i gruppi protetti. A questo punto è stata completata la creazione e la configurazione degli account di gestione per i gruppi protetti in questo dominio.  
  
    > [!NOTE]  
    > Tutti gli account che dispone dell'autorizzazione per scrivere l'appartenenza di un gruppo in Active Directory può anche aggiungere al gruppo. Questo comportamento è per impostazione predefinita e non può essere disabilitato. Per questo motivo, è consigliabile mantenere sempre gli account di gestione disabilitati quando non è in uso e gli account devono monitorare attentamente quando rimangono disabilitati e quando sono in uso.  
  
#### <a name="verifying-group-and-account-configuration-settings"></a>Verifica per determinare se le impostazioni di configurazione di Account e gruppo

Ora che si hanno creato e configurato gli account di gestione che consentono di modificare l'appartenenza di gruppi protetti nel dominio (che include i gruppi EA, DA e BA privilegi più elevati), è necessario verificare che siano stati gli account e il proprio gruppo di gestione creato correttamente. La verifica prevede queste attività generali:  
  
1.  Testare il gruppo che è possibile abilitare e disabilitare gli account di gestione per verificare che i membri del gruppo possono abilitano e disabilitare gli account e reimpostano le password, ma non è possibile eseguire altre attività amministrative negli account di gestione.  
  
2.  Gli account di gestione per verificare che possono aggiungere e rimuovere membri di gruppi nel dominio protetti, ma non è possibile modificare altre proprietà degli account protetti e gruppi di test.  
  
##### <a name="test-the-group-that-will-enable-and-disable-management-accounts"></a>Il gruppo che ti permetterà di disabilitazione degli account di gestione di test
  
1.  Per verificare l'abilitazione di un account di gestione e reimpostare la password, accedere a una workstation amministrativa sicura con un account membro del gruppo creato in [appendice i: Creazione di account di gestione per protette di gruppi e account in Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_142.gif)  
  
2.  Aprire **Active Directory Users and Computers**, fare doppio clic su account di gestione e fare clic su **Abilita Account**.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_143.gif)  
  
3.  Dovrebbe essere visualizzata una finestra di dialogo, che conferma che l'account sia stato abilitato.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_144.gif)  
  
4.  Successivamente, reimpostare la password dell'account di gestione. A tale scopo, fare clic nuovamente l'account e fare clic su **Reimposta Password**.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_145.gif)  
  
5.  Digitare una nuova password per l'account nel **nuova password** e **Conferma password** campi e fare clic su **OK**.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_146.gif)  
  
6.  Verrà visualizzata una finestra di dialogo, per confermare che è stata reimpostata la password per l'account.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_147.gif)  
  
7.  Ora provare a modificare le proprietà aggiuntive dell'account di gestione. Fare doppio clic su account e fare clic su **delle proprietà**, fare clic sui **controllo remoto** scheda.  
  
8.  Selezionare **abilitare il controllo remoto** e fare clic su **applica**. L'operazione avrà esito negativo e un **accesso negato** deve visualizzare messaggio di errore.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_148.gif)  
  
9. Scegliere il **Account** scheda per l'account e provare a modificare nome, orario di accesso o le workstation di accesso dell'account. Tutte devono avere esito negativo e opzioni che non sono controllate dall'account di **userAccountControl** attributo deve essere disabilitato e non disponibile per la modifica.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_149.gif)  
  
10. Prova ad aggiungere il gruppo di gestione a un gruppo protetto, ad esempio il gruppo DA. Quando fa clic su **OK**, verrà visualizzato il messaggio, che informa che non si autorizzati a modificare il gruppo.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_150.gif)  
  
11. Eseguire test aggiuntivi, come richiesto per verificare che non è possibile configurare alcuna operazione nell'account di gestione, ad eccezione **userAccountControl** impostazioni e la reimpostazione della password.  
  
    > [!NOTE]  
    > Il **userAccountControl** attributo controlla le opzioni di configurazione di più account. Non è possibile concedere l'autorizzazione per modificare solo alcune delle opzioni di configurazione quando si concede l'autorizzazione di scrittura per l'attributo.  
  
##### <a name="test-the-management-accounts"></a>L'account di gestione di test

Ora che è stato abilitato uno o più account che è possibile modificare l'appartenenza di gruppi protetti, è possibile testare gli account per garantire che è possibile modificare l'appartenenza al gruppo protetto, ma non è possibile eseguire altre modifiche su account protetti e gruppi.  
  
1.  Accedere a un host di amministrazione sicuro come il primo account di gestione.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_151.gif)  
  
2.  Avvio veloce **Active Directory Users and Computers** e individuare le **gruppo Domain Admins**.  
  
3.  Fare doppio clic il **Domain Admins** di gruppo e fare clic su **proprietà**.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_152.gif)  
  
4.  Nel **delle proprietà di Domain Admins**, fare clic sul **membri** scheda e **fare clic su** Add. Immettere il nome di un account dotato di privilegi Domain Admins temporanei e fare clic su **Controlla nomi**. Quando il nome dell'account è sottolineato, fare clic su **OK** da restituire per il **membri** scheda.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_153.gif)  
  
5.  Nel **membri** per la scheda le **Domain Admins Properties** della finestra di dialogo fare clic su **applica**. Dopo aver fatto clic **applica**, l'account deve essere un membro del gruppo e non si riceverà alcun messaggio di errore.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_154.gif)  
  
6.  Fare clic sui **gestito da** scheda le **Domain Admins Properties** finestra di dialogo casella e verificare che non è possibile immettere testo in tutti i campi e tutti i pulsanti sono disattivati.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_155.gif)  
  
7.  Fare clic sul **generali** scheda le **Domain Admins Properties** finestra di dialogo casella e verificare che non è possibile modificare le informazioni su tale scheda.  
  
    ![creazione di account di gestione](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_156.gif)  
  
8.  Ripetere questi passaggi per altri gruppi protetti in base alle esigenze. Al termine, accedere a un host amministrativi protetto con un account membro del gruppo creato per abilitare e disabilitare gli account di gestione. Reimpostare la password dell'account di gestione appena testata e disattiva l'account. È stata completata l'installazione del server gli account di gestione e il gruppo che sarà responsabile per abilitare e disabilitare gli account.  
