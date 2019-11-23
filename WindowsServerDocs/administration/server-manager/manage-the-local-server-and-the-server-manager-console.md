---
title: Manage the Local Server and the Server Manager Console
description: Server Manager
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 45bb0efcaf989cadd717ddbfde27230b76901113
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383115"
---
# <a name="manage-the-local-server-and-the-server-manager-console"></a>Manage the Local Server and the Server Manager Console

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In Windows Server Server Manager consente di gestire sia il server locale (se si esegue Server Manager in Windows Server e non in un sistema operativo client basato su Windows) sia i server remoti che eseguono Windows Server 2008 e versioni successive di Windows Sistema operativo server.

Nella pagina **server locale** di Server Manager vengono visualizzati i dati relativi a proprietà del server, eventi, servizi e contatori delle prestazioni e i risultati della Best Practices Analyzer (BPA) per il server locale. I riquadri evento, servizio, BPA e prestazioni funzionano come nelle pagine gruppo di server e di ruolo. Per altre informazioni su come configurare i dati visualizzati in questi riquadri, vedere [View e Configure Performance, Event, e Service Data](view-and-configure-performance-event-and-service-data.md) e [Run Best Practices Analyzer Scans e Manage Scan Results](run-best-practices-analyzer-scans-and-manage-scan-results.md).

I comandi di menu e le impostazioni nelle barre di intestazione della console di Server Manager si applicano globalmente a tutti i server del pool di server e consentono di utilizzare Server Manager per gestire l'intero pool di server.

In questo argomento sono incluse le sezioni seguenti.

-   [Arrestare il server locale](#BKMK_shutdown)

-   [Configura proprietà Server Manager](#BKMK_props)

-   [Gestire la console di Server Manager](#BKMK_managesm)

-   [Personalizzare gli strumenti visualizzati nel menu strumenti](#BKMK_tools)

-   [Gestire i ruoli nelle Home page del ruolo](#BKMK_roles)

## <a name="BKMK_shutdown"></a>Arrestare il server locale
Il menu **attività** nel riquadro **Proprietà** server locale consente di avviare una sessione di Windows PowerShell nel server locale, di aprire lo snap-in MMC **Gestione computer** o di aprire gli snap-in MMC per i ruoli o le funzionalità installate nel server locale. È anche possibile arrestare il server locale utilizzando il comando **Arresta server locale** nel menu **Attività** . Il comando **Arresta server locale** è inoltre disponibile per il server locale nel riquadro **Server** nella pagina **Tutti i server** o in qualsiasi pagina di gruppo o ruolo in cui è rappresentato il server locale.

L'arresto del server locale con questo metodo, a differenza dell'arresto di Windows Server 2016 dalla schermata **Start** , apre la finestra di dialogo **arresta Windows** , che consente di specificare i motivi per l'arresto nell'area **Individuazione evento di arresto** .

> [!NOTE]
> Solo i membri del gruppo Administrators possono arrestare o riavviare un server. Gli utenti standard non possono arrestare o riavviare un server. Facendo clic sul comando **Arresta server locale** si disconnettono gli utenti standard dalle sessioni server. Questa corrisponde all'esperienza di un utente standard che esegue il comando di arresto **ALT+F4** dal desktop del server.

## <a name="BKMK_props"></a>Configura proprietà Server Manager
È possibile visualizzare o modificare le impostazioni seguenti nel riquadro **Proprietà** nella pagina **Server locale** . Per modificare il valore di un'impostazione, fare clic sul valore Hypertext dell'impostazione.

> [!NOTE]
> In genere le proprietà visualizzate nel riquadro **Proprietà** del server locale possono essere modificate solo nel server locale. Non è possibile modificare le proprietà del server locale da un computer remoto utilizzando Server Manager perché il riquadro **Proprietà** può ottenere solo informazioni sul computer locale, non sui computer remoti.
> 
> Poiché molte proprietà visualizzate nel riquadro **Proprietà** sono controllate da strumenti che non fanno parte di Server Manager (ad esempio il pannello di controllo), le modifiche apportate alle impostazioni delle **Proprietà** non vengono sempre visualizzate immediatamente nel riquadro **proprietà** . Per impostazione predefinita, i dati nel riquadro **Proprietà** vengono aggiornati ogni due minuti. Per aggiornare immediatamente i dati del riquadro **Proprietà** , fare clic su **Aggiorna** nella barra degli indirizzi server Manager.

|Impostazione|Descrizione|
|------|--------|
|nome computer|Visualizza il nome descrittivo del computer e apre la finestra di dialogo **proprietà del sistema** , che consente di modificare il nome del server, l'appartenenza al dominio e altre impostazioni di sistema, ad esempio i profili utente.|
|Dominio (o Gruppo di lavoro, se il server non appartiene a un dominio)|Visualizza il dominio o il gruppo di lavoro di cui il server è un membro. Apre la finestra di dialogo **proprietà del sistema** , che consente di modificare il nome del server, l'appartenenza al dominio e altre impostazioni di sistema, ad esempio i profili utente.|
|Windows Firewall|Visualizza lo stato di Windows Firewall per il server locale. Apre il **Pannello di controllo\Sistema e sicurezza\Windows Firewall**. Per altre informazioni su come configurare Windows Firewall, vedere [Windows Firewall con sicurezza avanzata e IPSec](https://go.microsoft.com/fwlink/?LinkId=253465).|
|gestione remota|Visualizza Server Manager e lo stato di gestione remota di Windows PowerShell. Apre la finestra di dialogo **Configura gestione remota** . Per ulteriori informazioni sulla gestione remota, vedere [configurare la gestione remota in Server Manager](configure-remote-management-in-server-manager.md).|
|Desktop remoto|Mostra se gli utenti possono connettersi al server in modalità remota tramite sessioni di Desktop remoto. Apre la scheda **remoto** della finestra di dialogo **proprietà del sistema** .|
|Gruppo NIC|Mostra se il server locale partecipa al gruppo NIC. Apre la finestra di dialogo **Gruppo NIC** e consente di aggiungere il server locale a un gruppo NIC. Per altre informazioni sul gruppo NIC, vedere il [white paper relativo al gruppo NIC](https://go.microsoft.com/fwlink/?LinkID=253449).|
|Ethernet|Visualizza lo stato della connettività di rete del server. Apre il **Pannello di controllo\Rete e Internet\Connessioni di rete**.|
|Versione del sistema operativo|Questo campo di sola lettura contiene il numero di versione del sistema operativo Windows eseguito dal server locale.|
|Informazioni relative all'hardware|Questo campo di sola lettura contiene il nome del produttore e il numero e il nome del modello dell'hardware del server.|
|Ultimi aggiornamenti installati|Visualizza il giorno e l'ora dell'ultima installazione degli aggiornamenti Windows. Apre il **Pannello di controllo\Sistema e sicurezza\Windows Update**.|
|Windows Update|Visualizza le impostazioni di Windows Update per il server locale. Apre il **Pannello di controllo\Sistema e sicurezza\Windows Update**.|
|Ultima verifica disponibilità aggiornamenti|Visualizza il giorno e l'ora dell'ultima verifica della disponibilità di aggiornamenti di Windows. Apre il **Pannello di controllo\Sistema e sicurezza\Windows Update**.|
|Segnalazione errori Windows|Visualizza lo stato di consenso esplicito di Segnalazione errori Windows. Apre la finestra di dialogo **Configurazione di Segnalazione errori Windows** . Per altre informazioni su Segnalazione errori Windows, i relativi vantaggi, l'informativa sulla privacy e le impostazioni di consenso esplicito, vedere [Segnalazione errori Windows](https://go.microsoft.com/fwlink/?LinkID=245991).|
|Analisi utilizzo software|Visualizza lo stato di consenso esplicito del programma Analisi utilizzo software Windows. Apre la finestra di dialogo **Configurazione programma Analisi utilizzo software** . Per altre informazioni sul programma Analisi utilizzo software Windows, sui relativi vantaggi e sulle impostazioni del consenso esplicito, vedere [Programma Analisi utilizzo software Windows](https://go.microsoft.com/fwlink/?LinkID=245992).|
|Configurazione di Sicurezza avanzata di Internet Explorer (IE)|Mostra se la configurazione della sicurezza avanzata di Internet Explorer è attivata o disattivata. Apre la finestra di dialogo **Configurazione sicurezza avanzata di Internet Explorer**. Configurazione sicurezza avanzata di Internet Explorer è una misura di sicurezza per i server che impedisce l'apertura delle pagine Web in Internet Explorer. Per altre informazioni su Configurazione sicurezza avanzata di IE, i relativi vantaggi e impostazioni, vedere [Internet Explorer: Sicurezza avanzata](https://go.microsoft.com/fwlink/?LinkId=253461).|
|fuso orario|Visualizza il fuso orario del server locale. Apre la finestra di dialogo **data e ora** .|
|ID prodotto|Visualizza lo stato di attivazione di Windows e il numero ID prodotto (se Windows è stato attivato) del sistema operativo Windows Server 2016. Non si tratta del codice Product key di Windows. Apre la finestra di dialogo **Attivazione di Windows** .|
|Processori|Questo campo di sola lettura consente di visualizzare il produttore, il nome del modello e le informazioni sulla velocità dei processori del server locale.|
|Memoria installata (RAM)|Questo campo di sola lettura contiene la quantità di RAM disponibile in gigabyte.|
|Spazio su disco totale|Questo campo di sola lettura contiene la quantità di spazio disponibile su disco in gigabyte.|

## <a name="BKMK_managesm"></a>Gestire la console di Server Manager
Le impostazioni globali che si applicano all'intera console di Server Manager e a tutti i server remoti che sono stati aggiunti al pool di server di Server Manager, si trovano nelle barre di intestazione nella parte superiore della finestra della console Server Manager.

### <a name="add-servers-to-server-manager"></a>Aggiungi server a Server Manager
Il comando che apre la finestra di dialogo **Aggiungi server** consente di aggiungere server virtuali o fisici remoti al pool di server Server Manager, si trova nel menu **Gestisci** della console di Server Manager. Per informazioni dettagliate su come aggiungere server, vedere [aggiungere server a Server Manager](add-servers-to-server-manager.md).

### <a name="refresh-data-that-is-displayed-in-server-manager"></a>Aggiornare i dati che vengono visualizzati in Server Manager
È possibile configurare l'intervallo di aggiornamento per i dati visualizzati in Server Manager nella finestra di dialogo **proprietà Server Manager** , che è possibile aprire dal menu **Gestisci** .

##### <a name="to-configure-the-refresh-interval-in-server-manager"></a>Per configurare l'intervallo di aggiornamento in Server Manager

1.  Nel menu **Gestisci** della console di Server Manager fare clic su **Server Manager Proprietà**.

2.  Nella finestra di dialogo **proprietà Server Manager** specificare un periodo di tempo, in minuti, per la quantità di tempo trascorso tra gli aggiornamenti dei dati visualizzati nel server Manager. Il valore predefinito è 10 minuti. Al termine, fare clic su OK.

#### <a name="refresh-limitations"></a>Limitazioni dell'aggiornamento
L'aggiornamento si applica globalmente ai dati di tutti i server aggiunti al pool di server Server Manager. Non è possibile aggiornare i dati né configurare intervalli di aggiornamento diversi per server, ruoli o gruppi singoli.

Quando i server che si trovano in un cluster vengono aggiunti a Server Manager, sia che si tratti di computer fisici o macchine virtuali, il primo aggiornamento dei dati può avere esito negativo o visualizzare i dati solo per il server host per gli oggetti cluster. Con gli aggiornamenti successivi verranno mostrati dati accurati per i server fisici o virtuali in un cluster di server.

I dati visualizzati nelle Home page del ruolo in Server Manager per Servizi Desktop remoto, gestione indirizzi IP e servizi file e archiviazione non vengono aggiornati automaticamente. Aggiornare manualmente i dati visualizzati in queste pagine premendo **F5** o facendo clic su **Aggiorna** nell'intestazione della console Server Manager mentre si è in queste pagine.

### <a name="add-or-remove-roles-or-features"></a>aggiungere o rimuovere ruoli o funzionalità
I comandi che aprono l'aggiunta guidata ruoli e funzionalità e la rimozione guidata ruoli e funzionalità e consentono di aggiungere o rimuovere ruoli, servizi ruolo e funzionalità ai server nel pool di server, si trovano nel menu **Gestisci** della console di Server Manager e nel menu **attività** del riquadro **ruoli e funzionalità** nelle pagine ruolo o gruppo. Per informazioni dettagliate su come aggiungere o rimuovere ruoli o funzionalità, vedere [Install or Uninstall Roles, Role Services, or Features](install-or-uninstall-roles-role-services-or-features.md).

In Server Manager, i dati relativi ai ruoli e alle funzionalità vengono visualizzati nella lingua di base del sistema, denominata anche lingua GUI predefinita del sistema o la lingua selezionata durante l'installazione del sistema operativo.

### <a name="create-server-groups"></a>creazione di gruppi di server
Il comando che consente di aprire la finestra di dialogo **Crea gruppo di server** e di creare gruppi personalizzati di server si trova nel menu **Gestisci** della console di Server Manager. Per informazioni dettagliate su come creare gruppi di server, vedere [creare e gestire gruppi di server](create-and-manage-server-groups.md).

### <a name="prevent-server-manager-from-opening-automatically-at-logon"></a>Impedire l'apertura automatica di Server Manager all'accesso
La casella di controllo non **avviare Server Manager automaticamente all'accesso** nella finestra di dialogo **Proprietà Server Manager** controlla se Server Manager viene aperto automaticamente all'accesso per i membri del gruppo Administrators su un server locale. Questa impostazione non influisce Server Manager comportamento quando viene eseguito in Windows 10 come parte del Strumenti di amministrazione remota del server. Per ulteriori informazioni sulla configurazione di questa impostazione, vedere [Server Manager](server-manager.md).

### <a name="zoom-in-or-out"></a>Eseguire lo zoom avanti o indietro
Per ingrandire o ridurre la visualizzazione della console di Server Manager, è possibile utilizzare i comandi **Zoom** nel menu **Visualizza** oppure premere **CTRL + più (+)** per eseguire lo zoom avanti e **CTRL + meno (-)** per eseguire lo zoom indietro.

## <a name="BKMK_tools"></a>Personalizzare gli strumenti visualizzati nel menu strumenti
Il menu **strumenti** in Server Manager include collegamenti morbidi ai collegamenti nella cartella **strumenti di amministrazione** nel pannello di **controllo/sistema e sicurezza**. La **cartella strumenti di amministrazione** contiene un elenco di collegamenti o file lnk agli strumenti di gestione disponibili, ad esempio gli snap-in mmc. Server Manager popola il menu **strumenti** con collegamenti a tali scelte rapide e copia la struttura di cartelle della cartella **strumenti di amministrazione** nel menu **strumenti** . Per impostazione predefinita, gli strumenti della cartella Strumenti di amministrazione vengono disposti in un elenco ordinati per tipo e per nome. Nel menu**strumenti** Server Manager gli elementi vengono ordinati solo per nome e non per tipo.

Per personalizzare il menu **Strumenti** , copiare i collegamenti dello strumento o dello script che si desidera utilizzare nella cartella **Strumenti di amministrazione** . È anche possibile organizzare i collegamenti in cartelle per creare menu a discesa nel menu **Strumenti** . Inoltre, se si desidera limitare l'accesso agli strumenti personalizzati nel menu **strumenti** , è possibile impostare i diritti di accesso utente sia sulle cartelle strumenti personalizzate in strumenti di amministrazione, sia direttamente sullo strumento originale o sui file script.

Non è consigliabile riorganizzare gli strumenti di amministrazione e di sistema ed eventuali strumenti di gestione associati a ruoli e funzionalità installati nel server locale. Lo spostamento degli strumenti di gestione associati a ruoli e funzionalità potrebbe impedire la disinstallazione corretta di tali strumenti quando diventa necessario. Dopo la disinstallazione di un ruolo o di una funzionalità, è possibile che un collegamento non funzionale a uno strumento la cui scelta rapida è stata spostata rimanga nel menu **Strumenti**. Se si reinstalla il ruolo, viene creato un collegamento duplicato allo stesso strumento nel menu **Strumenti**, ma uno dei collegamenti non funzionerà.

Gli strumenti associati a ruoli e funzionalità installati come parte di Strumenti di amministrazione remota del server in un computer client basato su Windows possono essere organizzati in cartelle personalizzate. La disinstallazione della funzionalità o del ruolo padre non influisce sui collegamenti dello strumento disponibili in un computer remoto che esegue Windows 10.

Nella procedura riportata di seguito viene descritto come creare una cartella di esempio denominata *strumenti*e come spostare i collegamenti per due script di Windows PowerShell nella cartella accessibili dal menu strumenti di Server Manager.

#### <a name="to-customize-the-tools-menu-by-adding-shortcuts-in-administrative-tools"></a>Per personalizzare il menu Strumenti aggiungendo collegamenti negli Strumenti di amministrazione

1.  creare una nuova cartella denominata *strumenti* in una posizione comoda.

    > [!NOTE]
    > Dati i diritti di accesso limitati per la cartella **Strumenti di amministrazione** , non è consentito creare una nuova cartella direttamente nella cartella **Strumenti di amministrazione** . È necessario creare una nuova cartella altrove, ad esempio sul Desktop, quindi copiare la nuova cartella nella cartella **Strumenti di amministrazione** .

2.  spostare *o copiare gli strumenti* in **Pannello di controllo/sistema e sicurezza/strumenti di amministrazione**. Per impostazione predefinita, è necessario essere un membro del gruppo Administrators nel computer per apportare modifiche alla cartella **Strumenti di amministrazione** .

3.  Se non è necessario limitare i diritti di accesso degli utenti ai collegamenti personalizzati degli strumenti, andare al passaggio 6. In caso contrario, fare clic con il pulsante destro del mouse sul file dello strumento, o la cartella *MyTools*, quindi fare clic su **Proprietà**.

4.  Nella scheda **sicurezza** della finestra di dialogo **Proprietà** del file fare clic su **modifica**.

5.  per gli utenti per i quali si desidera limitare l'accesso agli strumenti, deselezionare le caselle di controllo per le autorizzazioni di **lettura & esecuzione**, **lettura**e **scrittura** . Queste autorizzazioni sono ereditate dal collegamento dello strumento nella cartella **Strumenti di amministrazione**.

    Se si modificano i diritti di accesso per un utente mentre l'utente utilizza Server Manager (o quando Server Manager è aperto), le modifiche non vengono visualizzate nel menu **strumenti** fino a quando l'utente non riavvia Server Manager.

    > [!NOTE]
    > Se si limita l'accesso a un'intera cartella copiata in strumenti di amministrazione, gli utenti con restrizioni possono non visualizzare né la cartella né il relativo contenuto nel menu**strumenti** Server Manager.
    > 
    > modificare le autorizzazioni per la cartella nella cartella **strumenti di amministrazione** . Poiché le cartelle e i file nascosti in strumenti di amministrazione sono sempre visualizzati nel menu**strumenti** di Server Manager, non usare l'impostazione **nascosta** nella finestra di dialogo **proprietà** di un file o di una cartella per limitare l'accesso degli utenti ai collegamenti personalizzati degli strumenti.
    > 
    > Le autorizzazioni **Nega** sovrascrivono sempre le autorizzazioni **Consenti**.

6.  Fare clic con il pulsante destro del mouse sullo strumento, sullo script o sul file eseguibile originale per cui si desidera aggiungere voci dal menu **strumenti** , quindi scegliere **Crea collegamento**.

7.  spostare il collegamento nella cartella *strumenti* di amministrazione.

8.  Aggiornare o riavviare Server Manager, se necessario, per visualizzare il collegamento personalizzato dello strumento nel menu **strumenti** .

## <a name="BKMK_roles"></a>Gestire i ruoli nelle Home page del ruolo
Dopo aver aggiunto i server al pool di server Server Manager e Server Manager raccoglie i dati di inventario sui server del pool, Server Manager aggiunge pagine al riquadro di navigazione per i ruoli individuati nei server gestiti. Nel riquadro **Server** nelle pagine ruolo sono elencati i server gestiti che eseguono il ruolo. Per impostazione predefinita, nei riquadri **Eventi**, **Best Practices Analyzer**, **Servizi**e **Prestazioni** vengono visualizzati dati per tutti i server che eseguono il ruolo. La selezione di server specifici nel riquadro **Server** limita l'ambito dei risultati per eventi, servizi, contatori di prestazioni e BPA ai soli server selezionati. Gli strumenti di gestione sono in genere disponibili nel menu **strumenti** della console di Server Manager, dopo l'installazione o la individuazione di un ruolo o di una funzionalità in un server gestito. È anche possibile fare clic con il pulsante destro del mouse sui server nel riquadro **Server** per un ruolo o un gruppo, quindi avviare lo strumento di gestione che si desidera utilizzare.

In Windows Server 2016 i ruoli e le funzionalità seguenti dispongono di strumenti di gestione integrati nella console di Server Manager come pagine.

-   **Servizi file e archiviazione.** Le pagine di Servizi file e archiviazione includono comandi e riquadri personalizzati per la gestione di volumi, condivisioni, dischi virtuali iSCSI e pool di archiviazione. Quando si apre il ruolo Servizi file e archiviazione home page in Server Manager, viene aperto un riquadro ritrazione che visualizza le pagine di gestione personalizzate per servizi file e archiviazione. Per altre informazioni sulla distribuzione e la gestione di Servizi file e archiviazione, vedere [Servizi file e archiviazione](https://go.microsoft.com/fwlink/p/?LinkId=241530).

-   **Servizi Desktop remoto.** Le pagine Servizi Desktop remoto includono comandi e riquadri personalizzati per la gestione di sessioni, licenze, gateway e desktop virtuali. Per ulteriori informazioni sulla distribuzione e la gestione di Servizi Desktop remoto, vedere [Servizi Desktop remoto (rdS)](https://go.microsoft.com/fwlink/p/?LinkId=241532).

-   **Gestione indirizzi IP (IPAM).** La pagina del ruolo Gestione indirizzi IP include un riquadro **introduttivo** contenente collegamenti alle attività di gestione e configurazione di Gestione indirizzi IP comuni, oltre a una procedura guidata per il provisioning di un server di Gestione indirizzi IP. La home page di Gestione indirizzi IP include inoltre riquadri per la visualizzazione della rete gestita, del riepilogo della configurazione e delle attività pianificate.

    Esistono alcune limitazioni alla gestione di gestione indirizzi IP in Server Manager. A differenza delle tipiche pagine ruolo e gruppo, IPAM non dispone di riquadri **Server**, **Eventi**, **Prestazioni**, **Best Practices Analyzer** o **Servizi**. Non è disponibile alcun modello di Best Practices Analyzer per gestione indirizzi IP; Best Practices Analyzer analisi su Gestione indirizzi IP non sono supportate. Per accedere ai server del pool che eseguono Gestione indirizzi IP, creare un gruppo personalizzato dei server che eseguono Gestione indirizzi IP e accedere all'elenco di server dal riquadro **Server** nella pagina gruppo personalizzata. In alternativa, accedere ai server IPAM dal riquadro **Server** nella pagina gruppo **Tutti i server**.

    Le anteprime del dashboard contengono inoltre righe limitate per Gestione indirizzi IP, rispetto alle anteprime per altri ruoli e gruppi. Facendo clic sulle righe delle anteprime IPAM, è possibile visualizzare eventi, dati di prestazioni e avvisi relativi allo stato di gestibilità per i server che eseguono IPAM. I servizi correlati a Gestione indirizzi IP possono essere gestiti dalle pagine per i gruppi di server che contengono i server Gestione indirizzi IP, ad esempio la pagina per il gruppo **Tutti i server** .

    Per ulteriori informazioni sulla distribuzione e la gestione di gestione [indirizzi IP, vedere IP Address Management (IPAM)](https://go.microsoft.com/fwlink/p/?LinkId=241533).

## <a name="see-also"></a>Vedi anche
[Server Manager](server-manager.md)
[aggiungere i server al server Manager](add-servers-to-server-manager.md)
[creare e gestire i gruppi di server](create-and-manage-server-groups.md)
[visualizzare e configurare i dati relativi a prestazioni, eventi e servizi](view-and-configure-performance-event-and-service-data.md)
[Servizi file e archiviazione](https://go.microsoft.com/fwlink/p/?LinkId=241530)
[Servizi Desktop remoto (rdS)](https://go.microsoft.com/fwlink/p/?LinkId=241532)
[Gestione indirizzi IP (IPAM)](https://go.microsoft.com/fwlink/p/?LinkId=241533)



