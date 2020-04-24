---
title: Panoramica di reindirizzamento cartelle, file offline e profili utente mobili
description: Panoramica delle tecnologie Reindirizzamento cartelle, File offline e Profili utente mobili.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: a7c37638e25fc0d16447ab57bf369255dab9c859
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "75950254"
---
# <a name="folder-redirection-offline-files-and-roaming-user-profiles-overview"></a>Panoramica di reindirizzamento cartelle, file offline e profili utente mobili

>Si applica a: Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2

Questo argomento illustra le tecnologie Reindirizzamento cartelle, File offline (memorizzazione nella cache sul lato client) e Profili utente mobili, incluse le novità e l'indicazione su dove reperire altre informazioni.

## <a name="technology-description"></a>Descrizione della tecnologia

Il reindirizzamento cartelle e i file offline vengono utilizzati in combinazione per reindirizzare il percorso di cartelle locali, come la cartella Documenti, su un percorso di rete, memorizzando al tempo stesso nella cache il contenuto in locale per garantire maggiore velocità e disponibilità. I profili utente mobili sono utilizzati per reindirizzare un profilo utente su un percorso di rete. Queste funzionalità venivano collettivamente indicate come Intellimirror.

- La funzionalità **Reindirizzamento cartelle** consente a utenti e amministratori di reindirizzare il percorso di una cartella nota su una nuova posizione, manualmente o tramite Criteri di gruppo. La nuova posizione può essere rappresentata da una cartella nel computer locale o da una directory in una condivisione file. Gli utenti possono interagire con la cartella reindirizzata come se si trovasse ancora nell'unità locale. È ad esempio possibile reindirizzare su un percorso di rete la cartella Documenti, generalmente archiviata in un'unità locale. I file presenti nella cartella saranno disponibili per l'utente da qualsiasi computer della rete.
- La funzionalità **File offline** rende i file di rete disponibili per un utente anche in caso di connessione di rete al server non disponibile o lenta. In modalità online, l'accesso ai file avviene alla velocità della rete e del server. In modalità offline, i file vengono recuperati dalla cartella dei file offline alle velocità di accesso locale. Un computer passa alla modalità offline nei seguenti casi:
  - È stata abilitata la nuova modalità **sempre offline**
  - Il server non è disponibile
  - La connessione di rete è più lenta rispetto a un valore di soglia configurabile
  - L'utente passa manualmente alla modalità offline tramite il pulsante **Offline** in Esplora risorse
- La funzionalità **Profili utente mobili** consente di reindirizzare i profili utente su una condivisione file, in modo che gli utenti ricevano le stesse impostazioni del sistema operativo e delle applicazioni su più computer. Quando un utente accede a un computer tramite un account configurato con una condivisione file come percorso del profilo, il profilo dell'utente viene scaricato nel computer locale e unito al profilo locale, se presente. Quando l'utente si disconnette dal computer, la copia locale del profilo, incluse eventuali modifiche, viene unita alla copia del profilo nel server. La funzionalità Profili utente mobili viene in genere abilitata per gli account di dominio da un amministratore di rete.

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
| Sincronizzazione che tiene conto dei costi | Nuova | Gli utenti possono evitare i costi elevati di utilizzo dei dati correlati alla sincronizzazione quando usano connessioni a consumo con limiti di utilizzo o durante il roaming sulla rete di un altro provider. |
| Supporto del computer primario | Nuova | Consente di limitare l'uso di una o di entrambe le funzionalità Reindirizzamento cartelle e Profili utente mobili ai soli computer primari di un utente. |

## <a name="always-offline-mode"></a>Modalità sempre offline

A partire da Windows 8 e Windows Server 2012 gli amministratori possono configurare l'esperienza degli utenti per la funzionalità File offline in modo che lavorino sempre in modalità offline, anche quando usano una connessione di rete ad alta velocità. I file vengono aggiornati da Windows nella cache dei file offline eseguendo una sincronizzazione in background ogni ora per impostazione predefinita.

### <a name="what-value-does-always-offline-mode-add"></a>Qual è il valore aggiunto apportato dalla modalità sempre offline?

La modalità sempre offline offre i vantaggi seguenti:

- Gli utenti possono accedere più rapidamente ai file nelle cartelle reindirizzate, come la cartella Documenti.
- Riduzione della larghezza di banda, con conseguente diminuzione di costi su connessioni WAN costose o connessioni a consumo, come la rete mobile 4G.

### <a name="how-has-always-offline-mode-changed-things"></a>Quali sono i cambiamenti derivanti dall'introduzione della modalità sempre offline?

Prima di Windows 8 e Windows Server 2012, gli utenti avrebbero alternato le modalità online e offline a seconda della disponibilità e delle condizioni della rete, anche quando la modalità Collegamento lento (nota anche come modalità Connessione lenta) era abilitata e impostata su una soglia di latenza di 1 millisecondo.

Con la modalità sempre offline, i computer non passano mai alla modalità online quando è configurata l'impostazione di Criteri di gruppo **Configura modalità collegamento lento** e il parametro di soglia **Latenza** è impostato su 1 millisecondo. Le modifiche vengono sincronizzate in background ogni 120 minuti per impostazione predefinita, ma la sincronizzazione è configurabile tramite l'impostazione di Criteri di gruppo **Configura sincronizzazione in background**.

Per altre informazioni, vedere [Enable the Always Offline Mode to Provide Faster Access to Files](enable-always-offline.md).

## <a name="cost-aware-synchronization"></a>Sincronizzazione che tiene conto dei costi

Con la sincronizzazione che tiene conto dei costi, viene disabilitata automaticamente la sincronizzazione in background quando l'utente usa una connessione di rete a consumo, come una rete mobile 4G, e l'abbonato è prossimo al limite di larghezza di banda o l'ha superato oppure è in roaming sulla rete di un altro provider.

> [!NOTE]
> Le connessioni di rete a consumo presentano in genere latenze di rete di round trip inferiori al valore di latenza predefinito di 35 millisecondi per il passaggio alla modalità offline (connessione lenta) in Windows 8, Windows Server 2019, Windows Server 2016 e Windows Server 2012. Per questo motivo, queste connessioni passano di solito automaticamente alla modalità Offline (connessione lenta).

### <a name="what-value-does-cost-aware-synchronization-add"></a>Qual è il valore aggiunto apportato dalla sincronizzazione che tiene conto dei costi?

La sincronizzazione che tiene conto dei costi consente agli utenti di evitare costi di utilizzo dei dati inaspettatamente alti durante l'uso di connessioni a consumo con limiti di utilizzo o in caso di roaming sulla rete di un altro provider.

### <a name="how-has-cost-aware-synchronization-changed-things"></a>Quali sono i cambiamenti derivanti dall'introduzione della sincronizzazione che tiene conto dei costi?

Prima di Windows 8 e Windows Server 2012, gli utenti che volevano contenere i costi durante l'uso della funzionalità File offline su connessioni di rete a consumo dovevano tenere traccia dell'utilizzo dei dati tramite strumenti forniti dal provider della rete mobile. Gli utenti avevano così la possibilità di passare manualmente alla modalità offline in caso di roaming, in prossimità del limite della larghezza di banda o in caso di superamento del limite.

Con la sincronizzazione che tiene conto dei costi, l'uso del roaming e i limiti di utilizzo della larghezza di banda sono controllati automaticamente da Windows nelle connessioni a consumo. In caso di roaming, in prossimità del limite di larghezza di banda o in caso di superamento del limite, viene attivata automaticamente la modalità offline e viene impedita del tutto la sincronizzazione. Gli utenti possono ancora avviare manualmente la sincronizzazione e gli amministratori possono evitare l'impostazione di sincronizzazione che tiene conto dei costi per utenti specifici, come i dirigenti.

Per altre informazioni, vedere [Enable Background File Synchronization on Metered Networks](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj127408(v%3dws.11)).

## <a name="primary-computers-for-folder-redirection-and-roaming-user-profiles"></a>Computer primari per il reindirizzamento cartelle e i profili utente mobili

Puoi ora designare un set di computer, noti come computer primari, per ogni utente di dominio, in modo da controllare i computer nei quali viene usata una o entrambe le funzionalità Reindirizzamento cartelle e Profili utente mobili. È facile designare i computer primari e si tratta di un metodo efficace per associare dati e impostazioni degli utenti a computer o dispositivi specifici, semplificare le attività di supervisione degli amministratori, migliorare la sicurezza dei dati e proteggere i profili utente da eventuali danneggiamenti.

### <a name="what-value-do-primary-computers-add"></a>Qual è il valore aggiunto apportato dai computer primari?

Esistono quattro vantaggi principali della designazione di computer primari per gli utenti:

- L'amministratore può specificare quali computer possono utilizzare gli utenti per accedere ai dati e alle impostazioni reindirizzati. L'amministratore può scegliere, ad esempio, di eseguire il roaming dei dati e delle impostazioni di un utente tra il desktop e il laptop dell'utente, ma di non eseguirne il roaming quando l'utente accede a qualsiasi altro computer, ad esempio un PC in una sala riunioni.
- La designazione dei computer primari riduce i rischi a livello di sicurezza e privacy che emergono quando si lasciano dati personali o aziendali residui nei computer a cui accedono gli utenti. Un direttore generale che accede temporaneamente al computer di un dipendente, ad esempio, non rischia di lasciare dati personali o aziendali riservati in tale computer.
- I computer primari consentono all'amministratore di ridurre i rischi correlati a un profilo configurato in modo non corretto o in altro modo danneggiato, in seguito al roaming tra computer con configurazioni diverse, come i computer x86 e x64.
- Il tempo necessario per il primo accesso di un utente a un computer non primario, come un server, è più veloce perché non vengono scaricati il profilo mobile e/o le cartelle reindirizzate dell'utente. Anche i tempi di disconnessione risultano ridotti, perché le modifiche al profilo utente non devono essere caricate nella condivisione file.

### <a name="how-have-primary-computers-changed-things"></a>Quali sono i cambiamenti derivanti dall'introduzione dei computer primari?

Per limitare il download dei dati utente privati nei computer primari, le tecnologie di reindirizzamento cartelle e profili utente mobili eseguono i controlli logici seguenti al momento dell'accesso a un computer:

1. Il sistema operativo Windows controlla le nuove impostazioni di Criteri di gruppo (**Scarica profili mobili solo nei computer primari** e **Reindirizza cartelle solo nei computer primari**) per stabilire se l'attributo **msDS-Primary-Computer** in Active Directory Domain Services deve influire sulla decisione di eseguire il roaming del profilo dell'utente o applicare Reindirizzamento cartelle.
2. Se l'impostazione dei criteri consente il supporto dei computer primari, Windows verifica che lo schema di Servizi di dominio Active Directory supporti l'attributo **msDS-Primary-Computer**. In caso affermativo, Windows stabilisce se il computer a cui sta accedendo l'utente è designato come computer primario, come segue:
    1. Se il computer è uno dei computer primari dell'utente, vengono applicate automaticamente le impostazioni di Reindirizzamento cartelle e Profili utente mobili.
    2. Se il computer non è uno dei computer primari dell'utente, viene caricato il profilo locale dell'utente memorizzato nella cache, se presente, oppure viene creato un nuovo profilo locale. Vengono inoltre rimosse eventuali cartelle reindirizzate esistenti, in base all'azione di rimozione specificata dall'impostazione di Criteri di gruppo applicata in precedenza e mantenuta nella configurazione locale di reindirizzamento cartelle.

Per altre informazioni, vedere [Deploy Primary Computers for Folder Redirection and Roaming User Profiles](deploy-primary-computers.md)

## <a name="hardware-requirements"></a>Requisiti hardware

Per il reindirizzamento cartelle, i file offline e i profili utente mobili è necessario un computer x64 o x86 e queste funzionalità non sono supportate in computer WOA (Windows on ARM).

## <a name="software-requirements"></a>Requisiti software

Per designare i computer primari, l'ambiente deve soddisfare i requisiti seguenti:

- Lo schema di Active Directory Domain Services deve essere aggiornato in modo da includere lo schema e le condizioni di Windows Server 2012. Lo schema viene aggiornato automaticamente con l'installazione di un controller di dominio Windows Server 2012 o versioni successive. Per altre informazioni sull'aggiornamento di Active Directory Domain Services, vedi [Aggiornare i controller di dominio a Windows Server 2016](../../identity/ad-ds/deploy/upgrade-domain-controllers.md).
- I computer client devono eseguire Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 per poter essere aggiunti al dominio di Active Directory che stai gestendo.

## <a name="more-information"></a>Altre informazioni

Per altre informazioni correlate, vedere le risorse seguenti.

| Tipo di contenuto | Riferimenti |
| --- | --- |
| Valutazione del prodotto | [Supporto degli Information Worker con servizi file e risorse di archiviazione affidabili](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831495(v%3dws.11)>)<br>[Novità di File offline](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff183315(v=ws.10)>) (Windows 7 e Windows Server 2008 R2)<br>[Novità di File offline per Windows Vista](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-vista/cc749449(v=ws.10)>)<br>[Modifiche apportate a File offline in Windows Vista](<https://technet.microsoft.com/library/2007.11.offline.aspx>) (TechNet Magazine) |
| Distribuzione | [Distribuire Reindirizzamento cartelle, File offline e Profili utente mobili](deploy-folder-redirection.md)<br>[Implementazione di una soluzione di centralizzazione dei dati degli utenti finali: distribuzione e convalida delle tecnologie Reindirizzamento cartelle e File offline](https://download.microsoft.com/download/3/0/1/3019A3DA-2F41-4F2D-BBC9-A6D24C4C68C4/Implementing%20an%20End-User%20Data%20Centralization%20Solution.docx)<br>[Guida alla distribuzione della gestione dei dati utente mobili](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-vista/cc766489(v=ws.10)>)<br>[Guida dettagliata alla configurazione di nuove funzionalità File non in linea per computer Windows 7](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff633429(v=ws.10)>)<br>[Utilizzo di Reindirizzamento cartelle](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753996(v=ws.11)>)<br>[Implementazione di Reindirizzamento cartelle](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc737434(v=ws.10)>) (Windows Server 2003) |
| Strumenti e impostazioni | [File offline su MSDN](https://msdn.microsoft.com/library/cc296092.aspx)<br>[Informazioni di riferimento per Criteri di gruppo File offline](https://msdn.microsoft.com/library/ms878937.aspx) (Windows 2000) |
| Risorse della community | [Forum di Servizi file e archiviazione](https://social.technet.microsoft.com/forums/windowsserver/home?forum=winserverfiles)<br>[Blog Hey Scripting Guy: Come si usa la funzionalità File offline in Windows?](<https://blogs.technet.microsoft.com/heyscriptingguy/2009/06/02/hey-scripting-guy-how-can-i-enable-and-disable-offline-files/>)<br>[Blog Hey Scripting Guy: Come si abilita e disabilita la funzionalità File offline?](<https://blogs.technet.microsoft.com/heyscriptingguy/2009/06/02/hey-scripting-guy-how-can-i-enable-and-disable-offline-files/>) |
| Tecnologie correlate|[Identità e accesso in Windows Server](../../identity/identity-and-access.md)<br>[Archiviazione in Windows Server](../storage.md)<br>[Gestione di accesso remoto e server](../../remote/index.md) |