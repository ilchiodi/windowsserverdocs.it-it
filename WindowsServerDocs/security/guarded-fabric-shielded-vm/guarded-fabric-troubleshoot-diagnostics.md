---
title: Risoluzione dei problemi mediante lo strumento di diagnostica dell'infrastruttura sorvegliata
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 07691d5b-046c-45ea-8570-a0a85c3f2d22
manager: dongill
author: huu
ms.technology: security-guarded-fabric
ms.openlocfilehash: 0fb257f693cc27c0bc6dd18fc89e8dc6328ee638
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447347"
---
# <a name="troubleshooting-using-the-guarded-fabric-diagnostic-tool"></a>Risoluzione dei problemi mediante lo strumento di diagnostica dell'infrastruttura sorvegliata

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

In questo argomento viene descritto l'utilizzo di sorvegliato Fabric strumento di diagnostica per identificare e correggere gli errori comuni nella distribuzione, configurazione e operazione in corso dell'infrastruttura sorvegliata. Ciò include il servizio sorveglianza Host (HGS), tutti gli host sorvegliati e i servizi di supporto, ad esempio Active Directory e DNS. Lo strumento di diagnostica è utilizzabile per eseguire un primo passo nella valutazione di un'infrastruttura di errore sorvegliati, fornendo agli amministratori un punto di partenza per risolvere interruzioni e identificazione degli asset con configurazione non valida. Lo strumento non è una sostituzione per audio. é quindi di funzionare a un'infrastruttura sorvegliata e serve solo per verificare rapidamente i problemi più comuni riscontrati durante le operazioni quotidiane.

Documentazione dei cmdlet usati in questo argomento sono disponibili nel [TechNet](https://technet.microsoft.com/library/mt718834.aspx).

[!INCLUDE [Guarded fabric diagnostics tool](../../../includes/guarded-fabric-diagnostics-tool.md)] 

# <a name="quick-start"></a>Avvio rapido

È possibile diagnosticare un host sorvegliato o un nodo HGS chiamando quanto segue da una sessione di Windows PowerShell con privilegi di amministratore locale:
```PowerShell
Get-HgsTrace -RunDiagnostics -Detailed
```
Verrà automaticamente il ruolo dell'host corrente di rilevare e diagnosticare eventuali problemi rilevanti che possono essere rilevati automaticamente.  Tutti i risultati generati durante questo processo vengono visualizzati a causa della presenza del `-Detailed` passare.

Il resto di questo argomento fornisce una procedura dettagliata sull'utilizzo avanzato dei `Get-HgsTrace` per eseguire operazioni quali la diagnosi di più host in una sola volta e il rilevamento di una configurazione errata tra nodi complessa.

## <a name="diagnostics-overview"></a>Panoramica di diagnostica
Infrastruttura sorvegliata diagnostica è disponibile in qualsiasi host con macchine virtuali schermate relative a strumenti e funzionalità installate, inclusi gli host che esegue Server Core.  Attualmente, la diagnostica è inclusa con i pacchetti o le funzionalità seguenti:

1. Ruolo del servizio sorveglianza host
2. Supporto Sorveglianza host Hyper-V
3. Strumenti di schermatura VM per la gestione dell'infrastruttura
4. Strumenti di amministrazione remota del server

Ciò significa che gli strumenti di diagnostica saranno disponibili in tutti gli host sorvegliati, i nodi HGS, alcuni server di gestione dell'infrastruttura e qualsiasi workstation di Windows 10 con [RSAT](https://www.microsoft.com/download/details.aspx?id=45520) installato.  Diagnostica può essere richiamata da una o più macchine precedente con lo scopo di diagnosticare eventuali host sorvegliato o nodo HGS in un'infrastruttura sorvegliata; Usa destinazioni di traccia remoto, diagnostica può individuare e connettersi all'host diversi da computer che esegue la diagnostica.

Tutti gli host di destinazione da diagnostica è detto "target di traccia".  Le destinazioni di traccia vengono identificate dal loro i nomi host e i ruoli.  I ruoli descrivono la funzione che esegue la destinazione di traccia specificato in un'infrastruttura sorvegliata.  Attualmente, è destinato a supporto di traccia `HostGuardianService` e `GuardedHost` ruoli.  Si noti è possibile che un host in modo che occupi più ruoli in una sola volta e ciò è supportata inoltre dalla diagnostica, tuttavia questo non deve essere eseguito in ambienti di produzione.  Gli host Hyper-V e HGS devono essere mantenuti separati e distinto in qualsiasi momento.

Gli amministratori possono iniziare qualsiasi attività diagnostiche eseguendo `Get-HgsTrace`.  Questo comando esegue due funzioni distinte basate le opzioni fornite in fase di runtime: raccolta e la diagnosi di traccia.  Questi due combinati costituiscono l'intero dello strumento Diagnostica Fabric protetta.  Anche se non esplicitamente richiesto, diagnostica più utile richiede le tracce che possono essere raccolti solo con le credenziali di amministratore nella destinazione traccia.  Se dispone di privilegi sufficienti vengono mantenuti attivi per l'utente che esegue la raccolta di traccia, le tracce che richiedono l'elevazione dei privilegi avrà esito negativo anche se tutti gli altri utenti passeranno.  In questo modo parziale diagnosi nel caso in cui un operatore con privilegi in difetto sta eseguendo la valutazione. 

### <a name="trace-collection"></a>Raccolta delle tracce
Per impostazione predefinita, `Get-HgsTrace` solo raccoglierà le tracce e salvarli in una cartella temporanea.  Le tracce assumono la forma di una cartella denominata dopo l'host di destinazione, compilato con file di formattazione speciale che descrivono come l'host è configurato.  Le tracce contengono anche i metadati che descrivono come sono stati richiamati i dati di diagnostica per raccogliere le tracce.  Questi dati vengano utilizzati dalla diagnostica per riattivare le informazioni relative all'host quando si esegue la diagnosi manuale.

Se necessario, le tracce possono essere rivisti manualmente.  Tutti i formati sono leggibili (XML) o deve essere facilmente controllati utilizzando gli strumenti standard (ad esempio X509 i certificati e le estensioni della Shell di crittografia di Windows).  Si noti tuttavia che le tracce non sono progettate per la diagnosi manuale ed è sempre più efficaci per elaborare le tracce con le funzionalità per la diagnosi di `Get-HgsTrace`.

I risultati dell'esecuzione di raccolta delle tracce non hanno alcuna indicazione per quanto riguarda l'integrità di un determinato host.  Indicano semplicemente che le tracce sono stati raccolti correttamente.  È necessario usare le funzionalità per la diagnosi di `Get-HgsTrace` per determinare se le tracce indicano un ambiente di failover.

Uso di `-Diagnostic` parametro, è possibile limitare la raccolta di traccia per solo tali tracce necessarie per svolgere i dati di diagnostica specificati.  In questo modo si riduce la quantità di dati raccolti, nonché le autorizzazioni necessarie per richiamare la diagnostica.

### <a name="diagnosis"></a>Diagnosi
Le tracce raccolte possono essere diagnosticate lanciando fornite `Get-HgsTrace` la posizione delle tracce tramite il `-Path` parametro e specificando la `-RunDiagnostics` passare.  È inoltre `Get-HgsTrace` eseguibili insieme e diagnostica in un unico passaggio fornendo il `-RunDiagnostics` switch e un elenco di destinazioni di traccia.  Se non vengono fornite alcuna destinazione di traccia, nel computer corrente viene utilizzato come una destinazione implicita, con il proprio ruolo dedotto esaminando i moduli di Windows PowerShell installati.

Diagnosi fornirà i risultati in un formato gerarchico che mostra quali le destinazioni di traccia, un set di diagnostica e diagnostica singoli è tenuta a un particolare tipo di errore.  Gli errori includono correzione e suggerimenti di risoluzione, se è possibile effettuare una determinazione per quanto riguarda l'azione che deve essere eseguita successivo.  Per impostazione predefinita, i risultati superati e non pertinenti sono nascosti.  Per visualizzare tutti gli elementi testati da diagnostica, specificare il `-Detailed` passare.  In questo modo tutti i risultati vengono visualizzati indipendentemente dal loro stato.

È possibile limitare il set di dati diagnostici che vengono eseguiti utilizzando il `-Diagnostic` parametro.  In questo modo è possibile specificare le destinazioni di traccia, è necessario eseguire delle classi di diagnostica e l'eliminazione di tutti gli altri.  Classi di diagnostica disponibili esempi di rete, best practices e client hardware.  Consultare il [documentazione del cmdlet](https://technet.microsoft.com/library/mt718831.aspx) per trovare un elenco aggiornato delle diagnostiche disponibili.

> [!WARNING]
> Diagnostica non è una sostituzione per una pipeline di risposta agli eventi imprevisti e il monitoraggio sicuro.  È disponibile per il monitoraggio delle infrastrutture sorvegliate, nonché diversi canali di log eventi che è possibile monitorare per rilevare in anticipo problemi di un pacchetto di System Center Operations Manager.  La diagnostica è quindi utilizzabile per valutare questi errori e stabilire una linea di condotta rapidamente.

## <a name="targeting-diagnostics"></a>Destinazione diagnostica

`Get-HgsTrace` opera in base alle destinazioni di traccia.  Una destinazione di traccia è un oggetto che corrisponde a un nodo di HGS o un host sorvegliato all'interno di un'infrastruttura sorvegliata.  Può essere considerato un'estensione di un `PSSession` che include le informazioni necessarie solo per la diagnostica, ad esempio il ruolo di host nell'infrastruttura.  Le destinazioni possono essere generati in modo implicito (ad esempio la diagnosi di manuale o locale) o in modo esplicito con la `New-HgsTraceTarget` comando.

### <a name="local-diagnosis"></a>Diagnosi locale

Per impostazione predefinita, `Get-HgsTrace` effettuerà l'host locale (ad esempio in cui il cmdlet viene richiamato).  Questa operazione viene definita la destinazione locale implicita.  La destinazione locale implicita viene usata solo quando nessuna destinazione sono disponibili nel `-Target` parametri e nessuna traccia pre-esistente, vedere il `-Path`.

La destinazione locale implicita Usa l'inferenza del ruolo per determinare quale ruolo ricopre l'host corrente in dell'infrastruttura sorvegliata.  Si basa su moduli di Windows PowerShell installati che corrispondono all'incirca alle quali funzionalità sono state installate nel sistema.  La presenza del `HgsServer` modulo determinerà la destinazione di traccia assumere il ruolo `HostGuardianService` e la presenza delle `HgsClient` modulo determinerà la destinazione di traccia assumere il ruolo `GuardedHost`.  È possibile che un determinato host a dispone di entrambi i moduli presenti in questo caso verrà considerato sia come un `HostGuardianService` e un `GuardedHost`.

Pertanto, la chiamata predefinita di diagnostica per la raccolta di tracce in locale:
```PowerShell
Get-HgsTrace
```
... è equivalente alla seguente:
```PowerShell
New-HgsTraceTarget -Local | Get-HgsTrace
```
> [!TIP]
> `Get-HgsTrace` può accettare le destinazioni tramite la pipeline o direttamente tramite il `-Target` parametro.  Non vi è alcuna differenza tra i due operativa.

### <a name="remote-diagnosis-using-trace-targets"></a>Destinazioni di traccia utilizzando la diagnosi remota

È possibile eseguire la diagnosi remota di un host tramite la generazione di destinazioni di traccia con le informazioni di connessione remota.  Tutto ciò che serve è il nome host e un insieme di credenziali in grado di connettersi tramite comunicazione remota di Windows PowerShell.
```PowerShell
$server = New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Role HostGuardianService -Credential (Enter-Credential)
Get-HgsTrace -RunDiagnostics -Target $server
```
In questo esempio genera una richiesta per raccogliere le credenziali dell'utente remoto e quindi diagnostica verrà eseguito utilizzando l'host remoto a `hgs-01.secure.contoso.com` per completare la raccolta di traccia.  Le tracce risultante vengono scaricate il localhost e quindi diagnosticate.  I risultati della diagnostica vengono presentati lo stesso come quando si esegue [diagnosi locale](#local-diagnosis).  Non è in modo analogo, necessario specificare un ruolo, come può essere dedotto basata su moduli di Windows PowerShell installati nel sistema remoto.

Diagnosi remoto Usa la comunicazione remota di Windows PowerShell per tutti gli accessi all'host remoto.  Di conseguenza è un prerequisito che la destinazione di traccia dispone di comunicazione remota di Windows PowerShell abilitata (vedere [PSRemoting abilitare](https://technet.microsoft.com/library/hh849694.aspx)) e che il localhost sia correttamente configurato per l'avvio di connessioni alla destinazione.

> [!NOTE]
> Nella maggior parte dei casi, è necessario solo che il localhost far parte della stessa foresta Active Directory e che viene utilizzato un nome host DNS valido.  Se l'ambiente utilizza un modello più complicato di federazione o si vogliono usare gli indirizzi IP diretti per la connettività, si potrebbe essere necessario eseguire una configurazione aggiuntiva, ad esempio l'impostazione di WinRM [host attendibili](https://technet.microsoft.com/library/ff700227.aspx).

È possibile verificare che una destinazione di traccia viene creata un'istanza correttamente e configurata per accettare le connessioni usando la `Test-HgsTraceTarget` cmdlet:
```PowerShell
$server = New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Role HostGuardianService -Credential (Enter-Credential)
$server | Test-HgsTraceTarget
```
Questo comando restituirà `$True` solo se `Get-HgsTrace` sarebbe in grado di stabilire una sessione di diagnostica remota con la destinazione di traccia.  In caso di errore, questo cmdlet restituisce informazioni sullo stato rilevanti per un'ulteriore risoluzione dei problemi di connessione della comunicazione remota di Windows PowerShell.

#### <a name="implicit-credentials"></a>Credenziali implicite

Quando si esegue la diagnosi remota da un utente con privilegi sufficienti per connettersi in remoto alla destinazione di traccia, non è necessario fornire le credenziali per `New-HgsTraceTarget`.  Il `Get-HgsTrace` cmdlet viene automaticamente riutilizzato per le credenziali dell'utente che ha richiamato il cmdlet quando si apre una connessione.

> [!WARNING]
> Vengono applicate alcune restrizioni per il riutilizzo delle credenziali, in particolare quando si esegue l'operazione nota come un "secondo hop."  Ciò si verifica durante il tentativo di riutilizzare le credenziali all'interno di una sessione remota in un altro computer.  È necessario [configurare CredSSP](https://technet.microsoft.com/library/hh849872.aspx) per supportare questo scenario, ma questo non è compreso l'ambito della gestione dell'infrastruttura sorvegliata e risoluzione dei problemi.

#### <a name="using-windows-powershell-just-enough-administration-jea-and-diagnostics"></a>Con Windows PowerShell Just Enough Administration (JEA) e la diagnostica

Diagnosi remota supporta l'uso di endpoint JEA con vincoli di Windows PowerShell. Per impostazione predefinita, le destinazioni di traccia remoto si connetteranno usando il valore predefinito `microsoft.powershell` endpoint.  Se la destinazione di traccia contiene il `HostGuardianService` ruolo, anche proverà a usare il `microsoft.windows.hgs` endpoint che viene configurata quando HGS è installato.

Se si desidera usare un endpoint personalizzato, è necessario specificare il nome della configurazione della sessione durante la costruzione di destinazione di analisi usando il `-PSSessionConfigurationName` parametro, ad esempio sotto:

```PowerShell
New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Role HostGuardianService -Credential (Enter-Credential) -PSSessionConfigurationName "microsoft.windows.hgs"
```

#### <a name="diagnosing-multiple-hosts"></a>La diagnosi più host

È possibile passare più destinazioni di traccia a `Get-HgsTrace` in una sola volta.  Ciò include una combinazione di destinazioni locali e remote.  Ogni destinazione verrà eseguita la traccia a sua volta e quindi le tracce da ogni destinazione sarà possibile diagnosticare contemporaneamente.  Lo strumento di diagnostica è possibile usare una maggiore conoscenza della distribuzione per identificare gli errori di configurazione tra nodi complesse che altrimenti non sarebbero rilevabili.  Utilizzare questa funzionalità richiede solo che fornisce le tracce da più host contemporaneamente (nel caso di diagnosi manuale) o tramite targeting di più host quando si chiama `Get-HgsTrace` (nel caso di diagnosi remota).

Ecco un esempio dell'uso di uno degli host sorvegliati in cui viene usato per avviare la diagnosi remota per la valutazione di un'infrastruttura è costituita da due nodi HGS e due host sorvegliati, `Get-HgsTrace`.

```PowerShell
$hgs01 = New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Credential (Enter-Credential)
$hgs02 = New-HgsTraceTarget -HostName "hgs-02.secure.contoso.com" -Credential (Enter-Credential)
$gh01 = New-HgsTraceTarget -Local
$gh02 = New-HgsTraceTarget -HostName "guardedhost-02.contoso.com"
Get-HgsTrace -Target $hgs01,$hgs02,$gh01,$gh02 -RunDiagnostics
```

> [!NOTE]
> Non devi diagnosticare l'intera infrastruttura sorvegliata durante la diagnosi di più nodi.  In molti casi è sufficiente per includere tutti i nodi che possono essere coinvolti in una determinata condizione di errore.  Si tratta in genere un subset degli host sorvegliati e un certo numero di nodi dal cluster servizio HGS.

## <a name="manual-diagnosis-using-saved-traces"></a>Diagnosi manuale grazie alle tracce salvate

A volte si desidera eseguire nuovamente la diagnostica senza raccogliere nuovamente le tracce o non si dispone delle credenziali necessarie per diagnosi remota tutti gli host nell'infrastruttura contemporaneamente.  Diagnosi manuale è un meccanismo mediante il quale è possibile eseguire ancora un intero fabric valutazione usando `Get-HgsTrace`, ma senza l'utilizzo di raccolta delle tracce remoto.

Prima di eseguire diagnosi manuale, è necessario assicurarsi che gli amministratori di ogni host nell'infrastruttura di cui verrà valutato sono pronti e disposti a eseguire i comandi per tuo conto.  Output di traccia di diagnostica non espone tutte le informazioni che viene in genere visualizzate come dati sensibili, tuttavia spetta all'utente di determinare se è sicuro per esporre queste informazioni ad altri utenti.

> [!NOTE]
> Le tracce non sono rese anonime e individuare la configurazione di rete, le impostazioni di infrastruttura a chiave pubblica e altre opzioni di configurazione che viene talvolta considerata informazioni private.  Pertanto, le tracce devono solo essere trasmesse a entità attendibili all'interno dell'organizzazione e mai registrato pubblicamente.

Come indicato di seguito sono riportati i passaggi per eseguire una diagnosi manuale:

1. Richiesta che ogni amministratore host eseguire `Get-HgsTrace` specificando una determinata `-Path` e l'elenco di diagnostica si prevede di eseguire le tracce risultante.  Ad esempio:

   ```PowerShell
   Get-HgsTrace -Path C:\Traces -Diagnostic Networking,BestPractices
   ```
2. Richiedere che ogni amministratore host cartella traces risultante del pacchetto e inviarlo all'utente.  Questo processo può essere determinato tramite posta elettronica, tramite le condivisioni file, o qualsiasi altro meccanismo di base i criteri e procedure stabilite dall'organizzazione operativa.

3. Unire tutte le tracce ricevute in una singola cartella, con nessun altro contenuto o le cartelle.

    * Si supponga ad esempio si ha la trasmissione di amministratori esegue la traccia raccolti dalle quattro macchine denominate HGS-01, HGS-02, 12 RR1N2608 e RR1N2608-13.  Ogni amministratore potrebbe avere inviato una cartella con lo stesso nome.  È necessario assemblare una struttura di directory che viene visualizzato come segue:

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

4. Eseguire la diagnostica, che fornisce il percorso della cartella di traccia assemblati nella `-Path` parametro e specificare il `-RunDiagnostics` passare oltre tali per cui è richiesto agli amministratori per raccogliere le tracce di diagnostica.  Diagnostica presupporrà che non possa accedere gli host disponibili all'interno del percorso e quindi proverà a usare soltanto le tracce di pre-raccolte.  Se tutte le tracce sono mancanti o danneggiati, diagnostica avrà esito negativo solo i test interessati e proseguono normalmente.  Ad esempio:

   ```PowerShell
   Get-HgsTrace -RunDiagnostics -Diagnostic Networking,BestPractices -Path ".\FabricTraces"
   ```

### <a name="mixing-saved-traces-with-additional-targets"></a>Combinazione di salvare le tracce con destinazioni aggiuntive

In alcuni casi, potrebbe essere un set di tracce pre-raccolti che si vuole integrare con le tracce di altri host.  È possibile combinare le tracce di pre-raccolte con destinazioni aggiuntive che verranno tracciate e diagnosticate in una singola chiamata di diagnostica.

Seguendo le istruzioni visualizzate per raccogliere e assemblare una cartella di traccia specificata in precedenza, chiamare `Get-HgsTrace` con destinazioni di traccia aggiuntivi non disponibili nella corrispondente cartella traccia pre-raccolti:

```PowerShell
$hgs03 = New-HgsTraceTarget -HostName "hgs-03.secure.contoso.com" -Credential (Enter-Credential)
Get-HgsTrace -RunDiagnostics -Target $hgs03 -Path .\FabricTraces
``` 

Il cmdlet di diagnostica identifica tutti gli host pre-raccolti e un solo host aggiuntivi deve ancora essere tracciati e verrà eseguite le analisi necessaria.  Sarà quindi possibile diagnosticare la somma di tutte le tracce di pre-raccolte e appena raccolte.  La cartella di traccia risultante conterrà entrambe le tracce di vecchi e nuove.
