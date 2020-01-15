---
title: Panoramica di reindirizzamento cartelle, file offline e profili utente mobili
description: Panoramica delle tecnologie per il reindirizzamento delle cartelle, il File offline e i profili utente mobili.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: a7c37638e25fc0d16447ab57bf369255dab9c859
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950254"
---
# <a name="folder-redirection-offline-files-and-roaming-user-profiles-overview"></a>Panoramica di reindirizzamento cartelle, file offline e profili utente mobili

>Si applica a: Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2

In questo argomento vengono illustrati il reindirizzamento delle cartelle, File offline (memorizzazione nella cache sul lato client) e i profili utente mobili (noti anche come RUP), incluse le novità e le posizioni in cui trovare informazioni aggiuntive.

## <a name="technology-description"></a>Descrizione della tecnologia

Il reindirizzamento cartelle e i file offline vengono utilizzati in combinazione per reindirizzare il percorso di cartelle locali, come la cartella Documenti, su un percorso di rete, memorizzando al tempo stesso nella cache il contenuto in locale per garantire maggiore velocità e disponibilità. I profili utente mobili sono utilizzati per reindirizzare un profilo utente su un percorso di rete. Queste funzionalità venivano collettivamente indicate come Intellimirror.

- **Reindirizzamento cartelle** consente agli utenti e agli amministratori di reindirizzare il percorso di una cartella nota a una nuova posizione, manualmente o utilizzando criteri di gruppo. La nuova posizione può essere rappresentata da una cartella nel computer locale o da una directory in una condivisione file. Gli utenti possono interagire con la cartella reindirizzata come se si trovasse ancora nell'unità locale. È ad esempio possibile reindirizzare su un percorso di rete la cartella Documenti, generalmente archiviata in un'unità locale. I file presenti nella cartella saranno disponibili per l'utente da qualsiasi computer della rete.
- **File offline** rende i file di rete disponibili per un utente, anche se la connessione di rete al server non è disponibile o lenta. In modalità online, l'accesso ai file avviene alla velocità della rete e del server. In modalità offline, i file vengono recuperati dalla cartella dei file offline alle velocità di accesso locale. Un computer passa alla modalità offline nei seguenti casi:
  - La modalità **Always offline** è stata abilitata
  - Il server non è disponibile
  - La connessione di rete è più lenta rispetto a un valore di soglia configurabile
  - L'utente passa manualmente alla modalità offline tramite il pulsante **Offline** in Esplora risorse
- I **profili utente mobili** reindirizza i profili utente a una condivisione file in modo che gli utenti ricevano le stesse impostazioni del sistema operativo e dell'applicazione in più computer. Quando un utente accede a un computer usando un account configurato con una condivisione file come percorso del profilo, il profilo dell'utente viene scaricato nel computer locale e Unito al profilo locale (se presente). Quando l'utente si disconnette dal computer, la copia locale del profilo, incluse eventuali modifiche, viene unita alla copia del profilo nel server. In genere, un amministratore di rete Abilita i profili utente mobili sugli account di dominio.

## <a name="practical-applications"></a>Applicazioni pratiche

Gli amministratori possono usare il reindirizzamento cartelle, i file offline e i profili utente mobili per centralizzare l'archiviazione di dati utente e impostazioni, nonché per offrire agli utenti la possibilità di accedere ai dati in modalità offline o in caso di interruzioni della rete o del server. Di seguito sono riportati alcuni esempi di applicazioni specifiche:

- Centralizzare i dati dai computer client per attività amministrative, come l'utilizzo di uno strumento di backup basato su server per eseguire il backup di cartelle e impostazioni degli utenti.
- Consentire agli utenti di continuare ad accedere ai file di rete, anche se si verifica un'interruzione di rete o del server.
- Ottimizzare l'utilizzo della larghezza di banda e migliorare l'esperienza degli utenti presso le succursali che accedono a file e cartelle ospitati in server aziendali che non si trovano in loco.
- Consentire agli utenti mobili di accedere a file di rete in modalità offline o in reti lente.

## <a name="new-and-changed-functionality"></a>Funzionalità nuove e modificate

Nella tabella seguente vengono descritte alcune delle principali modifiche introdotte per il reindirizzamento cartelle, i file offline e i profili utente mobili, disponibili in questa versione.

| Caratteristica/funzionalità | Novità o aggiornamento | Descrizione |
| --- | --- | --- |
| Modalità sempre offline | Nuova | Consente l'accesso più rapido ai file e un minore utilizzo della larghezza di banda utilizzando sempre la modalità offline, anche in presenza di una connessione alla rete ad alta velocità. |
| Sincronizzazione che tiene conto dei costi | Nuova | Consente agli utenti di evitare i costi elevati di utilizzo dei dati dalla sincronizzazione durante l'utilizzo di connessioni a consumo con limiti di utilizzo o durante il roaming nella rete di un altro provider. |
| Supporto del computer primario | Nuova | Consente di limitare l'uso del reindirizzamento cartelle, dei profili utente mobili o di entrambi i computer primari di un utente. |

## <a name="always-offline-mode"></a>Modalità sempre offline

A partire da Windows 8 e Windows Server 2012, gli amministratori possono configurare l'esperienza degli utenti di File offline per lavorare sempre offline anche quando sono connessi tramite una connessione di rete ad alta velocità. I file vengono aggiornati da Windows nella cache dei file offline eseguendo una sincronizzazione in background ogni ora per impostazione predefinita.

### <a name="what-value-does-always-offline-mode-add"></a>Quale valore viene sempre aggiunto in modalità offline?

La modalità sempre offline offre i vantaggi seguenti:

- Gli utenti possono accedere più rapidamente ai file nelle cartelle reindirizzate, come la cartella Documenti.
- Riduzione della larghezza di banda, con conseguente diminuzione di costi su connessioni WAN costose o connessioni a consumo, come la rete mobile 4G.

### <a name="how-has-always-offline-mode-changed-things"></a>Come è stata modificata la modalità sempre offline?

Prima di Windows 8, Windows Server 2012, gli utenti eseguivano la transizione tra le modalità online e offline, a seconda della disponibilità e delle condizioni della rete, anche quando la modalità di collegamento lento (nota anche come modalità di connessione lenta) era abilitata e impostata su 1 millisecondo. soglia di latenza.

Con la modalità always offline, i computer non passano mai alla modalità online quando è configurata l'impostazione di configurazione Criteri di gruppo a **collegamento lento** e il parametro soglia di **latenza** è impostato su 1 millisecondo. Le modifiche vengono sincronizzate in background ogni 120 minuti per impostazione predefinita, ma la sincronizzazione è configurabile tramite l'impostazione di Criteri di gruppo **Configure Background Sync** .

Per altre informazioni, vedere [Enable the Always Offline Mode to Provide Faster Access to Files](enable-always-offline.md).

## <a name="cost-aware-synchronization"></a>Sincronizzazione che tiene conto dei costi

Con la sincronizzazione compatibile con i costi, Windows Disabilita la sincronizzazione in background quando l'utente utilizza una connessione di rete a consumo, ad esempio una rete mobile 4G e l'abbonato è vicino o supera il limite di larghezza di banda oppure è in roaming sulla rete di un altro provider.

> [!NOTE]
> Le connessioni di rete a consumo presentano in genere latenze di rete di andata e ritorno inferiori al valore di latenza predefinito di 35 millisecondi per la transizione alla modalità offline (connessione lenta) in Windows 8, Windows Server 2019, Windows Server 2016 e Windows Server 2012. Per questo motivo, queste connessioni passano di solito automaticamente alla modalità Offline (connessione lenta).

### <a name="what-value-does-cost-aware-synchronization-add"></a>Quale valore aggiunge la sincronizzazione con i costi?

La sincronizzazione in grado di riconoscere i costi consente agli utenti di evitare costi di utilizzo dei dati inaspettatamente elevati durante l'utilizzo di connessioni a consumo con limiti di utilizzo o durante il roaming nella rete di un altro provider.

### <a name="how-has-cost-aware-synchronization-changed-things"></a>Come è cambiata la sincronizzazione con i costi?

Prima di Windows 8 e Windows Server 2012, gli utenti che volevano ridurre i costi durante l'utilizzo di File offline su connessioni di rete a consumo, dovevano tenere traccia dell'utilizzo dei dati tramite strumenti del provider di rete mobile. Gli utenti avevano così la possibilità di passare manualmente alla modalità offline in caso di roaming, in prossimità del limite della larghezza di banda o in caso di superamento del limite.

Con la sincronizzazione compatibile con i costi, Windows rileva automaticamente i limiti di utilizzo della larghezza di banda e del roaming durante le connessioni a consumo. In caso di roaming, in prossimità del limite di larghezza di banda o in caso di superamento del limite, viene attivata automaticamente la modalità offline e viene impedita del tutto la sincronizzazione. Gli utenti possono ancora avviare manualmente la sincronizzazione e gli amministratori possono evitare l'impostazione di sincronizzazione che tiene conto dei costi per utenti specifici, come i dirigenti.

Per altre informazioni, vedere [Enable Background File Synchronization on Metered Networks](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj127408(v%3dws.11)).

## <a name="primary-computers-for-folder-redirection-and-roaming-user-profiles"></a>Computer primari per il reindirizzamento cartelle e i profili utente mobili

È ora possibile designare un set di computer, noti come computer primari, per ogni utente di dominio, che consente di controllare quali computer utilizzano il reindirizzamento cartelle, i profili utente mobili o entrambi. È facile designare i computer primari e si tratta di un metodo efficace per associare dati e impostazioni degli utenti a computer o dispositivi specifici, semplificare le attività di supervisione degli amministratori, migliorare la sicurezza dei dati e proteggere i profili utente da eventuali danneggiamenti.

### <a name="what-value-do-primary-computers-add"></a>Valore aggiunto dai computer primari

Esistono quattro vantaggi principali della designazione di computer primari per gli utenti:

- L'amministratore può specificare quali computer possono utilizzare gli utenti per accedere ai dati e alle impostazioni reindirizzati. Ad esempio, l'amministratore può scegliere di eseguire il roaming dei dati e delle impostazioni utente tra il desktop e il computer portatile di un utente e di non eseguire il roaming delle informazioni quando l'utente accede a un altro computer, ad esempio un computer della sala riunioni.
- La designazione dei computer primari riduce i rischi a livello di sicurezza e privacy che emergono quando si lasciano dati personali o aziendali residui nei computer a cui accedono gli utenti. Ad esempio, un responsabile generale che accede al computer di un dipendente per l'accesso temporaneo non lascia alcun dato personale o aziendale.
- I computer primari consentono all'amministratore di ridurre i rischi correlati a un profilo configurato in modo non corretto o in altro modo danneggiato, in seguito al roaming tra computer con configurazioni diverse, come i computer x86 e x64.
- La quantità di tempo necessaria per il primo accesso di un utente in un computer non primario, ad esempio un server, è più veloce perché il profilo utente mobile dell'utente e/o le cartelle reindirizzate non vengono scaricati. Anche i tempi di disconnessione risultano ridotti, perché le modifiche al profilo utente non devono essere caricate nella condivisione file.

### <a name="how-have-primary-computers-changed-things"></a>In che modo i computer primari hanno modificato gli elementi?

Per limitare il download dei dati utente privati nei computer primari, le tecnologie di reindirizzamento cartelle e profili utente mobili eseguono i controlli logici seguenti al momento dell'accesso a un computer:

1. Il sistema operativo Windows controlla le nuove impostazioni di Criteri di gruppo (**Scarica profili mobili solo nei computer primari** e **Reindirizza cartelle solo nei computer primari**) per stabilire se l'attributo **msDS-Primary-computer** in Active Directory Domain Services (ad DS) deve influenzare la decisione di eseguire il roaming del profilo dell'utente o applicare il reindirizzamento cartelle.
2. Se l'impostazione dei criteri consente il supporto dei computer primari, Windows verifica che lo schema di Servizi di dominio Active Directory supporti l'attributo **msDS-Primary-Computer** . In caso affermativo, Windows stabilisce se il computer a cui sta accedendo l'utente è designato come computer primario, come segue:
    1. Se il computer è uno dei computer primari dell'utente, Windows applica i profili utente mobili e le impostazioni di Reindirizzamento cartelle.
    2. Se il computer non è uno dei computer primari dell'utente, viene caricato il profilo locale memorizzato nella cache dell'utente, se presente, oppure viene creato un nuovo profilo locale. Vengono inoltre rimosse eventuali cartelle reindirizzate esistenti, in base all'azione di rimozione specificata dall'impostazione di Criteri di gruppo applicata in precedenza e mantenuta nella configurazione locale di reindirizzamento cartelle.

Per altre informazioni, vedere [Deploy Primary Computers for Folder Redirection and Roaming User Profiles](deploy-primary-computers.md)

## <a name="hardware-requirements"></a>Requisiti hardware

Per il reindirizzamento cartelle, i file offline e i profili utente mobili è necessario un computer x64 o x86 e queste funzionalità non sono supportate in computer WOA (Windows on ARM).

## <a name="software-requirements"></a>Requisiti software

Per designare i computer primari, l'ambiente deve soddisfare i requisiti seguenti:

- È necessario aggiornare lo schema di Active Directory Domain Services (AD DS) per includere lo schema e le condizioni di Windows Server 2012 (l'installazione di un controller di dominio Windows Server 2012 o versione successiva aggiorna automaticamente lo schema). Per ulteriori informazioni sull'aggiornamento dello schema di servizi di dominio Active Directory, vedere [aggiornare controller di dominio a Windows Server 2016](../../identity/ad-ds/deploy/upgrade-domain-controllers.md).
- I computer client devono eseguire Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 ed essere aggiunti al dominio Active Directory che si sta gestendo.

## <a name="more-information"></a>Altre informazioni

Per altre informazioni correlate, vedi le risorse seguenti.

| Tipo di contenuto | Riferimenti |
| --- | --- |
| Valutazione del prodotto | [Supporto degli Information Worker con servizi file e archiviazione affidabili](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831495(v%3dws.11)>)<br>[Novità di file offline](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff183315(v=ws.10)>) (Windows 7 e windows Server 2008 R2)<br>[Novità di File offline per Windows Vista](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-vista/cc749449(v=ws.10)>)<br>[Modifiche apportate a file offline in Windows Vista](<https://technet.microsoft.com/library/2007.11.offline.aspx>) (TechNet Magazine) |
| Distribuzione | [Distribuire il reindirizzamento cartelle, File offline e i profili utente mobili](deploy-folder-redirection.md)<br>[Implementazione di una soluzione di centralizzazione dei dati per gli utenti finali: Reindirizzamento cartelle e File offline convalida e distribuzione della tecnologia](https://download.microsoft.com/download/3/0/1/3019A3DA-2F41-4F2D-BBC9-A6D24C4C68C4/Implementing%20an%20End-User%20Data%20Centralization%20Solution.docx)<br>[Guida alla gestione della distribuzione dei dati utente mobili](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-vista/cc766489(v=ws.10)>)<br>[Guida dettagliata alla configurazione di nuove funzionalità File non in linea per computer Windows 7](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff633429(v=ws.10)>)<br>[Uso del reindirizzamento cartelle](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753996(v=ws.11)>)<br>[Implementazione del reindirizzamento cartelle](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc737434(v=ws.10)>) (Windows Server 2003) |
| Strumenti e impostazioni | [File offline su MSDN](https://msdn.microsoft.com/library/cc296092.aspx)<br>Guida di [riferimento a File offline criteri di gruppo](https://msdn.microsoft.com/library/ms878937.aspx) (Windows 2000) |
| Risorse della community | [Forum su Servizi file e archiviazione](https://social.technet.microsoft.com/forums/windowsserver/home?forum=winserverfiles)<br>[Salve, Scripting Guy! Come è possibile utilizzare la funzionalità File offline di Windows?](<https://blogs.technet.microsoft.com/heyscriptingguy/2009/06/02/hey-scripting-guy-how-can-i-enable-and-disable-offline-files/>)<br>[Salve, Scripting Guy! Come è possibile abilitare e disabilitare File offline?](<https://blogs.technet.microsoft.com/heyscriptingguy/2009/06/02/hey-scripting-guy-how-can-i-enable-and-disable-offline-files/>) |
| Tecnologie correlate|[Identità e accesso in Windows Server](../../identity/identity-and-access.md)<br>[Archiviazione in Windows Server](../storage.md)<br>[Accesso remoto e gestione del server](../../remote/index.md) |