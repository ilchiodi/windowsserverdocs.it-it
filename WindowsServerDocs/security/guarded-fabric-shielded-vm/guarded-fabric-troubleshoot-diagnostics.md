---
title: Risoluzione dei problemi con lo strumento di diagnostica dell'infrastruttura sorvegliata
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 07691d5b-046c-45ea-8570-a0a85c3f2d22
manager: dongill
author: huu
ms.technology: security-guarded-fabric
ms.openlocfilehash: deeaa7eab01dd5da6d997dd6ec039a3319e5c2b7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386460"
---
# <a name="troubleshooting-using-the-guarded-fabric-diagnostic-tool"></a>Risoluzione dei problemi con lo strumento di diagnostica dell'infrastruttura sorvegliata

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

Questo argomento descrive l'uso dello strumento di diagnostica dell'infrastruttura sorvegliata per identificare e correggere gli errori comuni durante la distribuzione, la configurazione e il funzionamento continuo dell'infrastruttura sorvegliata. Sono inclusi il servizio sorveglianza host (HGS), tutti gli host sorvegliati e i servizi di supporto, ad esempio DNS e Active Directory. Lo strumento di diagnostica può essere usato per eseguire un primo passaggio al Triaging di un'infrastruttura sorvegliata in errore, offrendo agli amministratori un punto di partenza per la risoluzione delle interruzioni e l'identificazione delle risorse configurate in modo errato. Lo strumento non è un rimpiazzo per una buona comprensione del funzionamento di un'infrastruttura sorvegliata e serve solo per verificare rapidamente i problemi più comuni riscontrati durante le operazioni quotidiane.

La documentazione dei cmdlet usati in questo argomento è reperibile in [TechNet](https://technet.microsoft.com/library/mt718834.aspx).

[!INCLUDE [Guarded fabric diagnostics tool](../../../includes/guarded-fabric-diagnostics-tool.md)] 

# <a name="quick-start"></a>Avvio rapido

È possibile diagnosticare un host sorvegliato o un nodo HGS chiamando il comando seguente da una sessione di Windows PowerShell con privilegi di amministratore locale:
```PowerShell
Get-HgsTrace -RunDiagnostics -Detailed
```
Questo consente di rilevare automaticamente il ruolo dell'host corrente e di diagnosticare eventuali problemi rilevanti che possono essere rilevati automaticamente.  Tutti i risultati generati durante questo processo vengono visualizzati a causa della presenza dell'opzione `-Detailed`.

Nella parte restante di questo argomento verrà fornita una procedura dettagliata sull'utilizzo avanzato di `Get-HgsTrace` per eseguire operazioni quali la diagnosi di più host contemporaneamente e il rilevamento di errori di configurazione complessi tra i nodi.

## <a name="diagnostics-overview"></a>Panoramica della diagnostica
La diagnostica dell'infrastruttura sorvegliata è disponibile in qualsiasi host con le funzionalità e gli strumenti correlati alle macchine virtuali schermate installate, inclusi gli host che eseguono Server Core.  Attualmente, le funzionalità di diagnostica sono incluse con le funzionalità e i pacchetti seguenti:

1. Ruolo del servizio sorveglianza host
2. Supporto Sorveglianza host Hyper-V
3. Strumenti di schermatura VM per la gestione dell'infrastruttura
4. Strumenti di amministrazione remota del server

Questo significa che gli strumenti di diagnostica saranno disponibili in tutti gli host sorvegliati, nei nodi HGS, in alcuni server di gestione dell'infrastruttura e in tutte le workstation Windows 10 con strumenti di amministrazione [remota](https://www.microsoft.com/download/details.aspx?id=45520) del server installati.  La diagnostica può essere richiamata da uno qualsiasi dei computer sopra indicati con lo scopo di diagnosticare qualsiasi host sorvegliato o nodo HGS in un'infrastruttura sorvegliata; con le destinazioni di traccia remote, la diagnostica può individuare e connettersi a host diversi dal computer che esegue la diagnostica.

Ogni host di destinazione della diagnostica viene definito "destinazione della traccia".  Le destinazioni di traccia sono identificate dai rispettivi nomi host e ruoli.  I ruoli descrivono la funzione eseguita da una determinata destinazione di traccia in un'infrastruttura sorvegliata.  Attualmente, le destinazioni di traccia supportano i ruoli `HostGuardianService` e `GuardedHost`.  Si noti che è possibile che un host occupi più ruoli contemporaneamente e che sia supportato anche dalla diagnostica, tuttavia questa operazione non deve essere eseguita negli ambienti di produzione.  Gli host HGS e Hyper-V devono essere mantenuti separati e distinti in qualsiasi momento.

Gli amministratori possono iniziare le attività di diagnostica eseguendo `Get-HgsTrace`.  Questo comando esegue due funzioni distinte in base alle opzioni fornite in fase di esecuzione: raccolta di tracce e diagnosi.  Queste due combinazioni costituiscono l'intero strumento di diagnostica dell'infrastruttura sorvegliata.  Sebbene non sia richiesto in modo esplicito, la diagnostica più utile richiede tracce che possono essere raccolte solo con le credenziali di amministratore nella destinazione di traccia.  Se l'utente che esegue la raccolta di tracce non dispone di privilegi sufficienti, le tracce che richiedono l'elevazione dei privilegi avranno esito negativo mentre tutte le altre verranno superate.  Questo consente la diagnosi parziale nel caso in cui un operatore con privilegi limitati esegua la valutazione. 

### <a name="trace-collection"></a>Raccolta di tracce
Per impostazione predefinita, `Get-HgsTrace` raccoglie le tracce e le salva in una cartella temporanea.  Le tracce hanno il formato di una cartella, denominata dopo l'host di destinazione, compilata con file formattati in modo specifico che descrivono come viene configurato l'host.  Le tracce contengono anche i metadati che descrivono come è stata richiamata la diagnostica per raccogliere le tracce.  Questi dati vengono utilizzati dalla diagnostica per riattivare le informazioni sull'host durante l'esecuzione di una diagnosi manuale.

Se necessario, le tracce possono essere esaminate manualmente.  Tutti i formati sono leggibili (XML) o possono essere ispezionati prontamente usando gli strumenti standard (ad esempio, i certificati X509 e le estensioni di Windows Crypto Shell).  Si noti tuttavia che le tracce non sono progettate per la diagnosi manuale ed è sempre più efficace elaborare le tracce con le funzionalità di diagnostica di `Get-HgsTrace`.

I risultati dell'esecuzione della raccolta di tracce non indicano l'integrità di un determinato host.  Indica semplicemente che le tracce sono state raccolte correttamente.  Per determinare se le tracce indicano un ambiente con errori, è necessario utilizzare le funzionalità di diagnostica di `Get-HgsTrace`.

Utilizzando il parametro `-Diagnostic`, è possibile limitare la raccolta di tracce solo alle tracce necessarie per il funzionamento della diagnostica specificata.  In questo modo si riduce la quantità di dati raccolti, nonché le autorizzazioni necessarie per richiamare la diagnostica.

### <a name="diagnosis"></a>Diagnosi
Le tracce raccolte possono essere diagnosticate `Get-HgsTrace` il percorso delle tracce tramite il parametro `-Path` e specificando l'opzione `-RunDiagnostics`.  @No__t-0, inoltre, può eseguire la raccolta e la diagnosi in un unico passaggio, specificando l'opzione `-RunDiagnostics` e un elenco di destinazioni di traccia.  Se non viene fornita alcuna destinazione di traccia, il computer corrente viene usato come destinazione implicita, con il ruolo dedotto esaminando i moduli di Windows PowerShell installati.

La diagnosi fornirà i risultati in un formato gerarchico che mostra quali destinazioni di traccia, set di diagnostica e singoli diagnostica sono responsabili di un errore specifico.  Gli errori includono suggerimenti per la correzione e la risoluzione se è possibile stabilire quale azione sarà eseguita successivamente.  Per impostazione predefinita, i risultati di passaggio e irrilevante sono nascosti.  Per visualizzare tutti gli elementi testati dalla diagnostica, specificare l'opzione `-Detailed`.  Verranno visualizzati tutti i risultati indipendentemente dal relativo stato.

È possibile limitare il set di diagnostica eseguito utilizzando il parametro `-Diagnostic`.  In questo modo è possibile specificare quali classi di diagnostica devono essere eseguite sulle destinazioni di traccia ed evitando tutte le altre.  Esempi di classi di diagnostica disponibili includono la rete, le procedure consigliate e l'hardware client.  Consultare la [documentazione del cmdlet](https://technet.microsoft.com/library/mt718831.aspx) per trovare un elenco aggiornato della diagnostica disponibile.

> [!WARNING]
> La diagnostica non è una sostituzione per una pipeline di monitoraggio e di risposta agli eventi imprevisti avanzata.  È disponibile un pacchetto di System Center Operations Manager per il monitoraggio di infrastrutture sorvegliate, nonché diversi canali di log eventi che possono essere monitorati per rilevare tempestivamente i problemi.  È quindi possibile usare la diagnostica per valutare rapidamente questi errori e stabilire una linea di azione.

## <a name="targeting-diagnostics"></a>Diagnostica di destinazione

`Get-HgsTrace` opera sulle destinazioni di traccia.  Una destinazione di traccia è un oggetto che corrisponde a un nodo HGS o a un host sorvegliato all'interno di un'infrastruttura sorvegliata.  Può essere considerato come un'estensione di un `PSSession` che include le informazioni necessarie solo per la diagnostica, ad esempio il ruolo dell'host nell'infrastruttura.  Le destinazioni possono essere generate in modo implicito, ad esempio diagnosi locali o manuali, oppure in modo esplicito con il comando `New-HgsTraceTarget`.

### <a name="local-diagnosis"></a>Diagnosi locale

Per impostazione predefinita, `Get-HgsTrace` sarà destinata al localhost, ovvero la posizione in cui viene richiamato il cmdlet.  Questa operazione viene definita destinazione locale implicita.  La destinazione locale implicita viene utilizzata solo quando non viene specificata alcuna destinazione nel parametro `-Target` e non vengono rilevate tracce preesistenti nel `-Path`.

La destinazione locale implicita usa l'inferenza del ruolo per determinare il ruolo riprodotto dall'host corrente nell'infrastruttura sorvegliata.  Si basa sui moduli di Windows PowerShell installati che corrispondono approssimativamente alle funzionalità installate nel sistema.  La presenza del modulo `HgsServer` farà sì che la destinazione della traccia prenda il ruolo `HostGuardianService` e la presenza del modulo `HgsClient` provocherà il ruolo `GuardedHost` da parte della destinazione della traccia.  È possibile che in un determinato host siano presenti entrambi i moduli, nel qual caso verranno trattati sia come `HostGuardianService` che come `GuardedHost`.

Pertanto, la chiamata predefinita della diagnostica per la raccolta di tracce in locale:
```PowerShell
Get-HgsTrace
```
... equivale a quanto segue:
```PowerShell
New-HgsTraceTarget -Local | Get-HgsTrace
```
> [!TIP]
> `Get-HgsTrace` può accettare destinazioni tramite la pipeline o direttamente tramite il parametro `-Target`.  Non esiste alcuna differenza tra le due operazioni.

### <a name="remote-diagnosis-using-trace-targets"></a>Diagnosi remota tramite destinazioni di traccia

È possibile diagnosticare in remoto un host generando destinazioni di traccia con le informazioni di connessione remota.  È necessario solo il nome host e un set di credenziali in grado di connettersi tramite la comunicazione remota di Windows PowerShell.
```PowerShell
$server = New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Role HostGuardianService -Credential (Enter-Credential)
Get-HgsTrace -RunDiagnostics -Target $server
```
In questo esempio verrà generato un messaggio di richiesta per raccogliere le credenziali dell'utente remoto, quindi la diagnostica verrà eseguita utilizzando l'host remoto in `hgs-01.secure.contoso.com` per completare la raccolta delle tracce.  Le tracce risultanti vengono scaricate nel localhost e quindi diagnosticate.  I risultati della diagnosi vengono presentati allo stesso modo di quando si eseguono le [diagnosi locali](#local-diagnosis).  Analogamente, non è necessario specificare un ruolo perché può essere dedotto in base ai moduli di Windows PowerShell installati nel sistema remoto.

Diagnosi remota usa la comunicazione remota di Windows PowerShell per tutti gli accessi all'host remoto.  È pertanto preferibile che la destinazione della traccia disponga di comunicazione remota di Windows PowerShell abilitata (vedere [Enable PSRemoting](https://technet.microsoft.com/library/hh849694.aspx)) e che localhost sia configurato correttamente per avviare le connessioni alla destinazione.

> [!NOTE]
> Nella maggior parte dei casi, è necessario che il localhost faccia parte della stessa foresta Active Directory e che venga usato un nome host DNS valido.  Se l'ambiente utilizza un modello federativo più complesso o si desidera utilizzare indirizzi IP diretti per la connettività, potrebbe essere necessario eseguire una configurazione aggiuntiva, ad esempio l'impostazione degli [host attendibili](https://technet.microsoft.com/library/ff700227.aspx)WinRM.

È possibile verificare che la creazione di un'istanza di una destinazione di traccia sia corretta e configurata per l'accettazione delle connessioni utilizzando il cmdlet `Test-HgsTraceTarget`:
```PowerShell
$server = New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Role HostGuardianService -Credential (Enter-Credential)
$server | Test-HgsTraceTarget
```
Questo comando restituirà `$True` se e solo se `Get-HgsTrace` sarebbe in grado di stabilire una sessione di diagnostica remota con la destinazione di traccia.  In caso di errore, questo cmdlet restituirà le informazioni di stato rilevanti per ulteriore risoluzione dei problemi della connessione remota di Windows PowerShell.

#### <a name="implicit-credentials"></a>Credenziali implicite

Quando si esegue una diagnosi remota da un utente con privilegi sufficienti per connettersi in modalità remota alla destinazione di traccia, non è necessario specificare le credenziali per `New-HgsTraceTarget`.  Il cmdlet `Get-HgsTrace` riutilizzerà automaticamente le credenziali dell'utente che ha richiamato il cmdlet durante l'apertura di una connessione.

> [!WARNING]
> Alcune restrizioni si applicano al riutilizzo delle credenziali, in particolare quando si esegue l'operazione nota come "secondo hop".  Questo errore si verifica quando si tenta di riutilizzare le credenziali all'interno di una sessione remota in un altro computer.  È necessario [configurare CredSSP](https://technet.microsoft.com/library/hh849872.aspx) per supportare questo scenario, ma questo esula dall'ambito della gestione e della risoluzione dei problemi dell'infrastruttura sorvegliata.

#### <a name="using-windows-powershell-just-enough-administration-jea-and-diagnostics"></a>Uso di Windows PowerShell just enough Administration (JEA) e diagnostica

Diagnosi remota supporta l'uso di endpoint di Windows PowerShell vincolati da JEA. Per impostazione predefinita, le destinazioni della traccia remota si connetteranno utilizzando l'endpoint predefinito `microsoft.powershell`.  Se la destinazione della traccia ha il ruolo `HostGuardianService`, tenterà anche di usare l'endpoint `microsoft.windows.hgs` configurato al momento dell'installazione di HGS.

Se si desidera utilizzare un endpoint personalizzato, è necessario specificare il nome della configurazione di sessione durante la costruzione della destinazione di traccia utilizzando il parametro `-PSSessionConfigurationName`, come indicato di seguito:

```PowerShell
New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Role HostGuardianService -Credential (Enter-Credential) -PSSessionConfigurationName "microsoft.windows.hgs"
```

#### <a name="diagnosing-multiple-hosts"></a>Diagnosi di più host

È possibile passare più destinazioni di traccia a `Get-HgsTrace` in una sola volta.  Questo include una combinazione di destinazioni locali e remote.  Ogni destinazione verrà tracciata a turno, quindi le tracce da ogni destinazione verranno diagnosticate simultaneamente.  Lo strumento di diagnostica può utilizzare la conoscenza più approfondita della distribuzione per identificare le complesse configurazioni errate tra nodi che altrimenti non verrebbero rilevabili.  L'uso di questa funzionalità richiede solo la creazione di tracce da più host contemporaneamente (nel caso di diagnosi manuale) o la destinazione di più host quando si chiama `Get-HgsTrace` (in caso di diagnosi remota).

Di seguito è riportato un esempio di uso della diagnosi remota per valutare un'infrastruttura composta da due nodi HGS e due host sorvegliati, in cui uno degli host sorvegliati viene usato per avviare `Get-HgsTrace`.

```PowerShell
$hgs01 = New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Credential (Enter-Credential)
$hgs02 = New-HgsTraceTarget -HostName "hgs-02.secure.contoso.com" -Credential (Enter-Credential)
$gh01 = New-HgsTraceTarget -Local
$gh02 = New-HgsTraceTarget -HostName "guardedhost-02.contoso.com"
Get-HgsTrace -Target $hgs01,$hgs02,$gh01,$gh02 -RunDiagnostics
```

> [!NOTE]
> Non è necessario diagnosticare l'intera infrastruttura sorvegliata durante la diagnosi di più nodi.  In molti casi è sufficiente includere tutti i nodi che potrebbero essere necessari in una determinata condizione di errore.  Si tratta in genere di un subset degli host sorvegliati e di un numero di nodi del cluster HGS.

## <a name="manual-diagnosis-using-saved-traces"></a>Diagnosi manuale mediante tracce salvate

In alcuni casi potrebbe essere necessario rieseguire la diagnostica senza raccogliere nuovamente le tracce oppure è possibile che non si disponga delle credenziali necessarie per diagnosticare in remoto tutti gli host nell'infrastruttura simultaneamente.  La diagnosi manuale è un meccanismo mediante il quale è comunque possibile eseguire una valutazione dell'intera infrastruttura usando `Get-HgsTrace`, ma senza usare la raccolta di traccia remota.

Prima di eseguire la diagnosi manuale, è necessario assicurarsi che gli amministratori di ogni host nell'infrastruttura che saranno valutati siano pronti e disposti a eseguire comandi per conto dell'utente.  L'output di traccia diagnostica non espone le informazioni che vengono in genere visualizzate come sensibili, ma spetta all'utente determinare se è sicuro esporre tali informazioni ad altri utenti.

> [!NOTE]
> Le tracce non sono resi anonimi e rivelano la configurazione di rete, le impostazioni PKI e altre configurazioni a volte considerate informazioni private.  Pertanto, le tracce devono essere trasmesse solo a entità attendibili all'interno di un'organizzazione e mai pubblicate pubblicamente.

I passaggi per eseguire una diagnosi manuale sono i seguenti:

1. Richiedere che ogni amministratore host esegua `Get-HgsTrace` specificando una @no__t Nota-1 e l'elenco di diagnostica che si intende eseguire sulle tracce risultanti.  Esempio:

   ```PowerShell
   Get-HgsTrace -Path C:\Traces -Diagnostic Networking,BestPractices
   ```
2. Richiedere a ogni amministratore host di creare il pacchetto della cartella TRACES risultante e inviarla all'utente.  Questo processo può essere gestito tramite posta elettronica, condivisioni file o qualsiasi altro meccanismo basato sui criteri e le procedure operative stabiliti dall'organizzazione.

3. Unire tutte le tracce ricevute in una singola cartella, senza altri contenuti o cartelle.

    * Si supponga, ad esempio, che gli amministratori inviino le tracce raccolte da quattro computer denominati HGS-01, HGS-02, RR1N2608-12 e RR1N2608-13.  Ogni amministratore avrebbe inviato una cartella con lo stesso nome.  È necessario assemblare una struttura di directory simile alla seguente:

      ```
      FabricTraces
      |- HGS-01
      |  |- TargetMetadata.xml
      |  |- Metadata.xml
      |  |- [any other trace files for this host]
      |- HGS-02
      |  |- [...]
      |- RR1N2608-12
      |  |- [...]
      |- RR1N2608-13
         |- [..]
      ```

4. Eseguire la diagnostica, specificando il percorso della cartella di traccia assemblata nel parametro `-Path` e specificando l'opzione `-RunDiagnostics`, nonché la diagnostica per cui è stato richiesto agli amministratori di raccogliere le tracce.  Il sistema di diagnostica presuppone che non possa accedere agli host trovati all'interno del percorso e tenterà quindi di usare solo le tracce pre-raccolte.  Se sono presenti tracce mancanti o danneggiate, la diagnostica avrà esito negativo solo per i test interessati e continuerà normalmente.  Esempio:

   ```PowerShell
   Get-HgsTrace -RunDiagnostics -Diagnostic Networking,BestPractices -Path ".\FabricTraces"
   ```

### <a name="mixing-saved-traces-with-additional-targets"></a>Combinazione di tracce salvate con destinazioni aggiuntive

In alcuni casi, è possibile che si disponga di un set di tracce pre-raccolte che si desidera aumentare con tracce host aggiuntive.  È possibile combinare tracce pre-raccolte con destinazioni aggiuntive che verranno tracciate e diagnosticate in una singola chiamata di diagnostica.

Seguendo le istruzioni per raccogliere e assemblare una cartella di traccia specificata in precedenza, chiamare `Get-HgsTrace` con destinazioni di traccia aggiuntive non trovate nella cartella di traccia pre-raccolta:

```PowerShell
$hgs03 = New-HgsTraceTarget -HostName "hgs-03.secure.contoso.com" -Credential (Enter-Credential)
Get-HgsTrace -RunDiagnostics -Target $hgs03 -Path .\FabricTraces
``` 

Il cmdlet di diagnostica identificherà tutti gli host pre-raccolti e un host aggiuntivo che deve ancora essere tracciato e eseguirà la traccia necessaria.  Viene quindi diagnosticata la somma di tutte le tracce pre-raccolte e raccolte di recente.  La cartella di traccia risultante conterrà sia la vecchia che la nuova traccia.
