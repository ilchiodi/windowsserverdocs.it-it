---
title: Guida di sviluppo Server Performance Advisor Pack
description: Guida di sviluppo Server Performance Advisor Pack
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c647e8a335aac924067d92dcb41ab4d17e0cceef
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884862"
---
# <a name="server-performance-advisor-pack-development-guide"></a>Guida di sviluppo Server Performance Advisor Pack

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 8, Windows 10

In questa Guida di sviluppo per Microsoft Server Performance Advisor (SPA) vengono fornite linee guida per gli sviluppatori e amministratori di sistema sviluppare Pack di advisor per analizzare le prestazioni del server.

Si presuppone che si ha familiarità con i registri di prestazioni e AVVISI, i contatori delle prestazioni, le impostazioni del Registro di sistema, Strumentazione gestione Windows (WMI), analisi eventi per Windows (ETW) e Transact SQL (T-SQL).

Per ulteriori informazioni sull'utilizzo di SPA, vedi [manuale dell'utente di Server Performance Advisor](server-performance-advisor-users-guide.md).

## <a name="spa-advisor-pack-overview"></a>Cenni preliminari su Advisor SPA


Un pacchetto di advisor è solitamente progettato per un ruolo di server specifico e definisce le operazioni seguenti:

* Dati raccolti tramite erano messe a Disposizione, tra cui Strumentazione gestione Windows (WMI), i contatori delle prestazioni, le impostazioni del Registro di sistema, file e traccia eventi per Windows (ETW)

* Regole, che mostra gli avvisi e raccomandazioni

* Dati da visualizzare (dati non elaborati raccolti, i dati aggregati o 10 elenchi superiore)

* Statistiche per visualizzare un valore che cambia nel tempo

* Valori di statistiche che possono essere individuare le tendenze

Un pacchetto di preparazione include gli elementi seguenti:

* **Metadati XML** (ProvisionMetadata.xml)

    * [AVVISI e registri di prestazioni](https://msdn.microsoft.com/library/windows/desktop/aa372635.aspx) insieme agenti di raccolta dati

    * Layout del report

* **Script SQL**

    * Una stored procedure principale

    * Oggetti SQL quali stored procedure e funzioni definite dall'utente

* **File di schema ETW** (man) è facoltativo

### <a name="advisor-pack-workflow"></a>Flusso di lavoro di preparazione pack

![flusso di lavoro di preparazione pack](../media/server-performance-advisor/spa-dev-guide-workflow.png)

In questo diagramma di flusso cerchi verdi rappresentano Pack di advisor. Tutti gli altri cerchi rappresentano le fasi in cui sono in esecuzione in corso il framework di applicazione a singola PAGINA. SPA utilizza un pacchetto di advisor per raccogliere i dati, importare i dati nel database, inizializzare l'ambiente di esecuzione ed eseguire gli script SQL.

### <a name="collect-data"></a>Raccolta di dati

Quando un pacchetto di advisor viene accodato per un determinato server mediante SPA, il modulo di raccolta dati richiede il set di agenti di raccolta di dati XML da pack advisor e raccoglie i dati dal server di destinazione. I dati non elaborati vengono archiviati in una condivisione di file specificato dall'utente. La raccolta di dati non viene arrestata fino a quando non viene superato il SPA eseguire durata definita dall'utente.

### <a name="import-data-into-the-database"></a>importare i dati nel database

Una volta completata la raccolta dei dati, ogni tipo di dati viene importato in una tabella corrispondente nel database di SQL Server. Ad esempio, le impostazioni del Registro di sistema vengono importate in una tabella denominata \#chiavi registryKeys.

importazione di ETW file richiede un file di schema ETW per la decodifica il file con estensione etl. Il file di schema ETW è un file XML. Può essere generato utilizzando tracerpt.exe, incluso in Windows. Il file di schema ETW è solo necessario quando è necessario importare i dati ETW pack di advisor.

### <a name="switch-to-low-user-rights"></a>Passare a diritti utente bassa

Il framework SPA regola automaticamente i privilegi per ridurre al minimo il livello di accesso di sicurezza richiesto. Poiché Pack di advisor può essere sviluppato o modificato da alcun utente, è possibile che un pacchetto di advisor contenere gli script SQL alterati. Per ridurre il rischio di protezione, tutti gli script SQL per un pacchetto di advisor deve essere eseguito con diritti limitati. È possibile accedere solo gli oggetti di database limitato, ad esempio tabelle temporanee e le API di SPA esposte come stored procedure. Gli script SQL in un pacchetto di advisor possono chiamare le stored procedure per l'interazione con il framework di applicazione a singola PAGINA.

### <a name="initialize-execution-environment"></a>Inizializzare l'ambiente di esecuzione

Pack di Advisor può generare diversi tipi di output, ad esempio le notifiche, indicazioni, le tabelle dei fatti, statistiche e grafici per le statistiche. Gli script SQL eseguono determinati calcoli sui dati raccolti. I risultati prodotti vengono archiviati in tabelle temporanee tramite le API pubbliche SPA. in fase di inizializzazione, è necessario eseguirne il provisioning queste tabelle temporanee e altre risorse di sistema.

### <a name="run-sql-scripts"></a>Eseguire gli script SQL

Esiste una stored procedure principale, denominata dallo sviluppatore di Service pack di advisor. Il framework SPA chiama la stored procedure per iniziare il calcolo. La stored procedure utilizza i dati raccolti e comunica il risultato finale per il framework di applicazione a singola PAGINA.

### <a name="switch-to-administrative-rights"></a>Passare ai diritti amministrativi

Diritti di amministratore necessari per generare un report. Generazione di report è interamente controllata dallo SPA. È meno probabile che essere alterato.

### <a name="generate-a-report"></a>Generare un report

Prima di completamento della stored procedure principale per un pacchetto di advisor, tutti i risultati calcolati, ad esempio le notifiche e le statistiche, non sono persistenti. Durante questa fase, il framework SPA trasferisce i risultati finali da tabelle temporanee per le tabelle in un formato particolare. Una volta completata questa fase, è possibile visualizzare i report utilizzando la console SPA.

## <a name="authoring-an-advisor-pack"></a>Creazione di un pacchetto di advisor


### <a name="quick-guidelines"></a>Linee guida rapide

Diagramma di flusso seguente vengono descritti i passaggi per lo sviluppo di un Service pack di advisor completamente funzionale. In questa sezione sono inoltre inclusi esempi dettagliati per spiegare meglio ciascuna fase.

![processo di sviluppo di Service pack di Advisor](../media/server-performance-advisor/spa-dev-guide-dev-flowchart.png)

Un pacchetto di advisor è generalmente strutturato nel modo seguente:

Service pack di Advisor

ProvisionMetadata.xml

Script

Main.SQL

Func.SQL

Man

Ogni pacchetto advisor deve avere un file denominato ProvisionMetadata.xml. Definisce le informazioni di base advisor pack, quali dati raccogliere, notifiche e le regole e come il report dovrà essere archiviate e visualizzate. Il framework SPA utilizza queste informazioni per generare una tabella temporanea e quindi trasferire i risultati nella tabella temporanea in una tabella che gli utenti possono accedere.

Tutti gli script SQL devono essere salvati in una sottocartella denominata del report **script**. Per motivi di manutenzione, è consigliabile salvare gli oggetti di database diversi in diversi file di SQL Server. Come punto di ingresso principale deve essere presente almeno una stored procedure.

> [!NOTE]
> Il file man non è obbligatorio a meno che il pacchetto di advisor raccoglie tracce ETW. Questo file di schema viene utilizzato per descrivere lo schema degli eventi ETW e decodificare gli eventi ETW.

### <a name="defining-basic-information"></a>Definizione delle informazioni di base

In questa sezione vengono descritti alcuni degli elementi di base che costituiscono un pacchetto di advisor, inclusi ProvisionMetadata.xml e gli attributi.

Di seguito è un'intestazione di esempio per il file ProvisionMetadata.xml:

``` syntax
<advisorPack
xmlns="http://microsoft.com/schemas/ServerPerformanceAdvisor/ap/2010"
name="Microsoft.ServerPerformanceAdvisor.CoreOS.V2"
displayName="Microsoft CoreOS Advisor Pack V2"
description="Microsoft CoreOS Advisor Pack"
author="Microsoft"
version="1.0"
frameworkversion="3.0"
minOSversion="6.0"
reportScript="ReportScript">
</advisorPack>
```

### <a name="advisor-pack-version"></a>Versione Service pack di Advisor

nome attributo: **versione**

Gli sviluppatori di Service pack di Advisor possono definire versioni principali e secondarie per il pacchetto di advisor:

* Una versione principale comporta in genere significativi miglioramenti. I risultati generati da una versione precedente potrebbero non essere compatibili con quello nuovo. È consigliabile tra la versione principale nel nome del Service pack di advisor.

* SPA consente l'aggiornamento di versione secondaria quando sono presenti solo piccole modifiche senza problemi di incompatibilità dei dati.

per altre informazioni sul controllo delle versioni, vedi [argomenti avanzati](#bkmk-advancedtopics).

### <a name="script-entry-point"></a>Punto di ingresso di script

nome attributo: **reportScript**

Il framework SPA Cerca il nome di stored procedure principale dal punto di ingresso di script e viene eseguito in modo protetto.

### <a name="other-attributes"></a>Altri attributi

Di seguito sono elencati altri attributi che possono essere utilizzati per identificare un pacchetto di advisor:

* Nome visualizzato: **displayName**

* Descrizione: **descrizione**

* Autore: **autore**

* Versione Framework: **frameworkversion** (impostazione predefinita è 3.0)

* Versione minima del sistema operativo: **minOSversion** (riservato per un'estendibilità futura)

* Notifica degli eventi di perdita: **showEventLostWarning**

### <a href="" id="bkmk-definedatacollector"></a>Definire l'insieme agenti di raccolta dati

Un insieme agenti di raccolta dati definisce i dati sulle prestazioni raccolti in framework SPA dal server di destinazione. Supporta i contatori delle prestazioni WMI, le impostazioni del Registro di sistema, i file dal server di destinazione ed ETW.

``` syntax
<advisorPack>
<dataSourceDefinition xmlns="http://microsoft.com/schemas/ServerPerformanceAdvisor/dc/2010">
 <dataCollectorSet duration="10">
<registryKeys>
 ?<registryKey>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes\\</registryKey>
</registryKeys>
<managementpaths>
 ?<path>Root\Cimv2:select * FROM Win32_DiskDrive</path>
</managementpaths>
<performanceCounters interval="2">
 ?<performanceCounter>\PhysicalDisk(*)\Avg. Disk sec/Transfer</performanceCounter>
</performanceCounters>
<files>
 ?<path>%windir%\System32\inetsrv\config\applicationHost.config</path>
</files>
<providers>
 ?<provider session="NT Kernel Logger" guid="{9E814AAD-3204-11D2-9A82-006008A86939}" keywordsany="06010201" keywordsAll="00000000" level="00000000" />
</providers>
 </dataCollectorSet>
</dataSourceDefinition>
</advisorPack>
```

Il **durata** attributo di **&lt;dataCollectorSet /&gt;** definisce il periodo di raccolta dati (l'unità di tempo è secondi) nell'esempio precedente. **durata** è un attributo obbligatorio. Questa impostazione controlla la durata di raccolta che viene utilizzata dai contatori delle prestazioni ed ETW.

### <a name="collect-registry-data"></a>Raccogliere i dati del Registro di sistema

È possibile raccogliere dati del Registro di sistema dagli hive del Registro di sistema seguente:

* HKEY\_CLASSI\_RADICE

* HKEY\_CURrenT\_CONFIG

* HKEY\_CURrenT\_USER

* HKEY\_LOCALE\_MACCHINA

* HKEY\_UTENTI

Per raccogliere un'impostazione del Registro di sistema, specificare il percorso completo per il nome del valore: HKEY\_LOCAL\_MACHINE\\MyKey\\MyValue

Per raccogliere tutte le impostazioni in una chiave del Registro di sistema, specificare il percorso completo alla chiave del Registro di sistema: HKEY\_LOCAL\_MACHINE\\MyKey\\

Per raccogliere tutti i valori in una chiave del Registro di sistema e le relative chiavi secondarie (in modo ricorsivo PIA raccoglie i dati del Registro di sistema), utilizzare due barre rovesciate per l'ultimo delimitatore di percorso: HKEY\_LOCAL\_MACHINE\\MyKey\\\\

Per raccogliere le informazioni del Registro di sistema da un computer remoto, includere il nome del computer all'inizio del percorso del Registro di sistema: HKEY\_LOCAL\_MACHINE\\MyKey\\MyValue

Ad esempio, potrebbe essere una chiave del Registro di sistema che viene visualizzato come segue:

``` syntax
Windows registry editor version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes]
"activePowerScheme"="db310065-829b-4671-9647-2261c00e86ef"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes\db310065-829b-4671-9647-2261c00e86ef]
"Description"=
 FriendlyName = Power Source Optimized 

HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes\db310065-829b-4671-9647-2261c00e86ef \0012ee47-9041-4b5d-9b77-535fba8b1442\6738e2c4-e8a5-4a42-b16a-e040e769756e
"ACSettingIndex"=dword:000000b4
"DCSettingIndex"=dword:0000001e
```

Esempio 1: Restituire solo il PowerSchemes attivo e i relativi valori:

``` syntax
<registryKey>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes</registryKey>
```

Esempio 2: Restituisce tutte le coppie chiave-valore nel percorso seguente:

> [!NOTE]
> PIA viene eseguito con le credenziali dell'utente. Alcune chiavi del Registro di sistema richiedono credenziali amministrative. L'enumerazione si interrompe quando non è possibile accedere a una delle chiavi secondarie.

``` syntax
<registryKey>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes\\</registryKey>
```

Verranno importati tutti i dati raccolti in una tabella temporanea denominata  **\#registryKeys** prima di un report SQL viene eseguito lo script. Nella tabella seguente vengono illustrati i risultati, ad esempio 2:

KeyName | KeytypeId | Value
------ | ----- | -------
HKEY_LOCAL_MACHINE...\PowerSchemes | 1 | db310065-829b-4671-9647-2261c00e86ef
\db310065-829b-4671-9647-2261c00e86ef\Description | 2 | |
\db310065-829b-4671-9647-2261c00e86ef\FriendlyName | 2 | Con ottimizzazione per la fonte di alimentazione
...\6738e2c4-e8a5-4a42-b16a-e040e769756e\ACSettingIndex | 4 | 180
...\6738e2c4-e8a5-4a42-b16a-e040e769756e\DCSettingIndex | 4 | 30

Lo schema per il **#registryKeys** tabella è una classe come indicato di seguito:

Nome colonna | Tipo di dati SQL | Descrizione
-------- | -------- | --------
KeyName | Nvarchar(300) NON NULL | Nome del percorso completo della chiave del Registro di sistema
KeytypeId | Smallint NON NULL | Id di tipo interno
Value | Nvarchar (4000) NON NULL | Tutti i valori

Il **KeytypeID** colonna può avere uno dei tipi seguenti:
ID | Tipo
--- | ---
1 | Stringa
2 | expandString
3 | Binary
4 | DWord
5 | DWordBigEndian
6 | Collegamento
7 | MultipleString
8 | Resourcelist
9 | FullResourceDescriptor
10 | ResourceRequirementslist
11 | QWORD

### <a name="collect-wmi"></a>Raccogliere WMI

È possibile aggiungere qualsiasi query WMI. Per ulteriori informazioni sulla scrittura di query WMI, vedere [WQL (SQL per WMI)](https://msdn.microsoft.com/library/windows/desktop/aa394606.aspx). Nell'esempio seguente viene eseguita una query di un percorso di file di pagina:

``` syntax
<path>Root\Cimv2:select * FROM Win32_PageFileUsage</path>
```

Nell'esempio precedente la query restituisce un record:

Didascalia | Nome | PeakUsage
----- | ----- | -----
C:\pagefile.sys | C:\pagefile.sys | 215

Poiché WMI restituisce una tabella con colonne diverse, quando viene importati i dati raccolti in un database, SPA esegue la normalizzazione dei dati e viene aggiunto per le tabelle seguenti:

**\#Tabella WMIObjects**

SequenceID | Spazio dei nomi | ClassName | RelativePath | WmiqueryID
----- | ----- | ----- | ----- | -----
10 | Root\Cimv2 | Win32_PageFileUsage | Win32_PageFileUsage.Name=<br>C:\\Pagefile. sys | 1

**\#Tabella WmiObjectsProperties**

ID | query
--- | ---
1 | Root\Cimv2:select * FROM Win32_PageFileUsage

**\#Tabella WmiQueries**

ID | query
--- | ---
1 | Root\Cimv2:select * FROM Win32_PageFileUsage

**\#Tabella wmiobjects**

Nome colonna | Tipo di dati SQL | Descrizione
--- | --- | ---
SequenceId | Int NON NULL | Correlare la riga e le relative proprietà
Spazio dei nomi | Nvarchar (200) NON NULL | Spazio dei nomi WMI
ClassName | Nvarchar (200) NON NULL | Nome della classe WMI
RelativePath | Nvarchar (500) NON NULL | Percorso relativo a WMI
WmiqueryId | Int NON NULL | Correlare la chiave di #WmiQueries

**\#Tabella wmiobjectproperties**

Nome colonna | Tipo di dati SQL | Descrizione
--- | --- | ---
SequenceId | Int NON NULL | Correlare la riga e le relative proprietà
Nome | Nvarchar (1000) NON NULL | Nome proprietà
Value | Nvarchar (4000) NULL | Il valore della proprietà corrente

**\#Tabella wmiqueries**

Nome colonna | Tipo di dati SQL | Descrizione
--- | --- | ---
Id | Int NON NULL | > ID query univoco
query | Nvarchar (4000) NON NULL | Stringa di query originale nei metadati del provisioning

### <a name="collect-performance-counters"></a>Raccogliere i contatori delle prestazioni

Ecco un esempio di come raccogliere un contatore delle prestazioni s:

``` syntax
<performanceCounters interval="1">
  <performanceCounter>\PhysicalDisk(*)\Avg. Disk sec/Transfer</performanceCounter>
</performanceCounters>
```

Il **intervallo** attributo è un'impostazione globale necessaria per tutti i contatori delle prestazioni. Definisce l'intervallo (l'unità di tempo è secondi) di raccolta dati sulle prestazioni.

Nell'esempio precedente, contatore \\disco fisico (\*)\\Media Trasferimenti disco/sec interrogato ogni secondo.

È possibile che due istanze: **\_Totale** e **c 0: Unità d:**, e l'output potrebbe essere come segue:

timestamp | CategoryName | CounterName | Valore dell'istanza di totale | Valore dell'istanza di c 0: D:
---- | ---- | ---- | ---- | ----
13:45:52.630 | PhysicalDisk | Tempo medio Trasferimenti disco/sec | 0.00100008362473995 |0.00100008362473995
13:45:53.629 | PhysicalDisk | Tempo medio Trasferimenti disco/sec | 0.00280023414927187 | 0.00280023414927187
13:45:54.627 | PhysicalDisk | Tempo medio Trasferimenti disco/sec | 0.00385999853230048 | 0.00385999853230048
13:45:55.626 | PhysicalDisk | Tempo medio Trasferimenti disco/sec | 0.000933297607934224 | 0.000933297607934224

Per importare i dati al database, i dati vengono normalizzati in una tabella denominata **\#PerformanceCounters**.

CategoryDisplayName | InstanceName | CounterdisplayName | Value
---- | ---- | ---- | ----
PhysicalDisk | Totale | Tempo medio Trasferimenti disco/sec | 0.00100008362473995
PhysicalDisk | 0 C: D: | Tempo medio Trasferimenti disco/sec | 0.00100008362473995
PhysicalDisk | Totale | Tempo medio Trasferimenti disco/sec | 0.00280023414927187
PhysicalDisk | 0 C: D: | Tempo medio Trasferimenti disco/sec | 0.00280023414927187
PhysicalDisk | Totale | Tempo medio Trasferimenti disco/sec | 0.00385999853230048
PhysicalDisk | 0 C: D: | Tempo medio Trasferimenti disco/sec | 0.00385999853230048
PhysicalDisk | Totale | Tempo medio Trasferimenti disco/sec | 0.000933297607934224
PhysicalDisk | 0 C: D: | Tempo medio Trasferimenti disco/sec | 0.000933297607934224

**Nota** il localizzato nomi, ad esempio **CategoryDisplayName** e **CounterdisplayName**, variano in base alla lingua di visualizzazione utilizzata nel server di destinazione. Evitare di utilizzare tali campi, se si desidera creare un pacchetto di consulente indipendente dalla lingua.

**\#PerformanceCounters** lo schema della tabella

Nome colonna | Tipo di dati SQL | Descrizione
---- | ---- | ---- | ----
timestamp | datetime2 (3) non NULL | Le raccolte di data/ora nel UNC
CategoryName | Nvarchar (200) NON NULL | Nome della categoria
CategoryDisplayName | Nvarchar (200) NON NULL | Nome di categoria localizzata
InstanceName | NULL nvarchar (200) | Nome istanza
CounterName | Nvarchar (200) NON NULL | Nome contatore
CounterdisplayName | Nvarchar (200) NON NULL | Nome localizzato del contatore
Value | Float NON NULL | Il valore raccolto

### <a name="collect-files"></a>Raccogli file

I percorsi possono essere assoluto o relativo. Il nome del file può includere il carattere jolly (\*) e il punto interrogativo (?). Ad esempio, per raccogliere tutti i file nella cartella temporanea, è possibile specificare c:\\temp\\\*. Il carattere jolly si applica ai file nella cartella specificata.

Se si desidera inoltre raccogliere i file dalle sottocartelle della cartella specificata, utilizzare due barre rovesciate per l'ultimo delimitatore di cartella, ad esempio, c\\temp\\\\\*.

Qui s un esempio che esegue una query di **applicationHost. config** file:

``` syntax
<path>%windir%\System32\inetsrv\config\applicationHost.config</path>
```

I risultati sono reperibili in una tabella denominata **\#file**, ad esempio:

querypath | FullPath | Parentpath | FileName | Content
----- | ----- | ----- | ----- | -----
% windir %\.... \applicationHost.config |C:\Windows<br>\... \applicationHost.config | C:\Windows<br>\...\config | applicationHost.confi | 0x3C3F78

**\#Schema della tabella file**

Nome colonna | Tipo di dati SQL | Descrizione
---- | ---- | ----
querypath | Nvarchar(300) NON NULL | Istruzione di query originale
FullPath | Nvarchar(300) NON NULL | Percorso file assoluto e nome file
Parentpath | Nvarchar(300) NON NULL | Percorso del file
FileName | Nvarchar(300) NON NULL | Nome file
Content | Varbinary (max) NULL | Contenuto del file in formato binario

### <a name="defining-rules"></a>Definizione delle regole

Utilizzando erano messe a Disposizione da un server di destinazione vengono raccolti dati sufficienti, pack di advisor possono utilizzare questi dati per la convalida e Mostra un rapido riepilogo agli amministratori di sistema.

Le regole offrono una rapida panoramica sui server delle prestazioni di s. Che evidenziano i problemi e fornire consigli. È possibile elencare tutte le regole che si desidera convalidare un pacchetto di advisor. Ad esempio, se si desidera sviluppare un pack principale di preparazione del sistema operativo, potrebbero includere le regole possibili:

* Se la modalità di alimentazione della CPU è risparmio

* Se il server è in un ambiente virtualizzato

* Se è presente la pressione dei / o disco

Regole contengono i seguenti elementi:

* Soglia dipendenti (una parte di una regola configurabile)

* Definizione di regole (avvisi e indicazioni)

Ecco un esempio di una regola semplice s:

``` syntax
<advisorPack>
   
  <reportDefinition>
    <thresholds>
      <threshold  />
    </thresholds>
    <rules>
      <rule  />
      </rule>
    </rules>
  </reportDefinition>
</advisorPack>
```

### <a name="threshold"></a>Soglia

Soglia è un fattore configurabile che consente agli amministratori di sistema decidere quando una regola da visualizzare una buona o uno stato non valido. Nell'esempio seguente viene illustrata una regola per rilevare lo spazio disponibile su un'unità di sistema e un avviso quando lo spazio disponibile è inferiore a 10 GB.

``` syntax
<threshold name="freediskSize" caption="Free Disk Size (GB)" description="Free Disk Size  value="10" />
```

Tuttavia, in questo caso, l'amministratore di sistema ha un disco rigido più piccoli. Pensa di 5 GB di spazio disponibile potrebbe essere ancora una condizione valida e non desidera visualizzare un avviso. È possibile aggiornare il valore predefinito da 10 a 5 tramite la console SPA senza dover imparare a sviluppare un pacchetto di advisor.

Introduzione a una soglia consentono agli amministratori di modificare rapidamente il valore senza dover modificare il pacchetto di advisor.

Nell'esempio, tutti gli attributi ad eccezione di **Descrizione** sono necessari. È possibile utilizzare un numero qualsiasi di **valore**.

Una soglia può essere condivise tra le regole.

### <a name="alerts-and-recommendations"></a>Gli avvisi e suggerimenti

La definizione della regola non comporta alcun calcolo della logica. Definisce come potrebbe apparire l'interfaccia Utente e la modalità di SQL Server report script comunica i risultati all'interfaccia Utente.

Una regola ha tre parti:

* Avviso (didascalia regola)

* Raccomandazione (avviso)

* Soglia associata (informazioni facoltative sulle dipendenze)

Di seguito è riportato un esempio di una regola:

``` syntax
<rule name="freediskSize" caption="Free Disk Size on System Drive" description="This rule checks free disk size on system drive ">
<advice name="SuccessAdvice" level="Success" message="No issue found.">No Recommendation.</advice>
<advice name="WarningAdvice" level="Warning" message="Not enough free space on system drive.">
Install OS on larger disk.</advice>
<dependencies>
 <threshold ref="freediskSize"/>
</dependencies>
</rule>
```

È possibile definire quante consigli che si desidera, è possibile in genere definire indicazioni. Il **livello** di avviso può essere **successo** o **avviso**.

È possibile collegare a tante soglie desiderate. È anche possibile collegare a una soglia che è irrilevante per la regola corrente. Il collegamento consente la console SPA di gestire facilmente le soglie.

Il nome della regola e le indicazioni sono chiavi e i nomi siano univoci nel proprio ambito. Nessuna due regola può avere lo stesso nome, e nessuna due indicazione all'interno di una regola possono avere lo stesso nome. Questi nomi sarà molto importante quando si scrive un report di script SQL. È possibile chiamare il \[dbo\].\[SetNotification\] API per impostare lo stato della regola.

### <a name="defining-ui-display-elements"></a>Definizione di elementi di visualizzazione dell'interfaccia Utente

Dopo aver definite le regole, gli amministratori di sistema possono visualizzare il report riepilogo. Tuttavia, spesso gli amministratori di sistema che interessa i dati aggregati e si desidera controllare le origini dati che sono state utilizzate nelle regole di prestazioni.

Continuando con l'esempio precedente, l'utente sappia se è disponibile spazio sufficiente nell'unità di sistema. Gli utenti potrebbero essere interessati anche le dimensioni effettive dello spazio libero. Un gruppo singolo valore viene utilizzato per archiviare e visualizzare questi risultati. Più valori singoli possono essere raggruppati e visualizzati in una tabella nella console di SPA. La tabella include solo due colonne, nome e il valore, come illustrato di seguito.

Nome | Value
---- | ----
Dimensioni disco nell'unità di sistema (GB) | 100
Dimensione totale del disco installato (GB) | 500 

Se un utente desidera visualizzare un elenco di tutti i dischi rigidi installati nel server e le dimensioni del disco, potevamo chiamare un valore di elenco, che include tre colonne e più righe, come illustrato di seguito.

Disk | Dimensioni disco disponibile (GB) | Dimensione totale (GB)
---- | ---- | ----
0 | 100 | 500
1 | 20 | 320

In un pacchetto di advisor, potrebbero esistere molte tabelle (gruppi valore singolo e tabelle di valori di elenco). È possibile usare una sezione per organizzare e classificare le tabelle.

In sintesi, esistono tre tipi di elementi dell'interfaccia Utente:

* [Sezioni](#bkmk-ui-section)

* [Gruppi valore singolo](#bkmk-ui-svg)

* [elencare le tabelle di valore](#bkmk-ui-lvt)

In questo caso s un esempio che mostra gli elementi dell'interfaccia utente:

``` syntax
<advisorPack>
<dataSourceDefinition/>
<reportDefinition>
 <datatypes>
<datatype .../>
 </datatypes>
 <thresholds/>
 <rule/>
 <sections>
<section .../>
 </sections>
 <singleValues>
<singleValue .../>
 </singleValues>
 <listValues>
<listValue .../>
 </listValues>
</reportDefinition>
</advisorPack>
```

### <a href="" id="bkmk-ui-section"></a>Sezioni

Una sezione è esclusivamente per il layout dell'interfaccia Utente. Non è coinvolto in alcun calcolo logico. Ogni singolo report contiene un set di sezioni di primo livello che non dispongono di una sezione padre. Le sezioni di primo livello vengono presentate come schede nel report. Nelle sezioni possono avere sottosezioni, con un massimo di 10 livelli. Tutte le sottosezioni sotto le sezioni di primo livello vengono presentate nelle aree espandibile. Una sezione può contenere più sottosezioni, i gruppi valore singolo e tabelle di valori di elenco. Singolo valore gruppi e le tabelle elenco vengono presentate come tabelle.

Di seguito è riportato un esempio della sezione di primo livello.

``` syntax
<section name="CPU" caption="CPU"/>
```

Un nome di sezione deve essere univoco. Viene utilizzato come una chiave che può essere collegata a altre sezioni, i gruppi valore singolo e elencare le tabelle di valore.

Nell'esempio seguente include un attributo, **padre**, e fa riferimento alla sezione della CPU. CPUFacts è un elemento figlio della sezione denominata CPU. **padre** deve fare riferimento a un nome di sezione precedente, in caso contrario, può comportare un ciclo.

``` syntax
<section name="CPUFacts" caption="Facts" parent="CPU"/>
```

Il gruppo a valore singolo seguente include un attributo, **sezione**, e può puntare a qualsiasi sezione, in base alla progettazione dell'interfaccia Utente.

``` syntax
<singleValue name="CPUInformation" section="CPUFacts" caption="Physical CPU Information"> </singleValue>
```

### <a name="data-types"></a>Tipi di dati

Un gruppo singolo valore e una tabella di valori di elenco contengono diversi tipi di dati, ad esempio string, int e float. Poiché questi valori sono archiviati nel database di SQL Server, è possibile definire un tipo di dati SQL per ogni proprietà di dati. Tuttavia, la definizione di un tipo di dati SQL è piuttosto complicata. È necessario specificare la lunghezza o precisione, che potrà essere soggetta a modifica.

Per definire i tipi di dati logico, è possibile utilizzare il primo elemento figlio di **&lt;reportDefinition /&gt;**, ovvero in cui è possibile definire un mapping del tipo di dati SQL e il tipo di logico.

Nell'esempio seguente definisce due tipi di dati. Uno è **stringa** e l'altro è **companyCode**.

``` syntax
<datatype name="string" = sqltype="nvarchar(4000)" />
<datatype name="companyCode" sqltype="nvarchar(100)" />
```

Il nome di un tipo di dati può essere qualsiasi stringa valida. Ecco un elenco dei tipi di dati SQL consentiti:

* bigint

* file binario

* bit

* Char

* date

* Data/ora

* datetime2

* DateTimeOffset

* decimale

* float

* int

* Money

* nchar

* numerico

* nvarchar

* Real

* smalldatetime

* smallint

* smallmoney

* ora

* tinyint

* uniqueidentifier

* varbinary

* varchar

Per altre informazioni su questi tipi di dati SQL, vedere [tipi di dati (Transact-SQL)](https://msdn.microsoft.com/library/ms187752.aspx).

### <a href="" id="bkmk-ui-svg"></a>Gruppi valore singolo

Un gruppo singolo valore consente di raggruppare più valori singoli in modo da presentare in una tabella, come illustrato di seguito.

``` syntax
<singleValue name="Systemoverview" section="SystemoverviewSection" caption="Facts">
<value name="OsName" type="string" caption="Operating system" description="WMI: Win32_OperatingSystem/Caption"/>
<value name="Osversion" type="string" caption="OS version" description="WMI: Win32_OperatingSystem/version"/>
<value name="OsLocation" type="string" caption="OS location" description="WMI: Win32_OperatingSystem/SystemDrive"/>
</singleValue>
```

Nell'esempio precedente, è stato definito un gruppo singolo valore. Si tratta di un nodo figlio della sezione **SystemoverviewSection**. Questo gruppo dispone di valori singoli, che sono **OsName**, **Osversion**, e **OsLocation**.

Un singolo valore deve avere un attributo di nome univoco globale. In questo esempio, è l'attributo del nome univoco globale **Systemoverview**. Il nome univoco da utilizzare per generare una vista corrispondente per il report personalizzato. Ogni visualizzazione contiene il prefisso **vw**, ad esempio vwSystemoverview.

Anche se è possibile definire più gruppi di singolo valore, non due nomi di valore singolo possono essere lo stesso, anche se sono in gruppi diversi. Il nome del singolo valore viene utilizzato il report di script SQL per impostare il valore di conseguenza.

È possibile definire un tipo di dati per ogni singolo valore. L'input consentito per **tipo** definito nella  **&lt;datatype /&gt;**. Il report finale può avere un aspetto simile al seguente:

**Fact**

Nome | Value
--- | ---
Sistema operativo | &lt;_un valore verrà impostato dallo script di report_&gt;
Versione sistema operativo | &lt;_un valore verrà impostato dallo script di report_&gt;
Percorso del sistema operativo | &lt;_un valore verrà impostato dallo script di report_&gt;

Il **didascalia** attributo di **&lt;valore /&gt;** viene presentato nella prima colonna. Valori nella colonna valore vengono impostati in futuro per il report di script tramite \[dbo\].\[SetSingleValue\]. Il **Descrizione** attributo di **&lt;valore /&gt;** viene visualizzato in una descrizione comando. In genere la descrizione comando viene visualizzato agli utenti l'origine dei dati. Per ulteriori informazioni sulle descrizioni comandi, vedere [le descrizioni comandi](#bkmk-tooltips).

### <a href="" id="bkmk-ui-lvt"></a>elencare le tabelle di valore

Definizione di un valore di elenco è uguale alla definizione di una tabella.

``` syntax
<listValue name="NetworkAdapterInformation" section="NetworkIOFacts" caption="Physical network adapter information">
<column name="NetworkAdapterId" type="string" caption="ID" description="WMI: Win32_NetworkAdapter/DeviceID"/>
<column name="NetworkAdapterName" type="string" caption="Name" description="WMI: Win32_NetworkAdapter/Name"/>
<column name="type" type="string" caption="type" description="WMI: Win32_NetworkAdapter/Adaptertype"/>
<column name="Speed" type="decimal" caption="Speed (Mbps)" description="WMI: Win32_NetworkAdapter/Speed"/>
<column name="MACaddress" type="string" caption="MAC address" description="WMI: Win32_NetworkAdapter/MACaddress"/>
</listValue>
```

Il nome del valore di elenco deve essere globalmente univoco. Questo nome diventa il nome di una tabella temporanea. Nell'esempio precedente, la tabella denominata \#NetworkAdapterInformation verrà creato durante la fase di inizializzazione ambiente esecuzione, che contiene tutte le colonne descritte. Analogamente al nome di un singolo valore, il nome di un valore di elenco viene anche utilizzato come parte del nome di visualizzazione personalizzata, ad esempio, vwNetworkAdapterInformation.

@type dei &lt;colonna /&gt; è definito da &lt;datatype /&gt;

L'interfaccia Utente fittizio della relazione finale potrebbe apparire come segue:

**Informazioni scheda di rete fisica**

ID | Nome | Tipo | Velocità (Mbps) | Indirizzo MAC
--- | --- | --- | --- | ---
 | <br> | | |
 | | | |


Il **didascalia** attributo &lt;colonna /&gt; viene visualizzato come un nome di colonna e il **Descrizione** attributo di &lt;colonna /&gt; viene visualizzato come descrizione comando per l'intestazione di colonna corrispondente. In genere la descrizione comando viene visualizzato all'utente l'origine dei dati. Per ulteriori informazioni, vedere [le descrizioni comandi](#bkmk-tooltips).

In alcuni casi, una tabella può avere numerose colonne e poche righe, in modo che lo scambio di colonne e righe sarebbe la tabella un aspetto molto migliore. Per scambiare le colonne e righe, è possibile aggiungere l'attributo di stile seguente:

``` syntax
<listValue style="Transpose"  
```

### <a name="defining-charting-elements"></a>Definizione degli elementi grafici

È possibile selezionare qualsiasi chiave statistiche e visualizzare i valori in un grafico di andamento storico o un grafico di tendenza. Esistono due tipi di statistiche:

* **Statistiche statiche** un singolo valore, è noto in fase di progettazione. Lo spazio su disco disponibile in un'unità di sistema sarebbe ad esempio, una statistica statica.

* **Statistiche dinamiche** potrebbe non essere noto in fase di progettazione. Ad esempio, l'utilizzo medio della CPU di ciascun core è una statistica dinamica perché non si conosce il numero di core CPU potrebbero nel sistema in fase di progettazione.

La chiave delle statistiche dispone di una limitazione che i dati devono essere compatibili con tipo di dati double. Può essere un integer, decimal o una stringa che può essere convertita in double.

SPA utilizza un gruppo singolo valore per supportare le statistiche statiche e una tabella di valore di elenco per supportare le statistiche dinamiche. Nelle sezioni seguenti viene descritto come definire statistica statico e dinamico statistica chiavi.

### <a name="static-statistics"></a>Statistiche statiche

Come accennato in precedenza, una statistica statica è un singolo valore. Logicamente, qualsiasi valore singolo può essere definita come una statistica statica. Tuttavia, è significativo per visualizzare un singolo valore che non è possibile eseguire il cast in un tipo numerico. Per definire una statistica statica, è possibile aggiungere semplicemente l'attributo **trendable** alla singolo valore chiave corrispondente come mostrato di seguito:

``` syntax
<value name="freediskSize" type="int" trendable="true"  
```

### <a name="dynamic-statistics"></a>Statistiche dinamiche

Le chiavi dinamiche statistiche non sono noti in fase di progettazione, pertanto il numero di valori possibili è sconosciuto. Tuttavia, poiché i valori di elenco sono archiviati in più righe, sarebbe facile da utilizzare una tabella di valori di elenco per l'archiviazione dinamica delle statistiche.

ad esempio, se è necessario visualizzare grafici per l'utilizzo medio della CPU di core diversi, è possibile definire una tabella con colonne per **CpuId** e **AverageCpuUsage**:

``` syntax
<listValue name="CpuPerformance">
<column name="CpuId" type="string" caption="CPU ID" columntype="Key"/>
<column name="AverageCpuUsage" type="decimal" caption="Average" columntype="Value"/>
</listValue>
```

Un altro attributo, **columntype**, può essere **chiave**, **valore**, oppure **Informational**. Il tipo di dati di **chiave** colonna deve essere double o convertibile doppie. In un **chiave** colonna, è possibile inserire le stesse chiavi in una tabella. **Valore** o **informativo** colonne non hanno questo limite.

I valori delle statistiche vengono archiviati **valore** colonne.

**Informativo** sono colonne come colonne ordinarie nelle tabelle di valore di elenco normale. **Informativo** è il tipo di colonna predefinito se non si specifica uno. Tali colonne non influisce sul numero di chiavi di statistiche o partecipare nei calcoli di statistiche correlate.

Continuando con l'esempio precedente, se un server dispone di due core di CPU, il risultato nella tabella può avere un aspetto simile al seguente:

CpuId | AverageCpuUsage
:---: | :---:
0 | 10
1 | 30

Allo stesso tempo, le statistiche di due chiavi vengono generate dal framework di applicazione a singola PAGINA. Uno per la CPU 0 e l'altro per CPU 1.

Come nell'esempio seguente viene indicato più **valore** colonne con più **chiave** colonne è supportata.

CounterName | InstanceName | Media | Sum
--- | :---: | :---: | :---:
% tempo processore | Totale | 10 | 20
% tempo processore | CPU0 | 20 | 30 

In questo esempio sono presenti due **chiave** colonne e due **valore** colonne. SPA genera due chiavi di statistiche per la colonna Media e un altro due chiavi per la colonna Sum. Le chiavi di statistiche sono:

* CounterName (% tempo processore) / NomeIstanza (\_totale) / Media

* CounterName (% tempo processore) / NomeIstanza (CPU0) / Media

* CounterName (% tempo processore) / NomeIstanza (\_totale) / somma

* CounterName (% tempo processore) / NomeIstanza (CPU0) / somma

CounterName e InstanceName vengono combinati in una chiave. La chiave combinata non può avere qualsiasi la duplicazione.

SPA genera molte chiavi di statistiche. Alcune di esse potrebbero non essere interessanti ed eventualmente per nasconderle dall'interfaccia Utente. SPA consente agli sviluppatori di creare un filtro per visualizzare solo le chiavi di statistiche utili.

per l'esempio precedente, gli amministratori di sistema solo potrebbero risultare interessante chiavi in cui è InstanceName \_totali o CPU1. Il filtro può essere definito come segue:

``` syntax
<listValue name="CpuPerformance">
<column name="CounterName" type="string" columntype="Key"/>
<column name="InstanceName" type="string" columntype="Key">
 <trendableKeyValues>
<value>_Total</value>
<value>CPU1</value>
 </trendableKeyValues>
</column>
<column name="Average" type="decimal" columntype="Value"/>
<column name="Sum" type="decimal" columntype="Value"/>
</listValue>
```

**&lt;trendableKeyValues /&gt;**  possono essere definite in qualsiasi colonna di chiave. Se più di una colonna di chiave ha un filtro configurato e verrà applicata la logica.

### <a name="developing-report-scripts"></a>Sviluppo di script di report

Dopo aver definito i metadati di effettuare il provisioning, è possibile iniziare a scrivere lo script di report è una procedura T-SQL archiviate.

Esistono **nome** e **reportScript** attributi nell'intestazione di metadati del provisioning, come illustrato di seguito:

``` syntax
<advisorPack name="Microsoft.ServerPerformanceAdvisor.CoreOS.V1" reportScript="ReportScript"  
```

Lo script di report principale è denominato combinando il **nome** e **reportScript** attributi. Nell'esempio seguente, sarà \[Microsoft.ServerPerformanceAdvisor.CoreOS.V2\].\[ReportScript\].

``` syntax
create PROCEDURE [Microsoft.ServerPerformanceAdvisor.CoreOS.V2].[ReportScript] AS SET NOCOUNT ON

- Set alert and notification

- Prepare data for report view
```

Il **nome** attributo verrà utilizzato come un nome di schema di database, ad esempio uno spazio dei nomi. Questa regola si applica a tutte le altri oggetti di database che appartengono a pack advisor corrente, ad esempio il valore di elenco e le stored procedure.

I vantaggi con il nome dello schema prima gli oggetti di database sono:

* Come evitare conflitti di denominazione per Pack di advisor diversi

* Fornendo una maggiore protezione

Nel database di SQL Server, il nome di schema predefinito è **dbo**. Credenziali del proprietario del database sono in genere necessarie per svolgere gli oggetti di database in **dbo**. Se non si crea uno schema per ogni Service pack di advisor, è probabile che due pacchetti di advisor definirà un valore di elenco con lo stesso nome. Deve essere irrilevante perché è possibile inserire un nome di schema per risolvere questo problema. Inoltre, l'annullamento dell'implementazione di un pacchetto di advisor è molto più semplice. Poiché l'oggetto pack advisor appartiene a uno schema diverso da **dbo**, in questo modo il SISTEMA di utilizzare un privilegio inferiore per accedervi.

Uno script report normale effettua le seguenti operazioni:

* Accede ai dati raccolti non elaborati

* Consente di eseguire calcoli basati su dati non elaborati

* Gli avvisi di modifiche e suggerimenti

* Prepara i dati per la visualizzazione di report

### <a name="access-raw-collected-data"></a>Accedere ai dati raccolti non elaborati

Tutti i dati raccolti viene importato nelle seguenti tabelle corrispondenti. Per ulteriori informazioni sullo schema di tabella, vedere [che definisce il set di agenti di raccolta dati](#bkmk-definedatacollector).

* Registro di sistema

    * \#registryKeys

* WMI

    * \#WMIObjects

    * \#WmiObjectProperties

    * \#WmiQueries

* Contatore delle prestazioni

    * \#PerformanceCounters

* File

    * \#File

* ETW

    * \#Eventi

    * \#EventProperties

### <a name="set-rule-status"></a>Imposta lo stato regola

Il \[dbo\].\[SetNotification\] API imposta lo stato della regola, in modo da visualizzare un **successo** o **avviso** icona nell'interfaccia Utente.

* @ruleName nvarchar(50)

* @adviceName nvarchar(50)

I messaggi di avviso e indicazioni vengono archiviati nel file XML dei metadati di provisioning. In questo modo lo script di report più facile da gestire.

Inizialmente, lo stato di ogni regola è n/d. Questa API consente di impostare uno stato regola specificando il nome di un avviso. Il livello del nome dell'avviso da utilizzare come lo stato della regola.

È importante ricordare che è definita in precedenza la regola seguente:

``` syntax
<rule name="freediskSize" caption="Free Disk Size on System Drive" description="This rule checks free disk size on the system drive ">
<advice name="SuccessAdvice" level="Success" message="No issue found.">No recommendation.</advice>
<advice name="WarningAdvice" level="Warning" message="Not enough free space on system drive.">Install the operating system on a larger disk.</advice>
</rule>
```

Se lo spazio disponibile è inferiore a 2 GB, occorre impostiamo la regola il **avviso** livello. Lo script SQL sarà come segue:

``` syntax
if (@freediskSizeInGB < 2)
BEGIN
    exec dbo.SetNotification N'freediskSize', N'WarningAdvice'
END
ELSE
BEGIN
    exec dbo.SetNotification N'freediskSize', N'SuccessAdvice'
END 
```

### <a name="get-threshold-value"></a>Ottenere il valore di soglia

Il \[dbo\].\[GetThreshold\] API Ottiene le soglie:

* @key nvarchar(50)

* @value output float

> [!NOTE]
> È possibile farvi riferimento in tutte le regole le soglie sono coppie nome-valore. Gli amministratori di sistema possono utilizzare la console SPA per regolare le soglie.

 Continuando con l'esempio precedente, per una soglia, la definizione sarà come segue:

``` syntax
<thresholds>
  <threshold name="freediskSize" caption="Free Disk Size (GB)" description="Free Disk Size  value="10" />
</thresholds>
<rule name="freediskSize" caption="Free Disk Size on System Drive" description="This rule checks free disk size on system drive ">
<advice name="SuccessAdvice" level="Success" message="No issue found.">No recommendation.</advice>
<advice name="WarningAdvice" level="Warning" message="Not enough free space on the system drive.">
Install the operating system on a larger disk.</advice>
<dependencies>
 <threshold ref="freediskSize"/>
</dependencies>
</rule>
```

Lo script di report può essere modificato come illustrato di seguito:

``` syntax
DECLARE @freediskSize FLOat
exec dbo.GetThreshold N freediskSize , @freediskSize output

if (@freediskSizeInGB < @freediskSize)
 
```

### <a name="set-or-remove-the-single-value"></a>Per impostare o rimuovere il valore singolo

Il \[dbo\].\[SetSingleValue\] API imposta il valore singolo:

* @key nvarchar(50)

* @value sql\_variant

Questo valore può eseguire più volte la stessa chiave di valore singolo. L'ultimo valore viene salvato.

L'esempio seguente mostra che alcuni definiti valori singoli:

``` syntax
<singleValue section="Systemoverview" caption="Facts">
<value name="OsName" type="string" caption="Operating System" description="WMI: Win32_OperatingSystem/Caption"/>
<value name="Osversion" type="string" caption="OS version" description="WMI: Win32_OperatingSystem/version"/>
<value name="OsLocation" type="string" caption="OS Location" description="WMI: Win32_OperatingSystem/SystemDrive"/>
</singleValue>
```

È quindi possibile impostare il valore singolo come illustrato di seguito:

``` syntax
exec dbo.SetSingleValue N OsName ,  Windows 7 
exec dbo.SetSingleValue N Osversion ,  6.1.7601 
exec dbo.SetSingleValue N OsLocation ,  c:\ 
```

In rari casi, è possibile rimuovere il risultato impostato in precedenza usando il \[dbo\].\[ removeSingleValue\] API.

* @key nvarchar(50)

È possibile utilizzare lo script seguente per rimuovere configurate in precedenza valore.

``` syntax
exec dbo.removeSingleValue N Osversion 
```

### <a name="get-data-collection-information"></a>Ottenere informazioni sulla raccolta dati

Il \[dbo\].\[GetDuration\] API Ottiene l'utente designato durata in secondi per la raccolta dei dati:

* @duration output int

S ad esempio report qui lo script:

``` syntax
DECLARE @duration int
exec dbo.GetDuration @duration output
```

Il \[dbo\].\[GetInternal\] API Ottiene l'intervallo di un contatore delle prestazioni. Potrebbe restituire NULL se il report corrente non dispone di informazioni sui contatori delle prestazioni.

* @interval output int

S ad esempio report qui lo script:

``` syntax
DECLARE @interval int
exec dbo.GetInterval @interval output
```

### <a name="set-a-list-value-table"></a>Impostare una tabella di valori di elenco

È non disponibile alcuna API per l'aggiornamento delle tabelle di valore di elenco. Tuttavia, è possibile accedere direttamente le tabelle di valore di elenco. in fase di inizializzazione, una tabella temporanea corrispondente verrà creata per ogni valore di elenco.

L'esempio seguente mostra una tabella di valori di elenco:

``` syntax
<listValue name="NetworkAdapterInformation" section="NetworkIOFacts" caption="Physical Network Adapter Information">
<column name="NetworkAdapterId" type="string" caption="ID" description="WMI: Win32_NetworkAdapter/DeviceID"/>
<column name="NetworkAdapterName" type="string" caption="Name" description="WMI: Win32_NetworkAdapter/Name"/>
<column name="type" type="string" caption="type" description="WMI: Win32_NetworkAdapter/Adaptertype"/>
<column name="Speed" type="decimal" caption="Speed (Mbps)" description="WMI: Win32_NetworkAdapter/Speed"/>
<column name="MACaddress" type="string" caption="MAC address" description="WMI: Win32_NetworkAdapter/MACaddress"/>
</listValue>
```

È quindi possibile scrivere uno script SQL per inserire, aggiornare o eliminare i risultati:

``` syntax
INSERT INTO #NetworkAdapterInformation (
  NetworkAdapterId,
  NetworkAdapterName,
  type,
  Speed,
  MACaddress
)
VALUES (
   
)
```

## <a name="development-and-debugging"></a>Sviluppo e debug


### <a name="writing-logs"></a>Scrittura dei log

Se è presente qualsiasi informazione che si desidera comunicare agli amministratori di sistema, è possibile scrivere i log. Se non vi è alcun log di un report specifico, verrà visualizzato un banner giallo nell'intestazione del report. Nell'esempio seguente viene illustrato come scrivere un log:

``` syntax
exec dbo.WriteSystemLog N'Any information you want to show to the system administrators , N Warning 
```

Il primo parametro è il messaggio da visualizzare nel log. Il secondo parametro è il livello di registrazione. L'input valido per il secondo parametro potrebbe essere **informativo**, **avviso**, o **errore**.

### <a name="debug"></a>Debug

La console SPA può essere eseguita in due modalità, Debug o rilascio. Modalità di rilascio è il valore predefinito ed Elimina tutti i dati non elaborati raccolti dopo la generazione di report. La modalità di Debug mantiene tutti i dati non elaborati nella condivisione di file e database, in modo che è possibile eseguire il debug dello script di report in futuro.

**Eseguire il debug di uno script di report**

1.  Installare Microsoft SQL Server Management Studio (SSMS).

2.  Dopo l'avvio di SQL Server Management STUDIO, la connessione a localhost\\SQLExpress. Tenere presente che è necessario usare localhost, invece di. . In caso contrario, potrebbe non essere in grado di avviare il debugger in SQL Server.

3.  Eseguire lo script seguente per abilitare la modalità di Debug:

    ``` syntax
    USE SPADB
    UPdate dbo.Configurations
    SET Value = N'true'
    WHERE Name = N'Debugmode'
    ```

4.  Avviare la console SPA ed eseguono il pacchetto di advisor che si desidera eseguire il debug.

5.  Attendere il completamento dell'attività. Se è stato generato il report, tornare a SQL Server Management STUDIO e cercare l'ultima attività.

    ``` syntax
    select TOP 1 * FROM dbo.Tasks OrdER BY Id DESC
    ```

    Ad esempio, l'output potrebbe essere:

    Id | SessionId | AdvisoryPackageId | ReportStatusId | LastUpdatetime | ThresholdversionId
    :---: | :---: | :---: | :---: | :---: | :---:
    12 | 17 | 1 | 2 | 2011-05-11 05:35:24.387 | 1

6.  Le volte che si desidera eseguire lo script di report per Id 12, è possibile eseguire lo script seguente:

    ``` syntax
    exec dbo.DebugReportScript 12
    ```

    **Nota** è anche possibile premere F11 per eseguire l'istruzione precedente ed eseguire il debug.

     

Esecuzione di \[dbo\].\[DebugReportScript\] restituisce più set di risultati, tra cui:

1.  Preparazione pack registri e i messaggi di Microsoft SQL Server

2.  Risultati delle regole

3.  Statistiche chiavi e valori

4.  Valori singoli

5.  Tutte le tabelle di valore di elenco

## <a name="best-practices"></a>Procedure consigliate

### <a name="naming-convention-and-styles"></a>Convenzione di denominazione e stili

La convenzione Pascal maiuscole e minuscole | Convenzione camel | lettere maiuscole
--- | ---- | ---
<ul><li>Nomi di Provisionmetadata</li><li>Stored procedure</li><li>Funzioni</li><li>Nomi delle viste</li><li>Nomi delle tabelle temporanee</li></ul> | <ul><li>Nomi dei parametri</li><li>Variabili locali</li></ul> | Utilizzo per tutte le parole chiave riservate di SQL

### <a name="other-recommendations"></a>Altri suggerimenti

* Spostare le parti più logiche in altre stored procedure e funzioni definite dall'utente.

* Verificare lo script principale più breve possibile per scopi di manutenzione.

* Utilizzare il nome completo dell'oggetto SQL.

* Considerare il codice SQL distinzione tra maiuscole e minuscole.

* Aggiungere **SET NOCOUNT ON** all'inizio di ogni stored procedure.

* È consigliabile utilizzare le tabelle temporanee per trasferire grandi quantità di dati.

* Si consiglia di utilizzare **IMPOSTARE XACT\_ABORT ON** per terminare il processo se si verifica un errore.

* Includere sempre il numero di versione principale del nome visualizzato pack di advisor.

## <a href="" id="bkmk-advancedtopics"></a>Argomenti avanzati

### <a name="run-multiple-advisor-packs-simultaneously"></a>Eseguire contemporaneamente più Pack di advisor

SPA supporta l'esecuzione di più Pack advisor nello stesso momento. Ciò è particolarmente utile quando si desidera esaminare Internet Information Services (IIS) e le prestazioni del sistema operativo principale nello stesso momento. Molti agenti di raccolta dati che vengono utilizzati dal pacchetto di advisor IIS potrebbe essere utilizzati anche da pack preparazione del sistema operativo di base. Quando due o più pacchetti di advisor sono in esecuzione nello stesso computer di destinazione, SPA non raccogliere gli stessi dati due volte.

Nell'esempio seguente viene illustrato il flusso di lavoro per l'esecuzione di due pacchetti di advisor.

![esecuzione di più Pack di advisor](../media/server-performance-advisor/spa-dev-guide-multi-advisor-packs.png)

Insieme di raccolta dati unione è solo per la raccolta dei contatori delle prestazioni e le origini dati ETW. Si applicano le regole di unione seguenti:

1.  SPA accetta la maggiore durata come la nuova durata.

2.  In cui sono presenti conflitti di unione, vengono seguite le regole seguenti:

    1.  Accettare l'intervallo più piccolo come il nuovo intervallo.

    2.  Richiedere l'insieme completo dei contatori delle prestazioni. Ad esempio, con **processo (\*)\\% tempo processore** e **processo (\*)\\\*,\\processo (\*)\\ \***  restituisce più dati, pertanto **processo (\*)\\% tempo processore** e **processo (\*)\\ \***  viene rimosso dal set di agenti di raccolta dati uniti.

### <a name="collect-dynamic-data"></a>Raccogliere i dati dinamici

Esigenze SPA un agente di raccolta di dati definiti impostata in fase di progettazione. Non è sempre possibile sapere quali dati sono necessaria per la generazione di report, poiché i dati dinamici e il percorso della query non sono noti fino a quando i dati dipendenti sono disponibili.

ad esempio, se si desidera elencare tutti i nomi descrittivi delle schede di rete, è necessario eseguire una query WMI per enumerare tutte le schede di rete. Ogni oggetto restituito oggetto WMI ha un percorso della chiave del Registro di sistema, in cui archiviare il nome descrittivo. Il percorso della chiave del Registro di sistema non è noto in fase di progettazione. In questo caso, è necessario dati dinamici supportano.

Per enumerare tutte le schede di rete, è possibile utilizzare la seguente query WMI tramite Windows PowerShell:

``` syntax
Get-WmiObject -Namespace Root\Cimv2 -query "select PNPDeviceID FROM Win32_NetworkAdapter" | forEach-Object { Write-Output $_.PNPDeviceID }
```

Restituisce un elenco di oggetti scheda di rete. Ogni oggetto dispone di una proprietà denominata **ID di periferica PnP**, che gestisce il percorso di una chiave del Registro di sistema relativo. Ecco un esempio di output dalla query precedente s:

``` syntax
ROOT\*ISatAP\0001
PCI\VEN_8086&DEV_4238&SUBSYS_11118086&REV_35\4&372A6B86&0&00E4
ROOT\*IPHTTPS\0000
 
```

Per trovare le **FriendlyName** valore, aprire l'editor del Registro di sistema e passare all'impostazione del Registro di sistema combinando **HKEY\_locale\_macchina\\sistema\\ CurrentControlSet\\Enum\\**  con ogni riga nell'esempio precedente. Per esempio: **HKEY\_locale\_MACHINE\\SYSTEM\\CurrentControlSet\\Enum\\ radice\\\*IPHTTPS\\0000**.

Per convertire i passaggi precedenti in SPA provisioning metadati, aggiungere lo script nell'esempio di codice seguente:

``` syntax
<advisorPack>
<dataSourceDefinition xmlns="http://microsoft.com/schemas/ServerPerformanceAdvisor/dc/2010">
 <dataCollectorSet >
<registryKeys>
 ?<registryKey>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Enum\$(NetworkAdapter.PNPDeviceID)\FriendlyName</registryKey>
</registryKeys>
<managementpaths>
 ?<path name="NetworkAdapter">Root\Cimv2:select PNPDeviceID FROM Win32_NetworkAdapter</path>
</managementpaths>
```

In questo esempio, viene innanzitutto aggiungere una query WMI sotto managementpaths e definire il nome della chiave **NetworkAdapter**. È necessario aggiungere una chiave del Registro di sistema e fare riferimento a **NetworkAdapter** utilizzando la sintassi **$(NetworkAdapter.PNPDeviceID)**.

Nella tabella seguente definisce se un agente di raccolta dati in SPA supporta dati dinamici e se è possibile farvi riferimento da altri agenti di raccolta dati:

Tipo di dati | Supporto dei dati dinamici | È possibile fare riferimento
--- | :---: | :---:
Chiave del Registro di sistema | Yes | Yes
WMI | Yes | Yes
File | Yes | No
Contatore delle prestazioni | No | No
ETW | No | No

Per un agente di raccolta dati WMI, ogni oggetto WMI dispone di molti attributi collegati. Qualsiasi tipo di oggetto WMI dispone sempre di tre attributi: \_\_Spazio dei nomi \_ \_(classe), e \_ \_RELpath.

Per definire una raccolta di dati che fanno riferimento altri agenti di raccolta dati, assegnare il **nome** attributo con una chiave univoca di ProvisionMetadata.xml. Questa chiave è utilizzata da agenti di raccolta dati dipendenti per generare dati dinamici.

Qui s ad esempio per la chiave del Registro di sistema:

``` syntax
<registryKey  name="registry">HKEY_LOCAL_MACHINE </registryKey>
```

E un esempio di WMI:

``` syntax
<path name="wmi">Root\Cimv2:select PNPDeviceID FROM Win32_NetworkAdapter</path>
```

Per definire un agente di raccolta dati dipendenti, utilizzare la sintassi seguente: $(*{nome}*.*{attributo}*).

*{nome}* e *{attributo}* sono segnaposto.

Quando SPA raccoglie i dati da un server di destinazione, viene sostituito in modo dinamico il modello di $(\*.\*) con l'effettivo dei dati raccolti dal relativo agente di raccolta dati di riferimento (chiave del Registro di sistema / WMI), ad esempio:

``` syntax
<registryKey>HKEY_LOCAL_MACHINE\$(registry.key)\ </registryKey>
<registryKey  name="registry">HKEY_LOCAL_MACHINE\$(wmi.Relativeregistrypath)\ </registryKey>
<path name="wmi"> </path>
<file>$(wmi.FileName)</file>
```

**Nota** SPA supporta una profondità illimitata di riferimento, ma essere consapevoli dell'overhead delle prestazioni se si dispone di un numero eccessivo di livelli. Assicurarsi che sia presente alcun riferimento circolare o autoreferenziale che non è supportato.

### <a name="versioning-limitations"></a>limitazioni di controllo delle versioni

SPA supporta gli aggiornamenti di versione secondaria e reimpostata. Questi processi utilizzano lo stesso algoritmo. Il processo è di aggiornamento degli oggetti di database e delle impostazioni di soglia conservando i dati esistenti. Questo può essere l'aggiornamento a una versione successiva o il downgrade a una versione precedente. Selezionare il pacchetto di advisor e quindi fare clic su **reimpostare** nel **configurare Pack di Advisor** finestra di dialogo in SPA per reimpostare o applicare o gli aggiornamenti.

Questa funzionalità è soprattutto per gli aggiornamenti secondari. È possibile modificare in modo significativo gli elementi di visualizzazione dell'interfaccia Utente. Se si desidera apportare modifiche significative, è necessario creare un pacchetto diverso advisor. Il nome di Service pack di advisor, è necessario includere il numero di versione principale.

Le limitazioni di modifiche di versione secondaria sono che si **impossibilità** effettuare una delle operazioni seguenti:

* Modificare il nome dello schema

* Modificare il tipo di dati di un gruppo singolo valore o le colonne di una tabella di valori di elenco

* Aggiungere o rimuovere le soglie

* Aggiungere o rimuovere le regole

* Aggiungere o rimuovere consigli

* Aggiungere o rimuovere valori singoli

* Aggiungere o rimuovere valori elenco

* Aggiungere o rimuovere una colonna di valori di elenco

### <a href="" id="bkmk-tooltips"></a>Descrizioni comandi

Quasi tutti **Descrizione** attributi verranno visualizzati come descrizione comando nella console di SPA.

per una tabella di valori di elenco, è possibile ottenere una descrizione comando basata su riga aggiungendo l'attributo seguente:

``` syntax
<listValue descriptionColumn="Description">
<column name="Name"/>
<column name="Description"/>
</listValue>
```

Il **descriptionColumn** attributo fa riferimento al nome della colonna. In questo esempio, la colonna di descrizione non viene visualizzato come una colonna fisica. Tuttavia, Visualizza come descrizione comando quando si passa il mouse su ogni riga della prima colonna.

È consigliabile che la descrizione comando Visualizza l'origine dati all'utente. Di seguito sono i formati per visualizzare le origini dati:

Origine dati | Formato | Esempio
--- | --- | ---
WMI | WMI: &lt;wmiclass&gt;/&lt;campo&gt; | WMI: Win32_OperatingSystem/Caption
Contatore delle prestazioni | PerfCounter: &lt;CategoryName&gt;/&lt;NomeIstanza&gt; | PerfCounter: Tempo processore Process/%
Registro di sistema | registry: &lt;registerKey&gt; | Registro di sistema: HKLM\SOFTWARE\Microsoft<br>\\ASP.NET\\Rootver
File di configurazione | ConfigFile: &lt;FilePath&gt;\[; XPath: &lt;Xpath&gt;\]<br>**Nota**<br>XPath è facoltativo e è valido solo quando il file è un file xml. | ConfigFile: windir %\\System32\\inetsrv\config\\applicationHost. config<br>Xpath: configuration&frasl;system.webServer<br>&frasl;httpProtocol&frasl;@allowKeepAlive
ETW | ETW: &lt;Provider /&gt;(parole chiave) | ETW: Traccia Kernel Windows (net processo)

### <a name="table-collation"></a>Regole di confronto di tabella

Quando un pacchetto di advisor diventa più complesso, è possibile creare il proprio variabile tabelle o le tabelle temporanee per archiviare i risultati intermedi nello script di report.

Ordinamento di colonne di tipo stringa potrebbe essere problematica perché le regole di confronto di tabella creata potrebbe essere diverso da quello che viene creato dal framework di applicazione a singola PAGINA. Se correlare due colonne di stringhe in tabelle diverse, si verifichi un errore di regole di confronto. Per evitare questo problema, è consigliabile definire sempre la stringa per un confronto della colonna come **SQL\_Latin1\_Generale\_CP1\_CI\_AS** quando si definisce una tabella.

Qui s come definire una tabella di variabili:

``` syntax
DECLARE @filesIO TABLE (
 Name nvarchar(500) COLLatE SQL_Latin1_General_CP1_CI_AS,
 AverageFileAccessvolume float,
 AverageFileAccessCount float,
 Filepath nvarchar(500) COLLatE SQL_Latin1_General_CP1_CI_AS
)
```

### <a name="collect-etw"></a>Raccolta di dati ETW

Qui s come definire ETW in un file Provisionmetadata XML:

``` syntax
<dataSourceDefinition>
  <providers>
    <provider session="NT Kernel Logger" guid="{9E814AAD-3204-11D2-9A82-006008A86939}"/>
  </providers>
</dataSourceDefinition>
```

Gli attributi di provider seguenti sono disponibili per la raccolta di ETW:

Attributo | Tipo | Descrizione
--- | --- | ---
GUID | GUID | GUID del provider
sessione | string | Nome della sessione ETW (facoltativo, richiesto solo per gli eventi del kernel)
keywordsany | Hex | Tutte le parole chiave (facoltativo, senza il prefisso 0x)
keywordsAll | Hex | Tutte le parole chiave (facoltative)
proprietà | Hex | Proprietà (facoltativa)
level | Hex | Livello (facoltativo)
bufferSize | Int | Dimensione del buffer (facoltativo)
flushtime | Int | Scaricare ora (facoltativo)
maxBuffer | Int | Numero massimo di buffer (facoltativo)
minBuffer | Int | Minima del buffer (facoltativa)

Esistono due tabelle di output, come illustrato di seguito.

**\#Schema della tabella eventi**

Nome colonna | Tipo di dati SQL | Descrizione
--- | --- | ---
SequenceID | Int NON NULL | ID della sequenza di correlazione
EventtypeId | Int NON NULL | ID tipo di evento (fare riferimento a [dbo]. [ EventTypes])
ProcessId | BigInt NON NULL | ID del processo
ThreadId | BigInt NON NULL | ID del thread
timestamp | datetime2 non NULL | timestamp
Kerneltime | BigInt NON NULL | Tempo del kernel
Tempi | BigInt NON NULL | Tempo utente

**\#Tabella eventproperties**

Nome colonna | Tipo di dati SQL | Descrizione
--- | --- | ---
SequenceID | Int NON NULL | ID della sequenza di correlazione
Nome | Nvarchar(100) | Nome proprietà
Value | Nvarchar (4000) | Value

### <a name="etw-schema"></a>Schema ETW

Uno schema ETW può essere generato eseguendo il file con estensione etl tracerpt.exe. Viene generato un file man. Poiché il formato del file con estensione ETL è dipendente da computer, lo script seguente funziona solo nelle situazioni seguenti:

1.  Eseguire lo script nel computer in cui verrà raccolti il corrispondente file con estensione etl.

2.  In alternativa, eseguire lo script in un computer con lo stesso sistema operativo e i componenti installati.

``` syntax
tracerpt *.etl -export
```

## <a name="glossary"></a>Glossario


In questo documento vengono utilizzati i termini seguenti:

**Pacchetto di Advisor**

Un pacchetto di advisor è una raccolta di metadati e gli script SQL che elaborano i registri di prestazioni vengono raccolti dal server di destinazione. Il pacchetto di advisor genera quindi report dai dati di log delle prestazioni. I metadati nel pacchetto di advisor definiscono i dati da raccogliere da server di destinazione per le misurazioni delle prestazioni. I metadati definiscono inoltre il set di regole, le soglie e il formato del report. In genere, un pacchetto di advisor è scritto specificamente per un unico ruolo del server, ad esempio, Internet Information Services (IIS).

**Console SPA**

La console SPA fa riferimento a SpaConsole.exe, ovvero la parte centrale del Server Performance Advisor. SPA non è necessario eseguire nel server di destinazione che si sta verificando. La console SPA contiene tutte le interfacce utente per l'autenticazione SPA, dall'impostazione del progetto per l'esecuzione dell'analisi e visualizzazione di report. Per impostazione predefinita, il SISTEMA è un'applicazione a due livelli. La console SPA contiene il livello dell'interfaccia Utente e una parte del livello di logica di business. La console SPA pianifica ed elabora le richieste di analisi delle prestazioni.

**Framework SPA**

SPA contiene due parti principali, il framework e i pacchetti di advisor. Il framework SPA fornisce tutte le interfacce utente, elaborazione dei registri di prestazioni, configurazione, la gestione degli errori e procedure di gestione e API di database.

**Progetto SPA**

Un progetto di applicazione a singola PAGINA è un database che contiene tutte le informazioni su server di destinazione, Pack di advisor e prestazioni report di analisi che vengono generati nei server di destinazione per i pacchetti di advisor. È possibile confrontare e visualizzare i grafici cronologia e la tendenza nello stesso progetto di applicazione a singola PAGINA. L'utente può creare più di un progetto. I progetti SPA sono indipendenti una da altra e non sono presenti dati condivisi tra progetti.

**server di destinazione**

Il server di destinazione è il computer fisico o macchina virtuale che esegue Windows Server con determinati ruoli del server, ad esempio IIS.

**Sessione di analisi dei dati**

Una sessione di analisi di dati è un'analisi delle prestazioni in un server di destinazione specifico. Una sessione di analisi di dati può includere più Pack di advisor. I set di agenti di raccolta dati da tali Pack advisor vengono uniti in un insieme agenti di raccolta dati. Tutti i registri di prestazioni per una sessione di analisi dati vengono raccolti nello stesso periodo di tempo. Analisi dei report generati da Pack di advisor in esecuzione nella stessa sessione di analisi di dati consente agli utenti di comprendere la situazione complessiva delle prestazioni e identificare le cause principali problemi di prestazioni.

**Event Tracing for Windows**

[Event Tracing](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) per Windows (ETW) è un sistema di traccia ad alte prestazioni sovraccarica scalabile, che viene fornito nei sistemi operativi Windows. Fornisce funzionalità, che può essere utilizzata per risolvere una varietà di scenari di debug e profilatura. SPA utilizza gli eventi ETW come origine dati per la generazione di report di prestazioni. Per informazioni generali su ETW, vedere [migliora il debug e ottimizzazione delle prestazioni con ETW](https://msdn.microsoft.com/magazine/cc163437.aspx).

**Query WMI**

Strumentazione gestione Windows (WMI) è l'infrastruttura per i dati di gestione e operazioni nei sistemi operativi Windows. È possibile scrivere script WMI o applicazioni per automatizzare le attività amministrative sui computer remoti. WMI fornisce anche i dati di gestione ad altre parti del sistema operativo e ai prodotti. SPA utilizza i punti dati e informazioni sulle classi WMI come origini per la generazione di rapporti di prestazioni.

**Contatori delle prestazioni**

I contatori delle prestazioni vengono utilizzati per fornire informazioni sul funzionamento del sistema operativo o un'applicazione, servizio o driver è. I dati del contatore delle prestazioni consentono di determinare i colli di bottiglia e ottimizzare le prestazioni di sistema e dell'applicazione. Il sistema operativo, rete e dispositivi forniscono dati contatore che un'applicazione possono essere utilizzati per fornire agli utenti di visualizzare graficamente le prestazioni del sistema. SPA utilizza punti dati e informazioni sui contatori delle prestazioni come origini per generare report di prestazioni.

**Gli avvisi e registri di prestazioni**

AVVISI e registri di prestazioni è integrato nel sistema operativo Windows. È progettato per raccogliere tracce e registri di prestazioni e genera avvisi di prestazioni anche quando vengono soddisfatte determinate trigger. Erano messe a Disposizione consente di raccogliere i contatori delle prestazioni, eventi di traccia per Windows (ETW), query WMI, chiavi del Registro di sistema e configurazione di file. PIA supporta inoltre la raccolta dati remoti tramite chiamate di procedura remota (RPC). L'utente definisce un insieme agenti di raccolta dati, che include le informazioni sul dati da raccogliere, frequenza di raccolta dati, la durata di raccolta dati, filtri e un percorso per salvare il file dei risultati. SPA utilizza erano messe a Disposizione per raccogliere tutti i dati sulle prestazioni dal server di destinazione.

**Singolo report**

Singolo report è basato il report SPA generato per una sessione di analisi di dati per un Service pack di advisor su un singolo server di destinazione. Può contenere diverse sezioni di dati e notifiche.

**Report side-by-side**

Un report side-by-side è un report SPA che confronta due report singolo per lo stesso pack di advisor. I due report possono essere generati dal server di destinazione diversa o da esecuzioni dell'analisi delle prestazioni separato sullo stesso server di destinazione. Il report side-by-side consente di creare la funzionalità per confrontare due report per consentire agli utenti di identificare i comportamenti anomali o le impostazioni in uno dei report. Un report side-by-side contiene diverse sezioni di dati e notifiche. In ogni sezione dati da entrambi i report vengono elencati side-by-side.

**Grafico di tendenza**

Un grafico di tendenza è il report SPA viene utilizzato per analizzare modelli ricorrenti di problemi di prestazioni. Molti problemi di prestazioni ricorrenti sono causati da modifiche del carico server pianificato dal server o dal computer client, ciò può verificarsi giornaliera o settimanale. SPA fornisce un grafico di tendenza di 24 ore e un grafico di tendenza di 7 giorni di identificare tali problemi.

L'utente può scegliere una o più serie di dati alla volta, ovvero un valore numerico all'interno del singolo report, ad esempio **totale utilizzo medio della CPU**. più specificamente, un valore numerico è un valore scalare da un singolo server generato da un punto di accesso singolo in un'istanza di tempo specificato. SPA tali valori vengono raggruppati in 24 gruppi, uno per ogni ora del giorno (7 per un report di 7 giorni, uno per ogni giorno della settimana). SPA calcola una media, minimo, massimo e deviazioni standard per ogni gruppo.

**Grafico di andamento storico**

Un grafico di andamento storico è il report SPA viene utilizzato per visualizzare le modifiche in alcuni valori numerici all'interno di singolo report per un determinato server e una coppia pack advisor nel tempo. L'utente può scegliere di più serie di dati e visualizzarli insieme nel grafico cronologia per comprendere la correlazione tra le serie di dati diversi.

**Serie di dati**

Una serie di dati è numerico i dati raccolti dalla stessa origine dati in un periodo di tempo. La stessa origine significa che i dati deve provenire dallo stesso server di destinazione, ad esempio la lunghezza della coda Media delle richieste per IIS in un unico server.

**regole**

Le regole sono combinazioni di logica, le soglie e descrizioni. Rappresentano un potenziale problema di prestazioni. Ogni pacchetto di advisor contiene più regole. Ogni regola viene attivata da un processo di generazione di report. Una regola si applica la logica e le soglie per i dati nel report singola. Se vengono soddisfatti i criteri, viene generata una notifica di avviso. Se non è impostata la notifica di **OK** dello stato. Se non si applica la regola, la notifica viene impostata su non applicabile (**NA**) dello stato.

**Notifiche**

Le informazioni visualizzate agli utenti una regola è una notifica. Include lo stato della regola (**OK**, **NA**, o **avviso**), il nome della regola e indicazioni possibili per risolvere i problemi di prestazioni.
