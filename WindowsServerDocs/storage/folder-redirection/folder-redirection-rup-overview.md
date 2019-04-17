---
title: Panoramica del reindirizzamento delle cartelle, file non in linea e i profili utente
description: Panoramica delle tecnologie di reindirizzamento cartelle, file non in linea e i profili utente.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: 4e3cf32cd718b906f16fc09901284d8520177df8
ms.sourcegitcommit: 375e94dc70c76e7aa5679c32f0f4dbc26cf7dd83
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/10/2018
ms.locfileid: "2233497"
---
# <a name="folder-redirection-offline-files-and-roaming-user-profiles-overview"></a>Panoramica del reindirizzamento delle cartelle, file non in linea e i profili utente

>Si applica a: 10 Windows, Windows 8, Windows 8.1, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016

In questo argomento viene illustrato il reindirizzamento cartelle, file non in linea (la cache sul lato client o CSC) e i profili utente (a volte denominati dei profili utente comuni) tecnologie, tra cui le novità e dove trovare ulteriori informazioni.

## <a name="technology-description"></a>Descrizione di tecnologia

Il reindirizzamento cartelle e i file offline vengono utilizzati in combinazione per reindirizzare il percorso di cartelle locali, come la cartella Documenti, su un percorso di rete, memorizzando al tempo stesso nella cache il contenuto in locale per garantire maggiore velocità e disponibilità. I profili utente mobili sono utilizzati per reindirizzare un profilo utente su un percorso di rete. Queste funzionalità utilizzato per fare riferimento come Intellimirror.

- **Reindirizzamento cartelle** consente agli utenti e amministratori di reindirizzare il percorso della cartella nota in una nuova posizione, manualmente o tramite criteri di gruppo. Il nuovo percorso può essere una cartella nel computer locale o una directory in una condivisione file. Gli utenti interagiscono con i file nella cartella reindirizzata come se ancora erano presenti nell'unità locale. Ad esempio, è possibile reindirizzare la cartella documenti, che in genere viene archiviata in un'unità locale, in un percorso di rete. I file nella cartella saranno disponibili per l'utente da qualsiasi computer della rete.
- **File non in linea** rende i file di rete disponibile per un utente, anche se la connessione di rete per il server è lenta o non disponibile. Quando si lavora online, le prestazioni accesso ai file si trova la velocità della rete e server. Quando si lavora offline, i file vengono recuperati dalla cartella file non in linea velocità di accesso locale. Un computer passa alla modalità Offline quando:
  - È stata abilitata la modalità **Sempre fuori rete**
  - Il server non è disponibile
  - La connessione di rete è inferiore a una soglia configurabile
  - L'utente passa alla modalità Offline manualmente tramite il pulsante di **lavorare offline** in Esplora risorse
- **I profili utente** reindirizza i profili utente in una condivisione file in modo che gli utenti ricevono la stessa del sistema operativo e le impostazioni dell'applicazione in più computer. Quando un utente accede a un computer con un account configurato con una condivisione di file come il percorso del profilo, il profilo dell'utente viene scaricato nel computer locale e unito nel profilo locale (se presente). Quando l'utente accede all'esterno del computer, la copia locale del profilo, comprese le modifiche verrà unita con la copia sul server del profilo. In genere, un amministratore di rete attiva i profili utente in account di dominio.

## <a name="practical-applications"></a>Applicazioni pratiche

Gli amministratori possono utilizzare il reindirizzamento delle cartelle, file non in linea e i profili utente per centralizzare l'archiviazione di dati e impostazioni utente e per fornire agli utenti la possibilità di accedere ai propri dati mentre non in linea o in caso di un'interruzione del funzionamento della rete o del server. Alcune applicazioni specifici includono:

- Centralizzare dati dai computer client per le attività amministrative, ad esempio uno strumento basato su server backup per eseguire il backup delle impostazioni e le cartelle dell'utente.
- Consentire agli utenti di continuare l'accesso ai file di rete, anche se non esiste un'interruzione del funzionamento della rete o del server.
- Ottimizzare l'utilizzo della larghezza di banda e di migliorare l'esperienza degli utenti nelle succursali che accedono a file e cartelle ospitate dal server aziendale che si trovano fuori sede.
- Abilitare gli utenti mobili accedere a file di rete durante l'utilizzo offline o reti lente.

## <a name="new-and-changed-functionality"></a>Funzionalità nuove e modificate

Nella tabella seguente vengono descritte alcune delle modifiche principali in Reindirizzamento cartelle, file non in linea e i profili utente disponibili in questa versione.

|Caratteristica/funzionalità|Novità o aggiornamento|Descrizione|
|---|---|---|
|Modalità sempre non in linea|Nuovo|Consente di accedere più rapidamente a file e dati di utilizzo della larghezza di banda inferiore utilizzando sempre non in linea, anche quando è connesso tramite una connessione ad alta velocità di rete.|
|Sincronizzazione in grado di riconoscere costo|Nuovo|Consente agli utenti di evitare i costi di utilizzo elevato dati dalla sincronizzazione durante l'utilizzo di connessioni misurate con limiti di utilizzo o durante il roaming nella rete del provider.|
|Servizio di supporto Computer|Nuovo|Consente di limitare l'utilizzo del reindirizzamento cartelle, i profili utente o entrambi solo i computer principale dell'utente.|

## <a name="always-offline-mode"></a>Modalità sempre non in linea

A partire da Windows 8 e Windows Server 2012, gli amministratori possono configurare l'esperienza degli utenti di file non in linea per sempre lavorare offline, anche quando sono connessi tramite una connessione ad alta velocità di rete. I file nella cache del file non in linea di Windows Update sincronizzando oraria in background, per impostazione predefinita.

### <a name="what-value-does-always-offline-mode-add"></a>Il valore è sempre non in linea aggiungere modalità?

La modalità sempre non in linea offre i vantaggi seguenti:

- Gli utenti riscontrino un più rapido accesso ai file di reindirizzamento cartelle, ad esempio la cartella documenti.
- Larghezza di banda di rete è ridotto, ridurre i costi su connessioni WAN costose o misurata, ad esempio una rete mobile 4 G.

### <a name="how-has-always-offline-mode-changed-things"></a>Come è cambiato operazioni in modalità sempre non in linea?

Prima di Windows 8, Windows Server 2012, gli utenti potrebbero effettuare la transizione tra le modalità in linea e non in linea, a seconda della disponibilità della rete e condizioni, anche quando la modalità di collegamento lento (noto anche come la modalità di connessione lenta) è stata abilitata e impostata su 1 millisecondo latenza inferiore al limite.

Con la modalità sempre non in linea, i computer mai effettuare la transizione alla modalità Online quando viene configurato l'impostazione di criteri di gruppo **configurare la modalità di collegamento lento** e il parametro soglia **latenza** è impostato su 1 millisecondo. Le modifiche vengono sincronizzate in background ogni 120 minuti, per impostazione predefinita, ma la sincronizzazione è configurabile tramite l'impostazione di criteri di gruppo di **Configurare la sincronizzazione in Background** .

Per ulteriori informazioni, vedere [abilitare il sempre la modalità fuori rete per fornire più rapido accesso ai file](enable-always-offline.md).

## <a name="cost-aware-synchronization"></a>Sincronizzazione in grado di riconoscere costo

Con la sincronizzazione in grado di riconoscere costo, Windows consente di disabilitare la sincronizzazione in background quando l'utente è tramite una connessione di rete misurata, ad esempio una rete mobile 4G e il sottoscrittore è quasi o tramite i limiti di larghezza di banda o nella rete del provider di roaming.

>[!NOTE]
>Connessioni di rete misurata sono in genere latenze rete round trip più lente rispetto al valore di latenza millisecondo 35 predefinito per la transizione alla modalità non in linea (connessione lenta) in Windows 8, Windows Server 2012 e Windows Server 2016. Di conseguenza, le connessioni in genere effettuare la transizione alla modalità non in linea (connessione lenta) automaticamente.

### <a name="what-value-does-cost-aware-synchronization-add"></a>Il valore di aggiungere la sincronizzazione in grado di riconoscere costo?

Sincronizzazione in grado di riconoscere costo consente agli utenti di evitare i costi di utilizzo dei dati in modo imprevisto elevata durante l'utilizzo di connessioni misurate con limiti di utilizzo o durante il roaming nella rete del provider.

### <a name="how-has-cost-aware-synchronization-changed-things"></a>Come è stata in grado di riconoscere costo sincronizzazione modificata aspetti?

Prima di Windows 8 e Windows Server 2012, gli utenti che desiderano ridurre al minimo le tariffe durante l'utilizzo di file non in linea con connessioni di rete misurata con tenere traccia delle loro utilizzo dei dati tramite gli strumenti da provider di rete per dispositivi mobili. Gli utenti manualmente potrebbero quindi passare alla modalità Offline quando sono stati di roaming, quasi il limite di larghezza di banda o supera il limite.

Con sincronizzazione in grado di riconoscere costo, Windows tiene traccia automaticamente di roaming e larghezza di banda limiti di utilizzo in connessioni misurata. Quando l'utente si sposta vicino il limite di larghezza di banda o superato il limite, Windows passa alla modalità Offline e impedisce che i tipi di sincronizzazione. Gli utenti possono avviare comunque manualmente la sincronizzazione e gli amministratori possono eseguire l'override di sincronizzazione in grado di riconoscere costo per utenti specifici, ad esempio dirigenti.

Per ulteriori informazioni, vedere [Abilitare la sincronizzazione di File in Background su reti contatore](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj127408(v%3dws.11)).

## <a name="primary-computers-for-folder-redirection-and-roaming-user-profiles"></a>Computer principale per il reindirizzamento delle cartelle e i profili utente

È ora possibile designare un gruppo di computer, noto come computer principale, per ogni utente di dominio, che consente di controllare i computer utilizzano il reindirizzamento delle cartelle, i profili utente o entrambi. Designazione computer principale è un metodo semplice ed efficace per associare i dati utente e le impostazioni computer specifico o dispositivi, semplificano la supervisione dell'amministratore, migliorare la protezione dei dati e migliorare la protezione dei profili utente da danneggiamento.

### <a name="what-value-do-primary-computers-add"></a>Il valore è aggiungere computer principale?

Esistono quattro principali vantaggi per designare computer principale per gli utenti:

- L'amministratore può specificare quali computer gli utenti possono utilizzare per accedere alle loro dati reindirizzato e le impostazioni. Ad esempio, l'amministratore può scegliere il roaming delle impostazioni e dei dati utente tra un utente desktop e portatili e per il roaming non le informazioni quando l'utente accede a qualsiasi altro computer, ad esempio un computer in sala conferenze.
- Designazione primario computer consente di ridurre la protezione e il rischio di privacy di uscita dati personali o aziendali residui nei computer a cui l'utente ha eseguito l'accesso. Ad esempio, un responsabile generale che accede al computer di un dipendente per l'accesso temporaneo non tralasciare i dati personali o aziendali.
- Computer principale consentire all'amministratore di ridurre il rischio di configurata in modo improprio o in caso contrario profilo danneggiato, conseguenti roaming in modo diverso tra configurato sistemi, ad esempio tra computer basati su x64 e x86.
- La quantità di tempo necessario per il dell'utente prima accesso in un computer non primario, ad esempio un server, è più veloce perché profilo utente e/o le cartelle reindirizzate dell'utente non vengono scaricate. Sono inoltre ridotto orari di disconnessione, perché le modifiche al profilo utente non sono necessario essere caricati durante la condivisione file.

### <a name="how-have-primary-computers-changed-things"></a>Modalità principali computer sono cambiate aspetti?

Per limitare il download dei dati personali dell'utente ai computer principale, le tecnologie di reindirizzamento cartelle e i profili utente eseguono le verifiche di logica seguenti quando un utente accede a un computer:

1. Il sistema operativo Windows controlla le nuove impostazioni di criteri di gruppo (**Download i profili nei computer primario solo** e **reindirizzamento cartelle solo computer principale**) per determinare se l'attributo **MsDS-principale-Computer** nella attivo Servizi di dominio directory (AD DS) deve influenzare la decisione di roaming profilo dell'utente o si applica il reindirizzamento delle cartelle.
2. Se l'impostazione dei criteri Abilita il supporto di computer principale, viene verificato che lo schema di Active Directory supporta l'attributo **MsDS-principale-Computer** . In caso affermativo, Windows determina se il computer in cui l'utente è connesso il viene designato come computer principale per l'utente come indicato di seguito:
    1. Se il computer è uno dei computer principale dell'utente, Windows applica le impostazioni di reindirizzamento cartelle e i profili utente.
    2. Se il computer non è uno dei computer principale dell'utente, Windows viene caricato il profilo dell'utente memorizzata nella cache locale, se presente, o crea un nuovo profilo locale. Windows rimuove anche tutte le cartelle reindirizzate esistenti in base all'azione di rimozione specificato dall'impostazione di criteri di gruppo applicato in precedenza, viene mantenuto nella configurazione del reindirizzamento delle cartelle locale.

Per ulteriori informazioni, vedere [Distribuire computer primario per il reindirizzamento delle cartelle e i profili utente](deploy-primary-computers.md)

## <a name="hardware-requirements"></a>Requisiti hardware

Reindirizzamento cartelle, file non in linea e i profili utente richiede un computer basato su x86 o x64 e non supportati da parte di Windows nel computer che eseguono WOA ARM.

## <a name="software-requirements"></a>Requisiti software

Per designare i computer principale, l'ambiente deve soddisfare i requisiti seguenti:

- Lo schema di servizi di dominio Active Directory (AD DS) deve essere aggiornato per includere lo schema di Windows Server 2012 e condizioni (installazione automatica di una versione successiva controller di dominio o Windows Server 2012 consente di aggiornare lo schema). Per ulteriori informazioni sull'aggiornamento allo schema di Active Directory, vedere [Aggiornare i controller di dominio di Windows Server 2016](../../identity/ad-ds/deploy/upgrade-domain-controllers.md).
- Computer client deve eseguire 10 Windows, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 ed essere aggiunto al dominio di Active Directory che si sta gestendo.

## <a name="more-information"></a>Ulteriori informazioni

Per altre informazioni correlate, vedi le risorse seguenti.

|Tipo di contenuto|Riferimenti|
|---|---|
|Valutazione del prodotto|[Supporto di Information Worker con servizi File affidabile e l'archiviazione](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831495(v%3dws.11)>)<br>[What's New in file non in linea](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff183315(v=ws.10)>) (Windows 7 e Windows Server 2008 R2)<br>[What's New in file non in linea per Windows Vista](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-vista/cc749449(v=ws.10)>)<br>[Modifiche apportate al file non in linea in Windows Vista](<https://technet.microsoft.com/library/2007.11.offline.aspx>) (TechNet Magazine)|
|Distribuzione|[Distribuire il reindirizzamento delle cartelle, file non in linea e i profili utente](deploy-folder-redirection.md)<br>[Implementazione di una soluzione di centralizzazione dati dell'utente finale: reindirizzamento cartelle e file non in linea tecnologia convalida e la distribuzione](http://download.microsoft.com/download/3/0/1/3019A3DA-2F41-4F2D-BBC9-A6D24C4C68C4/Implementing%20an%20End-User%20Data%20Centralization%20Solution.docx)<br>[Gestione di Roaming Guida alla distribuzione dei dati utente](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-vista/cc766489(v=ws.10)>)<br>[Configurazione di nuovi Offline file funzionalità per la Guida dettagliata computer Windows 7](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff633429(v=ws.10)>)<br>[Utilizzo del reindirizzamento delle cartelle](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753996(v=ws.11)>)<br>[L'implementazione di reindirizzamento cartelle](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc737434(v=ws.10)>) (Windows Server 2003)|
|Strumenti e impostazioni|[File non in linea nel sito MSDN](https://msdn.microsoft.com/library/cc296092.aspx)<br>[Criteri di gruppo di file non in linea](https://msdn.microsoft.com/library/ms878937.aspx) (Windows 2000)|
|Risorse della community|[Forum di Servizi file e archiviazione](https://social.technet.microsoft.com/forums/windowsserver/home?forum=winserverfiles)<br>[Per la verità Mr Script! Come è possibile lavorare con la funzionalità di file non in linea in Windows?](<https://blogs.technet.microsoft.com/heyscriptingguy/2009/06/02/hey-scripting-guy-how-can-i-enable-and-disable-offline-files/>)<br>[Per la verità Mr Script! Come posso abilitare e disabilitare file non in linea?](<https://blogs.technet.microsoft.com/heyscriptingguy/2009/06/02/hey-scripting-guy-how-can-i-enable-and-disable-offline-files/>)|
Tecnologie correlate|[Identità e dell'accesso in Windows Server](../../identity/identity-and-access.md)<br>[Archiviazione in WindowsServer](../storage.md)<br>[Gestione di accesso remoto e server](../../remote/index.md)|