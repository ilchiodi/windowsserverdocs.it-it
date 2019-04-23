---
title: Impostazione delle sincronizzazioni degli aggiornamenti
description: Argomento di Windows Server Update Service (WSUS) - come impostare e configurare sincronizzazioni degli aggiornamenti
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ddd5c395-451b-44a0-8e08-a05db26d2282
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e381316372e68d2a43203b8fc90a243af5f40b02
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869212"
---
# <a name="setting-up-update-synchronizations"></a>Impostazione delle sincronizzazioni degli aggiornamenti

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Durante la sincronizzazione, un server WSUS scarica gli aggiornamenti (aggiornamento del file e metadati) da un'origine degli aggiornamenti. Inoltre, Scarica nuove classificazioni dei prodotti e le categorie, se presente. Quando il server WSUS viene sincronizzato per la prima volta, verrà scaricato tutti gli aggiornamenti specificati durante la configurazione delle opzioni di sincronizzazione. Dopo la prima sincronizzazione, il server WSUS scarica solo gli aggiornamenti dall'origine degli aggiornamenti, nonché le revisioni di metadati per gli aggiornamenti esistenti e le scadenze degli aggiornamenti.

La prima volta che un server WSUS scarica aggiornamenti può richiedere molto tempo. Se si imposta più server WSUS, è possibile velocizzare il processo in una certa misura scaricando tutti gli aggiornamenti in un server WSUS e quindi copiare gli aggiornamenti per la directory contenuto degli altri server WSUS.

È possibile copiare il contenuto dalla directory del contenuto del server WSUS a un altro. Quando si esegue la procedura di installazione di WSUS post, viene specificato il percorso della directory dei contenuti. È possibile utilizzare lo strumento wsusutil.exe per esportare i metadati dell'aggiornamento da un server WSUS in un file. È quindi possibile importare tale file in altri server WSUS.

## <a name="setting-up-update-synchronizations"></a>Impostazione delle sincronizzazioni degli aggiornamenti
Il **Opzioni** pagina è il punto di accesso centrale nella Console di amministrazione di WSUS per la personalizzazione di come il server WSUS Sincronizza gli aggiornamenti. È possibile specificare gli aggiornamenti vengono sincronizzati automaticamente, in cui il server Ottiene gli aggiornamenti, le impostazioni di connessione e la pianificazione della sincronizzazione. È anche possibile utilizzare la configurazione guidata di **Opzioni** pagina per configurare o riconfigurare il server WSUS in qualsiasi momento.

### <a name="synchronizing-update-by-product-and-classification"></a>Aggiornamento per prodotto e classificazione di sincronizzazione
Un server WSUS scarica gli aggiornamenti in base i prodotti o le famiglie di prodotti (ad esempio, Windows o Windows Server 2008, Datacenter edition) e le classificazioni (ad esempio, gli aggiornamenti critici o gli aggiornamenti della sicurezza) specificato. alla prima sincronizzazione, il server WSUS scarica tutti gli aggiornamenti disponibili per le categorie specificate. Durante le sincronizzazioni successive, il download server solo gli aggiornamenti più recenti di WSUS (o le modifiche per gli aggiornamenti già disponibili sul server WSUS) per le categorie specificate.

È possibile specificare aggiornamenti dei prodotti e classificazioni sul **Opzioni** pagina **prodotti e classificazioni**. I prodotti sono elencati in una gerarchia, raggruppata per famiglia di prodotti. Se si seleziona Windows, si selezionano automaticamente tutti i prodotti che rientrano in tale gerarchia di prodotto. Selezionando la casella di controllo padre selezionare tutti gli elementi, nonché tutte le versioni future. selezionando le caselle di controllo figlio non selezionerà le caselle di controllo padre. L'impostazione predefinita per i prodotti è tutti i prodotti Windows e l'impostazione predefinita per le classificazioni è fondamentale e gli aggiornamenti della sicurezza.

Se un server WSUS è in esecuzione in modalità di replica, non sarà in grado di eseguire questa attività. Per altre informazioni sulla modalità di replica, vedere [modalità di Replica WSUS in esecuzione](running-wsus-replica-mode.md), e [passaggio 1: Preparare la distribuzione WSUS](../plan/plan-your-wsus-deployment.md).

##### <a name="to-specify-update-products-and-classifications-for-synchronization"></a>Per specificare aggiornamenti dei prodotti e le classificazioni per la sincronizzazione

1.  Nella Console di amministrazione di WSUS, fare clic sui **Opzioni** nodo.

2.  Fare clic su **prodotti e classificazioni**, quindi fare clic su di **prodotti** scheda.

3.  Selezionare le caselle di controllo dei prodotti o le famiglie di prodotti si desidera aggiornare con WSUS e quindi fare clic su **OK**.

4.  Nel **classificazioni** Selezionare le caselle di controllo delle classificazioni di aggiornamento consigliabile che il server WSUS per sincronizzare e quindi fare clic su **OK**.

> [!NOTE]
> È possibile rimuovere i prodotti o classificazioni nello stesso modo. Il server WSUS verrà interrotta la sincronizzazione di nuovi aggiornamenti per i prodotti che è stata deselezionata. Tuttavia, gli aggiornamenti che sono stati sincronizzati per tali prodotti prima è stata deselezionata li rimarranno sul proprio server WSUS e verranno indicati come disponibili.
> 
> Per rimuovere questi prodotti, rifiutare l'aggiornamento, come documentato nel [operazioni di aggiornamento](updates-operations.md)e quindi usare i [pulizia Server della procedura guidata](the-server-cleanup-wizard.md) per rimuoverli.

### <a name="synchronizing-updates-by-language"></a>La sincronizzazione degli aggiornamenti dal linguaggio
Il server WSUS scarica gli aggiornamenti in base alle lingue specificate. È possibile sincronizzare gli aggiornamenti in tutte le lingue in cui sono disponibili, oppure è possibile specificare un sottoinsieme di lingue. Se si dispone di una gerarchia di server WSUS, è necessario scaricare gli aggiornamenti in lingue diverse, assicurarsi di avere specificato tutte le lingue necessarie nel server upstream. In un server downstream è possibile specificare un sottoinsieme di lingue specificate nel server upstream.

### <a name="synchronizing-updates-from-the-microsoft-update-catalog"></a>La sincronizzazione degli aggiornamenti dal catalogo di Microsoft Update
Per informazioni dettagliate sulla sincronizzazione degli aggiornamenti dal sito Microsoft Update Catalog, vedere: [WSUS e il sito del catalogo](wsus-and-the-catalog-site.md).

### <a name="synchronizing-device-updates-by-inventory-inventory-based-synchronization"></a>La sincronizzazione degli aggiornamenti di dispositivo dall'inventario (inventario basato su sincronizzazione)
Si consiglia di non sincronizzare le categorie di intere per il server WSUS determinate categorie di prodotti e classificazioni (ad esempio driver) contengono un numero molto elevato di aggiornamenti. In questo modo possa causare problemi di prestazioni e manutenzione. Il sistema di magazzino WSUS raccoglie informazioni di identificazione non dai dispositivi client e le informazioni di inventario vengono utilizzate per recuperare metadati sufficienti aggiornamento da Microsoft Update. Questo meccanismo è equivalente alla ricerca di Microsoft Update Catalog automaticamente, WSUS solo gli aggiornamenti per i dispositivi rilevati durante l'importazione di dispositivi gestiti.

Abilitazione di questa funzionalità di inventario è l'unico metodo supportato per ottenere determinati set manutenzione basato su modello che non vengono pubblicati nel catalogo di Microsoft Update e il firmware del dispositivo.

Gli aggiornamenti sincronizzati in questo modo vengono esaminati e approvati esattamente come qualsiasi altro aggiornamento e inoltre soggetto alle stesse regole di approvazione automatica, la sostituzione e scadenza e altri comportamenti associati aggiornamenti tradizionali.

WSUS esegue il filtro sul lato server quando i client richiedono determinati aggiornamenti di Driver e Firmware, inclusi gli aggiornamenti che sono stati importati dall'inventario. Pertanto, un computer client o un dispositivo riceverà i metadati e detectoids per i driver e gli aggiornamenti dei driver solo per i dispositivi realmente associati a tale dispositivo. Questo comportamento riduce al minimo il tempo di analisi di client e riduce i dati trasferiti tra il client e il server WSUS.

> [!NOTE]
> Quando è abilitata la sincronizzazione basata su inventario, WSUS mantiene l'inventario dei dispositivi per singolo dispositivo. solo un riepilogo roll-up (rimuovere duplicato elenco di ID) viene sempre inviato al server WSUS upstream. Server WSUS padre non si ricevono informazioni su quali dispositivi sono associati a quali computer, né il numero di istanze di un determinato dispositivo esiste all'interno della gerarchia WSUS. In generale, questo riepilogo rollup non può essere utilizzata per identificare o numero di dispositivi in una rete gestiti da WSUS.

## <a name="configuring-proxy-server-settings"></a>Configurazione delle impostazioni Server Proxy
È possibile configurare il server WSUS per utilizzare un server proxy durante la sincronizzazione con un server upstream o Microsoft Update. Questa impostazione verrà applicata solo quando il server WSUS in esecuzione le sincronizzazioni. Per impostazione predefinita, il server WSUS tenterà di connettersi direttamente al server upstream o a Microsoft Update.

#### <a name="to-specify-a-proxy-server-for-synchronization"></a>Per specificare un server proxy per la sincronizzazione

1.  Nella Console di amministrazione di WSUS, fare clic su **Opzioni**, quindi fare clic su **origine aggiornamenti e Server Proxy**.

2.  Nel **Server Proxy** selezionare il **Usa un server proxy durante la sincronizzazione** casella di controllo e quindi digitare il nome del server e numero di porta il server proxy.

    > [!NOTE]
    > Configurare WSUS con lo stesso numero di porta che il server proxy è configurato per utilizzare.

    -   Se si desidera connettersi al server proxy con le credenziali dell'utente specifico, selezionare la **usare le credenziali dell'utente per connettersi al server proxy** casella di controllo e quindi immettere il nome utente, dominio e password dell'utente nelle caselle corrispondenti .

    -   Se si desidera abilitare l'autenticazione di base per l'utente si connette al server proxy, selezionare la **Consenti autenticazione di base (password inviata in testo non crittografato)** casella di controllo.

3.  Fare clic su **OK**.

    > [!NOTE]
    > Poiché Windows Server Update SERVICES avvia tutto il traffico di rete, non è necessario configurare Windows Firewall in un server WSUS connesso direttamente a Microsoft update.

## <a name="configuring-the-update-source"></a>Configurazione dell'origine di aggiornamento
L'origine degli aggiornamenti è il percorso da cui il server WSUS Ottiene gli aggiornamenti e aggiornare i metadati. È possibile specificare che deve essere l'origine degli aggiornamenti Microsoft Update o un altro server WSUS (il server WSUS che funge da origine degli aggiornamenti è il server padre e il server è il server downstream).

Opzioni per personalizzare la modalità del server WSUS Sincronizza con origine degli aggiornamenti, tra cui:

-   È possibile specificare una porta personalizzata per la sincronizzazione. Per informazioni sulla configurazione delle porte, vedere [passaggio 3: Configurare WSUS](../deploy/2-configure-wsus.md) nella Guida alla distribuzione di WSUS.

-   È possibile utilizzare livelli SSL (Secure Socket) per la sincronizzazione protetta di informazioni relative all'aggiornamento tra server WSUS. Per ulteriori informazioni sull'utilizzo di SSL, vedere la sezione "3.5. Sicuro WSUS con Secure Sockets Layer Protocol"di [passaggio 3: Configurare WSUS](../deploy/2-configure-wsus.md) nella Guida alla distribuzione di WSUS.

## <a name="synchronizing-manually-or-automatically"></a>La sincronizzazione manualmente o automaticamente
È possibile sincronizzare manualmente il server WSUS o specificare un'ora per la sincronizzazione automatica.

#### <a name="to-manually-synchronize-the-wsus-server"></a>Per sincronizzare manualmente il server WSUS

1.  Nella Console di amministrazione di WSUS, fare clic su **Opzioni**, quindi fare clic su **pianificazione della sincronizzazione**.

2.  Fare clic su **sincronizzare manualmente**, quindi fare clic su **OK**.

#### <a name="to-set-up-an-automatic-synchronization-schedule"></a>Per impostare una pianificazione di sincronizzazione automatica

1.  Nella Console di amministrazione di WSUS, fare clic su **Opzioni**, quindi fare clic su **pianificazione della sincronizzazione**.

2.  Fare clic su **sincronizzare automaticamente**.

3.  Per **prima sincronizzazione**, selezionare l'ora in cui la sincronizzazione per ogni giorno di inizio.

4.  per la **sincronizzazioni giornaliere**, selezionare il numero di sincronizzazioni da eseguire ogni giorno. Ad esempio, se si desidera partire un giorno alle 3.00 quattro sincronizzazioni, quindi sincronizzazioni verificherà alle 3:00, dalle 09:00, alle 15.00 e 9:00 ogni giorno. Si noti che un offset casuale ora verranno aggiunte al tempo di sincronizzazione pianificato per lasciare spazi le connessioni del server a Microsoft Update.

5.  Fare clic su **OK**.

#### <a name="to-synchronize-your-wsus-server-immediately"></a>Per sincronizzare immediatamente il server WSUS

1.  Nella Console di amministrazione di WSUS, selezionare il nodo server principale.

2.  Nel **Panoramica** riquadro, in **lo stato di sincronizzazione**, fare clic su **Sincronizza**.

> [!NOTE]
> Viene avviata la sincronizzazione dal server downstream.
