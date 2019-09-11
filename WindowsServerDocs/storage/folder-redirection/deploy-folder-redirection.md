---
title: Distribuire il reindirizzamento cartelle con File offline
description: Come usare Windows Server per distribuire il reindirizzamento delle cartelle con File offline ai computer client Windows.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.openlocfilehash: 90b3e3d0b5030f8c0140e54c8b0bf55317437427
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867311"
---
# <a name="deploy-folder-redirection-with-offline-files"></a>Distribuire il reindirizzamento cartelle con File offline

>Si applica a Windows 10, Windows 7, Windows 8, Windows 8.1, Windows Vista, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2, Windows Server 2008 R2, Windows Server (canale semestrale)

Questo argomento descrive come usare Windows Server per distribuire il reindirizzamento delle cartelle con File offline ai computer client Windows.

Per un elenco delle modifiche apportate di recente a questo argomento, vedere [cronologia](#change-history)delle modifiche.

> [!IMPORTANT]
> A causa delle modifiche di sicurezza apportate in [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016), è stato aggiornato [il passaggio 3: Creare un oggetto Criteri di gruppo per il](#step-3-create-a-gpo-for-folder-redirection) Reindirizzamento cartelle di questo argomento in modo che Windows possa applicare correttamente i criteri di Reindirizzamento cartelle (e non ripristinare le cartelle reindirizzate nei PC interessati).

## <a name="prerequisites"></a>Prerequisiti

### <a name="hardware-requirements"></a>Requisiti hardware

Il reindirizzamento cartelle richiede un computer basato su x64 o su x86. non è supportata da Windows® RT.

### <a name="software-requirements"></a>Requisiti software

Il reindirizzamento cartelle presenta i requisiti software seguenti:

- Per amministrare il reindirizzamento cartelle, è necessario aver eseguito l'accesso come membro del gruppo di sicurezza Domain Administrators, Enterprise Administrators o del Criteri di gruppo Creator Owners.
- I computer client devono eseguire Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2019, Windows Server 2016, Windows Server (canale semestrale), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008.
- I computer client devono essere aggiunti ai Servizi di dominio Active Directory in corso di gestione.
- Deve essere disponibile un computer in cui siano installati l'editor Gestione Criteri di gruppo e il Centro amministrativo di Active Directory.
- È necessario che sia disponibile un file server per ospitare le cartelle reindirizzate.
    - Se la condivisione file usa gli spazi dei nomi DFS, le cartelle DFS (collegamenti) devono avere un'unica destinazione per impedire agli utenti di apportare modifiche conflittuali su server differenti.
    - Se la condivisione file usa la Replica DFS per replicare i contenuti con un altro server, gli utenti devono poter accedere all'unico server di origine per impedire agli utenti di apportare modifiche conflittuali su server differenti.
    - Quando si usa una condivisione file in cluster, disabilitare la disponibilità continua sulla condivisione file per evitare problemi di prestazioni con reindirizzamento cartelle e File offline. Inoltre, File offline potrebbe non passare alla modalità offline per 3-6 minuti dopo che un utente perde l'accesso a una condivisione file disponibile in modo continuo, che potrebbe impedire agli utenti che non usano ancora la modalità sempre offline di File offline.

> [!NOTE]
> Alcune funzionalità più recenti del reindirizzamento cartelle hanno requisiti aggiuntivi per il computer client e lo schema Active Directory. Per ulteriori informazioni, vedere la pagina relativa alla [distribuzione di computer primari](deploy-primary-computers.md), [disabilitazione di file offline in cartelle](disable-offline-files-on-folders.md), [Abilitazione della modalità always offline](enable-always-offline.md)e [Abilitazione](enable-optimized-moving.md)dello stato di una cartella

## <a name="step-1-create-a-folder-redirection-security-group"></a>Passaggio 1: Creare un gruppo di sicurezza di Reindirizzamento cartelle

Se l'ambiente non è già configurato con il reindirizzamento cartelle, il primo passaggio consiste nel creare un gruppo di sicurezza che contenga tutti gli utenti a cui si desidera applicare le impostazioni dei criteri di Reindirizzamento cartelle.

Di seguito viene illustrato come creare un gruppo di sicurezza per il reindirizzamento cartelle:

1. Aprire Server Manager in un computer in cui è installato Active Directory centro di amministrazione.
2. Scegliere **Active Directory centro di amministrazione**dal menu **strumenti** . Verrà visualizzato Centro amministrativo di Active Directory.
3. Fare clic con il pulsante destro del mouse sul dominio o sull'unità organizzativa appropriata, scegliere **nuovo**e quindi selezionare **gruppo**.
4. Nella sezione **Gruppo** della finestra **Crea gruppo** specificare le impostazioni seguenti:
    - Digitare il nome del gruppo di sicurezza in **Nome gruppo**, ad esempio: **Utenti Reindirizzamento cartelle**.
    - In **ambito del gruppo**selezionare **sicurezza**, quindi selezionare **globale**.
5. Nella sezione **membri** selezionare **Aggiungi**. Verrà visualizzata la finestra di dialogo Selezione utenti, computer, account servizio o gruppi.
6. Digitare i nomi degli utenti o dei gruppi a cui si desidera distribuire il reindirizzamento cartelle, selezionare **OK**, quindi fare di nuovo clic su **OK** .

## <a name="step-2-create-a-file-share-for-redirected-folders"></a>Passaggio 2: Creare una condivisione file per le cartelle reindirizzate

Se non si dispone già di una condivisione file per le cartelle reindirizzate, attenersi alla procedura seguente per creare una condivisione file in un server che esegue Windows Server 2012.

> [!NOTE]
> Alcune funzionalità potrebbero differire o non essere disponibili se si crea la condivisione file su un server che esegue un'altra versione di Windows Server.

Di seguito viene illustrato come creare una condivisione file in Windows Server 2019, Windows Server 2016 e Windows Server 2012:

1. Nel riquadro di spostamento Server Manager selezionare **Servizi file e archiviazione**e quindi selezionare **condivisioni** per visualizzare la pagina condivisioni.
2. Nel riquadro **condivisioni** selezionare **attività**, quindi selezionare **nuova condivisione**. Verrà visualizzata la Creazione guidata nuova condivisione.
3. Nella pagina **Seleziona profilo** selezionare **condivisione SMB-rapida**. Se Gestione risorse file server è installato e si usano le proprietà di gestione delle cartelle, selezionare invece **condivisione SMB-avanzate**.
4. Nella pagina **Percorso condivisione** selezionare il server e il volume sui quali creare la condivisione.
5. Nella pagina **nome condivisione** Digitare un nome per la condivisione (ad esempio, **Users $** ) nella casella **nome condivisione** .
    >[!TIP]
    >Quando si crea la condivisione, nasconderla inserendo ```$``` dopo il nome condivisione. La condivisione verrà nascosta dai browser occasionali.
6. Nella pagina **altre impostazioni** deselezionare la casella di controllo Abilita disponibilità continua, se presente, e selezionare facoltativamente le caselle di controllo **Abilita enumerazione basata sull'accesso** e **Crittografa accesso ai dati** .
7. Nella pagina **autorizzazioni** selezionare **Personalizza autorizzazioni...** . Viene visualizzata la finestra di dialogo Impostazioni di protezione avanzate.
8. Selezionare **Disabilita ereditarietà**, quindi selezionare **Converti autorizzazioni ereditate in autorizzazione esplicita per questo oggetto**.
9. Impostare le autorizzazioni come descritto nella tabella 1 e illustrata nella figura 1, rimuovendo le autorizzazioni per i gruppi e gli account non elencati e aggiungendo autorizzazioni speciali al gruppo utenti Reindirizzamento cartelle creato nel passaggio 1.
    
    ![Impostazione delle autorizzazioni per la condivisione delle cartelle reindirizzate](media/deploy-folder-redirection/setting-the-permissions-for-the-redirected-folders-share.png)
    
    **Figura 1** Impostazione delle autorizzazioni per la condivisione delle cartelle reindirizzate
10. Se si sceglie il profilo **Condivisione SMB - avanzata** selezionare il valore Utilizzo cartelle **File utente** nella pagina **Proprietà gestione** .
11. Se si sceglie il profilo **Condivisione SMB - avanzata** nella pagina **Quota** , selezionare facoltativamente una quota da applicare agli utenti della condivisione.
12. Nella pagina **conferma** selezionare **Crea.**

### <a name="required-permissions-for-the-file-share-hosting-redirected-folders"></a>Autorizzazioni necessarie per la condivisione file che ospita le cartelle reindirizzate

| Account utente  | Accesso  | Si applica a  |
| --------- | --------- | --------- |
| Account utente | Accesso | Si applica a |
| Sistema     | Controllo completo        |    Questa cartella, le sottocartelle e i file     |
| Administrators     | Controllo completo       | Solo questa cartella        |
| Autore/proprietario     |   Controllo completo      |   Solo sottocartelle e file      |
| Gruppo di sicurezza degli utenti che devono inserire dati nella condivisione (utenti Reindirizzamento cartelle)     |   Elenca cartella/lettura dati *(autorizzazioni avanzate)* <br /><br />Creazione di cartelle/Accodamento dei dati *(autorizzazioni avanzate)* <br /><br />Leggi attributi *(autorizzazioni avanzate)* <br /><br />Lettura attributi estesi *(autorizzazioni avanzate)* <br /><br />Autorizzazioni di lettura *(autorizzazioni avanzate)*      |  Solo questa cartella       |
| Altri gruppi e account     |  Nessuno (rimuovere)       |         |

## <a name="step-3-create-a-gpo-for-folder-redirection"></a>Passaggio 3: Creare un oggetto Criteri di gruppo per il reindirizzamento cartelle

Se non è già stato creato un oggetto Criteri di gruppo per le impostazioni di Reindirizzamento cartelle, utilizzare la procedura seguente per crearne uno.

Di seguito viene illustrato come creare un oggetto Criteri di gruppo per il reindirizzamento cartelle:

1. Aprire Server Manager in un computer in cui è installata la console Gestione Criteri di gruppo.
2. Dal menu **strumenti** selezionare **Gestione criteri di gruppo**.
3. Fare clic con il pulsante destro del mouse sul dominio o sull'unità organizzativa in cui si desidera configurare il reindirizzamento cartelle, quindi selezionare **Crea un oggetto Criteri di gruppo in questo dominio e collegarlo qui**.
4. Nella finestra di dialogo **nuovo oggetto Criteri** di gruppo digitare un nome per l'oggetto Criteri di gruppo, ad esempio **impostazioni di Reindirizzamento cartelle**, quindi fare clic su **OK**.
5. Fare clic con il pulsante destro del mouse sull'oggetto Criteri di gruppo appena creato e quindi deselezionare la casella di controllo **Collegamento abilitato** . Ciò impedisce di applicare l'oggetto Criteri di gruppo finché non sarà terminata la configurazione.
6. Selezionare l'oggetto Criteri di gruppo. Nella sezione **filtri di sicurezza** della scheda **ambito** Selezionare **utenti autenticati**, quindi selezionare **Rimuovi** per impedire che l'oggetto Criteri di gruppo venga applicato a tutti.
7. Nella sezione **filtri di sicurezza** selezionare **Aggiungi**.
8. Nella finestra di dialogo **Seleziona utente, computer o gruppo** Digitare il nome del gruppo di sicurezza creato nel passaggio 1, ad esempio **utenti Reindirizzamento cartelle**, quindi fare clic su **OK**.
9. Selezionare la scheda **delega** , selezionare **Aggiungi**, digitare **Authenticated Users**, selezionare **OK**, quindi selezionare di nuovo **OK** per accettare le autorizzazioni di lettura predefinite.
    
    Questo passaggio è necessario a causa delle modifiche di sicurezza apportate in [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016).

> [!IMPORTANT]
> A causa delle modifiche di sicurezza apportate in [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016), è ora necessario concedere al gruppo Authenticated Users le autorizzazioni di lettura delegate per l'oggetto Criteri di gruppo reindirizzamento cartelle; in caso contrario, l'oggetto Criteri di gruppo non verrà applicato agli utenti o, se è già applicato, l'oggetto Criteri di gruppo viene rimosso, il reindirizzamento di nuovo nel computer locale. Per altre informazioni, vedere [distribuzione criteri di gruppo aggiornamento della sicurezza MS16-072](https://blogs.technet.microsoft.com/askds/2016/06/22/deploying-group-policy-security-update-ms16-072-kb3163622/).

## <a name="step-4-configure-folder-redirection-with-offline-files"></a>Passaggio 4: Configurare il reindirizzamento cartelle con File offline

Dopo aver creato un oggetto Criteri di gruppo per le impostazioni di Reindirizzamento cartelle, modificare le impostazioni Criteri di gruppo per abilitare e configurare il reindirizzamento delle cartelle, come illustrato nella procedura seguente.

> [!NOTE]
> Per impostazione predefinita, File offline è abilitato per le cartelle reindirizzate nei computer client Windows e disabilitato nei computer che eseguono Windows Server, a meno che non vengano modificati dall'utente. Per usare Criteri di gruppo per controllare se la File offline è abilitata, usare l'impostazione **dei criteri di funzionalità Consenti o non consentire l'uso dell'file offline** .
> Per informazioni su alcune delle altre File offline Criteri di gruppo impostazioni, vedere [abilitare funzionalità di file offline avanzate](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn270369(v%3dws.11)>)e [configurare criteri di gruppo per file offline](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc759721(v%3dws.10)>).

Di seguito viene illustrato come configurare il reindirizzamento delle cartelle nel Criteri di gruppo:

1. In Gestione Criteri di gruppo fare clic con il pulsante destro del mouse sull'oggetto Criteri di gruppo creato, ad esempio **impostazioni di Reindirizzamento cartelle**, quindi scegliere **modifica**.
2. Nella finestra Editor Gestione Criteri di gruppo passare a **Configurazione utente**, quindi **criteri**, **impostazioni di Windows**e quindi **Reindirizzamento cartelle**.
3. Fare clic con il pulsante destro del mouse su una cartella che si desidera reindirizzare, ad esempio **documenti**, quindi scegliere **Proprietà**.
4. Nella finestra di dialogo **Proprietà** , nella casella **impostazione** selezionare **Basic-reindirizza la cartella Everyone allo stesso percorso**.

    > [!NOTE]
    > Per applicare il reindirizzamento cartelle ai computer client che eseguono Windows XP o Windows Server 2003, selezionare la scheda **Impostazioni** e selezionare i **criteri di reindirizzamento applica anche a Windows 2000, Windows 2000 Server, Windows XP e Windows Server 2003 operativo sistema** .

5. Nella sezione percorso **cartella di destinazione** selezionare **Crea una cartella per ogni utente nel percorso radice** e quindi nella casella **percorso radice** digitare il percorso della condivisione file che archivia le cartelle reindirizzate, ad esempio:  **\\ \\ utenti\\FS1.Corp.contoso.com $**
6. Selezionare la scheda **Impostazioni** e nella sezione **Rimozione criteri** Selezionare facoltativamente **reindirizzare la cartella al percorso locale UserProfile quando il criterio viene rimosso** . questa impostazione consente di migliorare il comportamento del reindirizzamento cartelle. prevedibile per adminisitrators e per gli utenti).
7. Selezionare **OK**, quindi scegliere **Sì** nella finestra di dialogo di avviso.

## <a name="step-5-enable-the-folder-redirection-gpo"></a>Passaggio 5: Abilitare l'oggetto Criteri di gruppo reindirizzamento cartelle

Una volta completata la configurazione del reindirizzamento cartelle Criteri di gruppo impostazioni, il passaggio successivo consiste nell'abilitazione dell'oggetto Criteri di gruppo, che consente l'applicazione agli utenti interessati.

> [!TIP]
> È consigliabile implementare il supporto del computer primario o altre impostazioni criterio prima di abilitare l'oggetto Criteri di gruppo. In tal modo, i dati utente non verranno copiati nei computer non primari prima di aver abilitato il supporto del computer primario.

Di seguito viene illustrato come abilitare l'oggetto Criteri di gruppo reindirizzamento cartelle:

1. Aprire Gestione Criteri di gruppo.
2. Fare clic con il pulsante destro del mouse sull'oggetto Criteri di gruppo creato, quindi scegliere **collegamento abilitato**. Accanto alla voce di menu verrà visualizzata una casella di controllo.

## <a name="step-6-test-folder-redirection"></a>Passaggio 6: Reindirizzamento cartelle test

Per testare il reindirizzamento cartelle, accedere a un computer con un account utente configurato per il reindirizzamento cartelle. Quindi verificare che le cartelle e i profili siano reindirizzati.

Di seguito viene illustrato come testare il reindirizzamento cartelle:

1. Accedere a un computer primario (se è stato abilitato il supporto per il computer primario) con un account utente per il quale è stato abilitato il reindirizzamento cartelle.
2. Se l'utente ha precedentemente eseguito l'accesso al computer, aprire un prompt dei comandi con privilegi elevati e quindi digitare il comando seguente per assicurarsi che vengano applicate le impostazioni Criteri di gruppo più recenti al computer client:
    
    ```PowerShell
    gpupdate /force
    ```
3. Aprire Esplora file.
4. Fare clic con il pulsante destro del mouse su una cartella reindirizzata, ad esempio la cartella documenti nella raccolta documenti, quindi scegliere **Proprietà**.
5. Selezionare la scheda **location** (percorso) e verificare che il percorso visualizzi la condivisione file specificata anziché un percorso locale.

## <a name="appendix-a-checklist-for-deploying-folder-redirection"></a>Appendice A: Elenco di controllo per la distribuzione del reindirizzamento cartelle

| Stato           | Azione |
| ---              | ---    |
| ☐<br>☐<br>☐    | 1. Preparare il dominio<br>-Aggiungere computer al dominio<br>-Creare gli account utente |
| ☐<br><br><br>   | 2. Crea gruppo di sicurezza per il reindirizzamento cartelle<br>-Nome gruppo:<br>Membri |
| ☐<br><br>       | 3. Creare una condivisione file per le cartelle reindirizzate<br>-Nome condivisione file: |
| ☐<br><br>       | 4. Creare un oggetto Criteri di gruppo per il reindirizzamento cartelle<br>-Nome oggetto Criteri di gruppo: |
| ☐<br><br>☐<br>☐<br>☐<br>☐<br>☐ | 5. Configurare il reindirizzamento cartelle e le impostazioni dei criteri di File offline<br>-Cartelle reindirizzate:<br>-Il supporto per Windows 2000, Windows XP e Windows Server 2003 è abilitato?<br>-File offline abilitato? (abilitato per impostazione predefinita nei computer client Windows)<br>-La modalità sempre offline è abilitata?<br>-Sincronizzazione file in background abilitata?<br>-Lo spostamento ottimizzato per le cartelle reindirizzate è abilitato? |
| ☐<br><br>☐<br><br>☐<br>☐ | 6. Opzionale Abilita supporto per computer primari<br>-Basato su computer o su utente?<br>-Designare i computer primari per gli utenti<br>-Percorso dei mapping di utente e computer primario:<br>-(Facoltativo) abilitare il supporto del computer primario per il reindirizzamento cartelle<br>-(Facoltativo) abilitare il supporto del computer primario per i profili utente mobili |
| ☐         | 7. Abilitare l'oggetto Criteri di gruppo reindirizzamento cartelle |
| ☐         | 8. Reindirizzamento cartelle test |

## <a name="change-history"></a>Cronologia delle modifiche

La tabella seguente include un riepilogo di alcune delle più importanti modifiche apportate a questo argomento.

| Date | Descrizione | `Reason`|
| --- | --- | --- |
| 18 gennaio 2017 | Aggiunta di un passaggio [al passaggio 3: Creare un oggetto Criteri di gruppo per il](#step-3-create-a-gpo-for-folder-redirection) Reindirizzamento cartelle per delegare le autorizzazioni di lettura agli utenti autenticati, che è ora necessario a causa di un aggiornamento della sicurezza Criteri di gruppo. | Feedback dei clienti |

## <a name="more-information"></a>Altre informazioni

* [Reindirizzamento cartelle, File offline e profili utente mobili](folder-redirection-rup-overview.md)
* [Distribuire i computer primari per il reindirizzamento cartelle e i profili utente mobili](deploy-primary-computers.md)
* [Abilita funzionalità avanzate di File offline](enable-always-offline.md)
* [Informativa sul supporto tecnico Microsoft sui dati del profilo utente replicati](https://blogs.technet.microsoft.com/askds/2010/09/01/microsofts-support-statement-around-replicated-user-profile-data/)
* [App sideload con gestione e manutenzione immagini distribuzione](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh852635(v=win.10)>)
* [Risoluzione dei problemi relativi a pacchetti, distribuzione e query delle app basate su Windows Runtime](https://msdn.microsoft.com/library/windows/desktop/hh973484.aspx)