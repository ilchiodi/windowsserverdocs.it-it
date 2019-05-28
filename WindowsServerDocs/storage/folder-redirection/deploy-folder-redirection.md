---
title: Distribuire reindirizzamento cartelle con i file Offline
description: Come usare Windows Server per distribuire il reindirizzamento delle cartelle con i file Offline nei computer client Windows.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 2bb15d5ae29da6c9dbcd6b58af280026d06febc8
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222740"
---
# <a name="deploy-folder-redirection-with-offline-files"></a>Distribuire reindirizzamento cartelle con i file Offline

>Si applica a: Windows 10, Windows 7, Windows 8, Windows 8.1, Windows Vista, Windows Server 2019, Windows Server 2016, Windows Server (canale semestrale), Windows Server 2012, Windows Server 2012 R2, Windows Server 2008 R2

Questo argomento descrive come usare Windows Server per distribuire il reindirizzamento delle cartelle con i file Offline nei computer client Windows.

Per un elenco delle modifiche recenti apportate a questo argomento, vedere [cronologia modifiche](#change-history).

>[!IMPORTANT]
>A causa della protezione modifiche apportate [MS16 072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016), sono stati aggiornati [passaggio 3: Creare un oggetto Criteri di gruppo per Reindirizzamento cartelle](#step-3-create-a-gpo-for-folder-redirection) di questo argomento in modo che Windows possa applicare correttamente i criteri di reindirizzamento cartelle (e non verranno ripristinate le cartelle reindirizzate nei computer interessati).

## <a name="prerequisites"></a>Prerequisiti

### <a name="hardware-requirements"></a>Requisiti hardware

Il reindirizzamento delle cartelle richiede un computer basato su x86 o x64. non è supportato Windows® RT

### <a name="software-requirements"></a>Requisiti software

Il reindirizzamento delle cartelle presenta i requisiti software seguenti:

- Per amministrare il reindirizzamento delle cartelle, è necessario essere connessi come membro del gruppo di sicurezza amministratori di dominio, il gruppo di sicurezza amministratori dell'organizzazione o il gruppo di sicurezza Group Policy Creator Owners.
- I computer client devono eseguire Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2019, Windows Server 2016, Windows Server (canale semestrale), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008.
- I computer client devono essere aggiunti ai Servizi di dominio Active Directory in corso di gestione.
- Deve essere disponibile un computer in cui siano installati l'editor Gestione Criteri di gruppo e il Centro amministrativo di Active Directory.
- Un file server deve essere disponibile per ospitare cartelle reindirizzate.
    - Se la condivisione file usa gli spazi dei nomi DFS, le cartelle DFS (collegamenti) devono avere un'unica destinazione per impedire agli utenti di apportare modifiche conflittuali su server differenti.
    - Se la condivisione file usa la Replica DFS per replicare i contenuti con un altro server, gli utenti devono poter accedere all'unico server di origine per impedire agli utenti di apportare modifiche conflittuali su server differenti.
    - Quando si usa una condivisione file cluster, disabilitare la disponibilità continua sulla condivisione file per evitare problemi di prestazioni con il reindirizzamento delle cartelle e file Offline. Inoltre, i file Offline potrebbe non passi alla modalità offline per 3-6 minuti dopo che un utente perde l'accesso a una condivisione file a disponibilità continua, che ciò può risultare frustrante gli utenti che non usano ancora la modalità sempre Offline dei file Offline.

>[!NOTE]
>Alcune funzionalità più recenti in Reindirizzamento cartelle hanno computer client aggiuntivi e requisiti dello schema di Active Directory. Per altre informazioni, vedi [distribuire computer primari](deploy-primary-computers.md), [Disabilita file Offline su cartelle](disable-offline-files-on-folders.md), [abilitare la modalità sempre Offline](enable-always-offline.md), e [Enable con ottimizzazione per la cartella lo spostamento ](enable-optimized-moving.md).

## <a name="step-1-create-a-folder-redirection-security-group"></a>Passaggio 1: Creare un gruppo di sicurezza di reindirizzamento cartelle

Se l'ambiente non è già configurato con il reindirizzamento delle cartelle, il primo passaggio consiste nel creare un gruppo di sicurezza che contiene tutti gli utenti a cui si desidera applicare le impostazioni dei criteri di reindirizzamento cartelle.

Di seguito viene illustrato come creare un gruppo di sicurezza per il reindirizzamento delle cartelle:

1. Aprire Server Manager in un computer con il centro di amministrazione di Active Directory installati.
2. Nel **degli strumenti** dal menu **centro di amministrazione di Active Directory**. Verrà visualizzato Centro amministrativo di Active Directory.
3. Fare doppio clic il dominio appropriato o unità Organizzativa, selezionare **New**, quindi selezionare **gruppo**.
4. Nella sezione **Gruppo** della finestra **Crea gruppo** specificare le impostazioni seguenti:
    - Digitare il nome del gruppo di sicurezza in **Nome gruppo**, ad esempio: **Gli utenti il reindirizzamento delle cartelle**.
    - Nella **ambito del gruppo**, selezionare **Security**, quindi selezionare **Global**.
5. Nel **membri** sezione, selezionare **Add**. Verrà visualizzata la finestra di dialogo Selezione utenti, computer, account servizio o gruppi.
6. Digitare i nomi degli utenti o gruppi a cui si vuole distribuire reindirizzamento cartelle, selezionare **OK**, quindi selezionare **OK** nuovamente.

## <a name="step-2-create-a-file-share-for-redirected-folders"></a>Passaggio 2: Creare una condivisione file per le cartelle reindirizzate

Se si dispone già di una condivisione file per le cartelle reindirizzate, usare la procedura seguente per creare una condivisione file in un server che esegue Windows Server 2012.

>[!NOTE]
>Alcune funzionalità potrebbero differire o non essere disponibili se si crea la condivisione file su un server che esegue un'altra versione di Windows Server.

Di seguito viene illustrato come creare una condivisione file in Windows Server 2012, Windows Server 2016 e Windows Server 2019:

1. Nel riquadro di spostamento di Server Manager, selezionare **servizi File e archiviazione**, quindi selezionare **condivisioni** per visualizzare la pagina condivisioni.
2. Nel **condivisioni** riquadro, seleziona **attività**, quindi selezionare **nuova condivisione**. Verrà visualizzata la Creazione guidata nuova condivisione.
3. Nel **selezionare il profilo** pagina, selezionare **condivisione SMB-rapida**. Se si dispone di gestione risorse File Server installato e si usano le proprietà di gestione di cartella, selezionare invece **condivisione SMB - avanzata**.
4. Nella pagina **Percorso condivisione** selezionare il server e il volume sui quali creare la condivisione.
5. Nel **nome condivisione** , digitare un nome per la condivisione (, ad esempio, **utenti$** ) nel **nome condivisione** casella.
    >[!TIP]
    >Quando si crea la condivisione, nasconderla inserendo ```$``` dopo il nome condivisione. Questa nasconderà la condivisione nascosta dai browser casuali.
6. Nel **altre impostazioni** pagina, deselezionare la casella di controllo Abilita disponibilità continua, se presente e, facoltativamente, selezionare la **Abilita enumerazione basata sull'accesso** e **crittografa accesso ai dati** caselle di controllo.
7. Nel **le autorizzazioni** pagina, selezionare **personalizzare le autorizzazioni...** . Viene visualizzata la finestra di dialogo Impostazioni di protezione avanzate.
8. Selezionare **Disabilita ereditarietà**, quindi selezionare **Converti autorizzazioni ereditate in autorizzazioni esplicite per questo oggetto**.
9. Impostare le autorizzazioni come descritto nella tabella 1 e mostrato in figura 1, la rimozione delle autorizzazioni per gli account e gruppi rimossi dall'elenco e l'aggiunta di autorizzazioni speciali al gruppo Users il reindirizzamento della cartella creata nel passaggio 1.
    
    ![Impostazione delle autorizzazioni per la condivisione di cartelle reindirizzate](media/deploy-folder-redirection/setting-the-permissions-for-the-redirected-folders-share.png)
    
    **Figura 1** impostano le autorizzazioni per le cartelle reindirizzate condividono
10. Se si sceglie il profilo **Condivisione SMB - avanzata** selezionare il valore Utilizzo cartelle **File utente** nella pagina **Proprietà gestione** .
11. Se si sceglie il profilo **Condivisione SMB - avanzata** nella pagina **Quota** , selezionare facoltativamente una quota da applicare agli utenti della condivisione.
12. Nel **conferma** pagina, selezionare **Create.**

### <a name="required-permissions-for-the-file-share-hosting-redirected-folders"></a>Le autorizzazioni necessarie per il file di condividono che ospita le cartelle reindirizzate


|Account utente  |Accesso  |Si applica a  |
|---------|---------|---------|
| Account utente | Accesso | Si applica a |
|Sistema     | Controllo completo        |    Questa cartella, le sottocartelle e i file     |
|Administrators     | Controllo completo       | Solo questa cartella        |
|Autore/proprietario     |   Controllo completo      |   Solo sottocartelle e file      |
|Gruppo di sicurezza degli utenti che devono inserire dati nella condivisione (utenti di reindirizzamento cartelle)     |   Elenco cartelle / lettura dati *(autorizzazioni avanzate)* <br /><br />Creazione cartelle / Aggiunta dati *(autorizzazioni avanzate)* <br /><br />Leggere gli attributi *(autorizzazioni avanzate)* <br /><br />Lettura attributi estesi *(autorizzazioni avanzate)* <br /><br />Le autorizzazioni di lettura *(autorizzazioni avanzate)*      |  Solo questa cartella       |
|Altri gruppi e account     |  Nessuno (rimuovere)       |         |

## <a name="step-3-create-a-gpo-for-folder-redirection"></a>Passaggio 3: Creare un oggetto Criteri di gruppo per Reindirizzamento cartelle

Se si dispone già di un oggetto Criteri di gruppo creato per le impostazioni di reindirizzamento cartelle, utilizzare la procedura seguente per crearne uno.

Di seguito viene illustrato come creare un oggetto Criteri di gruppo per Reindirizzamento cartelle:

1. Aprire Server Manager in un computer in cui è installata la console Gestione Criteri di gruppo.
2. Dal **degli strumenti** dal menu **Gestione criteri di gruppo**.
3. Fare doppio clic il dominio o unità Organizzativa in cui si desidera configurare il reindirizzamento delle cartelle, quindi selezionare **crea un oggetto Criteri di gruppo in questo dominio e crea qui un collegamento**.
4. Nel **nuovo GPO** della finestra di dialogo digitare un nome per l'oggetto Criteri di gruppo (ad esempio, **impostazioni di reindirizzamento cartelle**) e quindi selezionare **OK**.
5. Fare clic con il pulsante destro del mouse sull'oggetto Criteri di gruppo appena creato e quindi deselezionare la casella di controllo **Collegamento abilitato** . Ciò impedisce di applicare l'oggetto Criteri di gruppo finché non sarà terminata la configurazione.
6. Selezionare l'oggetto Criteri di gruppo. Nel **filtri di sicurezza** sezione il **ambito** scheda, seleziona **Authenticated Users**e quindi selezionare **rimuovere** per evitare che l'oggetto Criteri di gruppo di essere applicata a tutti gli utenti.
7. Nel **filtri di sicurezza** sezione, selezionare **Add**.
8. Nel **Seleziona utenti, Computer o gruppo** finestra di dialogo, digitare il nome di sicurezza gruppo creato nel passaggio 1 (ad esempio, **agli utenti di reindirizzamento cartelle**) e quindi selezionare **OK**.
9. Selezionare il **delega** scheda, seleziona **Add**, digitare **Authenticated Users**, selezionare **OK**e quindi selezionare **OK** nuovamente per accettare il valore predefinito di autorizzazioni di lettura.
    
    Questo passaggio è necessario a causa di modifiche di sicurezza apportate [MS16 072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016).

>[!IMPORTANT]
>A causa della protezione modifiche apportate [MS16 072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016), è ora necessario assegnare le autorizzazioni di lettura del gruppo delegata Authenticated Users per l'oggetto Criteri di gruppo reindirizzamento cartelle, in caso contrario, oggetto Criteri di gruppo non vengano applicate a utenti o se è già stato applicato, l'oggetto Criteri di gruppo è rimosso, il reindirizzamento cartelle nuovamente al computer locale. Per altre informazioni, vedi [distribuzione gruppo di criteri di sicurezza aggiornamento MS16-072](https://blogs.technet.microsoft.com/askds/2016/06/22/deploying-group-policy-security-update-ms16-072-kb3163622/).

## <a name="step-4-configure-folder-redirection-with-offline-files"></a>Passaggio 4: Configurare il reindirizzamento delle cartelle con i file Offline

Dopo aver creato un oggetto Criteri di gruppo per le impostazioni di reindirizzamento cartelle, modificare le impostazioni di criteri di gruppo per abilitare e configurare il reindirizzamento delle cartelle, come descritto nella procedura seguente.

>[!NOTE]
>I file offline è abilitata per impostazione predefinita per le cartelle reindirizzate nei computer client Windows e disabilitato nei computer che esegue Windows Server, a meno che non modificate dall'utente. Per usare i criteri di gruppo per controllare se i file Offline è abilitato, usare il **Consenti o impedisce l'uso della funzionalità file Offline** impostazione dei criteri.
> Per informazioni su alcune delle altre impostazioni file Offline i criteri di gruppo, vedere [abilitare Advanced non in linea i file di funzionalità](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn270369(v%3dws.11)>), e [configurazione di criteri di gruppo per file Offline](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc759721(v%3dws.10)>).

Di seguito viene illustrato come configurare il reindirizzamento delle cartelle in Criteri di gruppo:

1. In Gestione criteri di gruppo, fare doppio clic su oggetto Criteri di gruppo è stato creato (ad esempio, **impostazioni di reindirizzamento cartelle**), quindi selezionare **modificare**.
2. Nella finestra Editor Gestione criteri di gruppo, passare a **User Configuration**, quindi **criteri**, quindi **impostazioni di Windows**e quindi **cartella Il reindirizzamento**.
3. Fare doppio clic su una cartella che si desidera reindirizzare (ad esempio, **documenti**), quindi selezionare **proprietà**.
4. Nel **delle proprietà** della finestra di dialogo dal **impostazione** , quindi selezionare **base - reindirizza la cartella comune nello stesso percorso**.

    > [!NOTE]
    > Per applicare il reindirizzamento delle cartelle per i computer client che eseguono Windows XP o Windows Server 2003, selezionare la **le impostazioni** scheda e selezionare il **Applica criterio di reindirizzamento per Windows 2000, Windows 2000 Server, Windows XP, e Sistemi operativi Windows Server 2003** casella di controllo.
5. Nel **percorso cartella di destinazione** sezione, selezionare **creare una cartella per ogni utente nel percorso radice** e quindi il **percorso radice** , digitare il percorso per l'archiviazione di condivisione file reindirizzamento cartelle, ad esempio:  **\\ \\fs1.corp.contoso.com\\utenti$**
6. Selezionare il **le impostazioni** scheda e nella **rimozione dei criteri** sezione, selezionare facoltativamente **reindirizza la cartella al percorso del profilo utente locale quando il criterio viene rimosso** (questa impostazione facilitano il reindirizzamento cartelle si comportano in modo più prevedibile per adminisitrators e gli utenti).
7. Selezionare **OK**, quindi selezionare **Yes** nella finestra di dialogo di avviso.

## <a name="step-5-enable-the-folder-redirection-gpo"></a>Passaggio 5: Abilitare il reindirizzamento della cartella oggetti Criteri di gruppo

Dopo aver completato la configurazione delle impostazioni criteri di gruppo reindirizzamento cartelle, il passaggio successivo è abilitare l'oggetto Criteri di gruppo, consentendone l'applicati agli utenti interessati.

>[!TIP]
>È consigliabile implementare il supporto del computer primario o altre impostazioni criterio prima di abilitare l'oggetto Criteri di gruppo. In tal modo, i dati utente non verranno copiati nei computer non primari prima di aver abilitato il supporto del computer primario.

Di seguito viene illustrato come abilitare l'oggetto Criteri di gruppo reindirizzamento cartelle:

1. Aprire Gestione Criteri di gruppo.
2. Fare doppio clic su oggetto Criteri di gruppo creato e quindi selezionare **collegamento abilitato**. Verrà visualizzata una casella di controllo accanto alla voce di menu.

## <a name="step-6-test-folder-redirection"></a>Passaggio 6: Reindirizzamento delle cartelle di test

Per testare il reindirizzamento delle cartelle, accedere a un computer con un account utente configurato per il reindirizzamento cartelle. Quindi verificare che i profili e le cartelle vengano reindirizzati.

Di seguito viene illustrato come testare il reindirizzamento delle cartelle:

1. Accedere a un computer primario (se è abilitato il supporto di computer primario) con un account utente per cui è stato abilitato il reindirizzamento delle cartelle.
2. Se l'utente ha precedentemente eseguito l'accesso al computer, aprire un prompt dei comandi con privilegi elevati e quindi digitare il comando seguente per assicurarsi che vengano applicate le impostazioni Criteri di gruppo più recenti al computer client:
    
    ```PowerShell
    gpupdate /force
    ```
3. Aprire Esplora file.
4. Fare doppio clic su una cartella reindirizzata (ad esempio, la cartella documenti nella raccolta documenti) e quindi selezionare **proprietà**.
5. Selezionare il **posizione** scheda e verificare che il percorso siano presenti nella condivisione file specificata anziché un percorso locale.

## <a name="appendix-a-checklist-for-deploying-folder-redirection"></a>Appendice A: Elenco di controllo per la distribuzione di reindirizzamento cartelle

|Stato|Azione|
|:---:|---|
|☐<br>☐<br>☐|1. Preparare il dominio<br>-Aggiungere computer al dominio<br>-Creare account utente|
|☐<br><br><br>|2. Creare un gruppo di sicurezza per il reindirizzamento cartelle<br>-Nome gruppo:<br>-Membri:|
|☐<br><br>|3. Creare una condivisione file per le cartelle reindirizzate<br>-Nome della condivisione file:|
|☐<br><br>|4. Creare un oggetto Criteri di gruppo per Reindirizzamento cartelle<br>-Nome oggetto Criteri di gruppo:|
|☐<br><br>☐<br>☐<br>☐<br>☐<br>☐|5. Configurare le impostazioni di criteri di reindirizzamento cartelle e file Offline<br>-Reindirizzamento cartelle:<br>-Windows 2000, Windows XP e Windows Server 2003 supporto abilitato?<br>-File offline abilitati? (abilitata per impostazione predefinita nei computer client Windows)<br>-Abilitata la modalità sempre Offline?<br>-Sincronizzazione dei file in background abilitata?<br>-Con ottimizzazione per la spostamento delle cartelle reindirizzate abilitato?|
|☐<br><br>☐<br><br>☐<br>☐|6. (Facoltativo) Abilita supporto del computer primario<br>-Basata sul computer o basata sull'utente?<br>-Designare i computer primari per gli utenti<br>-Percorso dell'utente e di mapping dei computer primari:<br>-(Facoltativo) abilitare il supporto del computer primario per il reindirizzamento cartelle<br>-(Facoltativo) abilitare il supporto del computer primario per i profili utente mobili|
|☐|7. Abilitare il reindirizzamento della cartella oggetti Criteri di gruppo|
|☐|8. Reindirizzamento delle cartelle di test|

## <a name="change-history"></a>Cronologia delle modifiche

La tabella seguente include un riepilogo di alcune delle più importanti modifiche apportate a questo argomento.

|Date|Descrizione|`Reason`|
|---|---|---|
|18 gennaio 2017|Aggiungere un passaggio per [passaggio 3: Creare un oggetto Criteri di gruppo per Reindirizzamento cartelle](#step-3-create-a-gpo-for-folder-redirection) per delegare le autorizzazioni di lettura agli utenti autenticati, che è ora necessario a causa di un aggiornamento della sicurezza dei criteri di gruppo.|Commenti e suggerimenti dei clienti.|

## <a name="more-information"></a>Altre informazioni

* [Reindirizzamento cartelle, file Offline e profili utente mobili](folder-redirection-rup-overview.md)
* [Distribuire computer primari per il reindirizzamento delle cartelle e profili utente mobili](deploy-primary-computers.md)
* [Abilita SIMD avanzato funzionalità file Offline](enable-always-offline.md)
* [Informativa sul supporto di Microsoft relativamente ai dati di profilo utente replicati](https://blogs.technet.microsoft.com/askds/2010/09/01/microsofts-support-statement-around-replicated-user-profile-data/)
* [Sideload delle App con DISM](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh852635(v=win.10)>)
* [Risoluzione dei problemi di creazione dei pacchetti, distribuzione e query delle App basate su Windows Runtime](https://msdn.microsoft.com/library/windows/desktop/hh973484.aspx)