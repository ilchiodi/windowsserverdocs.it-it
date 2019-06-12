---
Title: Distribuzione di profili utente mobili
TOCTitle: Deploying Roaming User Profiles
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.date: 06/07/2019
ms.author: jgerend
ms.openlocfilehash: e6e2e32ff9aeb1b3bcfc8fed9027c7e92e13b118
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812492"
---
# <a name="deploying-roaming-user-profiles"></a>Distribuzione di profili utente mobili

>Si applica a: Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2019, Windows Server 2016, Windows Server (canale semestrale), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Questo argomento descrive come usare Windows Server per distribuire [profili utente mobili](folder-redirection-rup-overview.md) ai computer client Windows. I profili utente mobili reindirizzare i profili utente in una condivisione file in modo che gli utenti ricevono la stessa del sistema operativo e le impostazioni dell'applicazione in più computer.

Per un elenco delle modifiche recenti apportate a questo argomento, vedere la [cronologia modifiche](#change-history) sezione di questo argomento.

> [!IMPORTANT]
> A causa della protezione modifiche apportate [MS16 072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016), sono stati aggiornati [passaggio 4: Facoltativamente, creare un oggetto Criteri di gruppo per i profili utente mobili](#step-4-optionally-create-a-gpo-for-roaming-user-profiles) in questo argomento in modo che Windows possa applicare correttamente il criterio dei profili utente mobili (e non è stato ripristinato a criteri locali nei computer interessati).

> [!IMPORTANT]
>  Le personalizzazioni dell'utente per l'avvio viene perso dopo un aggiornamento sul posto del sistema operativo nella configurazione seguente:
> - Gli utenti siano configurati per un profilo mobile
> - Gli utenti possono apportare modifiche a Start
>
> Di conseguenza, nel menu Start viene reimpostato sul valore predefinito della nuova versione del sistema operativo dopo che il sistema operativo-aggiornamento sul posto. Per le soluzioni alternative, vedere [appendice c: Lavoro circa Reimposta layout di menu Start dopo ogni aggiornamento](#appendix-c-working-around-reset-start-menu-layouts-after-upgrades).

## <a name="prerequisites"></a>Prerequisiti

### <a name="hardware-requirements"></a>Requisiti hardware

I profili utente mobili richiede un computer basato su x86 o x64. non è supportata per Windows RT.

### <a name="software-requirements"></a>Requisiti software

I profili utente mobili devono soddisfare i requisiti software seguenti:

- Se si distribuiscono profili utente mobili con Reindirizzamento cartelle in un ambiente con profili utente locali, distribuire Reindirizzamento cartelle prima dei profili utente mobili in modo da ridurre al minimo le dimensioni dei profili di roaming. Dopo aver reindirizzato correttamente le cartelle utente, è possibile distribuire i profili utente mobili.
- Per amministrare i profili utente mobili, è necessario essere connessi come membro del gruppo di sicurezza amministratori di dominio, il gruppo di sicurezza amministratori dell'organizzazione o il gruppo di sicurezza Group Policy Creator Owners.
- I computer client devono eseguire Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008.
- I computer client devono essere aggiunti ai Servizi di dominio Active Directory in corso di gestione.
- Deve essere disponibile un computer in cui siano installati l'editor Gestione Criteri di gruppo e il Centro amministrativo di Active Directory.
- Per l'hosting dei profili utente mobili deve essere disponibile un file server.

    - Se la condivisione file usa gli spazi dei nomi DFS, le cartelle DFS (collegamenti) devono avere un'unica destinazione per impedire agli utenti di apportare modifiche conflittuali su server differenti.
    - Se la condivisione file usa la Replica DFS per replicare i contenuti con un altro server, gli utenti devono poter accedere all'unico server di origine per impedire agli utenti di apportare modifiche conflittuali su server differenti.
    - Se la condivisione file è di tipo cluster, disabilitare la disponibilità continua sulla condivisione file per evitare problemi di prestazioni.
- Per usare il supporto di computer primari nei profili utente mobili, sono presenti computer client aggiuntivi e requisiti dello schema di Active Directory. Per altre informazioni, vedere [distribuire computer primari per il reindirizzamento delle cartelle e profili utente mobili](deploy-primary-computers.md).
- Il layout dell'inizio di un utente dal menu non effettuano il roaming in Windows 10, Windows Server 2019 o Windows Server 2016, se si usa più di un PC, Host sessione Desktop remoto o virtualizzato Desktop Infrastructure (VDI) del server. In alternativa, è possibile specificare un layout avviare come descritto in questo argomento. È possibile impostare o utilizzo dei dischi dei profili utente, che in modo corretto il roaming delle impostazioni di menu Start se usato con i server Host sessione Desktop remoto o server VDI. Per altre informazioni, vedi [semplifica la gestione dei dati utente dischi dei profili utente in Windows Server 2012](https://blogs.technet.microsoft.com/enterprisemobility/2012/11/13/easier-user-data-management-with-user-profile-disks-in-windows-server-2012/).

### <a name="considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows"></a>Considerazioni sull'uso di profili utente mobili su più versioni di Windows

Se si decide di usare profili utente mobili in più versioni di Windows è consigliabile eseguire le azioni seguenti:

- Configurare Windows in modo da mantenere versioni di profili separate per ciascuna versione del sistema operativo. Ciò aiuta a prevenire i problemi indesiderati e imprevedibili, come il danneggiamento del profilo.
- Usare il Reindirizzamento cartelle per archiviare i file utente, quali documenti e immagini all'esterno dei profili utente. Ciò rende disponibili gli stessi file agli utenti di versioni differenti del sistema operativo e consente anche di contenere le dimensioni dei profili e di accelerare gli accessi.
- Prevedere risorse di archiviazione sufficienti per i profili utente mobili Se si supportano due versioni del sistema operativo, il numero dei profili raddoppia (e di conseguenza anche lo spazio totale consumato) perché viene mantenuto un profilo separato per ogni versione del sistema operativo.
- Non usare i profili utente mobili su computer che eseguono Windows Vista/Windows Server 2008 e Windows 7 e Windows Server 2008 R2. Il roaming tra queste versioni del sistema operativo non è supportato a causa di incompatibilità nelle rispettive versioni del profilo.
- Informare gli utenti che non verrà effettuato il roaming delle modifiche apportate da una versione a un'altra del sistema operativo.
- Quando si spostano l'ambiente a una versione di Windows che usa una versione del profilo diversi (ad esempio da Windows 10 a Windows 10, versione 1607, vedere [appendice b: Informazioni di riferimento di versione del profilo](#appendix-b-profile-version-reference-information) per un elenco), gli utenti ricevono un profilo utente mobile nuovo e vuoto. È possibile ridurre al minimo l'impatto di ottenere un nuovo profilo tramite reindirizzamento cartelle per reindirizzare cartelle più comuni. Non è supportato alcun metodo di migrazione dei profili utente comuni dalla versione di un profilo a un altro.

## <a name="step-1-enable-the-use-of-separate-profile-versions"></a>Passaggio 1: Abilitare l'uso delle versioni di profili separati

Se si distribuiscono profili utente mobili nei computer che eseguono Windows 8.1, Windows 8, Windows Server 2012 R2 o Windows Server 2012, è consigliabile apportare alcune modifiche all'ambiente Windows prima della distribuzione. Tali modifiche aiutano a garantire che i futuri aggiornamenti del sistema operativo avvengano in maniera fluida e semplificano la possibilità di eseguire simultaneamente più versioni di Windows con i profili utente mobili.

Per apportare queste modifiche, usare la procedura seguente.

1. Scaricare e installare l'aggiornamento software appropriato su tutti i computer sui quali si userà il roaming (profili predefiniti obbligatori, super obbligatori o predefiniti di dominio):

    - Windows 8.1 o Windows Server 2012 R2: installare l'aggiornamento del software descritto nell'articolo [2887595](http://support.microsoft.com/kb/2887595) della Microsoft Knowledge Base (quando rilasciato).
    - Windows 8 o Windows Server 2012: installare l'aggiornamento del software descritto nell'articolo [2887239](http://support.microsoft.com/kb/2887239) della Microsoft Knowledge Base.

2. In tutti i computer che eseguono Windows 8.1, Windows 8, Windows Server 2012 R2 o Windows Server 2012 nei quali si useranno i profili utente mobili, utilizzare l'Editor del Registro di sistema o criteri di gruppo per creare il seguente del Registro di sistema chiave DWORD e impostarla per `1`. Per informazioni sulla creazione delle chiavi del Registro di sistema con i Criteri di gruppo, vedere [Configurare un elemento Registro di sistema](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753092(v=ws.11)>).

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

1. Aprire Server Manager in un computer con il centro di amministrazione di Active Directory installati.
2. Nel **degli strumenti** dal menu **centro di amministrazione di Active Directory**. Verrà visualizzato Centro amministrativo di Active Directory.
3. Fare doppio clic il dominio appropriato o unità Organizzativa, selezionare **New**, quindi selezionare **gruppo**.
4. Nella sezione **Gruppo** della finestra **Crea gruppo** specificare le impostazioni seguenti:

    - Digitare il nome del gruppo di sicurezza in **Nome gruppo**, ad esempio: **Utenti e computer profili utente mobili**.
    - Nella **ambito del gruppo**, selezionare **Security**, quindi selezionare **Global**.

5. Nel **membri** sezione, selezionare **Add**. Verrà visualizzata la finestra di dialogo Selezione utenti, computer, account servizio o gruppi.
6. Se si desidera includere account computer nel gruppo di sicurezza, selezionare **tipi di oggetti**, selezionare la **computer** casella di controllo e quindi selezionare **OK**.
7. Digitare i nomi di utenti, gruppi, e/o computer a cui si desidera distribuire i profili utente, selezionare **OK**, quindi selezionare **OK** nuovamente.

## <a name="step-3-create-a-file-share-for-roaming-user-profiles"></a>Passaggio 3: Creare una condivisione file per i profili utente mobili

Se non si dispone già di una condivisione file separata per il roaming profili utente mobili (indipendenti da tutte le condivisioni per le cartelle reindirizzate per prevenire l'involontaria memorizzazione nella cache della cartella del profilo mobile), utilizzare la procedura seguente per creare una condivisione file in un server che esegue Windows Server.

> [!NOTE]
> Alcune funzionalità potrà differire o non essere disponibili a seconda della versione di Windows Server in uso.

Di seguito viene illustrato come creare una condivisione file in Windows Server:

1. Nel riquadro di spostamento di Server Manager, selezionare **servizi File e archiviazione**, quindi selezionare **condivisioni** per visualizzare la pagina condivisioni.
2. Nel riquadro condivisione, selezionare **attività**, quindi selezionare **nuova condivisione**. Verrà visualizzata la Creazione guidata nuova condivisione.
3. Nel **selezionare il profilo** pagina, selezionare **condivisione SMB-rapida**. Se si dispone di gestione risorse File Server installato e si usano le proprietà di gestione di cartella, selezionare invece **condivisione SMB - avanzata**.
4. Nella pagina **Percorso condivisione** selezionare il server e il volume sui quali creare la condivisione.
5. Nella pagina **Nome condivisione** digitare un nome per la condivisione (ad esempio **Profili utente$** ) nella casella **Nome condivisione**.

    > [!TIP]
    > Quando si crea la condivisione, nasconderla inserendo ```$``` dopo il nome condivisione. In tal modo, la condivisione verrà nascosta dai browser casuali.

6. Nella pagina **Altre impostazioni** deselezionare la casella di controllo **Abilita disponibilità continua**, se disponibile e selezionare facoltativamente le caselle di controllo **Abilita enumerazione basata sull'accesso** e **Crittografa accesso ai dati**.
7. Nel **le autorizzazioni** pagina, selezionare **personalizzare le autorizzazioni...** . Viene visualizzata la finestra di dialogo Impostazioni di protezione avanzate.
8. Selezionare **Disabilita ereditarietà**, quindi selezionare **Converti autorizzazioni ereditate in autorizzazioni esplicite per questo oggetto**.
9. Impostare le autorizzazioni come descritto in [autorizzazioni necessarie per il file di condividono che ospita i profili utente mobili](#required-permissions-for-the-file-share-hosting-roaming-user-profiles) e visualizzate nella schermata riportata di seguito, la rimozione delle autorizzazioni per gli account e gruppi rimossi dall'elenco e l'aggiunta di speciali autorizzazioni per il gruppo di computer e agli utenti di profili utente mobili creato nel passaggio 1.
    
    ![Finestra Impostazioni di sicurezza che mostra le autorizzazioni come descritto nella tabella 1 avanzato](media/advanced-security-user-profiles.jpg)
    
    **Figura 1** Impostare le autorizzazioni per la condivisione dei profili utente mobili
10. Se si sceglie il profilo **Condivisione SMB - avanzata** selezionare il valore Utilizzo cartelle **File utente** nella pagina **Proprietà gestione** .
11. Se si sceglie il profilo **Condivisione SMB - avanzata** nella pagina **Quota** , selezionare facoltativamente una quota da applicare agli utenti della condivisione.
12. Nel **conferma** pagina, selezionare **Create.**

### <a name="required-permissions-for-the-file-share-hosting-roaming-user-profiles"></a>Autorizzazioni necessarie per i file share hosting i profili utente mobili

| Account utente | Accesso | Si applica a |
|   -   |   -   |   -   |
|   Sistema    |  Controllo completo     |  Questa cartella, le sottocartelle e i file     |
|  Administrators     |  Controllo completo     |  Solo questa cartella     |
|  Autore/proprietario     |  Controllo completo     |  Solo sottocartelle e file     |
| Gruppo di sicurezza degli utenti che devono inserire dati nella condivisione (Utenti e computer profili utente mobili)      |  Elenco cartelle / lettura dati *(autorizzazioni avanzate)* <br />Creazione cartelle / Aggiunta dati *(autorizzazioni avanzate)* |  Solo questa cartella     |
| Altri gruppi e account   |  Nessuno (rimuovere)     |       |

## <a name="step-4-optionally-create-a-gpo-for-roaming-user-profiles"></a>Passaggio 4: Creare facoltativamente un oggetto Criteri di gruppo per i profili utente mobili

Se non è già stato creato un oggetto Criteri di gruppo per le impostazioni dei profili utente mobili, usare la procedura seguente per creare un oggetto Criteri di gruppo vuoto da usare con i profili utente mobili. Questo oggetto Criteri di gruppo consente di configurare le impostazioni dei profili utente mobili (ad esempio il supporto del computer primario, che verrà discusso in separata sede) e può anche essere usato per abilitare i profili utente mobili sui computer, come di norma è possibile nella distribuzione degli ambienti di desktop virtuale o con Servizi Desktop remoto.

Di seguito viene illustrato come creare un oggetto Criteri di gruppo per i profili utente mobili:

1. Aprire Server Manager in un computer in cui è installata la console Gestione Criteri di gruppo.
2. Dal **degli strumenti** dal menu **Gestione criteri di gruppo**. Viene visualizzato Gestione Criteri di gruppo.
3. Fare doppio clic il dominio o unità Organizzativa in cui si desidera configurare i profili utente, quindi selezionare **crea un oggetto Criteri di gruppo in questo dominio e crea qui un collegamento**.
4. Nel **nuovo GPO** della finestra di dialogo digitare un nome per l'oggetto Criteri di gruppo (ad esempio, **impostazioni profilo utente mobile**) e quindi selezionare **OK**.
5. Fare clic con il pulsante destro del mouse sull'oggetto Criteri di gruppo appena creato e quindi deselezionare la casella di controllo **Collegamento abilitato** . Ciò impedisce di applicare l'oggetto Criteri di gruppo finché non sarà terminata la configurazione.
6. Selezionare l'oggetto Criteri di gruppo. Nel **filtri di sicurezza** sezione il **ambito** scheda, seleziona **Authenticated Users**e quindi selezionare **rimuovere** per evitare che l'oggetto Criteri di gruppo di essere applicata a tutti gli utenti.
7. Nel **filtri di sicurezza** sezione, selezionare **Add**.
8. Nel **Seleziona utenti, Computer o gruppo** finestra di dialogo, digitare il nome di sicurezza gruppo creato nel passaggio 1 (ad esempio, **agli utenti di profili utente mobili e computer**) e quindi selezionare **OK** .
9. Selezionare il **delega** scheda, seleziona **Add**, digitare **Authenticated Users**, selezionare **OK**e quindi selezionare **OK** nuovamente per accettare il valore predefinito di autorizzazioni di lettura.
    
    Questo passaggio è necessario a causa di modifiche di sicurezza apportate [MS16 072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016).

>[!IMPORTANT]
>A causa della protezione modifiche apportate [MS16 072A](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016), è ora necessario assegnare le autorizzazioni di lettura del gruppo delegata Authenticated Users per l'oggetto Criteri di gruppo, in caso contrario, oggetto Criteri di gruppo non vengano applicate a utenti o se è già stato applicato, l'oggetto Criteri di gruppo viene rimosso, reindirizzamento profili utente mobili su PC locale. Per altre informazioni, vedi [distribuzione gruppo di criteri di sicurezza aggiornamento MS16-072](https://blogs.technet.microsoft.com/askds/2016/06/22/deploying-group-policy-security-update-ms16-072-kb3163622/).

## <a name="step-5-optionally-set-up-roaming-user-profiles-on-user-accounts"></a>Passaggio 5: Configurare facoltativamente i profili utente mobili sugli account utente

Se si distribuiscono profili utente mobili agli account utente, usare la procedura seguente per specificare i profili utente mobili per gli account utente in Servizi di dominio Active Directory. Se si distribuiscono profili utente mobili ai computer, come avviene in genere per Servizi Desktop remoto o le distribuzioni di desktop virtualizzate, utilizzare invece la procedura documentata al [passaggio 6: Configurare facoltativamente i profili utente mobili sui computer](#step-6-optionally-set-up-roaming-user-profiles-on-computers).

> [!NOTE]
> Se si configurano profili utente mobili sugli account utente usando Active Directory e sui computer usando Criteri di gruppo, l'impostazione criterio basata sul computer ha la precedenza.

Di seguito viene illustrato come configurare i profili utente mobili sugli account utente:

1. Nel Centro amministrativo di Active Directory passare al contenitore (o unità organizzativa) **Utenti** nel dominio appropriato.
2. Selezionare tutti gli utenti a cui si desidera assegnare un profilo utente mobile, fare doppio clic su utenti e quindi selezionare **proprietà**.
3. Nel **profilo** selezionare la **percorso profilo:** casella di controllo e quindi immettere il percorso della condivisione file in cui si desidera archiviare profilo utente mobile dell'utente, seguito da `%username%` (ovvero automaticamente sostituito con il nome utente al primo accesso all'utente in). Ad esempio:
    
    `\\fs1.corp.contoso.com\User Profiles$\%username%`
    
    Per specificare un profilo utente mobile obbligatorio, specificare il percorso del file Ntuser. man creato in precedenza, ad esempio, `fs1.corp.contoso.comUser Profiles$default`. Per altre informazioni, vedere [creare i profili utente obbligatori](https://docs.microsoft.com/windows/client-management/mandatory-user-profile).
4. Scegliere **OK**.

> [!NOTE]
> Per impostazione predefinita, la distribuzione di tutte le app basate su Windows® Runtime (Windows Store) è consentita durante l'uso dei profili utente mobili. Quando però si usa un profilo speciale, le app non vengono distribuite per impostazione predefinita. I profili speciali sono profili utente in cui le modifiche vengono rimosse subito dopo la disconnessione dell'utente:
> <br><br>Per rimuovere le limitazioni alla distribuzione delle app per i profili speciali, abilitare l'impostazione criterio **Allow deployment operations in special profiles** (nel percorso Configurazione computer\Criteri\Modelli amministrativi\Componenti di Windows\Distribuzione di pacchetti di app). Le app distribuite in questo scenario lasceranno comunque alcuni dati archiviati sul computer, che potrebbero accumularsi, ad esempio, se un unico computer è usato da centinaia di utenti. Per eliminare le app, trovare o sviluppare uno strumento che utilizza il [CleanupPackageForUserAsync](https://msdn.microsoft.com/library/windows/apps/windows.management.deployment.packagemanager.cleanuppackageforuserasync.aspx) API per pulire i pacchetti dell'app per gli utenti che non hanno più un profilo sul computer.
> <br><br>Per altre informazioni di carattere generale sulle app di Windows Store vedere [Gestisci l'accesso client a Windows Store](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh832040(v=ws.11)>).

## <a name="step-6-optionally-set-up-roaming-user-profiles-on-computers"></a>Passaggio 6: Configurare facoltativamente i profili utente mobili sui computer

Se si distribuiscono profili utente mobili ai computer, come di norma è possibile per le distribuzioni di Servizi Desktop remoto o desktop virtuale, usare la procedura seguente. Se si distribuiscono i profili utente mobili agli account utente, usare invece la procedura descritta in [passaggio 5: Configurare facoltativamente i profili utente mobili sugli account utente](#step-5-optionally-set-up-roaming-user-profiles-on-user-accounts).

È possibile utilizzare criteri di gruppo per applicare i profili utente mobili ai computer che eseguono Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008.

> [!NOTE]
> Se si configurano profili utente mobili sui computer usando Criteri di gruppo e sugli account utente usando Active Directory, l'impostazione criterio basata sul computer ha la precedenza.

Di seguito viene illustrato come configurare i profili utente mobili sui computer:

1. Aprire Server Manager in un computer in cui è installata la console Gestione Criteri di gruppo.
2. Dal **degli strumenti** dal menu **Gestione criteri di gruppo**. Verrà visualizzato Gestione criteri di gruppo.
3. In Gestione criteri di gruppo, fare doppio clic su oggetto Criteri di gruppo creato nel passaggio 3 (ad esempio, **impostazioni dei profili utente mobili**), quindi selezionare **modificare**.
4. Nella finestra Editor Gestione Criteri di gruppo passare a **Configurazione computer**, **Criteri**, **Modelli amministrativi**, **Sistema** e quindi **Profili utente**.
5. Fare doppio clic su **Imposta percorso profilo mobile per tutti gli utenti accedono al computer** e quindi selezionare **modificare**.
    > [!TIP]
    > La home directory di un utente, se configurata, è la cartella predefinita usata da alcuni programmi come Windows PowerShell. È possibile configurare un percorso di rete o locale alternativo in base al numero di utenti con la sezione **Home directory** delle proprietà dell'account utente in Servizi di dominio Active Directory. Per configurare il percorso della home directory per tutti gli utenti di un computer che esegue Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 in un ambiente di desktop virtuale, abilitare il **imposta home directory utente**  impostazione dei criteri, quindi specificare la lettera di unità e condivisione di file per eseguire il mapping (oppure specificare una cartella locale). Non usare variabili di ambiente o puntini di sospensione. L'alias dell'utente viene aggiunto alla fine del percorso specificato durante l'accesso dell'utente.
6. Nel **delle proprietà** finestra di dialogo **abilitato**
7. Nel **gli utenti accedono al computer devono utilizzare il percorso profilo mobile seguente** immettere il percorso della condivisione file in cui si desidera archiviare profilo utente mobile dell'utente, seguito da `%username%` (che verrà automaticamente sostituito con l'ora il primo nome utente di accesso dell'utente). Ad esempio:

    `\\fs1.corp.contoso.com\User Profiles$\%username%`

    Per specificare un profilo utente obbligatorio, ovvero un profilo preconfigurato al quale gli utenti non possono apportare modifiche permanenti (modificati vengono ripristinati alla disconnessione dell'utente), specificare il percorso del file Ntuser. man creato in precedenza, ad esempio, `\\fs1.corp.contoso.com\User Profiles$\default`. Per altre informazioni, vedere [Creazione di un profilo utente obbligatorio](https://docs.microsoft.com/windows/client-management/mandatory-user-profile).
8. Scegliere **OK**.

## <a name="step-7-optionally-specify-a-start-layout-for-windows-10-pcs"></a>Passaggio 7: Facoltativamente specificare un layout iniziale per i PC Windows 10

È possibile utilizzare criteri di gruppo per applicare un layout di menu Start specifico in modo che gli utenti visualizzano lo stesso layout avviare in tutti i PC. Se gli utenti accedono a più di un PC e si desidera che hanno un layout avviare coerente tra i computer, assicurarsi che l'oggetto Criteri di gruppo si applica a tutti i PC.

Per specificare un layout avviare, eseguire le operazioni seguenti:

1. Aggiornare i PC Windows 10 a Windows 10 versione 1607 (nota anche come l'aggiornamento dell'anniversario) o versione successiva e installare l'aggiornamento cumulativo 14 marzo 2017 ([KB4013429](https://support.microsoft.com/kb/4013429)) o versione successiva.
2. Creare un file completo o parziale Start menu layout XML. A tale scopo, vedere [esportazione layout Start e Personalizza](https://docs.microsoft.com/windows/configuration/customize-and-export-start-layout).
    * Se si specifica un *completo* layout Start, un utente non è possibile personalizzare qualsiasi parte del menu Start. Se si specifica un *parziale* layout Start, gli utenti possono personalizzare tutti gli elementi tranne i gruppi di riquadri è specificare bloccati. Tuttavia, con un layout avviare parziale, le personalizzazioni dell'utente nel menu Start non verranno effettuato il roaming, in altri computer.
3. Usare criteri di gruppo per applicare il layout Start personalizzato per l'oggetto Criteri di gruppo creato per i profili utente mobili. A tale scopo, vedere [usare i criteri di gruppo per applicare un layout Start personalizzato in un dominio](https://docs.microsoft.com/windows/configuration/customize-windows-10-start-screens-by-using-group-policy#bkmk-domaingpodeployment).
4. Usare criteri di gruppo per impostare il valore del Registro di sistema seguente nei PC Windows 10. A tale scopo, vedere [configurare una voce del Registro di sistema](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753092(v=ws.11)>).

| **Azione**   | **Aggiornamento**                  |
| ------------ | ------------                |
| Hive         | **HKEY_LOCAL_MACHINE**      |
| Percorso della chiave     | **Software\Microsoft\Windows\CurrentVersion\Explorer** |
| Nome del valore   | **SpecialRoamingOverrideAllowed** |
| Tipo di valore   | **REG_DWORD**               |
| Dati valore   | **1** (o **0** disabilitare) |
| Base         | **Decimal**                 |

5. (Facoltativo) Abilita le ottimizzazioni di primo accesso rendere l'accesso più rapido per gli utenti. A tale scopo, vedere [applicare i criteri per migliorare i tempi di accesso](https://docs.microsoft.com/windows/client-management/mandatory-user-profile#apply-policies-to-improve-sign-in-time).
6. (Facoltativo) Consente di ridurre ulteriormente i tempi di accesso tramite la rimozione di App non necessari dall'immagine di base di Windows 10 che consente di distribuire i PC client. 2019 Server Windows e Windows Server 2016 non sono presenti App pre-provisioning, in modo che è possibile ignorare questo passaggio nelle immagini di server.
    - Per rimuovere le app, usare il [Remove-AppxProvisionedPackage](https://docs.microsoft.com/powershell/module/dism/remove-appxprovisionedpackage?view=win10-ps) cmdlet in Windows PowerShell per disinstallare le applicazioni seguenti. Se i PC sono già stati distribuiti è possibile generare uno script la rimozione di queste App usando il [Remove-AppxPackage](https://docs.microsoft.com/powershell/module/appx/remove-appxpackage?view=win10-ps).
    
      - Microsoft.windowscommunicationsapps\_8wekyb3d8bbwe
      - Microsoft.BingWeather\_8wekyb3d8bbwe
      - Microsoft.DesktopAppInstaller\_8wekyb3d8bbwe
      - Microsoft.Getstarted\_8wekyb3d8bbwe
      - Microsoft.Windows.Photos\_8wekyb3d8bbwe
      - Microsoft.WindowsCamera\_8wekyb3d8bbwe
      - Microsoft.WindowsFeedbackHub\_8wekyb3d8bbwe
      - Microsoft.WindowsStore\_8wekyb3d8bbwe
      - Microsoft.XboxApp\_8wekyb3d8bbwe
      - Microsoft.XboxIdentityProvider\_8wekyb3d8bbwe
      - Microsoft.ZuneMusic\_8wekyb3d8bbwe

>[!NOTE]
>Disinstallazione di queste App riduce tempi di accesso, ma è possibile lasciarli installati qualora esigenze della distribuzione è uno di essi.

## <a name="step-8-enable-the-roaming-user-profiles-gpo"></a>Passaggio 8: Abilitare l'oggetto Criteri di gruppo dei profili utente mobili

Se si configurano i profili utente mobili sui computer usando i Criteri di gruppo oppure se sono state personalizzate altre impostazioni dei profili utente mobili usando i Criteri di gruppo, il passaggio successivo consiste nell'abilitazione dell'oggetto Criteri di gruppo, consentendone l'applicazione agli utenti interessati.

>[!TIP]
>Se si prevede di implementare il supporto per computer primari, farlo ora, prima di abilitare l'oggetto Criteri di gruppo. In tal modo, i dati utente non verranno copiati nei computer non primari prima di aver abilitato il supporto del computer primario. Per le impostazioni dei criteri specifici, vedere [distribuire computer primari per il reindirizzamento delle cartelle e profili utente mobili](deploy-primary-computers.md).

Ecco come abilitare l'oggetto Criteri di gruppo del profilo utente Roaming:

1. Aprire Gestione Criteri di gruppo.
2. Fare doppio clic su oggetto Criteri di gruppo è stato creato e quindi selezionare **collegamento abilitato**. Accanto alla voce del menu viene visualizzata una casella di controllo.

## <a name="step-9-test-roaming-user-profiles"></a>Passaggio 9: Testare i profili utente mobili

Per testare i profili utente mobili, accedere a un computer con un account utente configurato per i profili utente mobili oppure accedere a un computer configurato per i profili utente mobili. Confermare quindi che il profilo è reindirizzato.

Di seguito viene illustrato come testare i profili utente mobili:

1. Accedere a un computer primario (se è stato abilitato il supporto per il computer primario) con un account utente per il quale sono stati abilitati i profili utente mobili. Se sono stati abilitati i profili utente mobili su computer specifici, accedere a uno di questi.
2. Se l'utente ha precedentemente eseguito l'accesso al computer, aprire un prompt dei comandi con privilegi elevati e quindi digitare il comando seguente per assicurarsi che vengano applicate le impostazioni Criteri di gruppo più recenti al computer client:
    
    ```PowerShell
    GpUpdate /Force
    ```
3. Per verificare che il profilo utente di roaming, aprire **Pannello di controllo**, selezionare **sistema e sicurezza**, selezionare **System**, selezionare **impostazioni di sistema avanzate** , selezionare **delle impostazioni** nei profili utente della sezione e quindi cercare **Roaming** nel **tipo** colonna.

## <a name="appendix-a-checklist-for-deploying-roaming-user-profiles"></a>Appendice A: Elenco di controllo per la distribuzione dei profili utente mobili

| Stato                     | Azione                                                |
| ---                        | ------                                                |
| ☐<br>☐<br>☐<br>☐<br>☐   | 1. Preparare il dominio<br>-Aggiungere computer al dominio<br>-Abilitare l'uso di versioni di profili separati<br>-Creare account utente<br>-(Facoltativo) distribuire reindirizzamento cartelle |
| ☐<br><br><br>             | 2. Creare un gruppo di sicurezza per i profili utente mobili<br>-Nome gruppo:<br>-Membri: |
| ☐<br><br>                 | 3. Creare una condivisione file per i profili utente mobili<br>-Nome della condivisione file: |
| ☐<br><br>                 | 4. Creare un oggetto Criteri di gruppo per i profili utente mobili<br>-Nome oggetto Criteri di gruppo:|
| ☐                         | 5. Configurare le impostazioni criterio dei profili utente mobili    |
| ☐<br>☐<br>☐              | 6. Abilitare i profili utente<br>-Abilitati in Active Directory Domain Services per gli account utente?<br>-Abilitati in Criteri di gruppo sugli account computer?<br> |
| ☐                         | 7. (Facoltativo) Specificare un layout avviare obbligatorio per i PC Windows 10 |
| ☐<br>☐<br><br>☐<br><br>☐ | 8. (Facoltativo) Abilita supporto del computer primario<br>-Designare i computer primari per gli utenti<br>-Percorso dell'utente e di mapping dei computer primari:<br>-(Facoltativo) abilitare il supporto del computer primario per il reindirizzamento cartelle<br>-Basata sul computer o basata sull'utente?<br>-(Facoltativo) abilitare il supporto del computer primario per i profili utente mobili |
| ☐                        | 9. Abilitare l'oggetto Criteri di gruppo dei profili utente mobili                |
| ☐                        | 10. Testare i profili utente mobili                         |

## <a name="appendix-b-profile-version-reference-information"></a>Appendice B: Informazioni di riferimento relative alla versione del profilo

Ogni profilo è una versione del profilo che corrisponde approssimativamente alla versione di Windows in cui viene utilizzato il profilo. Ad esempio, Windows 10, versione 1703 e versione 1607 usano entrambi il. Versione del profilo v6. Microsoft crea una nuova versione del profilo solo quando necessario, per mantenere la compatibilità, ovvero il motivo per cui non tutte le versioni di Windows includono una nuova versione del profilo.

Nella tabella seguente è indicato il percorso dei profili utente mobili su varie versioni di Windows.

| Versione del sistema operativo | Percorso del profilo utente mobile |
| --- | --- |
| Windows XP e Windows Server 2003 | ```\\<servername>\<fileshare>\<username>``` |
| Windows Vista e Windows Server 2008 | ```\\<servername>\<fileshare>\<username>.V2``` |
| Windows 7 e Windows Server 2008 R2 | ```\\<servername>\<fileshare>\<username>.V2``` |
| Windows 8 e Windows Server 2012 | ```\\<servername>\<fileshare>\<username>.V3``` (dopo aver applicato la chiave del Registro di sistema e aggiornamento software)<br>```\\<servername>\<fileshare>\<username>.V2``` (prima il software di aggiornamento e del Registro di sistema la chiave vengono applicati) |
| Windows 8.1 e Windows Server 2012 R2 | ```\\<servername>\<fileshare>\<username>.V4``` (dopo aver applicato la chiave del Registro di sistema e aggiornamento software)<br>```\\<servername>\<fileshare>\<username>.V2``` (prima il software di aggiornamento e del Registro di sistema la chiave vengono applicati) |
| Windows 10 | ```\\<servername>\<fileshare>\<username>.V5``` |
| Windows 10, versione 1703 e versione 1607 | ```\\<servername>\<fileshare>\<username>.V6``` |

## <a name="appendix-c-working-around-reset-start-menu-layouts-after-upgrades"></a>Appendice C: Lavoro circa Reimposta il layout di menu Start dopo gli aggiornamenti

Ecco alcuni modi per aggirare i layout di menu Start introduzione reimpostati dopo un aggiornamento sul posto:

- Se solo un utente utilizza sempre il dispositivo e l'amministratore IT utilizza una strategia di distribuzione del sistema operativo gestita, ad esempio SCCM è possibile eseguire le operazioni seguenti:
    
  1. Esportare il layout del menu Start con Export-Startlayout prima dell'aggiornamento 
  2. Importare il layout del menu Start con Import-StartLayout dopo la configurazione guidata, ma prima che l'utente accede  
 
     > [!NOTE] 
     > L'importazione di un StartLayout consente di modificare il profilo utente predefinito. Tutti i profili utente creati dopo l'operazione di importazione otterranno il Layout Start importato.
 
- Gli amministratori IT possono scegliere di gestire il Layout dell'inizio con i criteri di gruppo. Usare criteri di gruppo offre una soluzione di gestione centralizzata per applicare un Layout avviare standardizzati per gli utenti. Sono disponibili 2 modalità per le modalità per tramite criteri di gruppo per la gestione di inizio. Blocco completa e blocco parziale. Lo scenario di blocco completa impedisce all'utente di apportare modifiche al layout dell'avvio. Lo scenario di blocco parziale consente di apportare modifiche a un'area specifica di inizio. Per altre informazioni, vedi [esportazione layout Start e Personalizza](https://docs.microsoft.com/windows/configuration/customize-and-export-start-layout).
        
   > [!NOTE]
   > Le modifiche apportate utente nello scenario di blocco parziale continuerà a essere perse durante l'aggiornamento.

- Consentire l'avvio Reimposta layout si verificano e consentire agli utenti finali riconfigurare l'inizio. Una notifica tramite posta elettronica o altre notifiche da inviare agli utenti finali a prevedere i layout Start per essere reimpostato dopo il sistema operativo esegue l'aggiornamento a impatto ridotto a icona. 

# <a name="change-history"></a>Cronologia delle modifiche

La tabella seguente include un riepilogo di alcune delle più importanti modifiche apportate a questo argomento.

| Date | Descrizione |`Reason`|
| --- | ---         | ---   |
| 1 ° maggio 2019 | Aggiunta degli aggiornamenti per Windows Server 2019 |
| Il 10 aprile 2018 | Discussione su aggiunto quando vengono perse le personalizzazioni dell'utente per l'avvio dopo un aggiornamento sul posto del sistema operativo|Problema noto di callout. |
| 13 marzo 2018 | Aggiornato per Windows Server 2016 | Spostati di fuori della libreria di versioni precedenti e aggiornato per la versione corrente di Windows Server. |
| 13 aprile 2017 | Aggiunte informazioni sul profilo di Windows 10 versione 1703 ed è stato chiarito come roaming del profilo versioni lavoro durante l'aggiornamento di sistemi operativi, vedere [considerazioni sull'uso di profili utente mobili su più versioni di Windows](#considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows). | Commenti e suggerimenti dei clienti. |
| Il 14 marzo 2017 | Passaggio facoltativo aggiuntivo per specificare un layout avviare obbligatorio per i PC Windows 10 in [appendice a: Elenco di controllo per la distribuzione dei profili utente mobili](#appendix-a-checklist-for-deploying-roaming-user-profiles). |Modifiche apportate alle funzionalità nell'aggiornamento più recente di Windows. |
| 23 gennaio 2017 | Aggiungere un passaggio per [passaggio 4: Facoltativamente, creare un oggetto Criteri di gruppo per i profili utente](#step-4-optionally-create-a-gpo-for-roaming-user-profiles) per delegare le autorizzazioni di lettura agli utenti autenticati, che è ora necessario a causa di un aggiornamento della sicurezza dei criteri di gruppo.|Relative alla sicurezza per l'elaborazione di criteri di gruppo. |
| 29 dicembre 2016 | Aggiunto un collegamento in [passaggio 8: Abilitare il GPO di profili utente mobili](#step-8-enable-the-roaming-user-profiles-gpo) per renderne più semplice ottenere informazioni su come impostare criteri di gruppo per i computer primari. Corretti anche alcuni riferimenti non corretto i passaggi 5 e 6 con i numeri.|Commenti e suggerimenti dei clienti. |
| Il 5 dicembre 2016 | Informazioni aggiunte che descrive delle impostazioni dal menu Start roaming problema. | Commenti e suggerimenti dei clienti. |
| 6 luglio 2016 | Aggiunta di suffissi della versione del profilo Windows 10 in [appendice b: Informazioni di riferimento di versione del profilo](#appendix-b-profile-version-reference-information). Rimosso anche dall'elenco dei sistemi operativi Windows XP e Windows Server 2003. | Aggiornamenti per le nuove versioni di Windows e rimosse le informazioni sulle versioni di Windows che non sono più supportate. |
| 7 luglio 2015 | Aggiunta di requisiti e passaggi per disabilitare la disponibilità continua quando si usa un file server in cluster. | Le condivisioni di file in cluster offrono migliori prestazioni per scritture di piccola entità (tipiche con i profili utente mobili) quando la disponibilità continua è disabilitata. |
| 19 marzo 2014 | Scritto in lettere maiuscole nei suffissi della versione del profilo (. V2. V3. V4) nel [appendice b: Informazioni di riferimento di versione del profilo](#appendix-b-profile-version-reference-information). | Anche se Windows è maiuscole e minuscole, se si usa NFS con la condivisione file, è importante avere la corretta grafia (maiuscole) del suffisso del profilo. |
| 9 ottobre 2013 | Rivisto per Windows Server 2012 R2 e Windows 8.1, spiegazione di alcuni elementi e aggiungere il [considerazioni sull'uso dei profili utente mobili su più versioni di Windows](#considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows) e [appendice b: Informazioni di riferimento di versione del profilo](#appendix-b-profile-version-reference-information) sezioni. | Aggiornamenti per la nuova versione; commenti e suggerimenti dei clienti. |

## <a name="more-information"></a>Altre informazioni

- [Distribuire reindirizzamento cartelle, file Offline e profili utente mobili](deploy-folder-redirection.md)
- [Distribuire computer primari per il reindirizzamento delle cartelle e profili utente mobili](deploy-primary-computers.md)
- [Implementazione della gestione dello stato utente](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc784645(v=ws.10)>)
- [Informativa sul supporto di Microsoft relativamente ai dati di profilo utente replicati](https://blogs.technet.microsoft.com/askds/2010/09/01/microsofts-support-statement-around-replicated-user-profile-data/)
- [Sideload delle App con DISM](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh852635(v=win.10)>)
- [Risoluzione dei problemi di creazione dei pacchetti, distribuzione e query delle App basate su Windows Runtime](https://msdn.microsoft.com/library/windows/desktop/hh973484.aspx)