---
title: Distribuzione di Profili utente mobili
TOCTitle: Deploying Roaming User Profiles
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.date: 06/07/2019
ms.author: jgerend
ms.openlocfilehash: 8feed2adb606edfb6068d7fe10c18baf142077ac
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "76822344"
---
# <a name="deploying-roaming-user-profiles"></a>Distribuzione di Profili utente mobili

>Si applica a: Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2019, Windows Server 2016, Windows Server (Canale semestrale), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Questo argomento descrive come usare Windows Server per distribuire [Profili utente mobili](folder-redirection-rup-overview.md) ai computer client Windows. Con Profili utente mobili è possibile reindirizzare i profili utente a una condivisione file, in modo che gli utenti ricevano le stesse impostazioni del sistema operativo e delle applicazioni in più computer.

Per ottenere un elenco delle modifiche apportate di recente a questo argomento, vedi più avanti la sezione [Cronologia delle modifiche](#change-history).

> [!IMPORTANT]
> A causa delle modifiche di sicurezza apportate nell'aggiornamento [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016), è stata aggiornata la sezione [Passaggio 4: Creare facoltativamente un oggetto Criteri di gruppo per Profili utente mobili](#step-4-optionally-create-a-gpo-for-roaming-user-profiles) di questo argomento, in modo che Windows possa applicare in modo appropriato il criterio Profili utente mobili invece di ripristinare i criteri locali nei PC interessati.

> [!IMPORTANT]
>  Le personalizzazioni utente relative al menu Start vanno perse dopo un aggiornamento sul posto del sistema operativo nella configurazione seguente:
> - Utenti configurati per un profilo mobile
> - Utenti autorizzati ad apportare modifiche al menu Start
>
> Di conseguenza, il menu Start viene reimpostato sulla configurazione predefinita della nuova versione del sistema operativo dopo l'aggiornamento sul posto del sistema operativo stesso. Per le soluzioni alternative, vedi [Appendice C: Ovviare alla reimpostazione dei layout del menu Start dopo gli aggiornamenti](#appendix-c-working-around-reset-start-menu-layouts-after-upgrades).

## <a name="prerequisites"></a>Prerequisiti

### <a name="hardware-requirements"></a>Requisiti hardware

La funzionalità Profili utente mobili richiede un computer basato su x64 o su x86 e non è supportata da Windows RT.

### <a name="software-requirements"></a>Requisiti software

I profili utente mobili devono soddisfare i requisiti software seguenti:

- Se si distribuiscono profili utente mobili con Reindirizzamento cartelle in un ambiente con profili utente locali, distribuire Reindirizzamento cartelle prima dei profili utente mobili in modo da ridurre al minimo le dimensioni dei profili di roaming. Dopo aver reindirizzato correttamente le cartelle utente, è possibile distribuire i profili utente mobili.
- Per amministrare Profili utente mobili, devi accedere come membro del gruppo di sicurezza Amministratori di dominio, Amministratori dell'organizzazione o Proprietari autori criteri di gruppo.
- I computer client devono eseguire Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008.
- I computer client devono essere aggiunti ai Servizi di dominio Active Directory in corso di gestione.
- Deve essere disponibile un computer in cui siano installati l'editor Gestione Criteri di gruppo e il Centro amministrativo di Active Directory.
- Per l'hosting dei profili utente mobili deve essere disponibile un file server.

    - Se la condivisione file usa gli spazi dei nomi DFS, le cartelle DFS (collegamenti) devono avere un'unica destinazione per impedire agli utenti di apportare modifiche conflittuali su server differenti.
    - Se la condivisione file usa la Replica DFS per replicare i contenuti con un altro server, gli utenti devono poter accedere all'unico server di origine per impedire agli utenti di apportare modifiche conflittuali su server differenti.
    - Se la condivisione file è di tipo cluster, disabilitare la disponibilità continua sulla condivisione file per evitare problemi di prestazioni.
- Per usare il supporto dei computer primari in Profili utente mobili, sono previsti altri requisiti per i computer client e lo schema di Active Directory. Per altre informazioni, vedi [Distribuire i computer primari per Reindirizzamento cartelle e Profili utente mobili](deploy-primary-computers.md).
- Per il layout del menu Start di un utente non viene effettuato il roaming in Windows 10, Windows Server 2019 o Windows Server 2016 se è in uso più di un PC, un server Host sessione Desktop remoto o un server VDI (Virtualized Desktop Infrastructure). Come soluzione alternativa, puoi specificare un layout Start come descritto in questo argomento. Oppure puoi usare i dischi dei profili utente, che effettuano correttamente il roaming delle impostazioni del menu Start se usati con server Host sessione Desktop remoto o VDI. Per altre informazioni, vedi [Gestione dati utente più semplice con i dischi dei profili utente in Windows Server 2012](https://blogs.technet.microsoft.com/enterprisemobility/2012/11/13/easier-user-data-management-with-user-profile-disks-in-windows-server-2012/).

### <a name="considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows"></a>Considerazioni sull'uso di profili utente mobili su più versioni di Windows

Se si decide di usare profili utente mobili in più versioni di Windows è consigliabile eseguire le azioni seguenti:

- Configurare Windows in modo da mantenere versioni di profili separate per ciascuna versione del sistema operativo. Ciò aiuta a prevenire i problemi indesiderati e imprevedibili, come il danneggiamento del profilo.
- Usare il Reindirizzamento cartelle per archiviare i file utente, quali documenti e immagini all'esterno dei profili utente. Ciò rende disponibili gli stessi file agli utenti di versioni differenti del sistema operativo e consente anche di contenere le dimensioni dei profili e di accelerare gli accessi.
- Prevedere risorse di archiviazione sufficienti per i profili utente mobili Se si supportano due versioni del sistema operativo, il numero dei profili raddoppia (e di conseguenza anche lo spazio totale consumato) perché viene mantenuto un profilo separato per ogni versione del sistema operativo.
- Non usare Profili utente mobili tra computer che eseguono Windows Vista/Windows Server 2008 e Windows 7/Windows Server 2008 R2. Il roaming tra queste versioni del sistema operativo non è supportato a causa di incompatibilità delle rispettive versioni del profilo.
- Informa gli utenti che per le modifiche apportate non verrà effettuato il roaming da una versione all'altra del sistema operativo.
- Quando trasferisci l'ambiente in una versione di Windows che usa una versione diversa del profilo (ad esempio, da Windows 10 a Windows 10 versione 1607; vedi [Appendice B: Informazioni di riferimento relative alla versione del profilo](#appendix-b-profile-version-reference-information) per un elenco), gli utenti ricevono un nuovo profilo utente mobile vuoto. Puoi ridurre al minimo l'effetto che si produce quando si ottiene un nuovo profilo usando Reindirizzamento cartelle per reindirizzare le cartelle comuni. Non è supportato alcun metodo di migrazione dei profili utente mobili da una versione del profilo all'altra.

## <a name="step-1-enable-the-use-of-separate-profile-versions"></a>Passaggio 1: Abilitare l'uso delle versioni di profili separati

Se distribuisci Profili utente mobili nei computer che eseguono Windows 8.1, Windows 8, Windows Server 2012 R2 o Windows Server 2012, è consigliabile apportare alcune modifiche all'ambiente Windows prima di avviare la distribuzione. Tali modifiche aiutano a garantire che i futuri aggiornamenti del sistema operativo avvengano in maniera fluida e semplificano la possibilità di eseguire simultaneamente più versioni di Windows con i profili utente mobili.

Per apportare queste modifiche, usare la procedura seguente.

1. Scarica e installa l'aggiornamento software appropriato in tutti i computer nei quali userai il roaming (profili predefiniti obbligatori, super obbligatori o predefiniti di dominio):

    - Windows 8.1 o Windows Server 2012 R2: installa l'aggiornamento software descritto nell'articolo [2887595](https://support.microsoft.com/kb/2887595) della Microsoft Knowledge Base (quando rilasciato).
    - Windows 8 o Windows Server 2012: installare l'aggiornamento del software descritto nell'articolo [2887239](https://support.microsoft.com/kb/2887239) della Microsoft Knowledge Base.

2. In tutti i computer che eseguono Windows 8.1, Windows 8, Windows Server 2012 R2 o Windows Server 2012 nei quali userai Profili utente mobili, usa l'editor del Registro di sistema oppure Criteri di gruppo per creare il valore DWORD della chiave del Registro di sistema seguente e impostarlo su `1`. Per informazioni sulla creazione delle chiavi del Registro di sistema con i Criteri di gruppo, vedere [Configurare un elemento Registro di sistema](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753092(v=ws.11)>).

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

Di seguito viene illustrato come creare un gruppo di sicurezza per Profili utente mobili:

1. Apri Server Manager in un computer in cui è installato Centro amministrativo di Active Directory.
2. Scegli **Centro amministrativo di Active Directory** dal menu **Strumenti**. Verrà visualizzato Centro amministrativo di Active Directory.
3. Fai clic con il pulsante destro del mouse sul dominio o sull'unità organizzativa appropriata, scegli **Nuovo** e quindi **Gruppo**.
4. Nella sezione **Gruppo** della finestra **Crea gruppo** specificare le impostazioni seguenti:

    - Digitare il nome del gruppo di sicurezza in **Nome gruppo**, ad esempio: **Utenti e computer profili utente mobili**.
    - In **Ambito del gruppo** seleziona **Sicurezza** e quindi **Globale**.

5. Nella sezione **Membri** seleziona **Aggiungi**. Verrà visualizzata la finestra di dialogo Selezione utenti, computer, account servizio o gruppi.
6. Per includere account computer nel gruppo di sicurezza, seleziona **Tipi di oggetto**, seleziona la casella di controllo **Computer** e quindi scegli **OK**.
7. Digita i nomi degli utenti, dei gruppi e/o dei computer ai quali vuoi distribuire Profili utente mobili, scegli **OK** e quindi di nuovo **OK**.

## <a name="step-3-create-a-file-share-for-roaming-user-profiles"></a>Passaggio 3: Creare una condivisione file per i profili utente mobili

Se non hai ancora una condivisione file separata per i profili utente mobili (indipendente da qualsiasi condivisione per le cartelle reindirizzate per impedire l'involontaria memorizzazione nella cache della cartella del profilo mobile), usa la procedura seguente per creare una condivisione file su un server che esegue Windows Server.

> [!NOTE]
> Alcune funzionalità potrebbero differire o non essere disponibili a seconda della versione di Windows Server in uso.

Di seguito viene illustrato come creare una condivisione file in Windows Server:

1. Nel riquadro di spostamento di Server Manager seleziona **Servizi file e archiviazione** e quindi **Condivisioni** per visualizzare la pagina Condivisioni.
2. Nel riquadro Condivisioni seleziona **Attività**e quindi **Nuova condivisione**. Verrà visualizzata la Creazione guidata nuova condivisione.
3. Nella pagina **Selezionare il profilo** seleziona **Condivisione SMB - rapida**. Se hai installato Gestione risorse file server e usi le proprietà di gestione delle cartelle, seleziona invece **Condivisione SMB - avanzata**.
4. Nella pagina **Percorso condivisione** selezionare il server e il volume sui quali creare la condivisione.
5. Nella pagina **Nome condivisione** digitare un nome per la condivisione (ad esempio **Profili utente$** ) nella casella **Nome condivisione**.

    > [!TIP]
    > Quando si crea la condivisione, nasconderla inserendo ```$``` dopo il nome condivisione. In tal modo, la condivisione verrà nascosta dai browser casuali.

6. Nella pagina **Altre impostazioni** deselezionare la casella di controllo **Abilita disponibilità continua**, se disponibile e selezionare facoltativamente le caselle di controllo **Abilita enumerazione basata sull'accesso** e **Crittografa accesso ai dati**.
7. Nella pagina **Autorizzazioni** seleziona **Personalizza autorizzazioni**. Viene visualizzata la finestra di dialogo Impostazioni di protezione avanzate.
8. Seleziona **Disabilita ereditarietà** e quindi **Converti autorizzazioni ereditate in autorizzazioni esplicite per questo oggetto**.
9. Imposta le autorizzazioni come descritto in [Autorizzazioni necessarie per la condivisione file che ospita i profili utente mobili](#required-permissions-for-the-file-share-hosting-roaming-user-profiles) e mostrato nello screenshot seguente, rimuovendo le autorizzazioni per i gruppi e gli account non elencati e aggiungendo autorizzazioni speciali al gruppo Utenti e computer profili utente mobili creato nel passaggio 1.
    
    ![Finestra Impostazioni di protezione avanzate che mostra le autorizzazioni come descritto nella tabella 1](media/advanced-security-user-profiles.jpg)
    
    **Figura 1** Impostare le autorizzazioni per la condivisione dei profili utente mobili
10. Se si sceglie il profilo **Condivisione SMB - avanzata** selezionare il valore Utilizzo cartelle **File utente** nella pagina **Proprietà gestione** .
11. Se si sceglie il profilo **Condivisione SMB - avanzata** nella pagina **Quota** , selezionare facoltativamente una quota da applicare agli utenti della condivisione.
12. Nella pagina **Conferma** seleziona **Crea**.

### <a name="required-permissions-for-the-file-share-hosting-roaming-user-profiles"></a>Autorizzazioni necessarie per la condivisione file che ospita i profili utente mobili

| Account utente | Accesso | Si applica a |
|   -   |   -   |   -   |
|   System    |  Controllo completo     |  Questa cartella, le sottocartelle e i file     |
|  Administrators     |  Controllo completo     |  Solo questa cartella     |
|  Autore/proprietario     |  Controllo completo     |  Solo sottocartelle e file     |
| Gruppo di sicurezza degli utenti che devono inserire dati nella condivisione (Utenti e computer profili utente mobili)      |  Visualizzazione contenuto cartella/Lettura dati *(Autorizzazioni avanzate)* <br />Creazione cartelle/Aggiunta dati *(Autorizzazioni avanzate)* |  Solo questa cartella     |
| Altri gruppi e account   |  Nessuno (rimuovere)     |       |

## <a name="step-4-optionally-create-a-gpo-for-roaming-user-profiles"></a>Passaggio 4: Creare facoltativamente un oggetto Criteri di gruppo per i profili utente mobili

Se non è già stato creato un oggetto Criteri di gruppo per le impostazioni dei profili utente mobili, usare la procedura seguente per creare un oggetto Criteri di gruppo vuoto da usare con i profili utente mobili. Questo oggetto Criteri di gruppo consente di configurare le impostazioni dei profili utente mobili (ad esempio il supporto del computer primario, che verrà discusso in separata sede) e può anche essere usato per abilitare i profili utente mobili sui computer, come di norma è possibile nella distribuzione degli ambienti di desktop virtuale o con Servizi Desktop remoto.

Di seguito viene illustrato come creare un oggetto Criteri di gruppo per Profili utente mobili:

1. Aprire Server Manager in un computer in cui è installata la console Gestione Criteri di gruppo.
2. Scegli **Gestione Criteri di gruppo** dal menu **Strumenti**. Viene visualizzato Gestione Criteri di gruppo.
3. Fai clic con il pulsante destro del mouse sul dominio o sull'unità organizzativa in cui vuoi configurare Profili utente mobili e quindi scegli **Crea un oggetto Criteri di gruppo in questo dominio e crea qui un collegamento**.
4. Nella finestra di dialogo **Nuovo oggetto Criteri di gruppo** digita un nome per l'oggetto Criteri di gruppo (ad esempio, **Impostazioni profilo utente mobile**) e quindi scegli **OK**.
5. Fare clic con il pulsante destro del mouse sull'oggetto Criteri di gruppo appena creato e quindi deselezionare la casella di controllo **Collegamento abilitato** . Ciò impedisce di applicare l'oggetto Criteri di gruppo finché non sarà terminata la configurazione.
6. Selezionare l'oggetto Criteri di gruppo. Nella sezione **Filtri di protezione** della scheda **Ambito** seleziona **Authenticated Users** e quindi scegli **Rimuovi** per impedire che l'oggetto Criteri di gruppo venga applicato a tutti.
7. Nella sezione **Filtri di protezione** scegli **Aggiungi**.
8. Nella finestra di dialogo **Seleziona utente, computer o gruppo** digita il nome del gruppo di sicurezza creato nel passaggio 1 (ad esempio, **Utenti e computer profili utente mobili**) e quindi scegli **OK**.
9. Fai clic sulla scheda **Delega**, scegli **Aggiungi**, digita **Authenticated Users**, scegli **OK** e quindi di nuovo **OK** per accettare le autorizzazioni di lettura predefinite.
    
    Questo passaggio è necessario a causa delle modifiche di sicurezza apportate nell'aggiornamento [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016).

>[!IMPORTANT]
>A causa delle modifiche di sicurezza apportate nell'aggiornamento [MS16-072A](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016), è ora necessario concedere al gruppo Authenticated Users le autorizzazioni di lettura delegate per l'oggetto Criteri di gruppo. In caso contrario, l'oggetto Criteri di gruppo non verrà applicato agli utenti o, se è già applicato, verrà rimosso e i profili utente verranno reindirizzati al PC locale. Per altre informazioni, vedi [Deploying Group Policy Security Update MS16-072](https://blogs.technet.microsoft.com/askds/2016/06/22/deploying-group-policy-security-update-ms16-072-kb3163622/) (Distribuzione dell'aggiornamento di sicurezza MS16-072 di Criteri di gruppo).

## <a name="step-5-optionally-set-up-roaming-user-profiles-on-user-accounts"></a>Passaggio 5: Configurare facoltativamente i profili utente mobili sugli account utente

Se si distribuiscono profili utente mobili agli account utente, usare la procedura seguente per specificare i profili utente mobili per gli account utente in Servizi di dominio Active Directory. Se distribuisci Profili utente mobili ai computer, come di norma è possibile per le distribuzioni di Servizi Desktop remoto o desktop virtuale, usa invece la procedura illustrata in [Passaggio 6: Configurare facoltativamente Profili utente mobili sui computer](#step-6-optionally-set-up-roaming-user-profiles-on-computers).

> [!NOTE]
> Se si configurano profili utente mobili sugli account utente usando Active Directory e sui computer usando Criteri di gruppo, l'impostazione criterio basata sul computer ha la precedenza.

Di seguito viene illustrato come configurare Profili utente mobili per gli account utente:

1. Nel Centro amministrativo di Active Directory passare al contenitore (o unità organizzativa) **Utenti** nel dominio appropriato.
2. Seleziona tutti gli utenti ai quali vuoi assegnare un profilo utente mobile, fai clic con il pulsante destro del mouse sugli utenti e quindi scegli **Proprietà**.
3. Nella sezione **Profilo** seleziona la casella di controllo **Percorso profilo** e quindi immetti il percorso della condivisione file in cui archiviare il profilo utente mobile, seguito da `%username%` (che verrà automaticamente sostituito con il nome utente al primo accesso dell'utente). Ad esempio:
    
    `\\fs1.corp.contoso.com\User Profiles$\%username%`
    
    Per specificare un profilo utente mobile obbligatorio, specifica il percorso del file NTuser.man creato in precedenza, ad esempio `fs1.corp.contoso.comUser Profiles$default`. Per altre informazioni, vedi [Creare profili utente obbligatori](https://docs.microsoft.com/windows/client-management/mandatory-user-profile).
4. Seleziona **OK**.

> [!NOTE]
> Per impostazione predefinita, la distribuzione di tutte le app basate su Windows® Runtime (Windows Store) è consentita durante l'uso dei profili utente mobili. Quando però si usa un profilo speciale, le app non vengono distribuite per impostazione predefinita. I profili speciali sono profili utente in cui le modifiche vengono rimosse subito dopo la disconnessione dell'utente:
> <br><br>Per rimuovere le limitazioni alla distribuzione delle app per i profili speciali, abilitare l'impostazione criterio **Allow deployment operations in special profiles** (nel percorso Configurazione computer\Criteri\Modelli amministrativi\Componenti di Windows\Distribuzione di pacchetti di app). Le app distribuite in questo scenario lasceranno comunque alcuni dati archiviati sul computer, che potrebbero accumularsi, ad esempio, se un unico computer è usato da centinaia di utenti. Per pulire le app, trova o sviluppa uno strumento che usi l'API [CleanupPackageForUserAsync](https://msdn.microsoft.com/library/windows/apps/windows.management.deployment.packagemanager.cleanuppackageforuserasync.aspx) per la pulizia dei pacchetti di app per gli utenti che non hanno più un profilo sul computer.
> <br><br>Per altre informazioni di carattere generale sulle app di Windows Store vedere [Gestisci l'accesso client a Windows Store](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh832040(v=ws.11)>).

## <a name="step-6-optionally-set-up-roaming-user-profiles-on-computers"></a>Passaggio 6: Configurare facoltativamente i profili utente mobili sui computer

Se si distribuiscono profili utente mobili ai computer, come di norma è possibile per le distribuzioni di Servizi Desktop remoto o desktop virtuale, usare la procedura seguente. Se distribuisci Profili utente mobili agli account utente, usa invece la procedura descritta in [Passaggio 5: Configurare facoltativamente Profili utente mobili sugli account utente](#step-5-optionally-set-up-roaming-user-profiles-on-user-accounts).

Per applicare Profili utente mobili ai computer che eseguono Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008, puoi usare Criteri di gruppo.

> [!NOTE]
> Se si configurano profili utente mobili sui computer usando Criteri di gruppo e sugli account utente usando Active Directory, l'impostazione criterio basata sul computer ha la precedenza.

Di seguito viene illustrato come configurare Profili utente mobili sui computer:

1. Aprire Server Manager in un computer in cui è installata la console Gestione Criteri di gruppo.
2. Scegli **Gestione Criteri di gruppo** dal menu **Strumenti**. Verrà visualizzata la finestra Gestione Criteri di gruppo.
3. In Gestione Criteri di gruppo fai clic con il pulsante destro del mouse sull'oggetto Criteri di gruppo creato nel passaggio 3 (ad esempio, **Impostazioni dei profili utente mobili**) e quindi scegli **Modifica**.
4. Nella finestra Editor Gestione Criteri di gruppo passare a **Configurazione computer**, **Criteri**, **Modelli amministrativi**, **Sistema** e quindi **Profili utente**.
5. Fai clic con il pulsante destro del mouse su **Imposta percorso profilo mobile per tutti gli utenti che accedono al computer** e quindi scegli **Modifica**.
    > [!TIP]
    > La home directory di un utente, se configurata, è la cartella predefinita usata da alcuni programmi come Windows PowerShell. È possibile configurare un percorso di rete o locale alternativo in base al numero di utenti con la sezione **Home directory** delle proprietà dell'account utente in Servizi di dominio Active Directory. Per configurare il percorso della home directory per tutti gli utenti di un computer che esegue Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 in un ambiente di desktop virtuale, abilita l'impostazione criterio **Imposta la home directory utente** e quindi specifica la condivisione file e la lettera di unità di cui eseguire il mapping (oppure specifica una cartella locale). Non usare variabili di ambiente o puntini di sospensione. L'alias dell'utente viene aggiunto alla fine del percorso specificato durante l'accesso dell'utente.
6. Nella finestra di dialogo **Proprietà** seleziona **Abilitato**.
7. Nella casella **Gli utenti che accedono al computer devono utilizzare il percorso del profilo mobile seguente** immetti il percorso della condivisione file in cui vuoi archiviare il profilo utente mobile, seguito da `%username%` (che verrà automaticamente sostituito con il nome utente al primo accesso dell'utente). Ad esempio:

    `\\fs1.corp.contoso.com\User Profiles$\%username%`

    Per specificare un profilo utente mobile obbligatorio, cioè un profilo preconfigurato al quale gli utenti non possono apportare modifiche definitive (gli elementi modificati vengono ripristinati alla disconnessione dell'utente), specifica il percorso del file NTuser.man creato in precedenza, ad esempio `\\fs1.corp.contoso.com\User Profiles$\default`. Per altre informazioni, vedere [Creazione di un profilo utente obbligatorio](https://docs.microsoft.com/windows/client-management/mandatory-user-profile).
8. Seleziona **OK**.

## <a name="step-7-optionally-specify-a-start-layout-for-windows-10-pcs"></a>Passaggio 7: Specificare facoltativamente un layout Start per i computer Windows 10

Usando Criteri di gruppo, puoi applicare uno specifico layout per il menu Start, in modo che gli utenti visualizzino lo stesso layout Start? su tutti i computer. Se gli utenti effettuano l'accesso a più di un computer e vuoi che il layout Start sia coerente su tutti i PC, assicurati che l'oggetto Criteri di gruppo venga applicato a tutti i loro computer.

Per specificare un layout Start, esegui queste operazioni:

1. Aggiorna i computer Windows 10 a Windows 10 versione 1607 (noto anche come Anniversary Update) o versioni successive e installa l'aggiornamento cumulativo del 14 marzo 2017 ([KB4013429](https://support.microsoft.com/kb/4013429)) o una versione successiva.
2. Crea un file XML di layout del menu Start completo o parziale. A tale scopo, vedi [Personalizzare ed esportare il layout della schermata Start](https://docs.microsoft.com/windows/configuration/customize-and-export-start-layout).
    * Se specifichi un layout Start *completo*, un utente non può personalizzare alcuna parte del menu Start. Se specifichi un layout Start *parziale*, gli utenti possono personalizzare tutti gli elementi, tranne i gruppi di riquadri bloccati che imposti. Tuttavia, con un layout Start parziale, per le personalizzazioni apportate dall'utente al menu Start non verrà effettuato il roaming in altri computer.
3. Usa Criteri di gruppo per applicare il layout Start personalizzato all'oggetto Criteri di gruppo creato per Profili utente mobili. A tale scopo, vedi [Usare Criteri di gruppo per applicare un layout personalizzato della schermata Start in un dominio](https://docs.microsoft.com/windows/configuration/customize-windows-10-start-screens-by-using-group-policy#bkmk-domaingpodeployment).
4. Usa Criteri di gruppo per impostare il valore del Registro di sistema seguente nei computer Windows 10. A tale scopo, vedi [Configurare un elemento del Registro di sistema](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753092(v=ws.11)>).

| **Azione**   | **Aggiornamento**                  |
| ------------ | ------------                |
| Hive         | **HKEY_LOCAL_MACHINE**      |
| Percorso chiave     | **Software\Microsoft\Windows\CurrentVersion\Explorer** |
| Nome del valore   | **SpecialRoamingOverrideAllowed** |
| Tipo di valore   | **REG_DWORD**               |
| Dati valore   | **1** (oppure **0** per disabilitare) |
| Base         | **Decimale**                 |

5. (Facoltativo) Abilita le ottimizzazioni del primo accesso per velocizzare l'accesso per gli utenti. A tale scopo, vedi [Applicare criteri per migliorare i tempi di accesso](https://docs.microsoft.com/windows/client-management/mandatory-user-profile#apply-policies-to-improve-sign-in-time).
6. (Facoltativo) Riduci ulteriormente i tempi di accesso rimuovendo le app non necessarie dall'immagine di base di Windows 10 usata per distribuire i PC client. Windows Server 2019 e Windows Server 2016 non dispongono di app con provisioning preliminare, quindi puoi ignorare questo passaggio sulle immagini server.
    - Per rimuovere le app, usa il cmdlet [Remove-AppxProvisionedPackage](https://docs.microsoft.com/powershell/module/dism/remove-appxprovisionedpackage?view=win10-ps) in Windows PowerShell per disinstallare le applicazioni elencate di seguito. Se i computer sono già stati distribuiti, puoi creare uno script per la rimozione di queste app tramite [Remove-AppxPackage](https://docs.microsoft.com/powershell/module/appx/remove-appxpackage?view=win10-ps).
    
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
>La disinstallazione di queste app consente di ridurre i tempi di accesso, ma puoi lasciarle installate se la distribuzione le richiede.

## <a name="step-8-enable-the-roaming-user-profiles-gpo"></a>Passaggio 8: Abilitare l'oggetto Criteri di gruppo dei profili utente mobili

Se si configurano i profili utente mobili sui computer usando i Criteri di gruppo oppure se sono state personalizzate altre impostazioni dei profili utente mobili usando i Criteri di gruppo, il passaggio successivo consiste nell'abilitazione dell'oggetto Criteri di gruppo, consentendone l'applicazione agli utenti interessati.

>[!TIP]
>Se intendi implementare il supporto dei computer primari, è consigliabile procedere ora, prima di abilitare l'oggetto Criteri di gruppo. In tal modo, i dati utente non verranno copiati nei computer non primari prima di aver abilitato il supporto del computer primario. Per le impostazioni criterio specifiche, vedi [Distribuire i computer primari per Reindirizzamento cartelle e Profili utente mobili](deploy-primary-computers.md).

Di seguito viene illustrato come abilitare l'oggetto Criteri di gruppo di Profili utente mobili:

1. Aprire Gestione Criteri di gruppo.
2. Fai clic con il pulsante destro del mouse sull'oggetto Criteri di gruppo creato e scegli **Collegamento abilitato**. Accanto alla voce del menu viene visualizzata una casella di controllo.

## <a name="step-9-test-roaming-user-profiles"></a>Passaggio 9: Testare i profili utente mobili

Per testare i profili utente mobili, accedere a un computer con un account utente configurato per i profili utente mobili oppure accedere a un computer configurato per i profili utente mobili. Confermare quindi che il profilo è reindirizzato.

Di seguito viene illustrato come testare Profili utente mobili:

1. Accedere a un computer primario (se è stato abilitato il supporto per il computer primario) con un account utente per il quale sono stati abilitati i profili utente mobili. Se sono stati abilitati i profili utente mobili su computer specifici, accedere a uno di questi.
2. Se l'utente ha precedentemente eseguito l'accesso al computer, aprire un prompt dei comandi con privilegi elevati e quindi digitare il comando seguente per assicurarsi che vengano applicate le impostazioni Criteri di gruppo più recenti al computer client:
    
    ```PowerShell
    GpUpdate /Force
    ```
3. Per verificare l'esecuzione del roaming del profilo utente, apri il **Pannello di controllo**, seleziona **Sistema e sicurezza**, **Sistema**, **Impostazioni di sistema avanzate**, **Impostazioni** nella sezione Profili utente e quindi cerca **Roaming** nella colonna **Tipo**.

## <a name="appendix-a-checklist-for-deploying-roaming-user-profiles"></a>Appendice A: Elenco di controllo per la distribuzione dei profili utente mobili

| Stato                     | Azione                                                |
| ---                        | ------                                                |
| ☐<br>☐<br>☐<br>☐<br>☐   | 1. Preparare il dominio<br>- Aggiungi i computer al dominio<br>- Abilita l'uso di versioni di profili separate<br>- Crea gli account utente<br>- (Facoltativo) Distribuisci Reindirizzamento cartelle |
| ☐<br><br><br>             | 2. Creare un gruppo di sicurezza per i profili utente mobili<br>- Nome gruppo:<br>- Membri: |
| ☐<br><br>                 | 3. Creare una condivisione file per i profili utente mobili<br>- Nome condivisione file: |
| ☐<br><br>                 | 4. Creare un oggetto Criteri di gruppo per i profili utente mobili<br>- Nome oggetto Criteri di gruppo:|
| ☐                         | 5. Configurare le impostazioni criterio dei profili utente mobili    |
| ☐<br>☐<br>☐              | 6. Abilitare Profili utente mobili<br>- Funzionalità abilitata in Active Directory Domain Services per gli account utente?<br>- Funzionalità abilitata in Criteri di gruppo per gli account computer?<br> |
| ☐                         | 7. (Facoltativo) Specificare un layout Start obbligatorio per i computer Windows 10 |
| ☐<br>☐<br><br>☐<br><br>☐ | 8. (Facoltativo) Abilitare il supporto dei computer primari<br>- Designa i computer primari per gli utenti<br>- Percorso dei mapping di utente e computer primario:<br>- (Facoltativo) Abilita il supporto dei computer primari per Reindirizzamento cartelle<br>- Basato su computer o su utente?<br>- (Facoltativo) Abilita il supporto dei computer primari per Profili utente mobili |
| ☐                        | 9. Abilitare l'oggetto Criteri di gruppo dei profili utente mobili                |
| ☐                        | 10. Testare i profili utente mobili                         |

## <a name="appendix-b-profile-version-reference-information"></a>Appendice B: Informazioni di riferimento relative alla versione del profilo

Ogni profilo ha una versione che corrisponde approssimativamente alla versione di Windows in cui viene usato il profilo. Ad esempio, le versioni 1703 e 1607 di Windows 10 usano entrambe la versione .V6 del profilo. Microsoft crea una nuova versione del profilo solo quando è necessario per mantenere la compatibilità, motivo per cui non tutte le versioni di Windows includono una nuova versione del profilo.

Nella tabella seguente è indicato il percorso dei profili utente mobili su varie versioni di Windows.

| Versione del sistema operativo | Percorso del profilo utente mobile |
| --- | --- |
| Windows XP e Windows Server 2003 | ```\\<servername>\<fileshare>\<username>``` |
| Windows Vista e Windows Server 2008 | ```\\<servername>\<fileshare>\<username>.V2``` |
| Windows 7 e Windows Server 2008 R2 | ```\\<servername>\<fileshare>\<username>.V2``` |
| Windows 8 e Windows Server 2012 | ```\\<servername>\<fileshare>\<username>.V3``` (dopo aver applicato l'aggiornamento software e la chiave del Registro di sistema)<br>```\\<servername>\<fileshare>\<username>.V2``` (prima di applicare l'aggiornamento software e la chiave del Registro di sistema) |
| Windows 8.1 e Windows Server 2012 R2 | ```\\<servername>\<fileshare>\<username>.V4``` (dopo aver applicato l'aggiornamento software e la chiave del Registro di sistema)<br>```\\<servername>\<fileshare>\<username>.V2``` (prima di applicare l'aggiornamento software e la chiave del Registro di sistema) |
| Windows 10 | ```\\<servername>\<fileshare>\<username>.V5``` |
| Windows 10, versione 1703 e versione 1607 | ```\\<servername>\<fileshare>\<username>.V6``` |

## <a name="appendix-c-working-around-reset-start-menu-layouts-after-upgrades"></a>Appendice C: Ovviare alla reimpostazione dei layout del menu Start dopo gli aggiornamenti

Ecco alcuni modi per ovviare alla reimpostazione dei layout del menu Start dopo un aggiornamento sul posto:

- Se un solo utente usa il dispositivo e l'amministratore IT usa una strategia di distribuzione del sistema operativo gestita, ad esempio Gestione configurazione, sono consentite queste operazioni:
    
  1. Esportare il layout del menu Start con Export-Startlayout prima dell'aggiornamento 
  2. Importare il layout del menu Start con Import-StartLayout dopo la configurazione guidata, ma prima dell'accesso dell'utente  
 
     > [!NOTE] 
     > L'importazione di un layout Start comporta la modifica del profilo utente predefinito. Tutti i profili utente creati dopo l'importazione riceveranno il layout Start importato.
 
- Gli amministratori IT possono scegliere di gestire il layout Start con Criteri di gruppo. L'uso di Criteri di gruppo costituisce una soluzione di gestione centralizzata per applicare agli utenti un layout Start standardizzato. Per usare Criteri di gruppo per la gestione del menu Start sono disponibili due modalità: blocco completo e blocco parziale. Lo scenario di blocco completo impedisce all'utente di apportare modifiche al layout Start. Lo scenario di blocco parziale invece consente all'utente di apportare modifiche a un'area specifica del menu Start. Per altre informazioni, vedi [Personalizzare ed esportare il layout della schermata Start](https://docs.microsoft.com/windows/configuration/customize-and-export-start-layout).
        
   > [!NOTE]
   > Le modifiche apportate dall'utente nello scenario di blocco parziale andranno comunque perse durante l'aggiornamento.

- È possibile lasciare che il layout Start venga reimpostato e consentire agli utenti finali di riconfigurarlo. È possibile inviare un messaggio di posta elettronica o un'altra notifica agli utenti finali per informarli di attendere il ripristino dei layout Start dopo l'aggiornamento del sistema operativo per ridurne al minimo le ripercussioni negative. 

## <a name="change-history"></a>Cronologia delle modifiche

La tabella seguente include un riepilogo di alcune delle più importanti modifiche apportate a questo argomento.

| Data | Descrizione |Motivo|
| --- | ---         | ---   |
| 1° maggio 2019 | Aggiunta di aggiornamenti per Windows Server 2019 |
| 10 aprile 2018 | Aggiunta di una discussione sulla tempistica con cui le personalizzazioni dell'utente per il menu Start vanno perse dopo un aggiornamento sul posto del sistema operativo|Problema noto di callout. |
| 13 marzo 2018 | Aggiornamento per Windows Server 2016 | Spostamento dalla raccolta relativa alle versioni precedenti e aggiornamento per la versione corrente di Windows Server. |
| 13 aprile 2017 | Aggiunta di informazioni sul profilo per Windows 10 versione 1703 e spiegazione del funzionamento delle versioni del profilo mobile in caso di aggiornamento dei sistemi operativi. Vedi [Considerazioni sull'uso di profili utente mobili su più versioni di Windows](#considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows). | Feedback dei clienti. |
| 14 marzo 2017 | Aggiunta di un passaggio facoltativo per specificare un layout Start obbligatorio per i computer Windows 10 in [Appendice A: Elenco di controllo per la distribuzione dei profili utente mobili](#appendix-a-checklist-for-deploying-roaming-user-profiles). |Modifiche relative alle funzionalità nell'aggiornamento più recente di Windows. |
| 23 gennaio 2017 | Aggiunta di un passaggio alla sezione [Passaggio 4: Creare facoltativamente un oggetto Criteri di gruppo per Profili utente mobili](#step-4-optionally-create-a-gpo-for-roaming-user-profiles) per delegare le autorizzazioni di lettura agli utenti autenticati, operazione ora necessaria a seguito di un aggiornamento di sicurezza di Criteri di gruppo.|Modifiche di sicurezza relative all'elaborazione di Criteri di gruppo. |
| 29 dicembre 2016 | Aggiunta di un collegamento in [Passaggio 8: Abilitare l'oggetto Criteri di gruppo dei profili utente mobili](#step-8-enable-the-roaming-user-profiles-gpo) per semplificare il recupero di informazioni su come impostare Criteri di gruppo per i computer primari. Correzione di alcuni riferimenti ai passaggi 5 e 6 che avevano numeri errati.|Feedback dei clienti. |
| 5 dicembre 2016 | Aggiunta di informazioni che illustrano un problema di roaming relativo alle impostazioni del menu Start. | Feedback dei clienti. |
| 6 luglio 2016 | Aggiunta dei suffissi della versione del profilo di Windows 10 in [Appendice B: Informazioni di riferimento relative alla versione del profilo](#appendix-b-profile-version-reference-information). Rimozione di Windows XP e Windows Server 2003 dall'elenco dei sistemi operativi supportati. | Aggiornamenti per le nuove versioni di Windows e rimozione delle informazioni sulle versioni di Windows che non sono più supportate. |
| 7 luglio 2015 | Aggiunta di requisiti e passaggi per disabilitare la disponibilità continua quando si usa un file server in cluster. | Le condivisioni di file in cluster offrono migliori prestazioni per scritture di piccola entità (tipiche con i profili utente mobili) quando la disponibilità continua è disabilitata. |
| 19 marzo 2014 | Uso delle lettere maiuscole nei suffissi della versione del profilo (.V2, .V3, .V4) in [Appendice B: Informazioni di riferimento relative alla versione del profilo](#appendix-b-profile-version-reference-information). | Nonostante Windows non operi distinzione tra maiuscole e minuscole, se usi NFS con la condivisione file, è importante indicare la corretta grafia (maiuscole) del suffisso del profilo. |
| 9 ottobre 2013 | Revisione per Windows Server 2012 R2 e Windows 8.1, spiegazione di alcuni elementi e aggiunta delle sezioni [Considerazioni sull'uso di profili utente mobili su più versioni di Windows](#considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows) e [Appendice B: Informazioni di riferimento relative alla versione del profilo](#appendix-b-profile-version-reference-information). | Aggiornamenti per la nuova versione; feedback dei clienti. |

## <a name="more-information"></a>Altre informazioni

- [Distribuire Reindirizzamento cartelle, File offline e Profili utente mobili](deploy-folder-redirection.md)
- [Distribuire i computer primari per Reindirizzamento cartelle e Profili utente mobili](deploy-primary-computers.md)
- [Implementazione della gestione stato utente](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc784645(v=ws.10)>)
- [Dichiarazione Microsoft a supporto della replica dei dati del profilo utente](https://blogs.technet.microsoft.com/askds/2010/09/01/microsofts-support-statement-around-replicated-user-profile-data/)
- [Trasferire localmente le app con Gestione e manutenzione immagini distribuzione](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh852635(v=win.10)>)
- [Risoluzione dei problemi di creazione di pacchetti, distribuzione e query delle app basate su Windows Runtime](https://msdn.microsoft.com/library/windows/desktop/hh973484.aspx)