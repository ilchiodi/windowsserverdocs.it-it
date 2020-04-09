---
title: Esegui analisi Best Practices Analyzer e Gestisci analisi Results_1
description: Server Manager
ms.prod: windows-server
ms.technology: manage-server-manager
ms.topic: article
ms.assetid: 232f1c80-88ef-4a39-8014-14be788c2766
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6ff854bcb25e4f5891e56f1e094fd4f387cf023f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851484"
---
# <a name="run-best-practices-analyzer-scans-and-manage-scan-results"></a>Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi

>Si applica a: Windows Server 2016

In ambito di gestione di Windows, con *procedure consigliate* si fa riferimento a linee guida considerate ideali, in circostanze tipiche, per configurare un server in base al parere degli esperti. Per la maggior parte delle applicazioni server, ad esempio, le procedure consigliate prevedono l'apertura solo delle porte necessarie per la comunicazione delle applicazioni con altri computer di rete e il blocco delle porte non usate. Sebbene il mancato rispetto delle procedure consigliate, anche di quelle più importanti, non sia necessariamente problematico, potrebbero verificarsi cali nelle prestazioni, scarsa affidabilità, conflitti imprevisti, aumento dei rischi per la sicurezza e altri problemi potenziali.

Best Practices Analyzer (BPA) è uno strumento di gestione server disponibile in Windows Server 2012 R2, Windows Server 2012 e Windows Server 2008 R2. BPA può aiutare gli amministratori a ridurre le violazioni delle procedure consigliate analizzando i ruoli installati nei server gestiti che eseguono Windows Server 2012 o Windows Server 2008 R2 e segnalando le violazioni delle procedure consigliate all'amministratore.

È possibile eseguire analisi di Best Practices Analyzer (BPA) da Server Manager, usando l'interfaccia utente grafica di BPA o i cmdlet in Windows PowerShell. a partire da Windows Server 2012, è possibile analizzare uno o più ruoli contemporaneamente, su più server, se si usa il riquadro Best Practices Analyzer nella console di Server Manager o i cmdlet di Windows PowerShell per eseguire le analisi. È inoltre possibile impostare BPA in modo da escludere o ignorare i risultati dell'analisi che non si desidera visualizzare.

In questo argomento sono contenute le seguenti sezioni.

-   [trova BPA](#BKMK_find)

-   [Funzionamento di BPA](#BKMK_how)

-   [Esecuzione di analisi dei Best Practices Analyzer sui ruoli](#BKMK_BPAscan)

-   [Gestire i risultati dell'analisi](#BKMK_manage)

## <a name="find-bpa"></a><a name=BKMK_find></a>trova BPA
È possibile trovare il riquadro Best Practices Analyzer nelle pagine dei gruppi di server e ruoli di Server Manager in Windows Server 2012 R2 e Windows Server 2012 oppure è possibile aprire una sessione di Windows PowerShell con diritti utente elevati per eseguire Best Practices Analyzer cmdlet.

## <a name="how-bpa-works"></a><a name=BKMK_how></a>Funzionamento di BPA
BPA funziona misurando la conformità di un ruolo alle regole delle procedure consigliate in otto diverse categorie di efficacia, attendibilità e affidabilità. I risultati delle analisi sono rappresentati da tre livelli di gravità descritti nella tabella seguente.

|Livello di gravità|Descrizione|
|---------|--------|
|Errore|I risultati di questo tipo vengono restituiti quando un ruolo non soddisfa le condizioni di una regola delle procedure consigliate e possono verificarsi problemi di funzionalità.|
|Informazioni|I risultati di questo tipo vengono restituiti quando un ruolo soddisfa le condizioni di una regola delle procedure consigliate.|
|Avviso|I risultati di questo tipo vengono restituiti se i risultati di non conformità possono causare problemi qualora le modifiche non vengano apportate. L'applicazione può risultare conforme in base al funzionamento attuale, ma potrebbe non soddisfare le condizioni di una regola qualora non vengano apportate modifiche alle impostazioni di configurazione o dei criteri. Da un'analisi di Servizi Desktop remoto potrebbe ad esempio risultare un avviso qualora per il ruolo non sia disponibile un server licenze in quanto, anche se al momento dell'analisi non vi sono connessioni remote attive, la mancanza di un server licenze impedisce alle nuove connessioni remote di ottenere licenze CAL (Client Access License) valide.|

### <a name="rule-categories"></a>Categorie di regole
Nella tabella seguente vengono descritte le categorie di regole per le procedure consigliate in base alle quali vengono misurati i ruoli durante un'analisi Best Practices Analyzer.

|Nome categoria|Descrizione|
|---------|--------|
|Sicurezza|Le regole di sicurezza vengono applicate per misurare il rischio relativo di un ruolo per l'esposizione a minacce quali utenti non autorizzati o malintenzionati o perdita o furto di dati riservati o proprietari.|
|Prestazioni|Le regole relative alle prestazioni vengono applicate per misurare la capacità di un ruolo di elaborare le richieste e svolgere i compiti previsti nell'organizzazione entro i periodi di tempo previsti in base al carico di lavoro del ruolo.|
|Configurazione|Le regole relative alla configurazione vengono applicate per identificare le impostazioni dei ruoli per le quali potrebbero essere necessarie modifiche per garantire il funzionamento ottimale del ruolo. Le regole relative alla configurazione possono aiutare a evitare conflitti tra impostazioni che possono comportare la visualizzazione di messaggi di errore o impedire a un ruolo di svolgere i compiti previsti in un'organizzazione.|
|Condizione|Le regole dei criteri vengono applicate per identificare Criteri di gruppo o le impostazioni del registro di sistema di Windows che potrebbero richiedere modifiche affinché un ruolo funzioni in modo ottimale e sicuro.|
|Operazione|Le regole delle operazioni vengono applicate per identificare i possibili casi in cui un ruolo non è in grado di svolgere i compiti previsti nell'organizzazione.|
|Pre-distribuzione|Le regole di pre-distribuzione vengono applicate prima che un ruolo installato venga distribuito nell'organizzazione. Consentono agli amministratori di valutare, prima di usare il ruolo nell'ambiente di produzione, se le procedure consigliate sono state soddisfatte.|
|Post-distribuzione|Le regole di post-distribuzione vengono applicate dopo che tutti i servizi necessari sono stati avviati per un ruolo e dopo che il ruolo è in esecuzione nell'organizzazione.|
|Prerequisiti|Le regole dei prerequisiti illustrano le impostazioni di configurazione, le impostazioni dei criteri e le funzionalità necessarie per un ruolo affinché tramite BPA possano essere applicate regole specifiche di altre categorie. Un prerequisito nei risultati di un'analisi indica che un'impostazione non corretta, un programma non presente, criteri abilitati o disabilitati in modo non corretto, un'impostazione di una chiave del Registro di sistema o un'altra configurazione ha impedito a BPA di applicare una o più regole durante un'analisi. Un risultato relativo a un prerequisito non implica la conformità o la non conformità, bensì indica che non è stato possibile applicare una regola che pertanto non è inclusa nei risultati dell'analisi.|

## <a name="performing-best-practices-analyzer-scans-on-roles"></a><a name=BKMK_BPAscan></a>Esecuzione di analisi dei Best Practices Analyzer sui ruoli
È possibile eseguire analisi BPA sui ruoli usando l'interfaccia utente grafica di BPA in Server Manager o i cmdlet di Windows PowerShell.

In Windows Server 2012 R2 e Windows Server 2012 alcuni ruoli richiedono l'impostazione di parametri aggiuntivi, ad esempio i nomi di server o condivisioni specifiche che eseguono parti del ruolo oppure gli ID dei sottomodelli, prima di avviare un'analisi BPA. Per le analisi BPA su modelli che richiedono di specificare parametri aggiuntivi, usare i cmdlet di BPA. L'interfaccia utente grafica di BPA non accetta parametri aggiuntivi quali gli ID dei sottomodelli. Ad esempio, l'ID del sottomodello **FSRM** rappresenta il sottomodello BPA di Servizi file per Gestione risorse file server, un servizio ruolo dei servizi File e Archiviazione. Per eseguire un'analisi solo sul servizio ruolo Gestione risorse file server, eseguire un'analisi BPA usando i cmdlet di Windows PowerShell e aggiungere il parametro `SubmodelId` al cmdlet.

Sebbene non sia possibile passare parametri aggiuntivi a un'analisi avviata nell'interfaccia utente grafica di BPA, il riquadro BPA in Server Manager Visualizza i risultati dell'analisi BPA più recente, indipendentemente dalla modalità di avvio dell'analisi.

-   [Analisi dei ruoli tramite l'interfaccia utente grafica di BPA](#BKMK_GUIscan)

-   [Analisi dei ruoli mediante i cmdlet di Windows PowerShell](#BKMK_PSscan)

### <a name="scanning-roles-by-using-the-bpa-gui"></a><a name=BKMK_GUIscan></a>Analisi dei ruoli tramite l'interfaccia utente grafica di BPA
Per analizzare uno o più ruoli usando l'interfaccia utente grafica di BPA, eseguire la procedura seguente.

##### <a name="to-scan-roles-by-using-the-bpa-gui"></a>Per analizzare i ruoli usando l'interfaccia utente grafica di BPA

1.  Eseguire una delle operazioni seguenti per aprire Server Manager se non è già aperto.

    -   Sulla barra delle applicazioni di Windows fare clic sul pulsante Server Manager.

    -   Nella schermata **Start** fare clic sul riquadro Server Manager.

2.  Nel pannello di navigazione aprire una pagina del ruolo o del gruppo.

    L'esecuzione di analisi BPA dalla pagina di un ruolo o di un gruppo comporta l'analisi di tutti i ruoli installati nei server appartenenti a tale gruppo.

3.  Nel menu **attività** del riquadro **Best Practices Analyzer** fare clic su **Avvia analisi BPA**.

4.  A seconda del numero di regole valutate per il ruolo o il gruppo selezionato, il completamento dell'analisi BPA potrebbe richiedere alcuni minuti.

### <a name="scanning-roles-by-using-windows-powershell-cmdlets"></a><a name=BKMK_PSscan></a>Analisi dei ruoli mediante i cmdlet di Windows PowerShell
Utilizzare le procedure seguenti per analizzare uno o più ruoli utilizzando i cmdlet di Windows PowerShell.

> [!NOTE]
> Nelle procedure descritte in questa sezione non sono mostrati tutti i parametri e i cmdlet di BPA. Per ulteriori informazioni sulle operazioni di BPA in Windows PowerShell, nella sessione di Windows PowerShell, immettere **Get-Help***BPACmdlet***-Full**, dove *BPACmdlet* può essere uno dei valori seguenti. È anche possibile trovare gli argomenti della guida sui cmdlet di BPA nel [TechCenter di Windows Server](https://go.microsoft.com/fwlink/p/?LinkId=240177).

-   **Get-BPAmodel**

-   **Invoke-BPAmodel**

-   **Get-BPAResult**

-   **Set-BPAResult**

#### <a name="to-scan-a-single-role-by-using-windows-powershell-cmdlets"></a><a name=BKMK_singlerole></a>Per analizzare un singolo ruolo usando i cmdlet di Windows PowerShell

1.  Eseguire una delle operazioni seguenti per eseguire Windows PowerShell con diritti utente elevati.

    -   Per eseguire Windows PowerShell come amministratore dalla schermata **Start** , fare clic con il pulsante destro del mouse sul riquadro **Windows PowerShell** nei risultati di **app** e quindi, nella barra dell'app, fare clic su **Esegui come amministratore**.

    -   Per eseguire Windows PowerShell come amministratore dal desktop, fare clic con il pulsante destro del mouse sul collegamento di **Windows PowerShell** nella barra delle applicazioni e quindi scegliere **Esegui come amministratore**.

2.  a partire da Windows PowerShell 3,0, i moduli dei cmdlet vengono importati automaticamente nella sessione di Windows PowerShell la prima volta che si usa un cmdlet dal modulo. Non è necessario importare o caricare il modulo del cmdlet di BPA.

3.  trovare gli ID modello di tutti i ruoli per i quali è possibile eseguire analisi BPA immettendo il cmdlet **Get-BPAModel** , come illustrato nell'esempio seguente.

    `Get-Bpamodel`

4.  Nei risultati del passaggio 3 individuare gli ID modello dei ruoli sui quali si desidera eseguire un'analisi BPA.

5.  Immettere uno dei comandi seguenti per avviare l'analisi BPA per un ruolo specifico. Per più ruoli, separare gli ID modello con una virgola.

    `Invoke-BPAmodel -modelId <modelID_from_Step3>`

    `Invoke-BPAmodel <modelID_from_Step3>`

    **Esempio:** `Invoke-BPAmodel -modelId Microsoft/Windows/DNSServer,Microsoft/Windows/FileServices`

    > [!NOTE]
    > L'ID modello include l'intero percorso visualizzato nella colonna **Id**, ad esempio **Microsoft/Windows/Hyper-V**.

    È anche possibile avviare un'analisi in un ruolo specifico dai risultati del passaggio 3 eseguendo il piping dei risultati del cmdlet `Get-BPAmodel` nel cmdlet `Invoke-BPAmodel` , come illustrato nell'esempio seguente.

    `Get-BPAmodel <model_ID> | Invoke-BPAmodel`

    L'esecuzione di questo cmdlet senza specificare un ID modello comporta il piping di tutti i modelli restituiti dal cmdlet `Get-BPAmodel` al cmdlet `Invoke-BPAmodel`, avviando l'analisi in tutti i modelli disponibili sui server aggiunti al pool di Server Manager Server.

#### <a name="to-scan-all-roles-by-using-windows-powershell-cmdlets"></a><a name=BKMK_allroles></a>Per analizzare tutti i ruoli usando i cmdlet di Windows PowerShell

1.  Aprire una sessione di Windows PowerShell con diritti utente elevati, se non ne è già aperta una. Per istruzioni, vedere la procedura precedente.

2.  Eseguire il piping di tutti i ruoli per i quali possono essere eseguite analisi BPA nel cmdlet `Invoke-BPAmodel` per avviare le analisi.

    `Get-BPAmodel | Invoke-BPAmodel`

3.  Una volta completata l'analisi, Windows PowerShell restituisce risultati simili ai seguenti per ogni ruolo analizzato.

    ```
    modelId                 :  Microsoft/Windows/FileServices
    SubmodelId              :
    Success                 :  True
    Scantime                :  1/01/2012  12:18:40 PM
    ScantimeUtcOffset       :  -08:00:00
    detail                  :  {server_name1, server_name2}

    ```

## <a name="manage-scan-results"></a><a name=BKMK_manage></a>Gestire i risultati dell'analisi
Dopo aver completato un'analisi BPA nell'interfaccia utente grafica, è possibile visualizzare i risultati nel riquadro BPA. Quando si seleziona un risultato in un riquadro, vengono visualizzate in anteprima le proprietà del risultato, unitamente all'indicazione della conformità o meno del ruolo alle procedure consigliate associate. Se un risultato non è conforme e si desidera essere in grado di risolvere i problemi descritti nelle proprietà del risultato, i collegamenti ipertestuali nelle proprietà dei risultati degli errori e degli avvisi aprono gli argomenti della Guida per la risoluzione dettagliata sul sito TechCenter di Windows Server.

> [!NOTE]
> I risultati di un'analisi BPA non vengono automaticamente salvati o archiviati. L'esecuzione di una nuova analisi in un modello o sottomodello sovrascrive i risultati dell'ultima analisi. Per salvare i risultati di un'analisi BPA per l'archiviazione, la stampa o l'invio ad altri utenti, vedere [Per visualizzare risultati BPA da sessioni di Windows PowerShell in diversi formati](#BKMK_formats) in questa sezione.

### <a name="exclude-and-include-bpa-results"></a>Escludere e includere risultati BPA
Se non è necessario visualizzare alcuni risultati BPA, ad esempio i risultati che si verificano frequentemente nelle analisi BPA ma che non richiedono alcuna risoluzione, è possibile escludere i risultati usando l'interfaccia utente grafica di BPA o i cmdlet di BPA in Windows PowerShell. I risultati possono essere inclusi di nuovo in qualsiasi momento.

> [!NOTE]
> Quando vengono esclusi, i risultati sono esclusi anche dalla visualizzazione sui server gestiti e non possono essere visualizzati nemmeno da altri amministratori. Per escludere i risultati dalla visualizzazione solo in una console di Server Manager locale, creare una query personalizzata anziché usare il comando **Escludi risultato** .

#### <a name="exclude-scan-results"></a><a name=BKMK_exclude></a>Escludi Risultati analisi
L'impostazione **Escludi** è permanente. I risultati esclusi rimangono tali nelle analisi successive dello stesso modello nello stesso computer finché non vengono inclusi di nuovo.

È possibile escludere i risultati dell'analisi usando il cmdlet `Set-BPAResult` con il parametro `Exclude` . Come nel riquadro Best Practices Analyzer Server Manager, è possibile escludere singoli oggetti risultato oppure escludere anche un set di risultati i cui campi (categoria, titolo e gravità, ad esempio) sono uguali o contengono valori specificati. È ad esempio possibile escludere tutti i risultati **Prestazioni** da un insieme di risultati dell'analisi per un modello.

> [!NOTE]
> Prima di poter usare le procedura descritte in questa sezione, è necessario eseguire almeno un'analisi BPA su un modello.

###### <a name="to-exclude-scan-results-by-using-the-gui"></a>Per escludere risultati dell'analisi usando l'interfaccia utente grafica

1.  Aprire una pagina di gruppo di ruoli o di server in Server Manager.

2.  Nel riquadro Best Practices Analyzer per il gruppo di ruoli o di server fare clic con il pulsante destro del mouse su un risultato nell'elenco, quindi scegliere **Escludi risultato**.

    Il risultato non verrà più visualizzato nell'elenco dei risultati.

3.  Per visualizzare i risultati esclusi nell'interfaccia grafica utente, eseguire la query predefinita **Risultati esclusi**. Fare clic su **Query di ricerca salvate** e quindi su **Risultati esclusi**.

    Dopo aver eseguito la query **Risultati esclusi**, il testo del sottotitolo del riquadro, una descrizione dei risultati visualizzati nell'elenco, viene modificato in **Risultati esclusi**. Nell'elenco vengono visualizzati solo i risultati esclusi.

###### <a name="to-exclude-scan-results-by-using-windows-powershell-cmdlets"></a>Per escludere risultati dell'analisi usando i cmdlet di Windows PowerShell

1.  Aprire un sessione di Windows PowerShell con diritti utente elevati.

2.  Escludere risultati specifici dall'analisi di un modello eseguendo il comando seguente.

    `Get-BPAResult -modelId <model ID> | Where { $_.<Field Name> -eq Value} | Set-BPAResult -Exclude $true`

    Il comando precedente consente di recuperare gli elementi dei risultati dell'analisi BPA per l'ID modello rappresentato da *ID modello*.

    La seconda sezione del comando consente di filtrare i risultati del cmdlet `Get-BPAResult` per recuperare solo quelli il cui valore di un campo, rappresentato da *Nome campo*, corrisponde al testo tra virgolette.

    La sezione finale del comando, dopo la seconda barra verticale, consente di escludere i risultati filtrati dalla sezione precedente del cmdlet.

    **Esempio:** `Get-BPAResult -Microsoft/Windows/FileServices | Where { $_.Severity -eq Information} | Set-BPAResult -Exclude $true`

#### <a name="include-scan-results"></a>Includere risultati dell'analisi
Quando si desidera visualizzare risultati dell'analisi esclusi in precedenza, è possibile includere tali risultati. L'impostazione **Includi** è permanente. I risultati inclusi rimangono tali nelle analisi successive dello stesso modello nello stesso computer.

##### <a name="to-include-scan-results-by-using-the-gui"></a><a name=BKMK_gui></a>Per includere i risultati dell'analisi usando l'interfaccia utente grafica

1.  Aprire una pagina di gruppo di ruoli o di server in Server Manager.

2.  Nel riquadro Best Practices Analyzer per il gruppo di ruoli o di server fare clic con il pulsante destro del mouse su un risultato escluso nell'elenco di query **risultati esclusi** , quindi scegliere **Includi risultato**.

    Il risultato non verrà più visualizzato nell'elenco dei risultati esclusi. Cancellare la query facendo clic su **Cancella tutto** per visualizzare il risultato incluso nell'elenco di tutti i risultati inclusi.

##### <a name="to-include-scan-results-by-using-windows-powershell-cmdlets"></a><a name=BKMK_cmdlets></a>Per includere i risultati dell'analisi usando i cmdlet di Windows PowerShell

1.  Aprire un sessione di Windows PowerShell con diritti utente elevati.

2.  Includere risultati specifici dell'analisi di un modello digitando il comando seguente e quindi premendo **Invio**.

    `Get-BPAResult -modelId <model Id> | Where { $_.<Field Name> -eq Value } | Set-BPAResult -Exclude $false`

    Il comando precedente consente di recuperare gli elementi dei risultati dell'analisi BPA per il modello rappresentato da *ID modello*.

    La seconda parte del comando, dopo il primo carattere barra verticale ( **|** ) filtra i risultati del cmdlet **Get-BPAResult** per recuperare solo i risultati dell'analisi per i quali il valore del campo risultato, rappresentato da *nome campo*, corrisponde al testo tra virgolette.

    La parte finale del comando, dopo la seconda barra verticale, consente di includere i risultati filtrati dalla seconda parte del cmdlet, impostando il valore del parametro **-Exclude** su **false**.

    **Esempio:** `Get-BPAResult -Microsoft/Windows/FileServices | Where { $_.Severity -eq Information} | Set-BPAResult -Exclude $false`

### <a name="view-and-export-bpa-scan-results-in-windows-powershell"></a>Visualizzare ed esportare risultati dell'analisi BPA in Windows PowerShell
Per visualizzare e gestire i risultati dell'analisi usando i cmdlet di Windows PowerShell, vedere le procedure riportate di seguito. Prima di poter usare le procedure seguenti, eseguire almeno un'analisi BPA su almeno un modello o sottomodello.

#### <a name="to-view-results-of-the-most-recent-scan-of-a-role-by-using-windows-powershell"></a><a name=BKMK_recentPS></a>Per visualizzare i risultati dell'analisi più recente di un ruolo usando Windows PowerShell

1.  Aprire un sessione di Windows PowerShell con diritti utente elevati.

2.  Ottenere i risultati dell'analisi più recente per un ID modello specificato. digitare quanto segue, in cui il modello è rappresentato da *ID modello*e quindi premere **invio**. È possibile ottenere risultati per più ID modello separandoli con virgole.

    `Get-BPAResult <model ID>`

    **Esempio:** `Get-BPAResult Microsoft/Windows/DNSServer,Microsoft/Windows/FileServices`

    Se è stato analizzato un sottomodello di un modello, ad esempio un servizio ruolo, ottenere i risultati solo per tale sottomodello includendo l'ID del sottomodello nel cmdlet.

    **Esempio:** `Get-BPAResult Microsoft/Windows/FileServices -SubmodelID FSRM`

#### <a name="to-view-or-save-bpa-results-from-windows-powershell-sessions-in-different-formats"></a><a name=BKMK_formats></a>Per visualizzare o salvare i risultati BPA dalle sessioni di Windows PowerShell in formati diversi

-   In Windows PowerShell ogni risultato BPA è simile al seguente.

    ```
    ResultNumber     :  14
    ResultId         :  1557706192
    modelId          :  Microsoft/Windows/FileServices
    SubmodelId       :  FSRM
    RuleId           :  16
    computerName     :  server_name1, server_name2
    Context          :  FileServices
    Source           :  server_name1
    Severity         :  Information
    Category         :  Configuration
    Title            :  Access Denied remediation requires remote management be enabled on this server
    Problem          :
    Impact           :
    Resolution       :
    compliance       :  The File Server Best Practices Analyzer scan has determined that you are in compliance with this best practice.
    help             :
    Excluded         :  False

    ```

    Effettuare una delle operazioni riportate di seguito.

    -   Per formattare i risultati BPA in una tabella, eseguire il cmdlet riportato di seguito, aggiungendo le proprietà dei risultati che si desidera visualizzare dall'esempio precedente.

        `Get-BPAResult model ID | format-Table -Property <property1,property2,property3...>`

        **Esempio:** `Get-BPAResult Microsoft/Windows/FileServices | format-Table -Property modelId,SubmodelId,computerName,Source,Severity,Category,Title,Problem,Impact,Resolution,compliance,help`

    -   Per formattare i risultati BPA in un visualizzatore di tipo griglia basato su interfaccia grafica utente, con un filtro per le stringhe di testo e intestazioni di colonna su cui è possibile fare clic per ordinare i risultati, eseguire il cmdlet riportato di seguito.

        `Get-BPAResult <model ID> | OGV`

    -   Per esportare i risultati BPA in un file HTML che può essere archiviato o inviato ai destinatari di posta elettronica, eseguire il cmdlet seguente, dove *path* rappresenta il percorso e il nome file in cui si desidera salvare i risultati HTML.

        `Get-BPAResult <model ID> | convertTo-Html | Set-Content <path>`

        **Esempio:** `Get-BPAResult Microsoft/Windows/FileServices | convertTo-Html | Set-Content C:\BPAResults\FileServices.htm`

    -   Per esportare i risultati BPA in un file di testo con valori delimitati da virgole (CSV), eseguire il cmdlet seguente, dove *path* rappresenta il percorso e il nome del file di testo in cui si desidera salvare i risultati di CSV. I risultati CSV possono essere importati in Microsoft Excel o in altri programmi che visualizzano i dati in fogli di calcolo o griglie.

        `Get-BPAResult <model ID> | Export-CSV <path>`

        **Esempio:** `Get-BPAResult Microsoft/Windows/FileServices | Export-CSV C:\BPAResults\FileServices.txt`

## <a name="see-also"></a>Vedi anche
[Best Practices Analyzer contenuto di risoluzione nel TechCenter di Windows Server](https://go.microsoft.com/fwlink/p/?LinkId=241597)
[filtrare, ordinare ed eseguire query sui dati nei riquadri Server Manager](filter-sort-and-query-data-in-server-manager-tiles.md)
[gestire più server remoti con Server Manager](manage-multiple-remote-servers-with-server-manager.md)
