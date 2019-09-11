---
title: Distribuzione dei profili utente mobili
TOCTitle: Deploying Roaming User Profiles
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.date: 06/07/2019
ms.author: jgerend
ms.openlocfilehash: 3442ad46590add695fb3fed607c6f728e2bc5ee1
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867286"
---
# <a name="deploying-roaming-user-profiles"></a>Distribuzione dei profili utente mobili

>Si applica a Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2019, Windows Server 2016, Windows Server (canale semestrale), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Questo argomento descrive come usare Windows Server per distribuire i [profili utente mobili](folder-redirection-rup-overview.md) ai computer client Windows. I profili utente mobili reindirizzano i profili utente a una condivisione file in modo che gli utenti ricevano le stesse impostazioni del sistema operativo e dell'applicazione in più computer.

Per un elenco delle modifiche apportate di recente a questo argomento, vedere la sezione [cronologia delle modifiche](#change-history) di questo argomento.

> [!IMPORTANT]
> A causa delle modifiche di sicurezza apportate in [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016), è stato aggiornato [il passaggio 4: Facoltativamente, creare un oggetto Criteri di gruppo per](#step-4-optionally-create-a-gpo-for-roaming-user-profiles) i profili utente mobili in questo argomento in modo che Windows possa applicare correttamente i criteri dei profili utente mobili (e non ripristinare i criteri locali nei PC interessati).

> [!IMPORTANT]
>  Le personalizzazioni utente da avviare vengono perse dopo un aggiornamento sul posto del sistema operativo nella configurazione seguente:
> - Utenti configurati per un profilo mobile
> - Gli utenti possono apportare modifiche all'avvio
>
> Di conseguenza, il menu Start viene reimpostato sul valore predefinito della nuova versione del sistema operativo dopo l'aggiornamento sul posto del sistema operativo. Per le soluzioni alternative, [vedere Appendice C: Aggirare i layout dei menu di avvio dopo](#appendix-c-working-around-reset-start-menu-layouts-after-upgrades)gli aggiornamenti.

## <a name="prerequisites"></a>Prerequisiti

### <a name="hardware-requirements"></a>Requisiti hardware

I profili utente mobili richiedono un computer basato su x64 o su x86. non è supportato in Windows RT.

### <a name="software-requirements"></a>Requisiti software

I profili utente mobili devono soddisfare i requisiti software seguenti:

- Se si distribuiscono profili utente mobili con Reindirizzamento cartelle in un ambiente con profili utente locali, distribuire Reindirizzamento cartelle prima dei profili utente mobili in modo da ridurre al minimo le dimensioni dei profili di roaming. Dopo aver reindirizzato correttamente le cartelle utente, è possibile distribuire i profili utente mobili.
- Per amministrare i profili utente mobili, è necessario aver eseguito l'accesso come membro del gruppo di sicurezza Domain Admins, Enterprise Administrators o del Criteri di gruppo Creator Owners.
- I computer client devono eseguire Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008.
- I computer client devono essere aggiunti ai Servizi di dominio Active Directory in corso di gestione.
- Deve essere disponibile un computer in cui siano installati l'editor Gestione Criteri di gruppo e il Centro amministrativo di Active Directory.
- Per l'hosting dei profili utente mobili deve essere disponibile un file server.

    - Se la condivisione file usa gli spazi dei nomi DFS, le cartelle DFS (collegamenti) devono avere un'unica destinazione per impedire agli utenti di apportare modifiche conflittuali su server differenti.
    - Se la condivisione file usa la Replica DFS per replicare i contenuti con un altro server, gli utenti devono poter accedere all'unico server di origine per impedire agli utenti di apportare modifiche conflittuali su server differenti.
    - Se la condivisione file è di tipo cluster, disabilitare la disponibilità continua sulla condivisione file per evitare problemi di prestazioni.
- Per utilizzare il supporto del computer primario nei profili utente mobili, sono disponibili ulteriori requisiti per i computer client e lo schema Active Directory. Per ulteriori informazioni, vedere la pagina relativa alla [distribuzione di computer primari per il reindirizzamento cartelle e i profili utente mobili](deploy-primary-computers.md).
- Il layout del menu Start di un utente non viene eseguito in Windows 10, Windows Server 2019 o Windows Server 2016 se utilizzano più di un computer, host sessione Desktop remoto o un server VDI (Virtual Desktop Infrastructure). Come soluzione alternativa, è possibile specificare un layout iniziale come descritto in questo argomento. In alternativa, è possibile usare i dischi dei profili utente, che consentono di eseguire correttamente il roaming delle impostazioni del menu Start quando vengono usate con host sessione Desktop remoto server o server VDI. Per altre informazioni, vedere [Gestione dati utente più semplice con i dischi dei profili utente in Windows Server 2012](https://blogs.technet.microsoft.com/enterprisemobility/2012/11/13/easier-user-data-management-with-user-profile-disks-in-windows-server-2012/).

### <a name="considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows"></a>Considerazioni sull'uso di profili utente mobili su più versioni di Windows

Se si decide di usare profili utente mobili in più versioni di Windows è consigliabile eseguire le azioni seguenti:

- Configurare Windows in modo da mantenere versioni di profili separate per ciascuna versione del sistema operativo. Ciò aiuta a prevenire i problemi indesiderati e imprevedibili, come il danneggiamento del profilo.
- Usare il Reindirizzamento cartelle per archiviare i file utente, quali documenti e immagini all'esterno dei profili utente. Ciò rende disponibili gli stessi file agli utenti di versioni differenti del sistema operativo e consente anche di contenere le dimensioni dei profili e di accelerare gli accessi.
- Prevedere risorse di archiviazione sufficienti per i profili utente mobili Se si supportano due versioni del sistema operativo, il numero dei profili raddoppia (e di conseguenza anche lo spazio totale consumato) perché viene mantenuto un profilo separato per ogni versione del sistema operativo.
- Non usare i profili utente mobili su computer che eseguono Windows Vista/Windows Server 2008 e Windows 7/Windows Server 2008 R2. Il roaming tra queste versioni del sistema operativo non è supportato a causa di incompatibilità nelle rispettive versioni del profilo.
- Informare gli utenti che le modifiche apportate a una versione del sistema operativo non verranno spostate in un'altra versione del sistema operativo.
- Quando si trasferisce l'ambiente a una versione di Windows che usa una versione diversa del profilo, ad esempio da Windows 10 a Windows 10, versione [1607, vedere l'Appendice B: Informazioni di riferimento sulla](#appendix-b-profile-version-reference-information) versione del profilo per un elenco), gli utenti ricevono un nuovo profilo utente mobile vuoto. È possibile ridurre al minimo l'effetto di ottenere un nuovo profilo usando il reindirizzamento cartelle per reindirizzare le cartelle comuni. Non esiste un metodo supportato per la migrazione dei profili utente mobili da una versione del profilo a un'altra.

## <a name="step-1-enable-the-use-of-separate-profile-versions"></a>Passaggio 1: Abilitare l'uso delle versioni di profili separati

Se si distribuiscono profili utente mobili su computer che eseguono Windows 8.1, Windows 8, Windows Server 2012 R2 o Windows Server 2012, è consigliabile apportare alcune modifiche all'ambiente Windows prima della distribuzione. Tali modifiche aiutano a garantire che i futuri aggiornamenti del sistema operativo avvengano in maniera fluida e semplificano la possibilità di eseguire simultaneamente più versioni di Windows con i profili utente mobili.

Per apportare queste modifiche, usare la procedura seguente.

1. Scaricare e installare l'aggiornamento software appropriato in tutti i computer in cui verranno usati i profili di roaming, obbligatori, Super-obbligatori o predefiniti di dominio:

    - Windows 8.1 o Windows Server 2012 R2: installare l'aggiornamento software descritto nell'articolo [2887595](http://support.microsoft.com/kb/2887595) della Microsoft Knowledge base (quando rilasciato).
    - Windows 8 o Windows Server 2012: installare l'aggiornamento del software descritto nell'articolo [2887239](http://support.microsoft.com/kb/2887239) della Microsoft Knowledge Base.

2. In tutti i computer che eseguono Windows 8.1, Windows 8, Windows Server 2012 R2 o Windows Server 2012 in cui si useranno i profili utente mobili, usare l'editor del registro di sistema o Criteri di gruppo per creare il valore DWORD della chiave `1`del registro di sistema seguente e impostarlo su. Per informazioni sulla creazione delle chiavi del Registro di sistema con i Criteri di gruppo, vedere [Configurare un elemento Registro di sistema](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753092(v=ws.11)>).

    ```
    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\ProfSvc\Parameters\UseProfilePathExtensionVersion
    ```

    > [!WARNING]
    > La modifica non corretta del Registro di sistema potrebbe danneggiare gravemente il sistema. Prima di apportare modifiche al Registro di sistema, si consiglia di effettuare il backup di tutti i dati importanti presenti sul computer.
3. Riavviare i computer.

## <a name="step-2-create-a-roaming-user-profiles-security-group"></a>Passaggio 2: Creare un gruppo di sicurezza dei profili utente mobili

Se l'ambiente non è ancora stato configurato con i profili utente mobili, il primo passaggio consiste nel creare un gruppo di sicurezza che contenga tutti gli utenti e/o computer ai quali applicare le impostazioni criterio dei profili utente mobili.

- Gli amministratori delle distribuzioni dei profili utente mobili generici creano di norma un gruppo di sicurezza per gli utenti.
- Gli amministratori di Servizi Desktop remoto o delle distribuzioni del desktop virtuale usano in genere un gruppo di sicurezza per gli utenti e i computer condivisi.

Di seguito viene illustrato come creare un gruppo di sicurezza per i profili utente mobili:

1. Aprire Server Manager in un computer in cui è installato Active Directory centro di amministrazione.
2. Scegliere **Active Directory centro di amministrazione**dal menu **strumenti** . Verrà visualizzato Centro amministrativo di Active Directory.
3. Fare clic con il pulsante destro del mouse sul dominio o sull'unità organizzativa appropriata, scegliere **nuovo**e quindi selezionare **gruppo**.
4. Nella sezione **Gruppo** della finestra **Crea gruppo** specificare le impostazioni seguenti:

    - Digitare il nome del gruppo di sicurezza in **Nome gruppo**, ad esempio: **Utenti e computer profili utente mobili**.
    - In **ambito del gruppo**selezionare **sicurezza**, quindi selezionare **globale**.

5. Nella sezione **membri** selezionare **Aggiungi**. Verrà visualizzata la finestra di dialogo Selezione utenti, computer, account servizio o gruppi.
6. Se si desidera includere account computer nel gruppo di sicurezza, selezionare **tipi di oggetto**, selezionare la casella di controllo **computer** e quindi fare clic su **OK**.
7. Digitare i nomi degli utenti, dei gruppi e/o dei computer a cui si desidera distribuire i profili utente mobili, selezionare **OK**, quindi fare di nuovo clic su **OK** .

## <a name="step-3-create-a-file-share-for-roaming-user-profiles"></a>Passaggio 3: Creare una condivisione file per i profili utente mobili

Se non si dispone già di una condivisione file separata per i profili utente mobili (indipendente da qualsiasi condivisione per le cartelle reindirizzate per impedire la memorizzazione accidentale nella cache della cartella del profilo mobile), utilizzare la procedura seguente per creare una condivisione file in un server che esegue Windows Server.

> [!NOTE]
> Alcune funzionalità potrebbero differire o non essere disponibili a seconda della versione di Windows Server in uso.

Ecco come creare una condivisione file in Windows Server:

1. Nel riquadro di spostamento Server Manager selezionare **Servizi file e archiviazione**e quindi selezionare **condivisioni** per visualizzare la pagina condivisioni.
2. Nel riquadro condivisioni selezionare **attività**, quindi selezionare **nuova condivisione**. Verrà visualizzata la Creazione guidata nuova condivisione.
3. Nella pagina **Seleziona profilo** selezionare **condivisione SMB-rapida**. Se Gestione risorse file server è installato e si usano le proprietà di gestione delle cartelle, selezionare invece **condivisione SMB-avanzate**.
4. Nella pagina **Percorso condivisione** selezionare il server e il volume sui quali creare la condivisione.
5. Nella pagina **Nome condivisione** digitare un nome per la condivisione (ad esempio **Profili utente$** ) nella casella **Nome condivisione**.

    > [!TIP]
    > Quando si crea la condivisione, nasconderla inserendo ```$``` dopo il nome condivisione. In tal modo, la condivisione verrà nascosta dai browser casuali.

6. Nella pagina **Altre impostazioni** deselezionare la casella di controllo **Abilita disponibilità continua**, se disponibile e selezionare facoltativamente le caselle di controllo **Abilita enumerazione basata sull'accesso** e **Crittografa accesso ai dati**.
7. Nella pagina **autorizzazioni** selezionare **Personalizza autorizzazioni...** . Viene visualizzata la finestra di dialogo Impostazioni di protezione avanzate.
8. Selezionare **Disabilita ereditarietà**, quindi selezionare **Converti autorizzazioni ereditate in autorizzazione esplicita per questo oggetto**.
9. Impostare le autorizzazioni come descritto in [autorizzazioni necessarie per la condivisione file che ospita i profili utente mobili](#required-permissions-for-the-file-share-hosting-roaming-user-profiles) e mostrata nello screenshot seguente, rimuovendo le autorizzazioni per i gruppi e gli account non elencati e aggiungendo autorizzazioni speciali all'utente comune Profili utenti e computer del gruppo creato nel passaggio 1.
    
    ![Finestra impostazioni di sicurezza avanzate che mostra le autorizzazioni come descritto nella tabella 1](media/advanced-security-user-profiles.jpg)
    
    **Figura 1** Impostare le autorizzazioni per la condivisione dei profili utente mobili
10. Se si sceglie il profilo **Condivisione SMB - avanzata** selezionare il valore Utilizzo cartelle **File utente** nella pagina **Proprietà gestione** .
11. Se si sceglie il profilo **Condivisione SMB - avanzata** nella pagina **Quota** , selezionare facoltativamente una quota da applicare agli utenti della condivisione.
12. Nella pagina **conferma** selezionare **Crea.**

### <a name="required-permissions-for-the-file-share-hosting-roaming-user-profiles"></a>Autorizzazioni necessarie per la condivisione file che ospita i profili utente mobili

| Account utente | Accesso | Si applica a |
|   -   |   -   |   -   |
|   Sistema    |  Controllo completo     |  Questa cartella, le sottocartelle e i file     |
|  Administrators     |  Controllo completo     |  Solo questa cartella     |
|  Autore/proprietario     |  Controllo completo     |  Solo sottocartelle e file     |
| Gruppo di sicurezza degli utenti che devono inserire dati nella condivisione (Utenti e computer profili utente mobili)      |  Elenca cartella/lettura dati *(autorizzazioni avanzate)* <br />Creazione di cartelle/Accodamento dei dati *(autorizzazioni avanzate)* |  Solo questa cartella     |
| Altri gruppi e account   |  Nessuno (rimuovere)     |       |

## <a name="step-4-optionally-create-a-gpo-for-roaming-user-profiles"></a>Passaggio 4: Creare facoltativamente un oggetto Criteri di gruppo per i profili utente mobili

Se non è già stato creato un oggetto Criteri di gruppo per le impostazioni dei profili utente mobili, usare la procedura seguente per creare un oggetto Criteri di gruppo vuoto da usare con i profili utente mobili. Questo oggetto Criteri di gruppo consente di configurare le impostazioni dei profili utente mobili (ad esempio il supporto del computer primario, che verrà discusso in separata sede) e può anche essere usato per abilitare i profili utente mobili sui computer, come di norma è possibile nella distribuzione degli ambienti di desktop virtuale o con Servizi Desktop remoto.

Di seguito viene illustrato come creare un oggetto Criteri di gruppo per i profili utente mobili:

1. Aprire Server Manager in un computer in cui è installata la console Gestione Criteri di gruppo.
2. Dal menu **strumenti** selezionare **Gestione criteri di gruppo**. Viene visualizzato Gestione Criteri di gruppo.
3. Fare clic con il pulsante destro del mouse sul dominio o sull'unità organizzativa in cui si desidera configurare i profili utente mobili, quindi selezionare **Crea un oggetto Criteri di gruppo in questo dominio e collegarlo qui**.
4. Nella finestra di dialogo **nuovo oggetto Criteri** di gruppo digitare un nome per l'oggetto Criteri di gruppo (ad esempio, **Impostazioni profilo utente mobile**) e quindi fare clic su **OK**.
5. Fare clic con il pulsante destro del mouse sull'oggetto Criteri di gruppo appena creato e quindi deselezionare la casella di controllo **Collegamento abilitato** . Ciò impedisce di applicare l'oggetto Criteri di gruppo finché non sarà terminata la configurazione.
6. Selezionare l'oggetto Criteri di gruppo. Nella sezione **filtri di sicurezza** della scheda **ambito** Selezionare **utenti autenticati**, quindi selezionare **Rimuovi** per impedire che l'oggetto Criteri di gruppo venga applicato a tutti.
7. Nella sezione **filtri di sicurezza** selezionare **Aggiungi**.
8. Nella finestra di dialogo **Seleziona utente, computer o gruppo** Digitare il nome del gruppo di sicurezza creato nel passaggio 1 (ad esempio, **utenti e computer profili utente mobili**) e quindi fare clic su **OK**.
9. Selezionare la scheda **delega** , selezionare **Aggiungi**, digitare **Authenticated Users**, selezionare **OK**, quindi selezionare di nuovo **OK** per accettare le autorizzazioni di lettura predefinite.
    
    Questo passaggio è necessario a causa delle modifiche di sicurezza apportate in [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016).

>[!IMPORTANT]
>A causa delle modifiche di sicurezza apportate in [MS16-072A](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016), è ora necessario concedere al gruppo Authenticated Users le autorizzazioni di lettura delegate per l'oggetto Criteri di gruppo. in caso contrario, l'oggetto Criteri di gruppo non verrà applicato agli utenti o, se è già stato applicato, l'oggetto Criteri di gruppo viene rimosso, reindirizza i profili utente sul computer locale. Per altre informazioni, vedere [distribuzione criteri di gruppo aggiornamento della sicurezza MS16-072](https://blogs.technet.microsoft.com/askds/2016/06/22/deploying-group-policy-security-update-ms16-072-kb3163622/).

## <a name="step-5-optionally-set-up-roaming-user-profiles-on-user-accounts"></a>Passaggio 5: Configurare facoltativamente i profili utente mobili sugli account utente

Se si distribuiscono profili utente mobili agli account utente, usare la procedura seguente per specificare i profili utente mobili per gli account utente in Servizi di dominio Active Directory. Se si distribuiscono profili utente mobili ai computer, come avviene in genere per le distribuzioni di Servizi Desktop remoto o desktop virtualizzate, usare invece la procedura documentata nel [passaggio 6: Configurare facoltativamente i profili utente mobili nei computer](#step-6-optionally-set-up-roaming-user-profiles-on-computers).

> [!NOTE]
> Se si configurano profili utente mobili sugli account utente usando Active Directory e sui computer usando Criteri di gruppo, l'impostazione criterio basata sul computer ha la precedenza.

Di seguito viene illustrato come configurare i profili utente mobili per gli account utente:

1. Nel Centro amministrativo di Active Directory passare al contenitore (o unità organizzativa) **Utenti** nel dominio appropriato.
2. Selezionare tutti gli utenti a cui si desidera assegnare un profilo utente mobile, fare clic con il pulsante destro del mouse sugli utenti e quindi scegliere **Proprietà**.
3. Nella sezione **profilo** selezionare la casella di controllo **percorso profilo:** e quindi immettere il percorso della condivisione file in cui si desidera archiviare il profilo utente mobile dell'utente, seguito da `%username%` (che viene automaticamente sostituito con il nome utente il primo ora di accesso dell'utente. Esempio:
    
    `\\fs1.corp.contoso.com\User Profiles$\%username%`
    
    Per specificare un profilo utente mobile obbligatorio, specificare il percorso del file NTuser. Man creato in precedenza, ad esempio `fs1.corp.contoso.comUser Profiles$default`. Per altre informazioni, vedere [creare profili utente obbligatori](https://docs.microsoft.com/windows/client-management/mandatory-user-profile).
4. Scegliere **OK**.

> [!NOTE]
> Per impostazione predefinita, la distribuzione di tutte le app basate su Windows® Runtime (Windows Store) è consentita durante l'uso dei profili utente mobili. Quando però si usa un profilo speciale, le app non vengono distribuite per impostazione predefinita. I profili speciali sono profili utente in cui le modifiche vengono rimosse subito dopo la disconnessione dell'utente:
> <br><br>Per rimuovere le limitazioni alla distribuzione delle app per i profili speciali, abilitare l'impostazione criterio **Allow deployment operations in special profiles** (nel percorso Configurazione computer\Criteri\Modelli amministrativi\Componenti di Windows\Distribuzione di pacchetti di app). Le app distribuite in questo scenario lasceranno comunque alcuni dati archiviati sul computer, che potrebbero accumularsi, ad esempio, se un unico computer è usato da centinaia di utenti. Per pulire le app, trovare o sviluppare uno strumento che usi l'API [CleanupPackageForUserAsync](https://msdn.microsoft.com/library/windows/apps/windows.management.deployment.packagemanager.cleanuppackageforuserasync.aspx) per pulire i pacchetti dell'app per gli utenti che non hanno più un profilo nel computer.
> <br><br>Per altre informazioni di carattere generale sulle app di Windows Store vedere [Gestisci l'accesso client a Windows Store](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh832040(v=ws.11)>).

## <a name="step-6-optionally-set-up-roaming-user-profiles-on-computers"></a>Passaggio 6: Configurare facoltativamente i profili utente mobili sui computer

Se si distribuiscono profili utente mobili ai computer, come di norma è possibile per le distribuzioni di Servizi Desktop remoto o desktop virtuale, usare la procedura seguente. Se si distribuiscono profili utente mobili agli account utente, usare invece la procedura descritta nel [passaggio 5: Configurare facoltativamente i profili utente mobili per gli account](#step-5-optionally-set-up-roaming-user-profiles-on-user-accounts)utente.

È possibile utilizzare Criteri di gruppo per applicare i profili utente mobili ai computer che eseguono Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008.

> [!NOTE]
> Se si configurano profili utente mobili sui computer usando Criteri di gruppo e sugli account utente usando Active Directory, l'impostazione criterio basata sul computer ha la precedenza.

Di seguito viene illustrato come configurare i profili utente mobili nei computer:

1. Aprire Server Manager in un computer in cui è installata la console Gestione Criteri di gruppo.
2. Dal menu **strumenti** selezionare **Gestione criteri di gruppo**. Verrà visualizzata la gestione Criteri di gruppo.
3. In Gestione Criteri di gruppo fare clic con il pulsante destro del mouse sull'oggetto Criteri di gruppo creato nel passaggio 3 (ad esempio, **impostazioni dei profili utente mobili**) e quindi scegliere **modifica**.
4. Nella finestra Editor Gestione Criteri di gruppo passare a **Configurazione computer**, **Criteri**, **Modelli amministrativi**, **Sistema** e quindi **Profili utente**.
5. Fare clic con il pulsante destro del mouse su **Imposta percorso profilo mobile per tutti gli utenti che accedono al computer** e quindi scegliere **modifica**.
    > [!TIP]
    > La home directory di un utente, se configurata, è la cartella predefinita usata da alcuni programmi come Windows PowerShell. È possibile configurare un percorso di rete o locale alternativo in base al numero di utenti con la sezione **Home directory** delle proprietà dell'account utente in Servizi di dominio Active Directory. Per configurare il percorso della Home Directory per tutti gli utenti di un computer in cui è in esecuzione Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 in un ambiente di desktop virtuale, abilitare la **Home directory dell'utente** . impostazione dei criteri, quindi specificare la condivisione file e la lettera di unità di cui eseguire il mapping (oppure specificare una cartella locale). Non usare variabili di ambiente o puntini di sospensione. L'alias dell'utente viene aggiunto alla fine del percorso specificato durante l'accesso dell'utente.
6. Nella finestra di dialogo **Proprietà** selezionare **abilitato**
7. Nella casella **utenti che accedono al computer devono utilizzare il percorso del profilo di roaming** , immettere il percorso della condivisione file in cui si desidera archiviare il profilo utente mobile dell'utente, seguito da `%username%` (che viene sostituito automaticamente con il nome utente la prima volta che l'utente accede. Esempio:

    `\\fs1.corp.contoso.com\User Profiles$\%username%`

    Per specificare un profilo utente mobile obbligatorio, che è un profilo preconfigurato a cui gli utenti non possono apportare modifiche permanenti (le modifiche vengono reimpostate quando l'utente si disconnette), specificare il percorso del file NTuser. Man creato in precedenza, ad `\\fs1.corp.contoso.com\User Profiles$\default`esempio. Per altre informazioni, vedere [Creazione di un profilo utente obbligatorio](https://docs.microsoft.com/windows/client-management/mandatory-user-profile).
8. Scegliere **OK**.

## <a name="step-7-optionally-specify-a-start-layout-for-windows-10-pcs"></a>Passaggio 7: Facoltativamente, specificare un layout iniziale per i PC Windows 10

È possibile usare Criteri di gruppo per applicare uno specifico layout del menu Start in modo che gli utenti visualizzino lo stesso layout iniziale in tutti i PC. Se gli utenti effettuano l'accesso a più di un PC e si vuole avere un layout di avvio coerente tra i PC, assicurarsi che l'oggetto Criteri di gruppo venga applicato a tutti i PC.

Per specificare un layout iniziale, eseguire le operazioni seguenti:

1. Aggiornare i PC Windows 10 a Windows 10 versione 1607 (anche noto come aggiornamento dell'anniversario) o versioni successive e installare il 14 marzo 2017 aggiornamento cumulativo ([KB4013429](https://support.microsoft.com/kb/4013429)) o versione successiva.
2. Creare un file XML di layout del menu Start completo o parziale. A tale scopo, vedere [personalizzare ed esportare il layout di avvio](https://docs.microsoft.com/windows/configuration/customize-and-export-start-layout).
    * Se si specifica un layout di avvio *completo* , un utente non potrà personalizzare alcuna parte del menu Start. Se si specifica un layout di avvio *parziale* , gli utenti possono personalizzare tutti gli elementi, tranne i gruppi bloccati dei riquadri specificati. Tuttavia, con un layout di avvio parziale, le personalizzazioni dell'utente nel menu Start non vengono spostate in altri computer.
3. Usare Criteri di gruppo per applicare il layout di avvio personalizzato all'oggetto Criteri di gruppo creato per i profili utente mobili. A tale scopo, vedere [usare criteri di gruppo per applicare un layout iniziale personalizzato in un dominio](https://docs.microsoft.com/windows/configuration/customize-windows-10-start-screens-by-using-group-policy#bkmk-domaingpodeployment).
4. Usare Criteri di gruppo per impostare il valore del registro di sistema seguente nei PC Windows 10. A tale scopo, vedere [configurare un elemento del registro di sistema](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753092(v=ws.11)>).

| **Azione**   | **Aggiornamento**                  |
| ------------ | ------------                |
| Alveare         | **HKEY_LOCAL_MACHINE**      |
| Percorso della chiave     | **Software\Microsoft\Windows\CurrentVersion\Explorer** |
| Nome del valore   | **SpecialRoamingOverrideAllowed** |
| Tipo di valore   | **REG_DWORD**               |
| Dati valore   | **1** (o **0** per disabilitare) |
| Base         | **Decimal**                 |

5. Opzionale Abilita le ottimizzazioni di accesso per la prima volta per rendere più veloce l'accesso per gli utenti. A tale scopo, vedere [applicare i criteri per migliorare l'ora di accesso](https://docs.microsoft.com/windows/client-management/mandatory-user-profile#apply-policies-to-improve-sign-in-time).
6. Opzionale Ridurre ulteriormente i tempi di accesso rimuovendo le app non necessarie dall'immagine di base di Windows 10 utilizzata per distribuire i PC client. Windows Server 2019 e Windows Server 2016 non dispongono di app pre-provisioning, quindi è possibile ignorare questo passaggio sulle immagini server.
    - Per rimuovere le app, usare il cmdlet [Remove-AppxProvisionedPackage](https://docs.microsoft.com/powershell/module/dism/remove-appxprovisionedpackage?view=win10-ps) di Windows PowerShell per disinstallare le applicazioni seguenti. Se i computer sono già stati distribuiti, è possibile creare uno script per la rimozione di queste app usando il metodo [Remove-AppxPackage](https://docs.microsoft.com/powershell/module/appx/remove-appxpackage?view=win10-ps).
    
      - Microsoft. windowscommunicationsapps\_8wekyb3d8bbwe
      - Microsoft. BingWeather\_8wekyb3d8bbwe
      - Microsoft. DesktopAppInstaller\_8wekyb3d8bbwe
      - Microsoft. getstarted\_8wekyb3d8bbwe
      - Microsoft. Windows. Photos\_8wekyb3d8bbwe
      - Microsoft. WindowsCamera\_8wekyb3d8bbwe
      - Microsoft. WindowsFeedbackHub\_8wekyb3d8bbwe
      - Microsoft. WindowsStore\_8wekyb3d8bbwe
      - Microsoft. XboxApp\_8wekyb3d8bbwe
      - Microsoft. XboxIdentityProvider\_8wekyb3d8bbwe
      - Microsoft. ZuneMusic\_8wekyb3d8bbwe

>[!NOTE]
>La disinstallazione di queste app riduce i tempi di accesso, ma è possibile lasciarli installati se la distribuzione ne richiede uno.

## <a name="step-8-enable-the-roaming-user-profiles-gpo"></a>Passaggio 8: Abilitare l'oggetto Criteri di gruppo dei profili utente mobili

Se si configurano i profili utente mobili sui computer usando i Criteri di gruppo oppure se sono state personalizzate altre impostazioni dei profili utente mobili usando i Criteri di gruppo, il passaggio successivo consiste nell'abilitazione dell'oggetto Criteri di gruppo, consentendone l'applicazione agli utenti interessati.

>[!TIP]
>Se si prevede di implementare il supporto dei computer primari, procedere ora prima di abilitare l'oggetto Criteri di gruppo. In tal modo, i dati utente non verranno copiati nei computer non primari prima di aver abilitato il supporto del computer primario. Per le impostazioni di criteri specifiche, vedere [distribuire computer primari per il reindirizzamento cartelle e i profili utente mobili](deploy-primary-computers.md).

Di seguito viene illustrato come abilitare l'oggetto Criteri di gruppo del profilo utente mobile:

1. Aprire Gestione Criteri di gruppo.
2. Fare clic con il pulsante destro del mouse sull'oggetto Criteri di gruppo creato, quindi scegliere **collegamento abilitato**. Accanto alla voce del menu viene visualizzata una casella di controllo.

## <a name="step-9-test-roaming-user-profiles"></a>Passaggio 9: Testare i profili utente mobili

Per testare i profili utente mobili, accedere a un computer con un account utente configurato per i profili utente mobili oppure accedere a un computer configurato per i profili utente mobili. Confermare quindi che il profilo è reindirizzato.

Ecco come testare i profili utente mobili:

1. Accedere a un computer primario (se è stato abilitato il supporto per il computer primario) con un account utente per il quale sono stati abilitati i profili utente mobili. Se sono stati abilitati i profili utente mobili su computer specifici, accedere a uno di questi.
2. Se l'utente ha precedentemente eseguito l'accesso al computer, aprire un prompt dei comandi con privilegi elevati e quindi digitare il comando seguente per assicurarsi che vengano applicate le impostazioni Criteri di gruppo più recenti al computer client:
    
    ```PowerShell
    GpUpdate /Force
    ```
3. Per verificare che il profilo utente sia in roaming, aprire il **Pannello di controllo**, selezionare **sistema e sicurezza**, selezionare **sistema**, **impostazioni di sistema avanzate**, selezionare **Impostazioni** nella sezione profili utente e cercare  **Roaming** nella colonna **tipo** .

## <a name="appendix-a-checklist-for-deploying-roaming-user-profiles"></a>Appendice A: Elenco di controllo per la distribuzione dei profili utente mobili

| Stato                     | Azione                                                |
| ---                        | ------                                                |
| ☐<br>☐<br>☐<br>☐<br>☐   | 1. Preparare il dominio<br>-Aggiungere computer al dominio<br>-Abilita l'uso di versioni del profilo separate<br>-Creare gli account utente<br>-(Facoltativo) Distribuisci Reindirizzamento cartelle |
| ☐<br><br><br>             | 2. Creare un gruppo di sicurezza per i profili utente mobili<br>-Nome gruppo:<br>Membri |
| ☐<br><br>                 | 3. Creare una condivisione file per i profili utente mobili<br>-Nome condivisione file: |
| ☐<br><br>                 | 4. Creare un oggetto Criteri di gruppo per i profili utente mobili<br>-Nome oggetto Criteri di gruppo:|
| ☐                         | 5. Configurare le impostazioni criterio dei profili utente mobili    |
| ☐<br>☐<br>☐              | 6. Abilita profili utente mobili<br>-Abilitato in servizi di dominio Active Directory per gli account utente?<br>-Abilitato in Criteri di gruppo sugli account computer?<br> |
| ☐                         | 7. Opzionale Specificare un layout di avvio obbligatorio per i PC Windows 10 |
| ☐<br>☐<br><br>☐<br><br>☐ | 8. Opzionale Abilita supporto per computer primari<br>-Designare i computer primari per gli utenti<br>-Percorso dei mapping di utente e computer primario:<br>-(Facoltativo) abilitare il supporto del computer primario per il reindirizzamento cartelle<br>-Basato su computer o su utente?<br>-(Facoltativo) abilitare il supporto del computer primario per i profili utente mobili |
| ☐                        | 9. Abilitare l'oggetto Criteri di gruppo dei profili utente mobili                |
| ☐                        | 10. Testare i profili utente mobili                         |

## <a name="appendix-b-profile-version-reference-information"></a>Appendice B: Informazioni di riferimento relative alla versione del profilo

Ogni profilo ha una versione del profilo che corrisponde approssimativamente alla versione di Windows in cui viene usato il profilo. Ad esempio, Windows 10, versione 1703 e la versione 1607 usano entrambi il. Versione del profilo V6. Microsoft crea una nuova versione del profilo solo quando è necessario per mantenere la compatibilità, motivo per cui non tutte le versioni di Windows includono una nuova versione del profilo.

Nella tabella seguente è indicato il percorso dei profili utente mobili su varie versioni di Windows.

| Versione del sistema operativo | Percorso del profilo utente mobile |
| --- | --- |
| Windows XP e Windows Server 2003 | ```\\<servername>\<fileshare>\<username>``` |
| Windows Vista e Windows Server 2008 | ```\\<servername>\<fileshare>\<username>.V2``` |
| Windows 7 e Windows Server 2008 R2 | ```\\<servername>\<fileshare>\<username>.V2``` |
| Windows 8 e Windows Server 2012 | ```\\<servername>\<fileshare>\<username>.V3```(dopo l'applicazione dell'aggiornamento software e della chiave del registro di sistema)<br>```\\<servername>\<fileshare>\<username>.V2```(prima dell'applicazione dell'aggiornamento software e della chiave del registro di sistema) |
| Windows 8.1 e Windows Server 2012 R2 | ```\\<servername>\<fileshare>\<username>.V4```(dopo l'applicazione dell'aggiornamento software e della chiave del registro di sistema)<br>```\\<servername>\<fileshare>\<username>.V2```(prima dell'applicazione dell'aggiornamento software e della chiave del registro di sistema) |
| Windows 10 | ```\\<servername>\<fileshare>\<username>.V5``` |
| Windows 10, versione 1703 e versione 1607 | ```\\<servername>\<fileshare>\<username>.V6``` |

## <a name="appendix-c-working-around-reset-start-menu-layouts-after-upgrades"></a>Appendice C: Aggirare i layout dei menu di avvio dopo gli aggiornamenti

Ecco alcuni modi per aggirare i layout dei menu Start per la reimpostazione dopo un aggiornamento sul posto:

- Se un solo utente usa il dispositivo e l'amministratore IT usa una strategia di distribuzione del sistema operativo gestita, ad esempio SCCM, è possibile eseguire le operazioni seguenti:
    
  1. Esportare il layout del menu Start con export-Startlayout prima dell'aggiornamento 
  2. Importare il layout del menu Start con Import-StartLayout dopo la configurazione guidata, ma prima dell'accesso dell'utente  
 
     > [!NOTE] 
     > L'importazione di un StartLayout modifica il profilo utente predefinito. Tutti i profili utente creati dopo l'importazione otterranno il layout iniziale importato.
 
- Gli amministratori IT possono scegliere di gestire il layout di avvio con Criteri di gruppo. L'uso di Criteri di gruppo fornisce una soluzione di gestione centralizzata per applicare agli utenti un layout di avvio standardizzato. Sono disponibili due modalità per l'uso di Criteri di gruppo per la gestione degli avvii. Blocco completo e blocco parziale. Lo scenario di blocco completo impedisce all'utente di apportare modifiche al layout di avvio. Lo scenario di blocco parziale consente all'utente di apportare modifiche a un'area di avvio specifica. Per altre informazioni, vedere [personalizzare ed esportare il layout di avvio](https://docs.microsoft.com/windows/configuration/customize-and-export-start-layout).
        
   > [!NOTE]
   > Le modifiche apportate dall'utente nello scenario di blocco parziale andranno comunque perse durante l'aggiornamento.

- Consentire la reimpostazione del layout di avvio e consentire agli utenti finali di riconfigurare l'avvio. È possibile inviare un messaggio di posta elettronica di notifica o un'altra notifica agli utenti finali per prevedere che i layout di avvio vengano reimpostati dopo l'aggiornamento del sistema operativo per ridurre al minimo l'effetto. 

# <a name="change-history"></a>Cronologia delle modifiche

La tabella seguente include un riepilogo di alcune delle più importanti modifiche apportate a questo argomento.

| Date | Descrizione |`Reason`|
| --- | ---         | ---   |
| 1 maggio, 2019 | Aggiunta di aggiornamenti per Windows Server 2019 |
| 10 aprile 2018 | È stata aggiunta una discussione quando le personalizzazioni dell'utente per l'avvio vengono perse dopo un aggiornamento sul posto del sistema operativo|Problema noto di callout. |
| 13 marzo, 2018 | Aggiornamento per Windows Server 2016 | Spostato dalla libreria versioni precedenti e aggiornato per la versione corrente di Windows Server. |
| 13 aprile, 2017 | Aggiunta di informazioni sul profilo per Windows 10, versione 1703 e spiegazione della modalità di funzionamento delle versioni del profilo roaming durante l'aggiornamento dei sistemi operativi. vedere [considerazioni sull'uso di profili utente mobili in più versioni di Windows](#considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows). | Suggerimenti dei clienti. |
| 14 marzo, 2017 | Aggiunta di un passaggio facoltativo per specificare un layout di avvio obbligatorio per i [PC Windows 10 nell'appendice a: Elenco di controllo per la distribuzione dei](#appendix-a-checklist-for-deploying-roaming-user-profiles)profili utente mobili. |Modifiche alle funzionalità nella versione più recente di Windows Update. |
| 23 gennaio 2017 | Aggiunta di un passaggio [al passaggio 4: Facoltativamente, creare un oggetto Criteri di gruppo per](#step-4-optionally-create-a-gpo-for-roaming-user-profiles) i profili utente mobili per delegare le autorizzazioni di lettura agli utenti autenticati, che è ora necessario a causa di un aggiornamento della sicurezza Criteri di gruppo.|Modifiche di sicurezza per l'elaborazione Criteri di gruppo. |
| 29 dicembre 2016 | Aggiunta di un collegamento [al passaggio 8: Abilitare l'oggetto criteri](#step-8-enable-the-roaming-user-profiles-gpo) di gruppo dei profili utente mobili per semplificare l'ottenimento di informazioni su come impostare criteri di gruppo per i computer primari. Sono stati corretti anche due riferimenti ai passaggi 5 e 6 con i numeri errati.|Suggerimenti dei clienti. |
| 5 dicembre 2016 | Sono state aggiunte informazioni che spiegano il problema del roaming delle impostazioni del menu Start. | Suggerimenti dei clienti. |
| 6 luglio 2016 | Aggiunta dei suffissi della versione del profilo [Windows 10 nell'Appendice B: Informazioni di riferimento sulla](#appendix-b-profile-version-reference-information)versione del profilo. Sono stati rimossi anche Windows XP e Windows Server 2003 dall'elenco dei sistemi operativi supportati. | Aggiornamenti per le nuove versioni di Windows e rimosse informazioni sulle versioni di Windows che non sono più supportate. |
| 7 luglio 2015 | Aggiunta di requisiti e passaggi per disabilitare la disponibilità continua quando si usa un file server in cluster. | Le condivisioni di file in cluster offrono migliori prestazioni per scritture di piccola entità (tipiche con i profili utente mobili) quando la disponibilità continua è disabilitata. |
| 19 marzo 2014 | Suffissi della versione del profilo in maiuscolo (. V2,. V3,. V4) nell' [Appendice B: Informazioni di riferimento sulla](#appendix-b-profile-version-reference-information)versione del profilo. | Anche se Windows non fa distinzione tra maiuscole e minuscole, se si usa NFS con la condivisione file, è importante avere la maiuscola corretta (maiuscole) per il suffisso del profilo. |
| 9 ottobre 2013 | Revisione per Windows Server 2012 R2 e Windows 8.1, spiegazione di alcuni elementi e aggiunta delle [considerazioni relative all'utilizzo di profili utente mobili su più versioni di Windows](#considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows) e [Appendice B: Sezioni informazioni di riferimento](#appendix-b-profile-version-reference-information) sulla versione del profilo. | Aggiornamenti per la nuova versione; suggerimenti dei clienti. |

## <a name="more-information"></a>Altre informazioni

- [Distribuire il reindirizzamento cartelle, File offline e i profili utente mobili](deploy-folder-redirection.md)
- [Distribuire i computer primari per il reindirizzamento cartelle e i profili utente mobili](deploy-primary-computers.md)
- [Implementazione della gestione dello stato utente](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc784645(v=ws.10)>)
- [Informativa sul supporto tecnico Microsoft sui dati del profilo utente replicati](https://blogs.technet.microsoft.com/askds/2010/09/01/microsofts-support-statement-around-replicated-user-profile-data/)
- [App sideload con gestione e manutenzione immagini distribuzione](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh852635(v=win.10)>)
- [Risoluzione dei problemi relativi a pacchetti, distribuzione e query delle app basate su Windows Runtime](https://msdn.microsoft.com/library/windows/desktop/hh973484.aspx)