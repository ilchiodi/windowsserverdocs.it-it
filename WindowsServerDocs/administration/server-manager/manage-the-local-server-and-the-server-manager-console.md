---
title: Manage the Local Server and the Server Manager Console
description: Server Manager
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eeb32f65-d588-4ed5-82ba-1ca37f517139
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1f22578cc54a22464fe5d9208731fe681be30481
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832982"
---
# <a name="manage-the-local-server-and-the-server-manager-console"></a>Manage the Local Server and the Server Manager Console

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In Windows Server, Server Manager consente di gestire sia il server locale (se si eseguono Server Manager in Windows Server e non su un sistema operativo client basati su Windows) e i server remoti che eseguono Windows Server 2008 e versioni successive di Windows Sistema operativo server.

Il **Server locale** pagina in Server Manager consente di visualizzare dati del contatore proprietà, eventi, servizi e delle prestazioni server, e i risultati di Best Practices Analyzer (BPA) per il server locale. I riquadri evento, servizio, BPA e prestazioni funzionano come nelle pagine gruppo di server e di ruolo. Per altre informazioni su come configurare i dati visualizzati in questi riquadri, vedere [View e Configure Performance, Event, e Service Data e [Run Best Practices Analyzer Scans e Manage Scan Results.

I comandi di menu e le impostazioni nelle barre di intestazione console Server Manager si applicano globalmente a tutti i server nel pool di server e consentono di usare Server Manager per gestire l'intero pool di server.

In questo argomento sono incluse le sezioni seguenti.

-   [Arrestare il server locale](#BKMK_shutdown)

-   [Configurare le proprietà di Server Manager](#BKMK_props)

-   [Gestire la console di Server Manager](#BKMK_managesm)

-   [Personalizzare gli strumenti vengono visualizzati nel menu Strumenti](#BKMK_tools)

-   [Gestire i ruoli nella home page ruolo](#BKMK_roles)

## <a name="BKMK_shutdown"></a>Arrestare il server locale
Il **attività** menu nel server locale **delle proprietà** consente di riquadro avviare una sessione di Windows PowerShell nel server locale, aprire il **computer Management** snap-in mmc, o open snap-in MMC per i ruoli o le funzionalità installate nel server locale. È anche possibile arrestare il server locale utilizzando il comando **Arresta server locale** nel menu **Attività** . Il comando **Arresta server locale** è inoltre disponibile per il server locale nel riquadro **Server** nella pagina **Tutti i server** o in qualsiasi pagina di gruppo o ruolo in cui è rappresentato il server locale.

Arresto del server locale utilizzando questo metodo, a differenza dell'arresto di Windows Server 2016 dal **avviare** , viene visualizzata la schermata il **arrestato verso il basso Windows** nella finestra di dialogo consente di specificare i motivi dell'arresto del sistema nel **Individuazione evento di arresto** area.

> [!NOTE]
> Solo i membri del gruppo Administrators possono arrestare o riavviare un server. Gli utenti standard non possono arrestare o riavviare un server. Facendo clic sul comando **Arresta server locale** si disconnettono gli utenti standard dalle sessioni server. Questa corrisponde all'esperienza di un utente standard che esegue il comando di arresto **ALT+F4** dal desktop del server.

## <a name="BKMK_props"></a>Configurare le proprietà di Server Manager
È possibile visualizzare o modificare le impostazioni seguenti nel riquadro **Proprietà** nella pagina **Server locale** . Per modificare un valore dell'impostazione, fare clic sul valore del collegamento ipertestuale dell'impostazione.

> [!NOTE]
> In genere le proprietà visualizzate nel riquadro **Proprietà** del server locale possono essere modificate solo nel server locale. Non è possibile modificare le proprietà del server locale da un computer remoto con Server Manager in quanto il **proprietà** riquadro Ottiene informazioni solo sul computer locale, non sui computer remoti.
> 
> Poiché molte proprietà visualizzate nel **delle proprietà** riquadro sono controllate dagli strumenti che non fanno parte di Server Manager (Pannello di controllo, ad esempio), diventa **proprietà** impostazioni non sono sempre visualizzato nei **proprietà** immediatamente del riquadro. Per impostazione predefinita, i dati nel riquadro **Proprietà** vengono aggiornati ogni due minuti. Per aggiornare **delle proprietà** immediatamente i dati del riquadro, fare clic su **aggiornare** nella barra degli indirizzi di Server Manager.

|Impostazione|Descrizione|
|------|--------|
|Nome del computer|Visualizza il nome del computer e si apre la **le proprietà di sistema** nella finestra di dialogo consente di modificare il nome del server, appartenenza al dominio e altre impostazioni di sistema come i profili utente.|
|Dominio (o Gruppo di lavoro, se il server non appartiene a un dominio)|Visualizza il dominio o il gruppo di lavoro di cui il server è un membro. Apre la **le proprietà di sistema** nella finestra di dialogo consente di modificare il nome del server, appartenenza al dominio e altre impostazioni di sistema come i profili utente.|
|Windows Firewall|Visualizza lo stato di Windows Firewall per il server locale. Apre il **Pannello di controllo\Sistema e sicurezza\Windows Firewall**. Per altre informazioni su come configurare Windows Firewall, vedere [Windows Firewall con sicurezza avanzata e IPSec](https://go.microsoft.com/fwlink/?LinkId=253465).|
|Gestione remota|Visualizza lo stato di gestione remota di Server Manager e Windows PowerShell. Apre la **configurare la gestione remota** nella finestra di dialogo. Per altre informazioni sulla gestione remota, vedere [configurare la gestione remota in Server Manager](configure-remote-management-in-server-manager.md).|
|Desktop remoto|Mostra se gli utenti possono connettersi al server in modalità remota tramite sessioni di Desktop remoto. Apre la **remoto** scheda della finestra di **le proprietà di sistema** nella finestra di dialogo.|
|Gruppo NIC|Mostra se il server locale partecipa al gruppo NIC. Apre la finestra di dialogo **Gruppo NIC** e consente di aggiungere il server locale a un gruppo NIC. Per altre informazioni sul gruppo NIC, vedere il [white paper relativo al gruppo NIC](https://go.microsoft.com/fwlink/?LinkID=253449).|
|Ethernet|Visualizza lo stato della connettività di rete del server. Apre il **Pannello di controllo\Rete e Internet\Connessioni di rete**.|
|Versione del sistema operativo|Questo campo di sola lettura contiene il numero di versione del sistema operativo Windows eseguito dal server locale.|
|Informazioni relative all'hardware|Questo campo di sola lettura contiene il nome del produttore e il numero e il nome del modello dell'hardware del server.|
|Ultimi aggiornamenti installati|Visualizza il giorno e l'ora dell'ultima installazione degli aggiornamenti Windows. Apre il **Pannello di controllo\Sistema e sicurezza\Windows Update**.|
|Windows Update|Visualizza le impostazioni di Windows Update per il server locale. Apre il **Pannello di controllo\Sistema e sicurezza\Windows Update**.|
|Ultima verifica disponibilità aggiornamenti|Visualizza il giorno e l'ora dell'ultima verifica della disponibilità di aggiornamenti di Windows. Apre il **Pannello di controllo\Sistema e sicurezza\Windows Update**.|
|Segnalazione errori Windows|Visualizza lo stato di consenso esplicito di Segnalazione errori Windows. Apre la finestra di dialogo **Configurazione di Segnalazione errori Windows** . Per altre informazioni su Segnalazione errori Windows, i relativi vantaggi, l'informativa sulla privacy e le impostazioni di consenso esplicito, vedere [Segnalazione errori Windows](https://go.microsoft.com/fwlink/?LinkID=245991).|
|Analisi utilizzo software|Visualizza lo stato di consenso esplicito del programma Analisi utilizzo software Windows. Apre la finestra di dialogo **Configurazione programma Analisi utilizzo software** . Per altre informazioni sul programma Analisi utilizzo software Windows, sui relativi vantaggi e sulle impostazioni del consenso esplicito, vedere [Programma Analisi utilizzo software Windows](https://go.microsoft.com/fwlink/?LinkID=245992).|
|Configurazione di Sicurezza avanzata di Internet Explorer (IE)|Mostra se la configurazione della sicurezza avanzata di Internet Explorer è attivata o disattivata. Apre la finestra di dialogo **Configurazione sicurezza avanzata di Internet Explorer**. Configurazione sicurezza avanzata di Internet Explorer è una misura di sicurezza per i server che impedisce l'apertura delle pagine Web in Internet Explorer. Per altre informazioni su configurazione sicurezza avanzata IE, i vantaggi e impostazioni, vedere [Internet Explorer: Configurazione sicurezza avanzata](https://go.microsoft.com/fwlink/?LinkId=253461).|
|Fuso orario|Consente di visualizzare fuso orario del server locale. Apre la **data e ora** nella finestra di dialogo.|
|ID prodotto|Visualizza il numero ID prodotto e lo stato del attivazione di Windows (se Windows è stato attivato) del sistema operativo Windows Server 2016. Non si tratta del codice Product key di Windows. Apre la finestra di dialogo **Attivazione di Windows** .|
|Processori|Questo campo di sola lettura Visualizza produttore, nome del modello e le informazioni sulla velocità sui processori del server locale.|
|Memoria installata (RAM)|Questo campo di sola lettura contiene la quantità di RAM disponibile in gigabyte.|
|Spazio su disco totale|Questo campo di sola lettura contiene la quantità di spazio disponibile su disco in gigabyte.|

## <a name="BKMK_managesm"></a>Gestire la console di Server Manager
Impostazioni globali che si applicano alla console Server Manager e a tutti i server remoti che sono stati aggiunti al pool di server di gestione, si trovano nelle barre di intestazione nella parte superiore della finestra della console Server Manager.

### <a name="add-servers-to-server-manager"></a>aggiungere server a Server Manager
Il comando che apre il **aggiungere i server** della finestra di dialogo e consente di aggiungere server virtuali o fisici per il pool di server di gestione, è nel **Gestisci** menu della console di Server Manager. Per informazioni dettagliate su come aggiungere server, vedere [aggiungere server a Server Manager](add-servers-to-server-manager.md).

### <a name="refresh-data-that-is-displayed-in-server-manager"></a>Aggiornare i dati che vengono visualizzati in Server Manager
È possibile configurare l'intervallo di aggiornamento per i dati che viene visualizzato in Server Manager nella **proprietà di Server Manager** della finestra di dialogo che è possibile aprire dalle **Gestisci** menu.

##### <a name="to-configure-the-refresh-interval-in-server-manager"></a>Per configurare l'intervallo di aggiornamento in Server Manager

1.  Nel **Gestisci** dal menu nella console di Server Manager, fare clic su **delle proprietà di Server Manager**.

2.  Nel **proprietà di Server Manager** finestra di dialogo, specificare un periodo di tempo, espresso in minuti, la quantità di tempo che deve trascorrere tra gli aggiornamenti dei dati che viene visualizzati in Server Manager. Il valore predefinito è 10 minuti. Al termine, fare clic su OK.

#### <a name="refresh-limitations"></a>Limitazioni dell'aggiornamento
L'aggiornamento si applica globalmente ai dati da tutti i server aggiunti al pool di server di gestione Server. Non è possibile aggiornare i dati né configurare intervalli di aggiornamento diversi per server, ruoli o gruppi singoli.

Quando i server appartenenti a un cluster vengono aggiunti a Server Manager, se sono computer fisici o macchine virtuali, il primo aggiornamento dei dati può avere esito negativo o visualizzare i dati solo per il server host degli oggetti cluster. Con gli aggiornamenti successivi verranno mostrati dati accurati per i server fisici o virtuali in un cluster di server.

I dati visualizzati nelle home page ruolo in Server Manager per Servizi Desktop remoto, indirizzo IP, gestione e servizi File e archiviazione non vengono aggiornati automaticamente. Aggiornare i dati che vengono visualizzati in queste pagine manualmente premendo **F5** o facendo clic su **aggiornare** nella console di Server Manager intestazione mentre si è dalle pagine stesse.

### <a name="add-or-remove-roles-or-features"></a>aggiungere o rimuovere ruoli o funzionalità
I comandi che aprono l'aggiunta guidata ruoli e funzionalità e rimuovere ruoli e funzionalità guidata e consentono aggiungere o rimuovere ruoli, servizi ruolo e funzionalità ai server nel pool di server, inclusi i **Gestisci** dal menu di Server Manager console e il **attività** dal menu delle **ruoli e funzionalità** riquadro nelle pagine ruolo o gruppo. Per informazioni dettagliate su come aggiungere o rimuovere ruoli o funzionalità, vedere [Install or Uninstall Roles, Role Services, or Features](install-or-uninstall-roles-role-services-or-features.md).

In Server Manager, i dati di ruolo e funzionalità vengono visualizzati nella lingua di base del sistema, denominato anche la lingua GUI predefinita del sistema o la lingua selezionata durante l'installazione del sistema operativo.

### <a name="create-server-groups"></a>creare gruppi di server
Il comando che apre il **creare gruppo di Server** della finestra di dialogo e ti permette di creare gruppi personalizzati di server, è nel **Gestisci** menu della console di Server Manager. Per informazioni dettagliate su come creare gruppi di server, vedere [creare e gestire gruppi di Server](create-and-manage-server-groups.md).

### <a name="prevent-server-manager-from-opening-automatically-at-logon"></a>Impedire l'apertura automatica di Server Manager all'accesso
Il **non avviare automaticamente Server Manager all'accesso** casella di controllo la **proprietà di Server Manager** nella finestra di dialogo consente di controllare se Server Manager viene aperto automaticamente all'accesso per i membri del Gruppo Administrators su un server locale. Questa impostazione influisce sul comportamento di Server Manager quando è in esecuzione in Windows 10 come parte di strumenti di amministrazione remota del Server. Per altre informazioni su come configurare questa impostazione, vedere [Server Manager](server-manager.md).

### <a name="zoom-in-or-out"></a>Eseguire lo zoom avanti o indietro
Per fare zoom avanti o indietro sulla visualizzazione della console di Server Manager, è possibile usare il **Zoom** comandi il **visualizzazione** menu oppure premere **Ctrl + più (+)** eseguire lo zoom e **Ctrl + meno (-)** eseguire lo zoom indietro.

## <a name="BKMK_tools"></a>Personalizzare gli strumenti vengono visualizzati nel menu Strumenti
Il **strumenti** dal menu in Server Manager sono inclusi collegamenti simbolici ai collegamenti nel **strumenti di amministrazione** cartella **Pannello di controllo/sistema e sicurezza**. Il **strumenti di amministrazione** cartella contiene un elenco di collegamenti o file LNK agli strumenti di gestione disponibili, ad esempio lo snap-in di mmc. Server Manager consente di popolare il **strumenti** menu con collegamenti a tali scelte rapide e copia la struttura della cartella il **strumenti di amministrazione** cartella in cui il **strumenti** menu. Per impostazione predefinita, gli strumenti della cartella Strumenti di amministrazione vengono disposti in un elenco ordinati per tipo e per nome. In Server Manager**strumenti** menu voci sono ordinate solo per nome, non per tipo.

Per personalizzare il menu **Strumenti** , copiare i collegamenti dello strumento o dello script che si desidera utilizzare nella cartella **Strumenti di amministrazione** . È anche possibile organizzare i collegamenti in cartelle per creare menu a discesa nel menu **Strumenti** . Inoltre, se si desidera limitare l'accesso agli strumenti personalizzati nel **strumenti** menu, è possibile impostare diritti di accesso utente in entrambe le cartelle strumenti personalizzate in strumenti di amministrazione o direttamente sui file di script o sullo strumento originale.

Non è consigliabile riorganizzare gli strumenti di amministrazione e di sistema ed eventuali strumenti di gestione associati a ruoli e funzionalità installati nel server locale. Lo spostamento degli strumenti di gestione associati a ruoli e funzionalità potrebbe impedire la disinstallazione corretta di tali strumenti quando diventa necessario. Dopo la disinstallazione di un ruolo o di una funzionalità, è possibile che un collegamento non funzionale a uno strumento la cui scelta rapida è stata spostata rimanga nel menu **Strumenti**. Se si reinstalla il ruolo, viene creato un collegamento duplicato allo stesso strumento nel menu **Strumenti**, ma uno dei collegamenti non funzionerà.

Gli strumenti associati a ruoli e funzionalità installati come parte di Strumenti di amministrazione remota del server in un computer client basato su Windows possono essere organizzati in cartelle personalizzate. La disinstallazione di funzionalità o del ruolo padre non influisce sui collegamenti dello strumento disponibili in un computer remoto che esegue Windows 10.

La procedura seguente viene descritto come creare una cartella di esempio denominata *MyTools*, e spostare i collegamenti di due script di Windows PowerShell nella cartella che sarà quindi accessibile dal menu Strumenti di Server Manager.

#### <a name="to-customize-the-tools-menu-by-adding-shortcuts-in-administrative-tools"></a>Per personalizzare il menu Strumenti aggiungendo collegamenti negli Strumenti di amministrazione

1.  creare una nuova cartella denominata *MyTools* in un'unica comoda posizione.

    > [!NOTE]
    > Dati i diritti di accesso limitati per la cartella **Strumenti di amministrazione** , non è consentito creare una nuova cartella direttamente nella cartella **Strumenti di amministrazione** . È necessario creare una nuova cartella altrove, ad esempio sul Desktop, quindi copiare la nuova cartella nella cartella **Strumenti di amministrazione** .

2.  spostare o copiare *MyTools* al **Pannello di controllo/sistema e sicurezza/strumenti di amministrazione**. Per impostazione predefinita, è necessario essere un membro del gruppo Administrators nel computer per apportare modifiche alla cartella **Strumenti di amministrazione** .

3.  Se non è necessaria limitare i diritti di accesso utente ai collegamenti personalizzati degli strumenti, andare al passaggio 6. In caso contrario, fare clic con il pulsante destro del mouse sul file dello strumento, o la cartella *MyTools*, quindi fare clic su **Proprietà**.

4.  Nel **sicurezza** scheda del file **delle proprietà** della finestra di dialogo fare clic su **modifica**.

5.  per gli utenti per cui si desidera limitare l'accesso agli strumenti, deselezionare le caselle di controllo per **lettura & execute**, **lettura**, e **scrivere** autorizzazioni. Queste autorizzazioni sono ereditate dal collegamento dello strumento nella cartella **Strumenti di amministrazione**.

    Se modificano i diritti di accesso per un utente mentre l'utente sta usando Server Manager (o anche se Server Manager è aprire), quindi le modifiche non vengono visualizzate nei **strumenti** menu finché l'utente riavvia Server Manager.

    > [!NOTE]
    > Se si limita l'accesso a un'intera cartella che è stata copiata negli strumenti di amministrazione, gli utenti con restrizioni possono vedere né la cartella né il relativo contenuto in Server Manager**strumenti** menu.
    > 
    > modificare le autorizzazioni per la cartella nel **strumenti di amministrazione** cartella. Poiché i file nascosti e le cartelle in strumenti di amministrazione sono sempre visualizzate in Server Manager**degli strumenti** menu, non utilizzare il **Hidden** impostazione su un file o della cartella **proprietà** finestra di dialogo per limitare l'accesso utente ai collegamenti personalizzati degli strumenti.
    > 
    > Le autorizzazioni **Nega** sovrascrivono sempre le autorizzazioni **Consenti**.

6.  Fare doppio clic su di strumento originale, script o file eseguibile per il quale si desidera aggiungere le voci nel **degli strumenti** menu e quindi fare clic su **Crea collegamento**.

7.  spostare il collegamento per il *MyTools* cartella in strumenti di amministrazione.

8.  Aggiornare o riavviare Server Manager, se necessario, per visualizzare il collegamento personalizzato dello strumento nel **strumenti** menu.

## <a name="BKMK_roles"></a>Gestire i ruoli nella home page ruolo
Dopo aver aggiunto server al pool di server di gestione Server, e Server Manager raccoglie i dati di inventario relativi ai pool, Server Manager aggiunge pagine al riquadro di spostamento per i ruoli che vengono rilevati nei server gestiti. Nel riquadro **Server** nelle pagine ruolo sono elencati i server gestiti che eseguono il ruolo. Per impostazione predefinita, nei riquadri **Eventi**, **Best Practices Analyzer**, **Servizi**e **Prestazioni** vengono visualizzati dati per tutti i server che eseguono il ruolo. La selezione di server specifici nel riquadro **Server** limita l'ambito dei risultati per eventi, servizi, contatori di prestazioni e BPA ai soli server selezionati. Gli strumenti di gestione sono in genere disponibili nella console di Server Manager **strumenti** menu, dopo che un ruolo o funzionalità ha installazione o il rilevamento in un server gestito. È anche possibile fare clic con il pulsante destro del mouse sui server nel riquadro **Server** per un ruolo o un gruppo, quindi avviare lo strumento di gestione che si desidera utilizzare.

In Windows Server 2016, i ruoli e funzionalità seguenti sono gli strumenti di gestione integrati nella console di Server Manager come pagine.

-   **Servizi file e archiviazione.** Le pagine di Servizi file e archiviazione includono comandi e riquadri personalizzati per la gestione di volumi, condivisioni, dischi virtuali iSCSI e pool di archiviazione. Quando si apre il File e verrà visualizzata la home page del ruolo Servizi di archiviazione in Server Manager, un riquadro comprimibile contenente che consente di visualizzare le pagine di gestione personalizzate per servizi File e archiviazione. Per altre informazioni sulla distribuzione e la gestione di Servizi file e archiviazione, vedere [Servizi file e archiviazione](https://go.microsoft.com/fwlink/p/?LinkId=241530).

-   **Servizi Desktop remoto.** Le pagine Servizi Desktop remoto includono comandi e riquadri personalizzati per la gestione di sessioni, licenze, gateway e desktop virtuali. Per altre informazioni sulla distribuzione e gestione di Servizi Desktop remoto, vedere [Servizi Desktop remoto (rdS)](https://go.microsoft.com/fwlink/p/?LinkId=241532).

-   **Indirizzo IP Management (IPAM).** La pagina del ruolo Gestione indirizzi IP include un riquadro **introduttivo** contenente collegamenti alle attività di gestione e configurazione di Gestione indirizzi IP comuni, oltre a una procedura guidata per il provisioning di un server di Gestione indirizzi IP. La home page di Gestione indirizzi IP include inoltre riquadri per la visualizzazione della rete gestita, del riepilogo della configurazione e delle attività pianificate.

    Esistono alcune limitazioni alla gestione IPAM in Server Manager. A differenza delle tipiche pagine ruolo e gruppo, IPAM non dispone di riquadri **Server**, **Eventi**, **Prestazioni**, **Best Practices Analyzer** o **Servizi**. Nessun modello Best Practices Analyzer è disponibile per IPAM; Non sono supportate le analisi di Analizzatore procedure consigliate migliore con gestione indirizzi IP. Per accedere ai server del pool che eseguono Gestione indirizzi IP, creare un gruppo personalizzato dei server che eseguono Gestione indirizzi IP e accedere all'elenco di server dal riquadro **Server** nella pagina gruppo personalizzata. In alternativa, accedere ai server IPAM dal riquadro **Server** nella pagina gruppo **Tutti i server**.

    Le anteprime del dashboard contengono inoltre righe limitate per Gestione indirizzi IP, rispetto alle anteprime per altri ruoli e gruppi. Facendo clic sulle righe delle anteprime IPAM, è possibile visualizzare eventi, dati di prestazioni e avvisi relativi allo stato di gestibilità per i server che eseguono IPAM. I servizi correlati a Gestione indirizzi IP possono essere gestiti dalle pagine per i gruppi di server che contengono i server Gestione indirizzi IP, ad esempio la pagina per il gruppo **Tutti i server** .

    per altre informazioni sulla distribuzione e la gestione di IPAM, vedere [indirizzo IP Management (IPAM)](https://go.microsoft.com/fwlink/p/?LinkId=241533).

## <a name="see-also"></a>Vedere anche
[Server Manager](server-manager.md)
[aggiungere server a Server Manager](add-servers-to-server-manager.md)
[creare e gestire gruppi di Server](create-and-manage-server-groups.md)
[View e Configure Prestazioni, eventi e i dati del servizio](view-and-configure-performance-event-and-service-data.md)
[servizi File e archiviazione](https://go.microsoft.com/fwlink/p/?LinkId=241530)
[Servizi Desktop remoto (rdS)](https://go.microsoft.com/fwlink/p/?LinkId=241532) 
 [ Indirizzo IP Management (IPAM)](https://go.microsoft.com/fwlink/p/?LinkId=241533)



