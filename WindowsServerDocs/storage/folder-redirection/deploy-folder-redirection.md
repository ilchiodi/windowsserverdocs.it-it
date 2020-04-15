---
title: Distribuire Reindirizzamento cartelle con File offline
description: Come usare Windows Server per distribuire ai computer client Windows la funzionalità Reindirizzamento cartelle con File offline.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.openlocfilehash: e8e6e5a29c75c117f6faa3c1d1b3f288582d81a2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855884"
---
# <a name="deploy-folder-redirection-with-offline-files"></a>Distribuire Reindirizzamento cartelle con File offline

>Si applica a: Windows 10, Windows 7, Windows 8, Windows 8.1, Windows Vista, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2, Windows Server 2008 R2, Windows Server (Canale semestrale)

Questo argomento descrive come usare Windows Server per distribuire Reindirizzamento cartelle con File offline ai computer client.

Per un elenco delle modifiche apportate di recente a questo argomento, vedi [Cronologia delle modifiche](#change-history).

> [!IMPORTANT]
> A causa delle modifiche di sicurezza apportate nell'aggiornamento [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016), è stata aggiornata la sezione [Passaggio 3: Creare un oggetto Criteri di gruppo per Reindirizzamento cartelle](#step-3-create-a-gpo-for-folder-redirection) di questo argomento in modo che Windows possa applicare correttamente i criteri di Reindirizzamento cartelle (e non ripristinare nei PC interessati le cartelle reindirizzate).

## <a name="prerequisites"></a>Prerequisiti

### <a name="hardware-requirements"></a>Requisiti hardware

La funzionalità Reindirizzamento cartelle richiede un computer basato su x64 o su x86 e non è supportata da Windows&reg; RT.

### <a name="software-requirements"></a>Requisiti software

La funzionalità Reindirizzamento cartelle prevede i requisiti software seguenti:

- Per amministrare Reindirizzamento cartelle, devi accedere come membro del gruppo di sicurezza Amministratori di dominio, Amministratori dell'organizzazione o Proprietari autori criteri di gruppo.
- I computer client devono eseguire Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2019, Windows Server 2016, Windows Server (Canale semestrale), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008.
- I computer client devono essere aggiunti ai Servizi di dominio Active Directory in corso di gestione.
- Deve essere disponibile un computer in cui siano installati l'editor Gestione Criteri di gruppo e il Centro amministrativo di Active Directory.
- Per l'hosting delle cartelle reindirizzate deve essere disponibile un file server.
    - Se la condivisione file usa gli spazi dei nomi DFS, le cartelle DFS (collegamenti) devono avere un'unica destinazione per impedire agli utenti di apportare modifiche conflittuali su server differenti.
    - Se la condivisione file usa la Replica DFS per replicare i contenuti con un altro server, gli utenti devono poter accedere all'unico server di origine per impedire agli utenti di apportare modifiche conflittuali su server differenti.
    - Quando viene usata una condivisione file in cluster, disabilita la disponibilità continua nella condivisione file per evitare problemi di prestazioni con Reindirizzamento cartelle e File offline. È anche possibile che File offline non passi alla modalità offline per 3-6 minuti dopo la perdita da parte di un utente dell'accesso a una condivisione file disponibile in modo continuo. Ciò può risultare frustrante per gli utenti che non usano ancora la modalità sempre offline di File offline.

> [!NOTE]
> Per alcune nuove funzionalità di Reindirizzamento cartelle sono previsti altri requisiti per i computer client e lo schema di Active Directory. Per altre informazioni, vedi [Distribuire i computer primari](deploy-primary-computers.md), [Disabilitare File offline nelle cartelle](disable-offline-files-on-folders.md), [Abilitare la modalità sempre offline](enable-always-offline.md) e [Abilitare lo spostamento ottimizzato di cartelle](enable-optimized-moving.md).

## <a name="step-1-create-a-folder-redirection-security-group"></a>Passaggio 1: Creare un gruppo di sicurezza di Reindirizzamento cartelle

Se l'ambiente non è ancora stato configurato con Reindirizzamento cartelle, il primo passaggio consiste nel creare un gruppo di sicurezza che contenga tutti gli utenti ai quali applicare le impostazioni criterio di Reindirizzamento cartelle.

Ecco come creare un gruppo di sicurezza per Reindirizzamento cartelle:

1. Apri Server Manager in un computer in cui è installato Centro amministrativo di Active Directory.
2. Scegli **Centro amministrativo di Active Directory** dal menu **Strumenti**. Verrà visualizzato Centro amministrativo di Active Directory.
3. Fai clic con il pulsante destro del mouse sul dominio o sull'unità organizzativa appropriata, scegli **Nuovo** e quindi **Gruppo**.
4. Nella sezione **Gruppo** della finestra **Crea gruppo** specificare le impostazioni seguenti:
    - Digitare il nome del gruppo di sicurezza in **Nome gruppo**, ad esempio: **Utenti di Reindirizzamento cartelle**.
    - In **Ambito del gruppo** seleziona **Protezione** e quindi **Globale**.
5. Nella sezione **Membri** seleziona **Aggiungi**. Verrà visualizzata la finestra di dialogo Selezione utenti, computer, account servizio o gruppi.
6. Digita i nomi degli utenti o dei gruppi ai quali vuoi distribuire Reindirizzamento cartelle, scegli **OK** e quindi di nuovo **OK**.

## <a name="step-2-create-a-file-share-for-redirected-folders"></a>Passaggio 2: Creare una condivisione file per le cartelle reindirizzate

Se non disponi già di una condivisione file per le cartelle reindirizzate, segui questa procedura per creare una condivisione file in un server che esegue Windows Server 2012.

> [!NOTE]
> Alcune funzionalità potrebbero differire o non essere disponibili se si crea la condivisione file su un server che esegue un'altra versione di Windows Server.

Ecco come creare una condivisione file in Windows Server 2019, Windows Server 2016 e Windows Server 2012:

1. Nel riquadro di spostamento di Server Manager seleziona **Servizi file e archiviazione** e quindi **Condivisioni** per visualizzare la pagina Condivisioni.
2. Nel riquadro **Condivisioni** seleziona **Attività** e quindi **Nuova condivisione**. Verrà visualizzata la Creazione guidata nuova condivisione.
3. Nella pagina **Selezionare il profilo** seleziona **Condivisione SMB - rapida**. Se hai installato Gestione risorse file server e usi le proprietà di gestione delle cartelle, seleziona invece **Condivisione SMB - avanzata**.
4. Nella pagina **Percorso condivisione** selezionare il server e il volume sui quali creare la condivisione.
5. Nella pagina **Nome condivisione** digita un nome per la condivisione (ad esempio **Utenti$** ) nella casella **Nome condivisione**.
    >[!TIP]
    >Quando si crea la condivisione, nasconderla inserendo ```$``` dopo il nome condivisione. In tal modo, la condivisione verrà nascosta ai browser casuali.
6. Nella pagina **Altre impostazioni** deseleziona la casella di controllo di abilitazione della disponibilità continua, se disponibile, e seleziona facoltativamente le caselle di controllo **Abilita enumerazione basata sull'accesso** e **Crittografa accesso ai dati**.
7. Nella pagina **Autorizzazioni** seleziona **Personalizza autorizzazioni**. Viene visualizzata la finestra di dialogo Impostazioni di protezione avanzate.
8. Seleziona **Disabilita ereditarietà** e quindi **Converti autorizzazioni ereditate in autorizzazioni esplicite per questo oggetto**.
9. Imposta le autorizzazioni come descritto nella tabella 1 e mostrato nella figura 1, rimuovendo le autorizzazioni per i gruppi e gli account non elencati e aggiungendo autorizzazioni di accesso speciali al gruppo Utenti di Reindirizzamento cartelle creato nel passaggio 1.
    
    ![Impostazione delle autorizzazioni per la condivisione delle cartelle reindirizzate](media/deploy-folder-redirection/setting-the-permissions-for-the-redirected-folders-share.png)
    
    **Figura 1**Impostazione delle autorizzazioni per la condivisione delle cartelle reindirizzate
10. Se si sceglie il profilo **Condivisione SMB - avanzata** selezionare il valore Utilizzo cartelle **File utente** nella pagina **Proprietà gestione** .
11. Se si sceglie il profilo **Condivisione SMB - avanzata** nella pagina **Quota** , selezionare facoltativamente una quota da applicare agli utenti della condivisione.
12. Nella pagina **Conferma** seleziona **Crea**.

### <a name="required-permissions-for-the-file-share-hosting-redirected-folders"></a>Autorizzazioni necessarie per la condivisione file che ospita le cartelle reindirizzate

| Account utente  | Accesso  | Si applica a  |
| --------- | --------- | --------- |
| Account utente | Accesso | Si applica a |
| System     | Controllo completo        |    Questa cartella, le sottocartelle e i file     |
| Administrators     | Controllo completo       | Solo questa cartella        |
| Autore/proprietario     |   Controllo completo      |   Solo sottocartelle e file      |
| Gruppo di utenti di sicurezza con l'esigenza di inserire dati nella condivisione (Utenti di Reindirizzamento cartelle)     |   Visualizzazione contenuto cartella/Lettura dati *(Autorizzazioni avanzate)* <p>Creazione cartelle/Aggiunta dati *(Autorizzazioni avanzate)* <p>Lettura attributi *(Autorizzazioni avanzate)* <p>Lettura attributi estesi *(Autorizzazioni avanzate)* <p>Autorizzazioni di lettura *(Autorizzazioni avanzate)*      |  Solo questa cartella       |
| Altri gruppi e account     |  Nessuno (rimuovere)       |         |

## <a name="step-3-create-a-gpo-for-folder-redirection"></a>Passaggio 3: Creare un oggetto Criteri di gruppo per Reindirizzamento cartelle

Se non disponi già di un oggetto Criteri di gruppo per le impostazioni di Reindirizzamento cartelle, segui questa procedura per crearne uno.

Ecco come creare un oggetto Criteri di gruppo per Reindirizzamento cartelle:

1. Aprire Server Manager in un computer in cui è installata la console Gestione Criteri di gruppo.
2. Scegli **Gestione Criteri di gruppo** dal menu **Strumenti**.
3. Fai clic con il pulsante destro del mouse sul dominio o sull'unità organizzativa in cui vuoi configurare Reindirizzamento cartelle e quindi scegli **Crea un oggetto Criteri di gruppo in questo dominio e crea qui un collegamento**.
4. Nella finestra di dialogo **Nuovo oggetto Criteri di gruppo** digita un nome per l'oggetto Criteri di gruppo (ad esempio, **Impostazioni di reindirizzamento cartelle**) e quindi scegli **OK**.
5. Fare clic con il pulsante destro del mouse sull'oggetto Criteri di gruppo appena creato e quindi deselezionare la casella di controllo **Collegamento abilitato** . Ciò impedisce di applicare l'oggetto Criteri di gruppo finché non sarà terminata la configurazione.
6. Selezionare l'oggetto Criteri di gruppo. Nella sezione **Filtri di protezione** della scheda **Ambito** seleziona **Authenticated Users** e quindi scegli **Rimuovi** per impedire che l'oggetto Criteri di gruppo venga applicato a tutti.
7. Nella sezione **Filtri di protezione** scegli **Aggiungi**.
8. Nella finestra di dialogo **Seleziona utente, computer o gruppo** digita il nome del gruppo di sicurezza creato nel passaggio 1 (ad esempio, **Utenti di Reindirizzamento cartelle**) e quindi scegli **OK**.
9. Fai clic sulla scheda **Delega**, scegli **Aggiungi**, digita **Authenticated Users**, scegli **OK** e quindi di nuovo **OK** per accettare le autorizzazioni di lettura predefinite.
    
    Questo passaggio è necessario a causa delle modifiche di sicurezza apportate nell'aggiornamento [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016).

> [!IMPORTANT]
> A causa delle modifiche di sicurezza apportate nell'aggiornamento [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016), è ora necessario concedere al gruppo Authenticated Users le autorizzazioni di lettura delegate per l'oggetto Criteri di gruppo Reindirizzamento cartelle. In caso contrario, l'oggetto Criteri di gruppo non verrà applicato agli utenti o, se è già applicato, verrà rimosso e le cartelle verranno reindirizzate al PC locale. Per altre informazioni, vedi [Deploying Group Policy Security Update MS16-072](https://blogs.technet.microsoft.com/askds/2016/06/22/deploying-group-policy-security-update-ms16-072-kb3163622/) (Distribuzione dell'aggiornamento di sicurezza MS16-072 di Criteri di gruppo).

## <a name="step-4-configure-folder-redirection-with-offline-files"></a>Passaggio 4: Configurare Reindirizzamento cartelle con File offline

Dopo aver creato un oggetto Criteri di gruppo per le impostazioni di Reindirizzamento cartelle, modifica le impostazioni di Criteri di gruppo per abilitare e configurare Reindirizzamento cartelle, come illustrato nella procedura seguente.

> [!NOTE]
> Per impostazione predefinita, la funzionalità File offline è abilitata per le cartelle reindirizzate nei computer client Windows e disabilitata nei computer che eseguono Windows Server, a meno che tale impostazione non venga modificata dall'utente. Per usare Criteri di gruppo per controllare se la funzionalità File offline è abilitata, usa l'impostazione criterio **Consenti o proibisci l'uso della funzione File non in linea**.
> Per informazioni su alcune delle altre impostazioni di Criteri di gruppo per File offline, vedi [Abilitare la funzionalità avanzata File offline](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn270369(v%3dws.11)>) e [Configurazione di Criteri di gruppo per File offline](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc759721(v%3dws.10)>).

Ecco come configurare Reindirizzamento cartelle in Criteri di gruppo:

1. In Gestione Criteri di gruppo fai clic con il pulsante destro del mouse sull'oggetto Criteri di gruppo creato (ad esempio, **Impostazioni di reindirizzamento cartelle**) e quindi scegli **Modifica**.
2. Nella finestra dell'Editor Gestione Criteri di gruppo passa a **Configurazione utente**, **Criteri**, **Impostazioni di Windows** e quindi **Reindirizzamento cartelle**.
3. Fai clic con il pulsante destro del mouse su una cartella da reindirizzare, ad esempio **Documenti**, e quindi scegli **Proprietà**.
4. Nella finestra di dialogo **Proprietà**, nella casella **Impostazione**, seleziona **Base - Reindirizza la cartella comune nello stesso percorso**.

    > [!NOTE]
    > Per applicare Reindirizzamento cartelle ai computer client che eseguono Windows XP o Windows Server 2003, fai clic sulla scheda **Impostazioni** e seleziona la casella di controllo **Applica criterio di reindirizzamento ai sistemi operativi Windows 2000, Windows 2000 Server, Windows XP e Windows Server 2003**.

5. Nella sezione **Percorso cartella di destinazione** seleziona **Crea una cartella per ogni utente nel percorso directory principale** e quindi nella casella **Percorso principale** digita il percorso della condivisione file in cui vengono archiviate le cartelle reindirizzate, ad esempio: **\\\\fs1.corp.contoso.com\\utenti$**
6. Fai clic sulla scheda **Impostazioni** e nella sezione **Rimozione criteri** seleziona facoltativamente **Reindirizza la cartella nel percorso del profilo utente locale quando i criteri vengono rimossi**. Questa impostazione rende il comportamento della funzionalità Reindirizzamento cartelle più prevedibile per gli amministratori e gli utenti.
7. Scegli **OK** e quindi **Sì** nella finestra di dialogo di avviso.

## <a name="step-5-enable-the-folder-redirection-gpo"></a>Passaggio 5: Abilitare l'oggetto Criteri di gruppo Reindirizzamento cartelle

Una volta completata la configurazione delle impostazioni di Criteri di gruppo Reindirizzamento cartelle, il passaggio successivo consiste nell'abilitare l'oggetto Criteri di gruppo in modo da poterlo applicare agli utenti interessati.

> [!TIP]
> È consigliabile implementare il supporto del computer primario o altre impostazioni criterio prima di abilitare l'oggetto Criteri di gruppo. In tal modo, i dati utente non verranno copiati nei computer non primari prima di aver abilitato il supporto del computer primario.

Ecco come abilitare l'oggetto Criteri di gruppo Reindirizzamento cartelle:

1. Aprire Gestione Criteri di gruppo.
2. Fai clic con il pulsante destro del mouse sull'oggetto Criteri di gruppo creato e quindi scegli **Collegamento abilitato**. Accanto alla voce del menu verrà visualizzata una casella di controllo.

## <a name="step-6-test-folder-redirection"></a>Passaggio 6: Testare Reindirizzamento cartelle

Per testare Reindirizzamento cartelle, accedi a un computer con un account utente configurato per Reindirizzamento cartelle. Verifica quindi che le cartelle e i profili siano reindirizzati.

Ecco come testare Reindirizzamento cartelle:

1. Accedi a un computer primario (se è stato abilitato il supporto per i computer primari) con un account utente per il quale è stata abilitata la funzionalità Reindirizzamento cartelle.
2. Se l'utente ha precedentemente eseguito l'accesso al computer, aprire un prompt dei comandi con privilegi elevati e quindi digitare il comando seguente per assicurarsi che vengano applicate le impostazioni Criteri di gruppo più recenti al computer client:
    
    ```PowerShell
    gpupdate /force
    ```
3. Aprire Esplora file.
4. Fai clic con il pulsante destro del mouse su una cartella reindirizzata (ad esempio, la cartella Documenti nella raccolta Documenti) e quindi scegli **Proprietà**.
5. Fai clic sulla scheda **Posizione** e verifica che come percorso venga visualizzata la condivisione file specificata anziché un percorso locale.

## <a name="appendix-a-checklist-for-deploying-folder-redirection"></a>Appendice A: Elenco di controllo per la distribuzione di Reindirizzamento cartelle

| Stato           | Azione |
| ---              | ---    |
| ☐<br>☐<br>☐    | 1. Preparare il dominio<br>- Aggiungere computer al dominio<br>- Creare account utente |
| ☐<br><br><br>   | 2. Creare un gruppo di sicurezza per Reindirizzamento cartelle<br>- Nome gruppo:<br>- Membri: |
| ☐<br><br>       | 3. Creare una condivisione file per le cartelle reindirizzate<br>- Nome condivisione file: |
| ☐<br><br>       | 4. Creare un oggetto Criteri di gruppo per Reindirizzamento cartelle<br>- Nome oggetto Criteri di gruppo: |
| ☐<br><br>☐<br>☐<br>☐<br>☐<br>☐ | 5. Configurare le impostazioni criterio Reindirizzamento cartelle e File offline<br>- Cartelle reindirizzate:<br>- Il supporto per Windows 2000, Windows XP e Windows Server 2003 è abilitato?<br>- La funzionalità File offline è abilitata? (È abilitata per impostazione predefinita nei computer client Windows)<br>- La modalità sempre offline è abilitata?<br>- La sincronizzazione dei file in background è abilitata?<br>- Lo spostamento ottimizzato di cartelle reindirizzate è abilitato? |
| ☐<br><br>☐<br><br>☐<br>☐ | 6. (Facoltativo) Abilitare il supporto dei computer primari<br>- Basato su computer o su utente?<br>- Designare i computer primari per gli utenti<br>- Percorso dei mapping di utente e computer primario:<br>- (Facoltativo) Abilitare il supporto dei computer primari per Reindirizzamento cartelle<br>- (Facoltativo) Abilitare il supporto del computer primario per Profili utente mobili |
| ☐         | 7. Abilitare l'oggetto Criteri di gruppo Reindirizzamento cartelle |
| ☐         | 8. Testare Reindirizzamento cartelle |

## <a name="change-history"></a>Cronologia delle modifiche

La tabella seguente include un riepilogo di alcune delle più importanti modifiche apportate a questo argomento.

| Data | Description | Motivo|
| --- | --- | --- |
| 18 gennaio 2017 | Aggiunta di un passaggio alla sezione [Passaggio 3: Creare un oggetto Criteri di gruppo per Reindirizzamento cartelle](#step-3-create-a-gpo-for-folder-redirection) per delegare le autorizzazioni di lettura agli utenti autenticati, operazione ora necessaria a seguito di un aggiornamento di sicurezza di Criteri di gruppo. | Feedback dei clienti |

## <a name="more-information"></a>Ulteriori informazioni

* [Reindirizzamento cartelle, File offline e Profili utente mobili](folder-redirection-rup-overview.md)
* [Distribuire computer primari per Reindirizzamento cartelle e Profili utente mobili](deploy-primary-computers.md)
* [Abilitare la funzionalità File offline avanzata](enable-always-offline.md)
* [Dichiarazione Microsoft a supporto della replica dei dati del profilo utente](https://blogs.technet.microsoft.com/askds/2010/09/01/microsofts-support-statement-around-replicated-user-profile-data/)
* [Trasferire localmente le app con Gestione e manutenzione immagini distribuzione](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh852635(v=win.10)>)
* [Risoluzione dei problemi di creazione di pacchetti, distribuzione e query delle app basate su Windows Runtime](https://msdn.microsoft.com/library/windows/desktop/hh973484.aspx)
