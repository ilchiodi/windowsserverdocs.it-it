---
title: Aggiornamenti di sicurezza estesi per Windows Server 2008 e 2008 R2
description: Informazioni su come usare gli aggiornamenti di sicurezza estesi per Windows Server 2008 e 2008 R2 dopo la fine del ciclo di vita del supporto.
ms.prod: windows-server
ms.technology: server-general
ms.mktglfcycl: manage
author: iainfoulds
ms.author: iainfou
ms.topic: get-started-article
ms.localizationpriority: high
ms.date: 02/21/2020
ms.openlocfilehash: 6c9d732b6ec3d8ceb65c691ab143f09dd8f10f23
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "77552529"
---
# <a name="how-to-use-windows-server-2008-and-2008-r2-extended-security-updates-esu"></a>Come usare gli aggiornamenti di sicurezza estesi per Windows Server 2008 e 2008 R2

>Si applica a: Windows Server 2008 e Windows Server 2008 R2

Windows Server 2008 e Windows Server 2008 R2 hanno raggiunto la fine del ciclo di vita del supporto il 14 gennaio 2020. L'opzione Long Term Servicing Channel (LTSC) di Windows Server prevede un minimo di dieci anni di supporto: cinque anni per il supporto Mainstream e cinque anni per il supporto esteso. Questo supporto include aggiornamenti regolari della sicurezza.

La fine del supporto implica anche la fine degli aggiornamenti di sicurezza. Questo scenario può causare problemi di sicurezza o di conformità e mettere a rischio le applicazioni aziendali. Microsoft consiglia di [eseguire l'aggiornamento alla versione corrente di Windows Server](modernize-windows-server-2008.md) per accedere ai livelli più avanzati di sicurezza, prestazioni e innovazione.

Se non hai ancora aggiornato i server, le opzioni seguenti ti consentiranno di proteggere le app e i dati durante la transizione:

* Esegui la migrazione degli attuali carichi di lavoro di Windows Server 2008 e 2008 R2 così come sono a macchine virtuali di Azure.
  * Questa migrazione ad Azure fornisce automaticamente altri tre anni di aggiornamenti di sicurezza estesi. Non sono previsti costi aggiuntivi per gli aggiornamenti di sicurezza estesi oltre il costo delle VM di Azure e non è necessaria alcuna configurazione aggiuntiva.
* Acquista una sottoscrizione di aggiornamenti di sicurezza estesi per i server e mantieni la protezione finché non sei pronto a passare a una versione più recente di Windows Server.
  * Questi aggiornamenti vengono forniti fino a tre anni dopo la data della fine del ciclo di vita del supporto.

Dopo tre anni di estensione, interromperemo gli aggiornamenti per Windows Server 2008 e 2008 R2. Ti consigliamo di aggiornare Windows Server a una versione più recente prima possibile.

## <a name="what-are-extended-security-updates-for-windows-server"></a>Che cosa sono gli aggiornamenti di sicurezza estesi per Windows Server?

Gli aggiornamenti di sicurezza estesi per Windows Server includono aggiornamenti di sicurezza e bollettini classificati come *critici* e *importanti*, per un massimo di tre anni dopo il 14 gennaio 2020. Gli aggiornamenti di sicurezza estesi non includono quanto segue:

* Nuove funzionalità
* Hotfix non di sicurezza richiesti dai clienti
* Richieste di modifica della progettazione

Per altre informazioni, vedi le[domande frequenti sugli aggiornamenti di sicurezza estesi](https://www.microsoft.com/cloud-platform/extended-security-updates).

## <a name="how-to-use-extended-security-updates"></a>Come usare gli aggiornamenti della sicurezza estesi

Se eseguite in Azure, le macchine virtuali Windows Server 2008 o 2008 R2 sono automaticamente abilitate a ricevere gli aggiornamenti della sicurezza estesi. Per usare gli aggiornamenti della sicurezza estesi con le macchine virtuali di Azure, non devi configurare alcuna impostazione e non sono previsti costi aggiuntivi. Gli aggiornamenti della sicurezza estesi vengono inviati automaticamente alle macchine virtuali di Azure opportunamente configurate per riceverli.

Se usi altri ambienti, ad esempio server fisici o macchine virtuali locali, devi richiedere e configurare manualmente gli aggiornamenti della sicurezza estesi. Puoi acquistare gli aggiornamenti della sicurezza estesi tramite i programmi per contratti multilicenza, ad esempio Enterprise Agreement (EA), Enterprise Agreement Subscription (EAS), Enrollment for Education Solutions (EES) o Server and Cloud Enrollment (SCE).

Dopo aver acquistato gli aggiornamenti della sicurezza estesi, puoi ottenere le chiavi usando uno dei metodi seguenti:

* Se vuoi ottenere le chiavi degli aggiornamenti della sicurezza estesi dal portale di Azure, puoi [registrarti per ricevere gli aggiornamenti della sicurezza estesi nel portale di Azure](#register-for-extended-security-updates-on-azure-portal).
* Per ottenere le chiavi senza usare il portale di Azure, puoi anche [accedere al Centro servizi per contratti multilicenza](#sign-in-to-the-microsoft-volume-licensing-service-center).

### <a name="register-for-extended-security-updates-on-azure-portal"></a>Registrarsi per ricevere gli aggiornamenti della sicurezza estesi nel portale di Azure

Per usare gli aggiornamenti della sicurezza estesi in macchine virtuali non di Azure, crea un codice ad attivazione multipla (MAK) e applicalo ai computer Windows Server 2008 e 2008 R2. Con il codice MAK, i server Windows Update riconoscono che puoi continuare a ricevere gli aggiornamenti della sicurezza. Per registrarti e ricevere gli aggiornamenti della sicurezza estesi e per gestire queste chiavi, usa il portale di Azure anche se usi solo computer locali.

> [!NOTE]
> Se esegui Windows Server 2008 e 2008 R2 in macchine virtuali di Azure, non è necessario che ti registri per ricevere gli aggiornamenti della sicurezza estesi. Se usi altri ambienti, ad esempio server fisici o macchine virtuali locali, [acquista gli aggiornamenti della sicurezza estesi](https://www.microsoft.com/licensing/how-to-buy/how-to-buy) prima di provare a registrarti e usarli.

Per registrare la macchina virtuale per ricevere gli aggiornamenti della sicurezza estesi e creare una chiave, apri il portale di Azure e segui queste istruzioni:

1. Accedi al [portale di Azure](https://portal.azure.com/).
2. Nella casella di ricerca nella parte superiore della portale di Azure cerca e seleziona **Aggiornamenti della sicurezza estesi**.

    ![Ricerca degli aggiornamenti della sicurezza estesi nel portale di Azure](media/extended-security-updates/esu-portal-search.png)

    Se non hai mai usato gli aggiornamenti della sicurezza estesi, seleziona **+ Crea** per creare prima una specifica risorsa. In caso contrario, seleziona la risorsa nell'elenco.

3. In **Register for Extended Service Updates** (Registrazione per aggiornamenti di sicurezza estesi) seleziona **Get started** (Attività iniziali).

    ![Attività iniziali per gli aggiornamenti della sicurezza estesi nel portale di Azure](media/extended-security-updates/get-started-with-esu.png)

4. Per creare la prima chiave, seleziona **Ottieni chiave**.

    ![Scegliere di creare una chiave nel portale di Azure](media/extended-security-updates/get-key.png)

    Per creare la risorsa e la chiave degli aggiornamenti della sicurezza estesi, devi avere una sottoscrizione di Azure associata all'account. Se non hai una sottoscrizione di Azure associata all'account, accedi con un account utente diverso oppure crea una sottoscrizione di Azure nel portale di Azure.

    Per il corretto funzionamento dell'aggiornamento della sicurezza, alla tua sottoscrizione di Azure deve essere assegnato anche il ruolo Collaboratore. Per verificare il ruolo, immetti "Sottoscrizioni" nella casella di ricerca. Verrà visualizzata una tabella che mostra il ruolo accanto all'ID e al nome della sottoscrizione.

    Se non sei un collaboratore, puoi chiedere al proprietario della sottoscrizione di modificare il tuo ruolo. Per conoscere il proprietario della tua sottoscrizione, passa alla tabella dei ruoli descritta nel paragrafo precedente e seleziona il nome della sottoscrizione. Passa quindi al menu a sinistra della pagina e seleziona **Controllo di accesso (IAM)**  > **Assegnazioni di ruolo** e cerca la sezione "Proprietari" nella tabella.

5. Se viene visualizzata una pagina con la dicitura "Per ottenere un codice ad attivazione multipla, effettuare la registrazione", devi richiedere l'accesso all'anteprima privata prima di poter usare gli aggiornamenti della sicurezza estesi. Se questa pagina non viene visualizzata, vai avanti al passaggio 6.

   Per richiedere l'accesso, seleziona l'opzione per **partecipare all'anteprima privata**. Verrà visualizzata una finestra di messaggio di posta elettronica. Questo messaggio di posta elettronica è la richiesta di accesso al team del prodotto.
  
    Includi le informazioni seguenti nella richiesta:

    * Nome cliente
    * ID sottoscrizione di Azure
    * Numero contratto (per gli aggiornamenti della sicurezza estesi)
    * Numero di server per gli aggiornamenti della sicurezza estesi

    Al termine, invia il messaggio di posta elettronica.

    Il team esaminerà le informazioni fornite nel messaggio di posta elettronica della richiesta. Se tutto risulta corretto, ti aggiungeranno all'elenco approvato.

    Se il team non approva la richiesta, verrà visualizzato l'errore seguente:

    [Non è possibile trovare il tipo di risorsa nello spazio dei nomi 'Microsoft.WindowsESU'](https://social.msdn.microsoft.com/Forums/office/94b16a89-3149-43da-865d-abf7dba7b977/the-resource-type-could-not-be-found-in-the-namespace-microsoftwindowsesu-for-api-version).

6. In **Dettagli su Azure** seleziona la sottoscrizione di Azure, un gruppo di risorse e una posizione per la chiave.

    In **Dettagli registrazione** immetti le informazioni seguenti:

    | Impostazione             | Valore |
    |---------------------|-------|
    | Nome chiave            | Il nome visualizzato per la chiave, ad esempio *Agreement01*. |
    | Numero di contratto    | Il numero di contratto generato dal sistema Volume Licensing Contract Management, o MSLicense, per i programmi con contratto Enterprise. |
    | Numero di computer | Scegli il numero di computer in cui vuoi installare gli aggiornamenti di sicurezza estesi con questa chiave. |
    | Sistema operativo    | Scegli il sistema operativo in cui usare questa chiave, ad esempio Windows Server 2008 o Windows Server 2008 R2. |

    Al termine, seleziona **Review + register** (Revisione e registrazione).

    >[!NOTE]
    >Assicurati di aver selezionato la sottoscrizione di Azure con cui hai partecipato all'anteprima privata nel filtro globale. Seleziona il pulsante **Filtro** nella barra multifunzione del portale di Azure per verificare il filtro delle sottoscrizioni globali.
    >
    > ![Immagine della barra multifunzione del portale di Azure con il pulsante Filtro selezionato](media/azure-ribbon-filter.png)

7. Una volta completata la convalida, viene visualizzato un riepilogo delle scelte per la nuova risorsa del Registro di sistema. Se necessario, correggi gli eventuali errori di convalida o aggiorna la scelta della configurazione. Sono disponibili le [condizioni per l'utilizzo](https://azure.microsoft.com/support/legal/) e l'[informativa sulla privacy](https://privacy.microsoft.com/privacystatement) di Azure.

    Seleziona la casella per confermare di avere computer idonei e che la chiave verrà usata solo all'interno dell'organizzazione:

    ![Conferma che la chiave verrà usata solo dall'organizzazione](media/extended-security-updates/confirm-key-usage.png)

    Al termine, seleziona **Crea** per generare il codice Product Key MAK.

La registrazione per gli aggiornamenti della sicurezza estesi è ora disponibile per essere usata con i tuoi computer. La chiave creata deve essere applicata ai computer Windows Server 2008 e 2008 R2 che vuoi mantenere idonei per gli aggiornamenti di sicurezza.

### <a name="sign-in-to-the-microsoft-volume-licensing-service-center"></a>Accedere al Centro servizi per contratti multilicenza

Se non puoi accedere al portale di Azure, puoi usare il Centro servizi per contratti multilicenza per visualizzare e scaricare i codici di attivazione.

Per ottenere i codici dal Centro servizi per contratti multilicenza:

1. Passa alla [pagina del Centro servizi per contratti multilicenza](https://www.microsoft.com/vlsc) e accedi con le credenziali di Azure.

2. Seleziona **Licenze** > **Riepilogo relazioni** > **ID di licenza** > **Codici "Product Key"** .

Per altre informazioni su come ottenere gli aggiornamenti della sicurezza estesi per i dispositivi Windows idonei, vedi il [post della community tecnica](https://techcommunity.microsoft.com/t5/windows-it-pro-blog/obtaining-extended-security-updates-for-eligible-windows-devices/ba-p/1167091#).

## <a name="download-and-apply-extended-security-updates"></a>Scaricare e applicare gli aggiornamenti della sicurezza estesi

Le operazioni di recapito, download e applicazione degli aggiornamenti della sicurezza estesi non sono diverse dalle procedure di distribuzione esistenti. Gli aggiornamenti forniti tramite Aggiornamenti della sicurezza estesi riguardano solo la *sicurezza* e vengono rilasciati ogni martedì in cui vengono distribuite le patch.

Puoi installarli usando qualsiasi strumento e processo già implementato. L'unica differenza è che per scaricare e installare gli aggiornamenti, è necessario che il sistema venga registrato con la chiave generata nella sezione precedente.

Per le macchine virtuali di Azure, la procedura di abilitazione del computer per gli aggiornamenti della sicurezza estesi viene eseguita automaticamente. Gli aggiornamenti dovrebbero essere scaricati e installati senza alcuna configurazione aggiuntiva.
