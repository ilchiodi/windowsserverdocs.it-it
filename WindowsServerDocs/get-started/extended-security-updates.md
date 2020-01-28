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
ms.date: 01/23/2020
ms.openlocfilehash: 0f3ea0dacc200adaaec5064d19754ad6de0042a6
ms.sourcegitcommit: ff0db5ca093a31034ccc5e9156f5e9b45b69bae5
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725776"
---
# <a name="how-to-use-windows-server-2008-and-2008-r2-extended-security-updates-esu"></a>Come usare gli aggiornamenti di sicurezza estesi per Windows Server 2008 e 2008 R2

>Si applica a: Windows Server 2008/2008 R2

Windows Server 2008 e Windows Server 2008 R2 raggiungono la fine del ciclo di vita del supporto il 14 gennaio 2020. Il canale di manutenzione Long Term Servicing Channel (LTSC) di Windows Server prevede un minimo di dieci anni di supporto, di cui cinque anni mainstream e cinque esteso. Questo supporto include aggiornamenti regolari della sicurezza.

La fine del supporto implica anche la fine degli aggiornamenti di sicurezza. Questo scenario può causare problemi di sicurezza o di conformità e mettere a rischio le applicazioni aziendali. Microsoft consiglia di [eseguire l'aggiornamento alla versione corrente di Windows Server](modernize-windows-server-2008.md) per accedere ai livelli più avanzati di sicurezza, prestazioni e innovazione.

Se non è possibile aggiornare tutti i server entro la scadenza della fine del ciclo di vita del supporto, le opzioni seguenti consentono di proteggere applicazioni e dati durante la transizione dell'aggiornamento:

* Esegui la migrazione degli attuali carichi di lavoro di Windows Server 2008 e 2008 R2 così come sono a macchine virtuali di Azure.
    * Questa migrazione ad Azure fornisce automaticamente altri tre anni di aggiornamenti di sicurezza estesi. Non sono previsti costi aggiuntivi per gli aggiornamenti di sicurezza estesi oltre il costo delle VM di Azure e non è necessaria alcuna configurazione aggiuntiva.
* Acquista una sottoscrizione di aggiornamenti di sicurezza estesi per i server e mantieni la protezione finché non sei pronto a passare a una versione più recente di Windows Server.
    * Questi aggiornamenti vengono forniti fino a tre anni dopo la data della fine del ciclo di vita del supporto.

Al termine del periodo di tre anni di aggiornamenti estesi, non sono disponibili opzioni per ricevere altri aggiornamenti nei computer.

## <a name="what-are-extended-security-updates-for-windows-server"></a>Che cosa sono gli aggiornamenti di sicurezza estesi per Windows Server?

Gli aggiornamenti di sicurezza estesi per Windows Server includono aggiornamenti di sicurezza e bollettini classificati come *critici* e *importanti*, per un massimo di tre anni dopo il 14 gennaio 2020. Gli aggiornamenti di sicurezza estesi non includono quanto segue:

* Nuove funzionalità
* Hotfix non di sicurezza richiesti dai clienti
* Richieste di modifica della progettazione

Per altre informazioni, vedi le[domande frequenti sugli aggiornamenti di sicurezza estesi](https://www.microsoft.com/cloud-platform/extended-security-updates).

## <a name="how-to-use-extended-security-updates"></a>Come usare gli aggiornamenti di sicurezza estesi

Se eseguite in Azure, le macchine virtuali Windows Server 2008/2008 R2 sono automaticamente abilitate a ricevere gli aggiornamenti di sicurezza estesi. Per usare gli aggiornamenti di sicurezza estesi con macchine virtuali di Azure, non è necessario configurare alcuna impostazione e non sono previsti costi aggiuntivi. Gli aggiornamenti di sicurezza estesi vengono inviati automaticamente alle macchine virtuali di Azure opportunamente configurate per riceverli.

Se usi altri ambienti, ad esempio server fisici o macchine virtuali locali, devi richiedere e configurare manualmente gli aggiornamenti di sicurezza estesi. Se hai già acquistato gli aggiornamenti di sicurezza estesi, disponibili tramite programmi per contratti multilicenza come Enterprise Agreement (EA), Enterprise Agreement Subscription (EAS), Enrollment for Education Solutions (EES) o Server and Cloud Enrollment (SCE), puoi usare una delle procedure seguenti per ottenere una chiave di attivazione:

* Accedi a [Microsoft Volume Licensing Service Center](https://www.microsoft.com/Licensing/servicecenter/default.aspx) per visualizzare e ottenere le chiavi di attivazione.
* Registrati per ricevere gli aggiornamenti di sicurezza estesi nel portale di Azure e ottenere le chiavi di attivazione di Windows Server 2008/R2.
    * Per informazioni su come completare la procedura, vedi i passaggi seguenti in questo articolo.

## <a name="register-for-extended-security-updates"></a>Registrati per ricevere gli aggiornamenti di sicurezza estesi

Per usare gli aggiornamenti di sicurezza estesi, è necessario creare un codice Product Key per attivazione multipla (MAK) e applicarlo ai computer Windows Server 2008 e 2008 R2. Con questa chiave, i server Windows Update riconoscono che puoi continuare a ricevere gli aggiornamenti della sicurezza. Per registrarti e ricevere gli aggiornamenti di sicurezza estesi e per gestire questi codici, usa il portale di Azure, anche se usi solo computer locali.

> [!NOTE]
>
> Se esegui Windows Server 2008 e 2008 R2 in macchine virtuali di Azure, non è necessario che ti registri per ricevere gli aggiornamenti di sicurezza estesi. Se usi altri ambienti, ad esempio server fisici o macchine virtuali locali, [acquista gli aggiornamenti di sicurezza estesi](https://www.microsoft.com/licensing/how-to-buy/how-to-buy) prima di provare a registrarti e usare gli aggiornamenti.

> [!IMPORTANT]
>
> Segui i passaggi precedenti per acquistare gli aggiornamenti di sicurezza estesi tramite il programma per contratti multilicenza. Prima di eseguire la procedura che segue, invia un messaggio di posta elettronica a [winsvresuchamps@microsoft.com](mailto:winsvresuchamps@microsoft.com) con le informazioni seguenti per ottenere l'approvazione all'uso della funzionalità:
>
> * Nome cliente:
> * Sottoscrizione di Azure:
> * Numero di contratto EA (per gli aggiornamenti di sicurezza estesi):
> * Numero di server per gli aggiornamenti di sicurezza estesi:
>
> Il team esaminerà le informazioni fornite e aggiungerà l'utente o la sottoscrizione all'elenco approvato.
>
> Se il richiedente non viene approvato, può verificarsi l'errore seguente:
>
> [Non è possibile trovare il tipo di risorsa nello spazio dei nomi 'Microsoft.WindowsESU'](https://social.msdn.microsoft.com/Forums/office/94b16a89-3149-43da-865d-abf7dba7b977/the-resource-type-could-not-be-found-in-the-namespace-microsoftwindowsesu-for-api-version).

Per registrare VM non di Azure per gli aggiornamenti di sicurezza estesi e creare una chiave, completa la procedura seguente nel portale di Azure:

1. Accedi al [portale di Azure](https://portal.azure.com/).
1. Nella casella di ricerca nella parte superiore della portale di Azure cerca e seleziona **Aggiornamenti di sicurezza estesi**.

    ![Ricerca di aggiornamenti di sicurezza estesi nel portale di Azure](media/extended-security-updates/esu-portal-search.png)

    Se non hai mai usato gli aggiornamenti di sicurezza estesi, scegli prima **+ Crea** per creare una risorsa corrispondente. In caso contrario, seleziona la risorsa nell'elenco.

1. In **Register for Extended Service Updates** (Registrazione per aggiornamenti di sicurezza estesi) seleziona **Get started** (Attività iniziali).

    ![Attività iniziali per gli aggiornamenti di sicurezza estesi nel portale di Azure](media/extended-security-updates/get-started-with-esu.png)

1. Per creare la prima chiave, seleziona **Ottieni chiave**.

    ![Scegli di creare una chiave nel portale di Azure](media/extended-security-updates/get-key.png)

    > [!NOTE]
    > Per creare la risorsa e la chiave per gli aggiornamenti di sicurezza estesi, è necessario avere una sottoscrizione di Azure associata all'account. Se non hai una sottoscrizione di Azure associata all'account, accedi con un account utente diverso oppure crea una sottoscrizione di Azure seguendo la procedura dettagliata mostrata nel portale.

1. In **Dettagli su Azure** seleziona la sottoscrizione di Azure, un gruppo di risorse e una posizione per la chiave.

    In **Dettagli registrazione** immetti le informazioni seguenti:

    | Impostazione             | Value |
    |---------------------|-------|
    | Nome chiave            | Il nome visualizzato per la chiave, ad esempio *Agreement01*. |
    | Numero di contratto    | Il numero di contratto generato dal sistema Volume Licensing Contract Management, o MSLicense, per i programmi con contratto Enterprise. |
    | Numero di computer | Scegli il numero di computer in cui vuoi installare gli aggiornamenti di sicurezza estesi con questa chiave. |
    | Sistema operativo    | Scegli il sistema operativo in cui usare questa chiave, ad esempio Windows Server 2008 o Windows Server 2008 R2. |

    Al termine, seleziona **Review + register** (Revisione e registrazione).

1. Una volta completata la convalida, viene visualizzato un riepilogo delle scelte per la nuova risorsa del Registro di sistema. Se necessario, correggi gli eventuali errori di convalida o aggiorna la scelta della configurazione. Sono disponibili le [condizioni per l'utilizzo](https://azure.microsoft.com/support/legal/) e l'[informativa sulla privacy](https://privacy.microsoft.com/privacystatement) di Azure.

    Seleziona la casella per confermare di avere computer idonei e che la chiave verrà usata solo all'interno dell'organizzazione:

    ![Conferma che la chiave verrà usata solo dall'organizzazione](media/extended-security-updates/confirm-key-usage.png)

    Al termine, seleziona **Crea** per generare il codice Product Key MAK.

La registrazione degli aggiornamenti di sicurezza estesi è ora disponibile per l'uso con i computer. La chiave creata deve essere applicata ai computer Windows Server 2008 e 2008 R2 che vuoi mantenere idonei per gli aggiornamenti di sicurezza.

## <a name="download-and-apply-extended-security-updates"></a>Scarica e applica gli aggiornamenti di sicurezza estesi

Le procedure di distribuzione, download e applicazione degli aggiornamenti di sicurezza estesi non sono diverse dalla distribuzione. Gli aggiornamenti forniti tramite gli aggiornamenti di sicurezza estesi riguardano solo la *sicurezza* e vengono rilasciati ogni martedì di distribuzione di patch.

Puoi installarli usando qualsiasi strumento e processo già implementato. L'unica differenza è che per scaricare e installare gli aggiornamenti, è necessario che il sistema venga registrato con la chiave generata nella sezione precedente.

Per le VM di Azure, il processo di abilitazione del computer per gli aggiornamenti di sicurezza estesi viene completato automaticamente. Gli aggiornamenti dovrebbero essere scaricati e installati senza alcuna configurazione aggiuntiva.
