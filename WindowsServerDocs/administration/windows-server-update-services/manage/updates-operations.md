---
title: Operazioni di aggiornamento
description: Argomento di Windows Server Update Service (WSUS)-come gestire gli aggiornamenti, incluso il processo di approvazione
ms.prod: windows-server
ms.technology: manage-wsus
ms.topic: article
ms.assetid: 4cb7ff54-3014-4e91-842a-a7b831ea59ff
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 327bff2e678e278dcba05ce1df807dc3842a56cd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80828494"
---
# <a name="updates-operations"></a>Operazioni di aggiornamento

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Dopo gli aggiornamenti sono stati sincronizzati con il server WSUS, essi verrà eseguite automaticamente la rilevanza ai computer client del server. Tuttavia, è necessario approvare gli aggiornamenti prima di distribuirli ai computer della rete. Quando si approva un aggiornamento, sostanza si istruisce WSUS cosa fare con esso (le scelte disponibili sono **installare** o **Rifiuta** per un nuovo aggiornamento). È possibile approvare gli aggiornamenti per il gruppo **tutti i computer** o per i sottogruppi. Se non si approva un aggiornamento, lo stato di approvazione rimane **non approvato**, e il server WSUS consente ai client di valutare se è necessario l'aggiornamento.

Se il server WSUS è in esecuzione in modalità di replica, non sarà possibile approvare gli aggiornamenti nel server WSUS. Per ulteriori informazioni sulla modalità di replica, vedere [esecuzione della modalità di replica WSUS](running-wsus-replica-mode.md).

## <a name="approving-updates"></a>Approvazione di aggiornamenti
È possibile approvare l'installazione degli aggiornamenti per tutti i computer della rete di Windows Server Update SERVICES o per gruppi di computer diverso. Dopo l'approvazione di un aggiornamento, è possibile eseguire uno o più di quanto segue:

-   Applicare questa approvazione ai gruppi figlio, se presente.

-   Impostare una scadenza per l'installazione automatica. Quando si seleziona questa opzione, impostare determinati orari e date per installare gli aggiornamenti, si esegue l'override di tutte le impostazioni nei computer client. Inoltre, è possibile specificare una data già trascorsa per la scadenza se si desidera approvare un aggiornamento immediatamente (per l'installazione la volta successiva che i computer client contattano il server WSUS).

-   Rimuovere un aggiornamento installato questo aggiornamento supporta la rimozione.

È necessario tenere presente due aspetti importanti:

-   In primo luogo, è possibile impostare una scadenza per l'installazione automatica per un aggiornamento se l'input dell'utente è necessario (ad esempio, specificando un'impostazione relativa all'aggiornamento). Per determinare se un aggiornamento potrebbe richiedere l'input dell'utente, esaminare il **potrebbe richiedere l'input dell'utente** nelle proprietà dell'aggiornamento per un aggiornamento visualizzato nel campo di **Aggiorna** pagina. Controllare anche la presenza di un messaggio nella casella **Approva aggiornamenti** . **l'aggiornamento selezionato richiede l'input dell'utente e non supporta la scadenza dell'installazione**.

-   Se sono disponibili aggiornamenti per il componente server WSUS, non è possibile approvare altri aggiornamenti per i sistemi client fino a quando non viene approvata l'aggiornamento WSUS. Questo messaggio di avviso verrà visualizzato nella finestra di dialogo Approva aggiornamenti: sono presenti aggiornamenti WSUS che non sono stati approvati. È necessario approvare gli aggiornamenti WSUS prima di approvare l'aggiornamento. In questo caso, si deve fare clic sul nodo gli aggiornamenti di WSUS e assicurarsi che tutti gli aggiornamenti in tale visualizzazione stati approvati prima di restituire gli aggiornamenti generali.

#### <a name="to-approve-updates"></a>Per approvare gli aggiornamenti

1.  Nella console di amministrazione di WSUS, fare clic su **aggiornamenti** e quindi fare clic su **tutti gli aggiornamenti**.

2.  Nell'elenco degli aggiornamenti, selezionare l'aggiornamento che si desidera approvare e mouse (o passare al riquadro azioni), nella finestra di dialogo di approvazione degli aggiornamenti, selezionare il gruppo di computer per cui si desidera approvare l'aggiornamento e fare clic sulla freccia accanto a esso.

3.  Selezionare **approvati per l'installazione**, quindi fare clic su **Approva**.

4.  Il **stato approvazione** finestra verrà visualizzato lo stato di avanzamento verso il completamento l'approvazione. Quando il processo è completo, il **Chiudi** viene visualizzato il pulsante. Fare clic su **Chiudi**.

5.  È possibile selezionare una scadenza facendo l'aggiornamento, selezionando il gruppo di computer appropriato, facendo clic sulla freccia accanto a esso e quindi fare clic su **scadenza**.

    -   È possibile selezionare uno dei termini standard (una settimana, due settimane, un mese), oppure è possibile fare clic su **personalizzato** per specificare una data e ora.

    -   Se si desidera che un aggiornamento da installare non appena il contatto di computer client il server, fare clic su **personalizzata**, quindi impostare una data e ora per la data e ora correnti o a uno in passato.

#### <a name="to-approve-multiple-updates"></a>Per approvare gli aggiornamenti più

1.  Nella console di amministrazione di WSUS, fare clic su **aggiornamenti** e quindi fare clic su **tutti gli aggiornamenti**.

2.  Per selezionare più aggiornamenti contigui, premere **MAIUSC** mentre si selezionano gli aggiornamenti. Per selezionare più aggiornamenti non contigui, premere e tenere premuto **CTRL** durante la selezione di aggiornamenti.

3.  Fare doppio clic la selezione e fare clic su **Approva**. Il **Approva aggiornamenti** verrà visualizzata la finestra di dialogo con il **lo stato di approvazione** impostato su **mantenere le impostazioni di approvazione** e **OK** pulsante disabilitato.

4.  È possibile modificare le approvazioni per i singoli gruppi, ma in tal modo non avrà effetto sulle approvazioni figlio. Selezionare il gruppo per il quale si desidera modificare l'approvazione, quindi fare clic sulla freccia a sinistra. Nel menu di scelta rapida, fare clic su **approvati per l'installazione**.

5.  L'approvazione per il gruppo selezionato diventa **installare**. Se sono presenti tutti i gruppi figlio, di approvazione rimane **mantenere approvazione esistente**. Per modificare l'approvazione per i relativi gruppi figlio, fare clic sul gruppo e fare clic sulla freccia a sinistra. Nel menu di scelta rapida, fare clic su **applica agli elementi figlio**.

6.  Per impostare un oggetto figlio specifico da cui ereditare tutti l'approvazione dal padre, selezionare l'elemento figlio e fare clic sulla freccia a sinistra. Nel menu di scelta rapida, fare clic su **come padre**. Se si imposta un elemento figlio ereditano le approvazioni, ma non desidera modificare le approvazioni padre, figlio ereditano le approvazioni esistenti dell'elemento padre.

7.  Se si desidera modificare il comportamento di approvazione per tutti gli elementi figlio, approvare **tutti i computer**, quindi scegliere **applica agli elementi figlio**.

8.  Fare clic su **OK** dopo aver impostato tutte le approvazioni. Il **stato approvazione** finestra verrà visualizzato lo stato di avanzamento verso il completamento l'approvazione. Quando il processo è completo, il **Chiudi** pulsante sarà disponibile. Fare clic su **Chiudi**.

## <a name="declining-updates"></a>Rifiuto degli aggiornamenti
Se si seleziona questa opzione, l'aggiornamento viene rimosso dall'elenco predefinito di aggiornamenti disponibili e il server WSUS non offrirà l'aggiornamento ai client, per la valutazione o l'installazione. È possibile raggiungere questa opzione selezionando un aggiornamento o un gruppo di aggiornamenti e pulsante destro del mouse o passare al riquadro azioni. Gli aggiornamenti rifiutati verranno visualizzato nell'elenco degli aggiornamenti solo se si seleziona **rifiutato** nell'elenco di approvazione quando si specifica il filtro per l'elenco di aggiornamenti in **Vista**.

#### <a name="to-decline-updates"></a>Per rifiutare gli aggiornamenti

1.  Nella console di amministrazione di WSUS, fare clic su **aggiornamenti**, quindi fare clic su **tutti gli aggiornamenti**.

2.  Nell'elenco degli aggiornamenti, selezionare uno o più aggiornamenti che si desidera rifiutare.

3.  Selezionare **rifiuta**, quindi fare clic su **Sì** nel messaggio di conferma.

## <a name="cleaning-up-declined-updates"></a>Pulizia di aggiornamenti rifiutati
Gli aggiornamenti rifiutati continueranno a utilizzare alcune risorse di server WSUS. È consigliabile eseguire la pulitura guidata del server per rimuovere gli aggiornamenti rifiutati dal database WSUS. Per ulteriori informazioni, vedere: [Pulitura guidata del server](the-server-cleanup-wizard.md).

## <a name="reinstating-declined-updates"></a>Sospensione di aggiornamenti rifiutati
Dopo un aggiornamento rifiutato, è possibile ripristinare il.

#### <a name="to-reinstate-declined-updates"></a>Per ripristinare gli aggiornamenti rifiutati

1.  Nella console di amministrazione di WSUS, fare clic su **aggiornamenti** e quindi fare clic su **tutti gli aggiornamenti**.

2.  modificare l' **approvazione** in **rifiutato** e fare clic su **Aggiorna**. Carica l'elenco degli aggiornamenti rifiutati.

3.  Nell'elenco degli aggiornamenti, selezionare uno o più aggiornamenti rifiutati che si desidera ripristinare.

4.  Per ripristinare un aggiornamento specifico, fare clic con l'aggiornamento e selezionare **Approva**. Nella finestra di dialogo **Approva aggiornamenti** fare clic su **OK** per applicare nuovamente lo stato di approvazione predefinito non approvato. L'aggiornamento verrà visualizzato nell'elenco come **non approvato** anziché rifiutato.

Dopo la pulizia di un aggiornamento rifiutato utilizzando la pulitura guidata del server WSUS, questo verrà eliminato dal server WSUS e non verrà più visualizzato nella vista tutti gli aggiornamenti. È possibile importare nuovamente rifiutato, puliti aggiornamenti dal catalogo di Microsoft Update. Per ulteriori informazioni, vedere [WSUS e il sito del catalogo](wsus-and-the-catalog-site.md).

## <a name="change-an-approved-update-to-not-approved"></a>Modificare un aggiornamento approvato non approvata
Se è stato approvato un aggiornamento e si decide di non installarlo in questo momento e invece si desidera salvarlo per un secondo momento, è possibile modificare l'aggiornamento a uno stato non approvato. Ciò significa che l'aggiornamento rimarrà nell'elenco predefinito di aggiornamenti disponibili e segnalerà conformità dei client, ma non verrà installato nei client.

#### <a name="to-change-an-update-from-approved-to-not-approved"></a>Per modificare l'aggiornamento di approvazione per i non approvato

1.  Nella console di amministrazione di WSUS, fare clic su **aggiornamenti**, quindi fare clic su **tutti gli aggiornamenti**.

2.  Nell'elenco degli aggiornamenti, selezionare uno o più aggiornamenti approvati che si desidera modificare non approvata.

3.  Nel menu di scelta rapida o **azioni** selezionare **non approvato**, quindi fare clic su **Sì** nel messaggio di conferma.

## <a name="approving-updates-for-removal"></a>Approvazione degli aggiornamenti per la rimozione
È possibile approvare un aggiornamento per la rimozione (ovvero, per disinstallare un aggiornamento già installato). Questa opzione è disponibile solo se l'aggiornamento è già installato e supporta la rimozione. È possibile specificare una scadenza per l'aggiornamento da disinstallare, o specificare una data già trascorsa per la scadenza, se si desidera rimuovere immediatamente l'aggiornamento (il successivo computer client contattano il server WSUS).

È importante ricordare che non tutti gli aggiornamenti supportano la rimozione. È possibile verificare se un aggiornamento supporta la rimozione selezionando un singolo aggiornamento ed esaminando il riquadro dei **Dettagli** . In **dettagli aggiuntivi**viene visualizzata la categoria **rimovibile** . Se l'aggiornamento non può essere rimosso tramite WSUS, in alcuni casi può essere rimosso con **Installazione applicazioni** dal pannello di **controllo**.

#### <a name="to-approve-updates-for-removal"></a>Per approvare gli aggiornamenti per la rimozione

1.  Nella console di amministrazione di WSUS, fare clic su **aggiornamenti** e quindi fare clic su **tutti gli aggiornamenti**.

2.  Nell'elenco degli aggiornamenti, selezionare uno o più aggiornamenti che si desidera approvare per la rimozione e fare doppio clic su essi (o visitare il **azioni** riquadro).

3.  Nel **Approva aggiornamenti** finestra di dialogo, selezionare il gruppo di computer da cui si desidera rimuovere l'aggiornamento e fare clic sulla freccia accanto a esso.

4.  Selezionare **approvato per la rimozione**, quindi fare clic sul pulsante **Rimuovi** .

5.  Dopo l'approvazione di installazione è stata completata, è possibile selezionare una scadenza facendo nuovamente l'aggiornamento, selezionando il gruppo di computer appropriato e quindi facendo clic sulla freccia accanto a esso. Selezionare quindi **scadenza**. È possibile selezionare uno dei termini standard (una settimana, due settimane, un mese) oppure è possibile fare clic su **personalizzato** per selezionare una data e ora specifiche.

6.  Se si desidera che un aggiornamento da rimuovere non appena il contatto di computer client il server, fare clic su **personalizzata**, e impostare una data nel passato.

## <a name="approving-updates-automatically"></a>Approvare automaticamente gli aggiornamenti
È possibile configurare il server WSUS per l'approvazione automatica di alcuni aggiornamenti. È inoltre possibile specificare l'approvazione automatica delle revisioni agli aggiornamenti esistenti appena diventano disponibili. Questa opzione è selezionata per impostazione predefinita. Una revisione è una versione di un aggiornamento che è state apportate modifiche (ad esempio, è possibile che sia scaduto o potrebbero essere state modificate le regole di applicabilità). Se si sceglie di non approvare automaticamente la versione aggiornata di un aggiornamento, WSUS utilizzerà la versione precedente, e necessario approvare manualmente la revisione dell'aggiornamento.

È possibile creare regole che il server WSUS verrà applicato automaticamente durante la sincronizzazione. Specificare gli aggiornamenti da approvare automaticamente per l'installazione, in base alla classificazione di aggiornamento, per prodotto e dal gruppo di computer. Si applica solo ai nuovi aggiornamenti, anziché gli aggiornamenti rivisti. È inoltre possibile specificare una scadenza di approvazione di aggiornamento, che consente di impostare un numero di giorni e un orario specifico dell'offerta prima di approvazione degli aggiornamenti è installato Data di scadenza. Queste impostazioni sono disponibili nel **Opzioni** riquadro, in **delle approvazioni automatiche**.

#### <a name="to-automatically-approve-updates"></a>Per approvare automaticamente gli aggiornamenti

1.  Nella console di amministrazione di WSUS, fare clic su **Opzioni**, quindi fare clic su **delle approvazioni automatiche**.

2.  In **Regole di aggiornamento** fare clic su **Nuova regola**.

3.  Nella finestra di dialogo **Aggiungi regola** , in **passaggio 1: selezionare le proprietà**, scegliere se usare **quando un aggiornamento è in una classificazione specifica** o **quando un aggiornamento è in un prodotto specifico** (o entrambi) come criterio. Facoltativamente, selezionare se **impostare una scadenza** per l'approvazione.

4.  In **passaggio 2: modificare le proprietà** fare clic sulle proprietà sottolineate per selezionare le classificazioni, i prodotti e i gruppi di computer per i quali si desidera approvazioni automatiche, come applicabile. Facoltativamente, scegliere il giorno e l'ora di scadenza dell'approvazione dell'aggiornamento.

5.  In **passaggio 3: specificare un nome**, digitare un nome univoco per la regola.

6.  Fare clic su **OK**.

Regole di approvazione automatica non verranno applicata agli aggiornamenti che richiedono un contratto (LICENZA) che non è stato ancora accettato nel server. Se si ritiene che l'applicazione di una regola di approvazione automatica non causi tutti gli aggiornamenti importanti di approvazione, è necessario approvare manualmente gli aggiornamenti.

## <a name="automatically-approving-revisions-to-updates-and-declining-expired-updates"></a>Approvare automaticamente le revisioni per gli aggiornamenti e rifiuto scaduto aggiornamenti
La sezione delle approvazioni automatiche del riquadro delle opzioni contiene un'opzione predefinita per approvare automaticamente le revisioni degli aggiornamenti approvati. È inoltre possibile impostare il server WSUS per rifiutare automaticamente aggiornamenti scaduti. Se si sceglie di non approvare automaticamente la versione aggiornata di un aggiornamento, il server WSUS utilizzerà la revisione precedente, e necessario approvare manualmente la revisione dell'aggiornamento.

> [!NOTE]
> Una revisione è una versione di un aggiornamento che è stato modificato (ad esempio, potrebbe essere scaduto oppure sono state aggiornate le regole di applicabilità).

#### <a name="to-automatically-approve-revisions-to-updates-and-decline-expired-updates"></a>Per approvare le revisioni degli aggiornamenti e rifiutare automaticamente aggiornamenti scaduti

1.  Nella console di amministrazione di WSUS, fare clic su **Opzioni**, quindi fare clic su **delle approvazioni automatiche**.

2.  Nella scheda **Avanzate** assicurarsi che entrambe le **nuove revisioni degli aggiornamenti approvati vengano approvate** automaticamente e **rifiutare automaticamente gli aggiornamenti quando viene selezionata una nuova revisione che ne provoca la scadenza** .

3.  Fai clic su OK.

    > [!NOTE]
    > Mantenendo i valori predefiniti per queste opzioni consente che gestire buone prestazioni della rete di Windows Server Update SERVICES. Se si preferisce non aggiornamenti scaduti per essere rifiutato automaticamente, assicurarsi che non si desidera eseguire manualmente su base periodica.

## <a name="automatically-declining-superseded-updates"></a>Rifiuto automaticamente gli aggiornamenti sostituiti
Quando si approva un nuovo aggiornamento che sostituisce un aggiornamento esistente che viene approvato automaticamente, l'aggiornamento sostituito diventa non applicabile a un computer o a un dispositivo dopo l'installazione dell'aggiornamento più recente. Nella console di WSUS è possibile verificare che un aggiornamento non è applicabile per tutti i computer. Una volta che il caso, l'aggiornamento può essere tranquillamente rifiutata. Inoltre, è possibile che l'aggiornamento venga rifiutato automaticamente quando si esegue la pulitura guidata del server WSUS.

Per cercare gli aggiornamenti sostituiti, è possibile selezionare la colonna flag sostituita nella vista tutti gli aggiornamenti e ordinarla in tale colonna. Esisterà quattro gruppi:

-   Gli aggiornamenti che non sono mai state sostituito (un'icona vuota).

-   Gli aggiornamenti che sono stati sostituiti, ma non hanno sostituito un altro aggiornamento (un'icona con un quadrato blu nella parte inferiore).

-   Aggiornamenti che sono stati sostituiti e sono sostituiti un altro aggiornamento (un'icona con un quadrato blu al centro).

-   Aggiornamenti che sono sostituiti, un'icona con un quadrato blu nella parte superiore, a un altro aggiornamento.

Non è disponibile alcuna funzionalità in Windows Server Update Services che automaticamente rifiuta gli aggiornamenti sostituiti all'approvazione da parte di un aggiornamento più recente. Si consiglia di impostare prima l'approvazione su non approvato, quindi utilizzare la pulizia guidata del server per rifiutare automaticamente l'aggiornamento quando sono state soddisfatte tutte le condizioni pertinenti. Per ulteriori informazioni, vedere la pagina relativa [alla pulitura guidata del server](the-server-cleanup-wizard.md).

## <a name="approving-superseding-or-superseded-updates"></a>Approvazione degli aggiornamenti in sostituzione o sostituiti
In genere, un aggiornamento che sostituisce altri aggiornamenti comporta uno o più dei seguenti:

-   Migliora o migliora aggiunge per la correzione fornita da uno o più aggiornamenti rilasciati in precedenza.

-   Migliora l'efficienza del relativo pacchetto di file di aggiornamento, che viene installato nei computer client se l'aggiornamento è approvato per l'installazione. Ad esempio, l'aggiornamento sostituito potrebbe contenere file che non sono più rilevanti per la correzione o per i sistemi operativi ora supportati dal nuovo aggiornamento, pertanto tali file non vengono inclusi nel pacchetto di file dell'aggiornamento sostitutivo.

-   Aggiorna versioni più recenti dei sistemi operativi. È anche importante notare che l'aggiornamento sostitutivo potrebbe non supportare le versioni precedenti dei sistemi operativi.

Al contrario, un aggiornamento sostituito da un altro aggiornamento esegue le operazioni seguenti:

-   Consente di correggere un problema simile a quello dell'aggiornamento che lo sostituisce. Tuttavia, l'aggiornamento che lo sostituisce può migliorare la correzione che fornisce l'aggiornamento sostituito.

-   Aggiorna le versioni precedenti dei sistemi operativi. In alcuni casi, queste versioni dei sistemi operativi non vengano più aggiornate per l'aggiornamento sostitutivo.

Nel riquadro dei dettagli di un singolo aggiornamento, un'icona informativa e un messaggio nella parte superiore indica che sostituisce o è stato sostituito da un altro aggiornamento. Inoltre, è possibile determinare quali aggiornamenti sostituiscono o vengono sostituiti dall'aggiornamento esaminando gli **aggiornamenti che sostituiscono l'aggiornamento** e **gli aggiornamenti sostituiti da queste** voci di aggiornamento nella sezione **dettagli aggiuntivi** delle **Proprietà**. Riquadro dei dettagli di un aggiornamento viene visualizzato sotto l'elenco degli aggiornamenti.

WSUS non automaticamente sostituiti gli aggiornamenti vengono rifiutati, e si consiglia di non presupporre che gli aggiornamenti sostituiti debbano essere rifiutati a favore dei nuovi aggiornamenti in sostituzione. Prima di rifiutare un aggiornamento sostituito, assicurarsi che non è più necessario per i computer client. Di seguito è riportati esempi di scenari in cui potrebbe essere necessario installare un aggiornamento sostituito:

-   Se un aggiornamento sostitutivo supporta solo più recenti versioni di un sistema operativo e alcuni dei computer client eseguono versioni precedenti del sistema operativo.

-   Se un aggiornamento sostitutivo ha un'applicabilità più limitata rispetto all'aggiornamento che sostituisce, che potrebbe renderlo inappropriato per alcuni computer client.

-   Se un aggiornamento non sostituisca più un aggiornamento rilasciato in precedenza a causa di nuove modifiche. È possibile che tramite le modifiche apportate a ogni rilascio, un aggiornamento non sostituisca più un aggiornamento che sostituiva in una versione precedente. In questo scenario, è comunque verrà visualizzato un messaggio relativo all'aggiornamento sostituito, anche se l'aggiornamento che lo sostituisce è stato sostituito da un aggiornamento che non esiste.


