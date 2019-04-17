---
title: Distribuire cartelle di reindirizzamento cartelle con i file Offline
description: Come usare Windows Server per distribuire cartelle di reindirizzamento cartelle con i file Offline per i computer client Windows.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 33942db34314e0ff60b24d4b9c8e5e33b4ca92fd
ms.sourcegitcommit: 5549ac178f8f3d116e88761a95223063a636ac94
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/04/2018
ms.locfileid: "4376625"
---
# Distribuire cartelle di reindirizzamento cartelle con i file Offline

>Si applica a: Windows 10, Windows 7, Windows 8, Windows 8.1, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016, Windows Vista

In questo argomento descrive come usare Windows Server per distribuire cartelle di reindirizzamento cartelle con i file Offline per i computer client Windows.

Per un elenco delle modifiche recenti in questo argomento, vedi [cronologia delle modifiche](#change-history).

>[!IMPORTANT]
>A causa di sicurezza le modifiche apportate nel [MS16-072](https://support.microsoft.com/en-us/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016), abbiamo aggiornato [passaggio 3: creare un oggetto Criteri di gruppo per cartelle di reindirizzamento cartelle](#step-3:-create-a-gpo-for-folder-redirection) di questo argomento, in modo che Windows possa correttamente applica i criteri di reindirizzamento (e non ripristinare le cartelle reindirizzate nei computer interessati).

## Prerequisiti

### Requisiti hardware

Reindirizzamento richiede un computer basato su x86 o x64. non è supportata da Windows® RT

### Requisiti software

Reindirizzamento presenta i requisiti software seguenti:

- Per amministrare il reindirizzamento delle cartelle, è necessario essere connessi come membro del gruppo di sicurezza amministratori di dominio, il gruppo di sicurezza amministratori dell'organizzazione o il gruppo di sicurezza proprietari di creatore di criteri di gruppo.
- I computer client devono eseguire Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008.
- I computer client devono essere aggiunto a di Active Directory Domain Services (AD DS) che si sta gestendo.
- Un computer deve essere disponibile con Gestione criteri di gruppo e installato centro di amministrazione di Active Directory.
- Un file server deve essere disponibile per ospitare le cartelle reindirizzate.
    - Se la condivisione file Usa spazi dei nomi DFS, le cartelle DFS (collegamenti) devono avere un'unica destinazione per impedire agli utenti di apportare modifiche in conflitto in server diversi.
    - Se la condivisione file Usa replica DFS per replicare il contenuto con un altro server, gli utenti devono essere in grado di accedere solo il server di origine per impedire agli utenti di apportare modifiche in conflitto in server diversi.
    - Quando si utilizza una condivisione file cluster, disabilita disponibilità continua nella condivisione file per evitare problemi di prestazioni con reindirizzamento cartelle e i file Offline. Inoltre, file Offline potrebbe non eseguire la transizione alla modalità offline per 3-6 minuti dopo che un utente perde l'accesso a una condivisione file continuamente disponibili, che potrebbe causare frustrazione gli utenti che non sono ancora usando la modalità Offline sempre di file Offline.

>[!NOTE]
>Alcune funzionalità più recenti di reindirizzamento hanno computer client aggiuntivi e requisiti dello schema di Active Directory. Per altre info, vedi [distribuire computer primario](deploy-primary-computers.md), [Disabilitare i file Offline nelle cartelle](disable-offline-files-on-folders.md), [abilitare sempre modalità Offline](enable-always-offline.md)e [abilitare lo spostamento ottimizzato cartella](enable-optimized-moving.md).

## Passaggio 1: Creare un gruppo di sicurezza di reindirizzamento cartelle

Se l'ambiente non è già configurata con reindirizzamento cartelle, il primo passaggio consiste nel creare un gruppo di sicurezza che contiene tutti gli utenti a cui si desidera applicare le impostazioni dei criteri di reindirizzamento.

Ecco come creare un gruppo di sicurezza per cartelle di reindirizzamento cartelle:

1. Apri Server Manager in un computer con il centro di amministrazione di Active Directory installato.
2. Dal menu **Strumenti** , seleziona **Il centro di amministrazione di Active Directory**. Verrà visualizzato Centro amministrativo di Active Directory.
3. Mouse il dominio appropriato o unità Organizzativa, seleziona **Nuovo**e quindi seleziona **gruppo**.
4. Nella sezione **Gruppo** della finestra **Crea gruppo** specifica le impostazioni seguenti:
    - **Nome del gruppo**, digita il nome del gruppo di sicurezza, ad esempio: **Gli utenti reindirizzamento delle cartelle**.
    - Nell' **ambito del gruppo**, seleziona **sicurezza**e quindi selezionare **globale**.
5. Nella sezione **membri** , scegli **Aggiungi**. Verrà visualizzata la finestra di dialogo Selezione utenti, Contatti, Computer, Account di servizio o Gruppi.
6. Digita i nomi degli utenti o gruppi a cui si desidera distribuire cartelle di reindirizzamento cartelle, fare **clic su OK**e quindi seleziona **OK** nuovamente.

## Passaggio 2: Creare una condivisione file per le cartelle reindirizzate

Se non si dispone già di una condivisione file per le cartelle reindirizzate, utilizzare la procedura seguente per creare una condivisione file in un server che esegue Windows Server 2012.

>[!NOTE]
>Alcune funzionalità potrebbero diversa o non essere disponibile se decidi di creare la condivisione file in un server che esegue un'altra versione di Windows Server.

Ecco come creare una condivisione file in Windows Server 2012 e Windows Server 2016:

1. Nel riquadro di spostamento di Server Manager, seleziona **servizi File e archiviazione**e quindi seleziona **condivisioni** per visualizzare la pagina di condivisioni.
2. Nel riquadro **condivisioni** , seleziona **le attività**e quindi seleziona **Nuova condivisione**. Viene visualizzata la creazione guidata nuova condivisione.
3. Nella pagina **Seleziona profilo** , seleziona **Condivisione SMB-rapida**. Se hai Gestione risorse File Server installati e stai usando le proprietà di gestione di cartella, seleziona invece **Condivisione SMB - Advanced**.
4. Nella pagina **Condividono posizione** , seleziona il server e il volume in cui si desidera creare la condivisione.
5. Nella pagina **Nome della condivisione** , digita un nome per la condivisione (ad esempio, **gli utenti$**) nella casella **nome condivisione** .
    >[!TIP]
    >Quando si crea la condivisione, nascondere la condivisione inserendo un ```$``` dopo il nome della condivisione. Questo consente di nascondere la condivisione accidentalmente.
6. Nella pagina **Altre impostazioni** , deseleziona la casella di controllo attiva la disponibilità continua, se presente e facoltativamente seleziona le caselle di controllo di **accesso ai dati crittografa** e **abilitare l'enumerazione basata sull'accesso** .
7. Nella pagina **delle autorizzazioni** , selezionare **Personalizza autorizzazioni...**. Viene visualizzata la finestra di dialogo Impostazioni avanzate di sicurezza.
8. Seleziona **disabilitare l'ereditarietà**e quindi seleziona **Converti ereditato le autorizzazioni in autorizzazione esplicita dell'utente su questo oggetto**.
9. Impostare le autorizzazioni come descritto nella tabella 1 e illustrato nella figura 1, rimuovendo le autorizzazioni per gli account e gruppi non elencati e aggiungendo le autorizzazioni speciali il gruppo di utenti di reindirizzamento cartella che hai creato nel passaggio 1.
    
    ![Impostazione delle autorizzazioni per la condivisione di cartelle reindirizzate](media/deploy-folder-redirection/setting-the-permissions-for-the-redirected-folders-share.png)
    
    **Figura 1** Impostazione delle autorizzazioni per la condivisione di cartelle reindirizzate
10. Se si sceglie il profilo di **Condivisione SMB - Advanced** , la pagina delle **Proprietà di gestione** , selezionare il valore di utilizzo di cartella **File degli utenti** .
11. Se hai scelto il profilo della **Condivisione SMB - Advanced** , nella pagina **Quota** , selezionare facoltativamente una quota per applicare agli utenti della condivisione.
12. Nella pagina di **Conferma** , seleziona **Crea.**

### Autorizzazioni necessarie per il file di condividono le cartelle reindirizzate di hosting

<table>
<tbody>
<tr class="odd">
<td>Account utente</td>
<td>Access</td>
<td>Ambito di applicazione</td>
</tr>
<tr class="even">
<td>Sistema</td>
<td>Controllo completo</td>
<td>Questa cartella, sottocartelle e file</td>
</tr>
<tr class="odd">
<td>Administrators</td>
<td>Controllo completo</td>
<td>Questa cartella solo</td>
</tr>
<tr class="even">
<td>Autore /</td>
<td>Controllo completo</td>
<td>Solo sottocartelle e file</td>
</tr>
<tr class="odd">
<td>Gruppo di sicurezza di utenti che necessitano di inserire i dati nella condivisione (gli utenti reindirizzamento delle cartelle)</td>
<td>Elencare cartella / leggere i dati<sup>1</sup><br />
<br />
Creare cartelle / append dati<sup>1</sup><br />
<br />
Leggi gli attributi<sup>1</sup><br />
<br />
Leggi attributi estesi<sup>1</sup><br />
<br />
Autorizzazioni di lettura<sup>1</sup></td>
<td>Questa cartella solo</td>
</tr>
<tr class="even">
<td>Altri gruppi e account</td>
<td>None (rimuovere)</td>
<td></td>
</tr>
</tbody>
</table>

1 autorizzazioni avanzate

## Passaggio 3: Creare un oggetto Criteri di gruppo per cartelle di reindirizzamento cartelle

Se non si dispone già di un oggetto Criteri di gruppo creato per le impostazioni di cartelle di reindirizzamento cartelle, utilizzare la procedura seguente per crearne uno.

Ecco come creare un oggetto Criteri di gruppo per cartelle di reindirizzamento cartelle:

1. Apri Server Manager in un computer con Gestione criteri di gruppo installato.
2. Dal menu **Strumenti** , seleziona **Gestione criteri di gruppo**.
3. Fai clic al dominio o unità Organizzativa in cui si desidera configurare cartelle di reindirizzamento cartelle, quindi selezionare **Crea un oggetto Criteri di gruppo in questo dominio e collegarlo qui**.
4. Nella finestra di dialogo **Nuovo oggetto Criteri di gruppo** , digita un nome per l'oggetto Criteri di gruppo (ad esempio, **Impostazioni di reindirizzamento cartelle**) e quindi seleziona **OK**.
5. Fai l'oggetto Criteri di gruppo appena creato e quindi deselezionare la casella di controllo **Collegamento abilitato** . Ciò impedisce che l'oggetto Criteri di gruppo viene applicato fino a quando non completare la configurazione.
6. Seleziona l'oggetto Criteri di gruppo. Nella sezione **Filtri di sicurezza** della scheda **ambito** , selezionare **Authenticated Users**e quindi seleziona **rimuovere** per impedire che l'oggetto Criteri di gruppo viene applicato a tutti gli utenti.
7. Nella sezione **Filtri di sicurezza** , scegli **Aggiungi**.
8. Nella finestra di dialogo **Selezione utenti, Computer o gruppo** , digita il nome del gruppo di sicurezza creato nel passaggio 1 (ad esempio, **Gli utenti reindirizzamento delle cartelle**) e quindi seleziona **OK**.
9. Seleziona la scheda **delega** , scegli **Aggiungi**, digitare **Authenticated Users**, fare **clic su OK**e quindi seleziona **OK** nuovamente per accettare le autorizzazioni di lettura predefinito.
    
    Questo passaggio è necessario a causa di sicurezza le modifiche apportate nel [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016).

>[!IMPORTANT]
>A causa la funzionalità di sicurezza le modifiche apportate nell' [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016), è ora necessario assegnare il gruppo Authenticated Users Delega autorizzazioni di lettura per l'oggetto Criteri di gruppo reindirizzamento cartelle - in caso contrario, l'oggetto Criteri di gruppo non vengono applicate agli utenti di, o se è già stato applicato, l'oggetto Criteri di gruppo viene rimosso, reindirizzando cartelle indietro per il PC locale. Per altre info, vedi [la distribuzione di gruppo di criteri di sicurezza Update MS16-072](https://blogs.technet.microsoft.com/askds/2016/06/22/deploying-group-policy-security-update-ms16-072-kb3163622/).

## Passaggio 4: Configurare cartelle di reindirizzamento cartelle con i file Offline

Dopo aver creato un oggetto Criteri di gruppo per le impostazioni di cartelle di reindirizzamento cartelle, modificare le impostazioni di criteri di gruppo per abilitare e configurare il reindirizzamento cartelle, come descritto nella procedura seguente.

>[!NOTE]
>File offline è abilitata per impostazione predefinita per le cartelle reindirizzate nei computer client Windows e disabilitato nei computer che eseguono Windows Server, a meno che non modificate dall'utente. Per usare criteri di gruppo per controllare se i file Offline è abilitato, Usa il **Consenti o non consentire l'uso della funzionalità di file Offline** impostazione dei criteri.
> Per informazioni su alcune delle impostazioni di altri file Offline i criteri di gruppo, vedi [Abilitare Advanced Offline i file di funzionalità](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn270369(v%3dws.11)>)e [Configurazione di criteri di gruppo per i file Offline](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc759721(v%3dws.10)>).

Ecco come configurare il reindirizzamento delle cartelle in Criteri di gruppo:

1. In Gestione criteri di gruppo, fai l'oggetto Criteri di gruppo creato (ad esempio, **Impostazioni di reindirizzamento cartelle**) e quindi seleziona **Modifica**.
2. Nella finestra di Editor Gestione criteri di gruppo, passa alla **Configurazione utente**, quindi **i criteri**, quindi **Le impostazioni di Windows**e quindi **Il reindirizzamento delle cartelle**.
3. Fai clic su una cartella in cui vuoi reindirizzare (ad esempio, **documenti**) e quindi seleziona **proprietà**.
4. Nella finestra di dialogo **proprietà** , dalla casella di **impostazione** , selezionare **Basic - reindirizza la cartella nello stesso percorso**.

    > [!NOTE]
    > Per applicare il reindirizzamento delle cartelle per i computer client che eseguono Windows XP o Windows Server 2003, seleziona la scheda **Impostazioni** e selezionare il **anche Applica criteri di reindirizzamento Windows 2000, Windows 2000 Server, Windows XP e Windows Server 2003 operativa sistemi** casella di controllo.
5. Nella sezione **percorso cartella di destinazione** , selezionare **Crea una cartella per ogni utente nel percorso radice** e quindi nella casella di **Percorso radice** , digita il percorso di condivisione file archiviare le cartelle reindirizzate, ad esempio: \\\fs1.corp.contoso.com\\ ** gli utenti$**
6. Seleziona la scheda **Impostazioni** e nella sezione **La rimozione dei criteri** , selezionare facoltativamente **reindirizzare la cartella nel percorso del profilo utente locale quando il criterio viene rimossa** (questa impostazione possa contribuire a rendere il reindirizzamento delle cartelle si comportano più prevedibile per adminisitrators e gli utenti).
7. Seleziona **OK**e quindi seleziona **Sì** nella finestra di dialogo di avviso.

## Passaggio 5: Abilitare il reindirizzamento oggetto Criteri di gruppo

Dopo aver completato la configurazione delle impostazioni dei criteri di gruppo reindirizzamento cartelle, il passaggio successivo consiste nel consentire l'oggetto Criteri di gruppo, che consentano di essere applicate a utenti interessati.

>[!TIP]
>Se si prevede di implementare il supporto di computer principale o altre impostazioni dei criteri, fallo ora, prima di abilitare l'oggetto Criteri di gruppo. Ciò impedisce che i dati utente da copiare ai computer non principale prima di supporto per computer principale è abilitato.

Ecco come abilitare l'oggetto Criteri di gruppo reindirizzamento cartelle:

1. Gestione criteri di gruppo Apri.
2. Fai l'oggetto Criteri di gruppo che hai creato e quindi seleziona **Collegamento abilitato**. Verrà visualizzata una casella di controllo accanto alla voce di menu.

## Passaggio 6: Verificare il reindirizzamento delle cartelle

Per testare il reindirizzamento delle cartelle, accedi a un computer con un account utente configurato per cartelle di reindirizzamento cartelle. Quindi verifica che le cartelle e profili vengono reindirizzati.

Ecco come verificare il reindirizzamento delle cartelle:

1. Accedi a un computer primario (se è abilitato il supporto di computer principale) con un account utente per cui è stato attivato il reindirizzamento delle cartelle.
2. Se l'utente ha connesso in precedenza al computer, aprire un prompt dei comandi con privilegi elevati e quindi digitare il comando seguente per garantire che vengono applicate le impostazioni di criteri di gruppo più recenti per il computer client:
    
    ```PowerShell
    gpupdate /force
    ```
3. Apri Esplora File.
4. Fai una cartella reindirizzata (ad esempio, la cartella documenti nella raccolta di documenti) e quindi seleziona **proprietà**.
5. Seleziona la scheda di **posizione** e verificare che il percorso visualizzato nella condivisione di file specificati invece di un percorso locale.

## Appendice a: elenco di controllo per la distribuzione di cartelle di reindirizzamento cartelle

|Stato|Azione|
|:---:|---|
|☐<br>☐<br>☐|1. Prepara dominio<br>-Aggiungere computer al dominio<br>-Creare un account utente|
|☐<br><br><br>|2. creare gruppo di sicurezza per cartelle di reindirizzamento cartelle<br>-Nome gruppo:<br>-Membri:|
|☐<br><br>|3. creare una condivisione file per le cartelle reindirizzate<br>-Nome della condivisione file:|
|☐<br><br>|4. creare un oggetto Criteri di gruppo per cartelle di reindirizzamento cartelle<br>-Nome oggetto Criteri di gruppo:|
|☐<br><br>☐<br>☐<br>☐<br>☐<br>☐|5. configurare le impostazioni di criteri di reindirizzamento cartelle e i file Offline<br>-Reindirizzate cartelle:<br>-Windows 2000, Windows XP e Windows Server 2003 supporto abilitato?<br>-File offline abilitati? (abilitata per impostazione predefinita nei computer client Windows)<br>-Modalità Offline sempre abilitata?<br>-Sincronizzazione dei file in background abilitata?<br>-Ottimizzato spostamento delle cartelle reindirizzate abilitato?|
|☐<br><br>☐<br><br>☐<br>☐|6. (facoltativo) abilita il supporto per computer principale<br>-Basate sui computer o basate sull'utente?<br>-Designare computer primario per gli utenti<br>-Posizione dell'utente e di mapping principale computer:<br>-Abilita (facoltativo) il supporto per computer primario per il reindirizzamento cartelle<br>-Abilita (facoltativo) il supporto per computer primario per i profili utente|
|☐|7. Abilita il reindirizzamento oggetto Criteri di gruppo|
|☐|8. test reindirizzamento|

## Cronologia delle modifiche

La tabella seguente riepiloga alcune delle modifiche più importanti di questo argomento.

|Data|Descrizione|Motivo|
|---|---|---|
|18 gennaio 2017|Aggiunta di un passaggio di [passaggio 3: creare un oggetto Criteri di gruppo per cartelle di reindirizzamento cartelle](#step-3:-create-a-gpo-for-folder-redirection) delegare le autorizzazioni di lettura per Authenticated Users, che è ora necessario a causa di un aggiornamento di sicurezza di criteri di gruppo.|Feedback dei clienti.|

## Ulteriori informazioni

* [Reindirizzamento cartelle, file offline e profili utente mobili](folder-redirection-rup-overview.md)
* [Distribuire i computer primari per cartelle di reindirizzamento cartelle e profili utente mobili](deploy-primary-computers.md)
* [Abilitare le funzionalità avanzate file Offline](enable-always-offline.md)
* [Dichiarazione di supporto Microsoft per i dati del profilo utente replicato](https://blogs.technet.microsoft.com/askds/2010/09/01/microsofts-support-statement-around-replicated-user-profile-data/)
* [Trasferire localmente le app con DISM](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh852635(v=win.10)>)
* [Risoluzione dei problemi di creazione di pacchetti, la distribuzione e query delle App basate su Windows Runtime](https://msdn.microsoft.com/library/windows/desktop/hh973484.aspx)