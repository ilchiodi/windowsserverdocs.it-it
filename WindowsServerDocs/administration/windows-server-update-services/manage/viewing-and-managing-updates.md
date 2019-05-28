---
title: Visualizzazione e gestione degli aggiornamenti
description: Argomento di Windows Server Update Service (WSUS) - come visualizzare e gestire gli aggiornamenti nella console di WSUS
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ac70192b-0309-4385-b697-2e8eda51911c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f2ffc6680239fa468b058b74f9acf26d22f2b65f
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222614"
---
# <a name="viewing-and-managing-updates"></a>Visualizzazione e gestione degli aggiornamenti

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Per visualizzare e gestire gli aggiornamenti, è possibile utilizzare la console di WSUS.

## <a name="viewing-updates"></a>Visualizzazione degli aggiornamenti
Nel **aggiornamenti** pagina, è possibile eseguire le operazioni seguenti:

-   Visualizzare gli aggiornamenti. Cenni preliminari sull'aggiornamento Visualizza gli aggiornamenti che sono stati sincronizzati dall'origine degli aggiornamenti al server WSUS e sono disponibili per l'approvazione.

-   Filtrare gli aggiornamenti. Nella visualizzazione predefinita è possibile filtrare gli aggiornamenti per lo stato di approvazione e lo stato di installazione. L'impostazione predefinita è per gli aggiornamenti non approvati che sono necessari per alcuni client o che hanno subito gli errori di installazione in alcuni client. È possibile modificare questa vista modificando i filtri di stato approvazione stato e l'installazione e quindi fare clic su **aggiornamento**.

-   Creare nuove viste di aggiornamento. Nel **azioni** riquadro, fare clic su **nuova vista di aggiornamento**. È possibile filtrare gli aggiornamenti per la classificazione, prodotto, il gruppo per cui sono stati approvati e la data di sincronizzazione. È possibile ordinare l'elenco facendo clic sull'intestazione di colonna appropriata nella barra del titolo.

-   Ricerca di aggiornamenti. È possibile cercare un aggiornamento individuale o un set di aggiornamenti per titolo, descrizione, articolo della Knowledge Base o il numero di Microsoft Security Response Center per l'aggiornamento.

-   Visualizzare i dettagli, stato e la cronologia delle revisioni per ogni aggiornamento.

-   Approvare e rifiutare gli aggiornamenti.

#### <a name="to-view-updates"></a>Per visualizzare gli aggiornamenti

1.  Nella console di amministrazione di WSUS, espandere il nodo aggiornamenti e quindi fare clic su tutti gli aggiornamenti.

2.  Per impostazione predefinita, gli aggiornamenti vengono visualizzati con il titolo, la classificazione, percentuale applicabile/non installato e lo stato di approvazione. Se si desidera visualizzare le proprietà di aggiornamento più o diversi, il pulsante destro sulla barra di intestazione di colonna e selezionare le colonne appropriate.

3.  Per ordinare in base a criteri diversi, ad esempio lo stato di download, title, classificazione, la data di rilascio o lo stato di approvazione, fare clic sull'intestazione di colonna appropriata.

#### <a name="to-filter-the-list-of-updates-displayed-on-the-updates-page"></a>Per filtrare l'elenco di aggiornamenti visualizzati nella pagina aggiornamenti

1.  Nella console di amministrazione WSUS espandere **aggiornamenti**, quindi fare clic su **tutti gli aggiornamenti**.

2.  Nel riquadro centrale accanto a **approvazione**, selezionare lo stato di approvazione desiderato e accanto a **stato** Selezionare lo stato di installazione desiderato. Fare clic su **aggiornamento**.

#### <a name="to-create-a-new-update-view-on-wsus"></a>Per creare una nuova vista update in WSUS

1.  Nella console di amministrazione WSUS espandere **aggiornamenti**, quindi fare clic su **tutti gli aggiornamenti**.

2.  Nel **azioni** riquadro, fare clic su **nuova vista di aggiornamento**.

3.  Nel **Aggiungi visualizzazione aggiornare** finestra, sotto **passaggio 1: selezionare le proprietà**, selezionare le proprietà necessarie per filtrare la visualizzazione di aggiornamento:

    -   Selezionare gli aggiornamenti sono in una specifica classificazione per filtrare gli aggiornamenti appartenenti a uno o più classificazioni degli aggiornamenti.

    -   Selezionare gli aggiornamenti sono di un prodotto specifico filtrare gli aggiornamenti per uno o più prodotti o le famiglie di prodotti.

    -   Selezionare gli aggiornamenti vengono approvati per uno specifico gruppo filtrare gli aggiornamenti approvati per uno o più gruppi di computer.

    -   Selezionare gli aggiornamenti sono stati sincronizzati entro un periodo di tempo specifico per filtrare gli aggiornamenti sincronizzati in un momento specifico.

    -   Selezionare gli aggiornamenti sono aggiornamenti WSUS per filtrare gli aggiornamenti di WSUS.

4.  Sotto **passaggio 2: modificare le proprietà**, fare clic sulle parole sottolineate per selezionare i valori desiderati.

5.  In **passaggio 3: Specificare un nome**, denominare la nuova vista.

6.  Fare clic su **OK**.

La nuova visualizzazione verrà visualizzato nel riquadro di visualizzazione albero in aggiornamenti. Verrà visualizzato, ad esempio le visualizzazioni standard, nel riquadro centrale quando si seleziona.

#### <a name="to-search-for-an-update"></a>Per eseguire la ricerca di un aggiornamento

1.  Selezionare il **aggiornamenti** nodo (o qualsiasi nodo sottostante).

2.  Nel **azioni** riquadro, fare clic su **ricerca**.

3.  Nel **ricerca** finestra via il **aggiornamenti** immettere i criteri di ricerca. È possibile utilizzare testo dal **titolo**, **Descrizione**, e **al numero dell'articolo della Microsoft Knowledge Base (KB)** campi. Ognuno di questi elementi è una proprietà elencata nella **dettagli** scheda nelle proprietà dell'aggiornamento.

#### <a name="to-view-the-properties-for-an-update"></a>Per visualizzare le proprietà per un aggiornamento

1.  Nella console di amministrazione WSUS espandere **aggiornamenti**, quindi fare clic su **tutti gli aggiornamenti**.

2.  Nell'elenco degli aggiornamenti, selezionare l'aggiornamento che si desidera visualizzare.

3.  Nel riquadro inferiore, si vedrà le sezioni di proprietà diversi:

    -   La barra del titolo visualizza il titolo dell'aggiornamento. ad esempio, sicurezza aggiornamento per Windows Media Player 9 (KB911565).

    -   Nella sezione stato Visualizza lo stato di installazione dell'aggiornamento (il computer in cui deve essere installato, i computer in cui è stato installato con errori, computer in cui è stato installato oppure non è applicabile e i computer che non hanno segnalato lo stato per l'aggiornamento), nonché informazioni generali (KB e MSRC numeri data di rilascio e così via).

    -   La sezione Descrizione visualizza una breve descrizione dell'aggiornamento.

    -   La sezione dettagli aggiuntivi visualizza le informazioni seguenti:

        -   Il comportamento di installazione dell'aggiornamento (o meno è rimovibile, viene richiesto un riavvio, richiede l'input dell'utente, oppure deve essere installato in modo esclusivo).

        -   Se l'aggiornamento è condizioni di licenza Software Microsoft

        -   I prodotti a cui si applica l'aggiornamento

        -   Gli aggiornamenti che sostituiscono questo aggiornamento

        -   Gli aggiornamenti che sono stati sostituiti da questo aggiornamento

        -   Le lingue supportate dall'aggiornamento

        -   L'ID di aggiornamento

Si noti che è possibile eseguire questa procedura in un solo aggiornamento alla volta. Se si selezionano più aggiornamenti, verrà visualizzato solo il primo aggiornamento nell'elenco nella **proprietà** riquadro.

## <a name="managing-updates-with-wsus"></a>Gestione degli aggiornamenti con WSUS
Gli aggiornamenti vengono utilizzati per l'aggiornamento o fornendo una sostituzione completa per il software installato in un computer. Ogni aggiornamento è disponibile in Microsoft Update è costituito da due componenti:

-   Metadati: Vengono fornite informazioni sull'aggiornamento. Ad esempio, i metadati forniscono informazioni per le proprietà di un aggiornamento, consentendo di individuare per quali l'aggiornamento è utile. Inoltre, i metadati includono condizioni di licenza Software Microsoft. Il pacchetto di metadati scaricato per un aggiornamento è in genere molto inferiore rispetto al pacchetto di file di aggiornamento effettivo.

-   File di aggiornamento: I file effettivi necessari per installare un aggiornamento in un computer.

Quando gli aggiornamenti vengono sincronizzati con il server WSUS, i metadati e i file di aggiornamento vengono archiviati in due posizioni diverse. I metadati vengono archiviati nel database WSUS. File di aggiornamento possono essere archiviati nel server WSUS o nei server di Microsoft Update, a seconda di come è stato configurato le opzioni di sincronizzazione. Se si sceglie di archiviare i file di aggiornamento sul server di Microsoft Update, vengono scaricati solo i metadati al momento della sincronizzazione. approvare gli aggiornamenti tramite la console WSUS e quindi i computer client ottengono i file di aggiornamento direttamente da Microsoft Update al momento dell'installazione. Per ulteriori informazioni sulle opzioni per l'archiviazione degli aggiornamenti, vedere sezione [1.3. Scegliere una strategia di archiviazione WSUS](../plan/plan-your-wsus-deployment.md#13-choose-a-wsus-storage-strategy) del passaggio 1: Preparare la distribuzione di WSUS, nella Guida alla distribuzione di WSUS.

Verrà impostazione e l'esecuzione di sincronizzazioni, aggiunta di computer e gruppi di computer e la distribuzione degli aggiornamenti a intervalli regolari. Nell'elenco seguente vengono forniti esempi di attività generali che potrebbe intraprendere nell'aggiornamento dei computer con Windows Server Update SERVICES.

-   Determinare un piano di gestione aggiornamento generale in base alla topologia di rete e larghezza di banda, alle esigenze aziendali e struttura organizzativa.

    -   Se si desidera impostare una gerarchia di server WSUS e come deve essere strutturata della gerarchia.

    -   Gruppi di computer per creare e come assegnare computer a questi (destinazione lato server o lato client).

    -   Il database da utilizzare per i metadati dell'aggiornamento (ad esempio, Database interno di Windows, SQL Server).

    -   Se gli aggiornamenti devono essere sincronizzati automaticamente e l'ora.

-   Impostare le opzioni di sincronizzazione, ad esempio l'origine degli aggiornamenti, classificazione prodotto e aggiornamento, lingua, le impostazioni di connessione, percorso di archiviazione e pianificazione della sincronizzazione.

-   Ottenere gli aggiornamenti e i metadati associati nel server WSUS tramite la sincronizzazione da Microsoft Update o un server WSUS upstream.

-   Approvare o rifiutare gli aggiornamenti. È la possibilità di consentire agli utenti di installare gli aggiornamenti (se sono amministratori locali nei computer client).

-   Configurare le approvazioni automatiche. È inoltre possibile configurare se si desidera abilitare l'approvazione automatica delle revisioni agli aggiornamenti esistenti o approvare manualmente le revisioni. Se si sceglie di approvare manualmente le revisioni, il server WSUS continuerà con la versione precedente, fino a quando non sono state approvate manualmente nella nuova revisione.

-   Controllare lo stato degli aggiornamenti. È possibile visualizzare lo stato di aggiornamento, stampare un report o configurare la posta elettronica per i rapporti di stato normale.

## <a name="update-products-and-classifications"></a>Aggiornamenti dei prodotti e classificazioni
Gli aggiornamenti disponibili in Microsoft Update si differenziano per prodotto (o una famiglia di prodotti) e la classificazione.

Un prodotto è un'edizione specifica di un sistema operativo o un'applicazione, ad esempio, Windows Server 2012. Una famiglia di prodotti è il sistema operativo di base o l'applicazione da cui derivano i singoli prodotti. Un esempio di una famiglia di prodotti è Microsoft Windows, di cui Windows Server 2012 è un membro. È possibile selezionare i prodotti o le famiglie di prodotti per cui si desidera che il server per sincronizzare gli aggiornamenti. È possibile specificare una famiglia di prodotti o singoli prodotti all'interno della famiglia. Selezione di qualsiasi prodotto o famiglia di prodotti riceveranno aggiornamenti per le versioni attuali e future del prodotto.

Classificazioni degli aggiornamenti rappresentano il tipo di aggiornamento. Per un determinato prodotto o famiglia di prodotti, gli aggiornamenti possono essere disponibili in più classificazioni di aggiornamento (ad esempio, la famiglia di Windows 7 gli aggiornamenti critici e aggiornamenti della sicurezza). Nella tabella seguente sono elencate le classificazioni degli aggiornamenti.

| Classificazioni aggiornamento  | Descrizione   |
|--|--|
|Aggiornamenti critici|Ampiamente correzioni rilasciate per problemi specifici che risolvono bug correlati critico, non correlato alla protezione.|
|Aggiornamento delle definizioni|Aggiornamenti a virus o altri file di definizione.|
|Driver|Componenti software progettati per supportare nuovo hardware.|
|Feature Pack|Rilasci di nuove funzionalità, in genere eseguito il rollback in prodotti alla versione successiva.|
|Aggiornamenti della sicurezza|Correzioni rilasciate su vasta scala per i prodotti specifici, problemi di protezione.|
|Service Pack|Un set cumulativo di tutti gli aggiornamenti rapidi, aggiornamenti della sicurezza, aggiornamenti critici e aggiornamenti creati dopo il rilascio del prodotto. Service Pack possono inoltre contenere un numero limitato di funzionalità o modifiche della progettazione richieste dai clienti.|
|Strumenti|Utilità o funzionalità che facilitano l'esecuzione di un'attività o un set di attività.|
|Aggiornamenti cumulativi|Un set cumulativo di aggiornamenti rapidi, aggiornamenti della sicurezza, aggiornamenti critici e altri aggiornamenti sono riuniti insieme per semplificare la distribuzione. In genere è un rollup destinato a un'area specifica, ad esempio, sicurezza o un componente specifico, ad esempio Internet Information Services (IIS).|
|Aggiornamenti|Ampiamente correzioni rilasciate per problemi specifici che risolvono bug correlati non critico, non correlato alla protezione.|

## <a name="icons-used-for-updates-in-windows-server-update-services"></a>Icone usate per gli aggiornamenti in Windows Server Update Services
 Gli aggiornamenti in WSUS sono rappresentati da una delle icone seguenti.  
 Per visualizzare queste icone, è necessario abilitare la colonna di sostituzione nella console di Update Services.
 
### <a name="no-icon"></a>Nessuna icona
 L'aggiornamento non ha alcuna relazione di sostituzione con qualsiasi altro aggiornamento.

 **Problemi operativi:**  

 Non sono presenti problemi operativi.  
 
### <a name="superseding-icon"></a>Icona sostituzione
 ![icona](../../media/wsus/wsus-superseding.png) Questo aggiornamento sostituisce altri aggiornamenti.

 **Problemi operativi:**  

 Non sono presenti problemi operativi.  

### <a name="superseded--superseding-icon"></a>Icona sostituito & sostitutivo
 ![icona](../../media/wsus/wsus-superseded.png) Questo aggiornamento è stato sostituito da un altro aggiornamento e sostituisce altri aggiornamenti.

 **Problemi operativi:**  

 Sostituire questi aggiornamenti con gli aggiornamenti in sostituzione, quando possibile.
 
### <a name="superseded-icon"></a>Icona sostituito
 ![icona](../../media/wsus/wsus-superseded-leaf.png) Questo aggiornamento è stato sostituito da un altro aggiornamento.

 **Problemi operativi:**  

 Sostituire questi aggiornamenti con gli aggiornamenti in sostituzione, quando possibile.
