---
title: Prestazioni di potenza e ottimizzazione
description: Risparmio energia dei processori (PPM) di ottimizzazione per il piano di risparmio di energia bilanciata di Windows Server
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Qizha;TristanB
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 91bc02e5edbdfbbbf3ccf600f3536a783e49eb79
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814912"
---
# <a name="power-and-performance-tuning"></a>Risparmio energia e ottimizzazione delle prestazioni

L'efficienza energetica è sempre più importante per gli ambienti di enterprise e data center e aggiunge un'altra serie di compromessi alla combinazione di opzioni di configurazione.

Windows Server 2016 è ottimizzato per l'efficienza energetica eccellenti con impatto minimo sulle prestazioni in un'ampia gamma di carichi di lavoro dei clienti. [Ottimizzazione di Processor Power Management (PPM) al piano di risparmio energia con bilanciamento del Server di Windows](processor-power-management-tuning.md) descrive i carichi di lavoro utilizzati per ottimizzare i parametri predefiniti in Windows Server 2016 e vengono forniti suggerimenti per se personalizzato.

In questa sezione consente di espandere i compromessi di livello di efficienza energetica per poter prendere decisioni informate, se è necessario modificare le impostazioni di risparmio energia predefinite nel server. Tuttavia, la maggior parte dei carichi di lavoro e hardware del server non dovrebbe richiedere amministratore ottimizzazione alimentazione durante l'esecuzione di Windows Server 2016.

## <a name="calculating-server-energy-efficiency"></a>Il calcolo del risparmio energetico server

Quando si ottimizza il server per il risparmio di energia, è necessario considerare anche le prestazioni. Ottimizzazione influisce sulle prestazioni e l'alimentazione, talvolta in quantità sproporzionata. Per ogni regolazione possibili, considerare gli obiettivi di prestazioni e budget power per determinare se il compromesso è accettabile.

È possibile calcolare rapporto di efficienza energetica del server per una metrica utile che incorpora le informazioni di potenza e le prestazioni. L'efficienza energetica è rapporto tra il lavoro svolto per il risparmio di energia medio durante un periodo di tempo specificato.

![formula di efficienza energetica](../../media/perftune-guide-power-formula.png)

Questa metrica è possibile utilizzare per impostare obiettivi pratici che rispettano il compromesso tra prestazioni e dell'alimentazione. Al contrario, un obiettivo del 10% di risparmio di energia nel data Center non riesce acquisire gli effetti corrispondenti sulle prestazioni e viceversa.

Analogamente, se si ottimizzazione il server per migliorare le prestazioni di % 5, e che causa il consumo di energia superiore al 10%, il risultato totale potrebbe o non può essere accettabile per gli obiettivi aziendali. La metrica di efficienza energetica consente per decisioni più informate rispetto a metriche di alimentazione o di prestazioni autonoma.

## <a name="measuring-system-energy-consumption"></a>Misurazione dei consumi di sistema

È necessario stabilire una misura di risparmio energia della linea di base prima ottimizzare il server per l'efficienza energetica.

Se il server ha il supporto necessario, è possibile utilizzare la potenza di controllo e del budget di funzionalità in Windows Server 2016 per visualizzare il consumo energetico a livello di sistema utilizzando Performance Monitor.

Un modo per determinare se il server dispone di supporto per la misurazione e budget consiste nell'esaminare i [catalogo di Windows Server](http://www.windowsservercatalog.com). Se il modello di server è idonea per la qualificazione Enhanced Power Management di nuovo nel programma di certificazione Hardware Windows, è garantito per supportare le funzionalità di misurazione e budget.

Un altro modo per verificare la presenza di supporto di controllo consiste nel cercare manualmente i contatori in Performance Monitor. Aprire Performance Monitor, selezionare **Aggiungi contatori**, quindi individuare il **misuratore di alimentazione** gruppo di contatori.

Se le istanze denominate di misuratori di alimentazione vengono visualizzati nella casella **le istanze dell'oggetto selezionato**, supporta la piattaforma di controllo. Il **Power** contatore che mostra power in watt viene visualizzato nel gruppo di contatori selezionati. La derivazione esatta del valore di dati power non è specificata. Ad esempio, è possibile un consumo energetico istantaneo o un disegno di energia medio in un certo intervallo di tempo.

Se la piattaforma del server non supporta la misurazione, è possibile utilizzare un dispositivo di controllo fisico connesso all'input power supply per misurare il consumo di disegno o l'energia risparmio energia del sistema.

Per stabilire una linea di base, è necessario misurare l'energia medio obbligatorio in vari punti di carico di sistema, dopo l'inattività fino al 100% (velocità effettiva massima) per generare una linea di carico. La figura seguente mostra le righe di caricamento per tre configurazioni di esempio:

![esempi di righe di caricamento](../../media/perftune-guide-sample-loadlines.png)

È possibile usare le righe di caricamento per valutare e confrontare le prestazioni e consumo energetico delle configurazioni in tutti i punti di carico. In questo particolare esempio, è facile vedere che cos'è la migliore configurazione. Tuttavia, può facilmente esistere scenari in cui una configurazione ottimale per carichi di lavoro e uno è adatta per carichi di lavoro leggeri.

È necessario comprendere i requisiti del carico di lavoro per scegliere una configurazione ottimale. Non dare per scontato che, quando si individua una configurazione valida, verrà sempre rimangano ottima. È necessario misurare sistema utilizzo del consumo di energia e a intervalli regolari e dopo le modifiche in carichi di lavoro, i livelli di carico di lavoro o hardware del server.

## <a name="diagnosing-energy-efficiency-issues"></a>Diagnosi dei problemi di efficienza energetica

**PowerCfg.exe** supporta un'opzione della riga di comando che è possibile usare per analizzare l'efficienza energetica di inattività del server. Quando si esegue PowerCfg.exe con il **/energy** opzione, lo strumento esegue un test di 60 secondi per rilevare i problemi di efficienza energetica potenziale. Lo strumento genera un semplice rapporto HTML nella directory corrente.

>[!Important]
> Per garantire un'analisi accurata, assicurarsi che tutte le app locali siano chiuse prima di eseguire **PowerCfg.exe**. 

Abbreviati tassi di segni di graduazione del timer, driver di supporto di risparmio energia mancanza e utilizzo eccessivo della CPU sono solo alcuni dei problemi funzionali che vengono rilevati mediante il **powercfg /energy** comando. Questo strumento fornisce un modo semplice per identificare e risolvere i problemi di gestione di alimentazione, causando potenzialmente risparmi significativi sui costi in un Data Center di grandi dimensioni.

Per altre informazioni sulle PowerCfg.exe, vedi [utilizzo di PowerCfg per valutare l'efficienza energetica del sistema](https://msdn.microsoft.com/windows/hardware/gg463250.aspx).

## <a name="using-power-plans-in-windows-server"></a>Utilizzo di energia in Windows Server

Windows Server 2016 ha tre combinazioni risparmio di energia predefinite progettate per soddisfare diversi set di esigenze aziendali. Questi piani forniscono un modo semplice per personalizzare un server per soddisfare gli obiettivi di alimentazione o prestazioni. Nella tabella seguente descrive i piani, sono elencati gli scenari comuni in cui usare ogni piano e offre alcuni dettagli di implementazione per ogni piano.

| **Piano** | **Descrizione** | **Scenari comuni applicabili** | **Fondamenti dell'implementazione** |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Bilanciamento (scelta consigliata) | Impostazione predefinita. Ha come destinazione l'efficienza energetica buona con impatto minimo sulle prestazioni. | Attività di elaborazione generali | Corrisponde a una capacità a richiesta. Le funzionalità di risparmio energia bilanciare il risparmio energia e le prestazioni. |
| Prestazioni elevate | Aumenta le prestazioni a discapito di consumo di energia elevata. Risparmio energia e limitazioni termiche, operativo spese e considerazioni sull'affidabilità si applicano. | Le app a bassa latenza e il codice di app che è sensibile alle modifiche delle prestazioni processore | Processori rimangono sempre lo stato delle prestazioni più alto (incluso "turbo? frequenze). Tutti i core sono unparked. Output termico potrebbe essere significativo. |
| Risparmio di energia | Limita le prestazioni per risparmiare energia e ridurre i costi operativi. Non è consigliabile senza eseguire test approfonditi per migliorare le prestazioni che è sufficiente. | Distribuzioni con budget di alimentazione limitate e vincoli termici | Frequenza del processore in percentuale del valore massimo (se supportata) per le coperture e Abilita altre funzionalità di risparmio energia. |


Questi piani di risparmio energia in Windows esistono per corrente alternata (CA) e corrente continua (CD) basate su sistemi, ma si presuppone che i server usano sempre una fonte di alimentazione CA.

Per altre informazioni sul risparmio di energia e le configurazioni di criteri di risparmio energia, vedere [configurazione dei criteri di risparmio energia e la distribuzione in Windows](https://msdn.microsoft.com/windows/hardware/gg463243.aspx).

>[!Note]
> Alcuni produttori di server hanno le proprie opzioni di risparmio energia disponibili tramite le impostazioni del BIOS. Se il sistema operativo non dispone di controllo del risparmio di energia, la modifica di risparmio di energia in Windows non influirà sulle prestazioni e alimentazione del sistema.

## <a name="tuning-processor-power-management-parameters"></a>Parametri di regolazione processor power management

Ogni piano di risparmio energia rappresenta una combinazione dei numerosi parametri di gestione power sottostante. I piani incorporati sono tre raccolte di impostazioni consigliate che coprono un'ampia gamma di scenari e i carichi di lavoro. Tuttavia, è possibile riconoscere questi piani non soddisferà le esigenze di ogni cliente.

Le sezioni seguenti descrivono come ottimizzare alcuni parametri di gestione specifico processore power per soddisfare gli obiettivi non interessati dalle tre combinazioni personalizzate. Se è necessario comprendere una gamma più ampia di parametri di risparmio energia, vedere [configurazione dei criteri di risparmio energia e la distribuzione in Windows](https://msdn.microsoft.com/windows/hardware/gg463243.aspx).

## <a name="processor-performance-boost-mode"></a>Modalità di incremento delle prestazioni processore

Tecnologie Intel Turbo Boost e AMD Turbo CORE sono funzionalità che consentono di processori da ottenere prestazioni aggiuntive quando si è molto utile (vale a dire, al sistema è elevata carica). Tuttavia, questa funzionalità aumenta consumo di energia di core CPU, in modo che Windows Server 2016 consente di configurare le tecnologie Turbo in base ai criteri di risparmio energia che è in uso e l'implementazione di processore specifica.

La modalità Turbo è abilitata per i piani di risparmio energia ad alte prestazioni su tutti i processori Intel e AMD ed è disabilitata per i piani di risparmio energia di risparmio di energia. Per i piani di risparmio di energia bilanciata nei sistemi che si basano su gestione frequenza tradizionali basate su P-sullo stato, la modalità Turbo è abilitata per impostazione predefinita solo se la piattaforma supporta il registro EPB.

>[!Note]
> Il registratore di cassa EPB è supportata solo in Intel Westmere e processori più avanti.

Per i processori Nehalem Intel e AMD, la modalità Turbo è disabilitata per impostazione predefinita su piattaforme basate su P-state. Tuttavia, se un sistema supporta la collaborazione processore prestazioni controllo (CPPC), che è una nuova modalità alternative di comunicazione delle prestazioni tra il sistema operativo e l'hardware (definito nella versione 5.0 ACPI), la modalità Turbo potrebbe essere attivata se il funzionamento di Windows sistema richiede in modo dinamico l'hardware per offrire i più alti livelli di prestazioni possibili.

Per abilitare o disabilitare la funzionalità Turbo Boost, il parametro di modalità di incremento delle prestazioni processore deve essere configurato dall'amministratore o le impostazioni predefinite dei parametri per il risparmio di energia scelto. Modalità di incremento delle prestazioni processore ha cinque valori consentiti, come illustrato nella tabella 5.

Per un controllo basato sugli P-stati, le scelte disponibili sono disabilitate, Enabled (la modalità Turbo è disponibile per l'hardware ogni volta che vengano richiesto nominale delle prestazioni), efficiente e (la modalità Turbo è disponibile solo se viene implementato il registro EPB).

Per un controllo basato su CPPC, le scelte disponibili sono disabilitate, attivate efficaci (Windows indica la quantità esatta di Turbo per fornire) e stile di Guida aggressivo (Windows chiede per "ottenere prestazioni ottimali? Per abilitare la modalità Turbo).

In Windows Server 2016, il valore predefinito per la modalità di aumento di priorità è 3.

| **Name** | **Comportamento basate su P-stato** | **Comportamento CPPC** |
|--------------------------|------------------------|-------------------|
| 0 (disabilitato) | Disabled | Disabled |
| 1 (abilitato) | Enabled | Efficiente abilitata |
| 2 (stile di Guida aggressivo) | Enabled | Aggressiva |
| 3 (efficiente abilitato) | Efficiente | Efficiente abilitata |
| 4 (efficiente aggressiva) | Efficiente | Aggressiva |

 
I comandi seguenti abilitano la modalità di aumento di priorità le prestazioni del processore nel risparmio di energia corrente (specifica il criterio utilizzando un alias GUID):

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor PERFBOOSTMODE 1
Powercfg -setactive scheme_current
```

>[!Important]  È necessario eseguire la **powercfg - setactive** comando per abilitare le nuove impostazioni. Non necessario il riavvio del server.

Per impostare questo valore per i piani di risparmio energia diverso dal piano selezionato, è possibile usare gli alias, ad esempio lo schema\_MAX (risparmio di energia), uno schema\_MIN (prestazioni elevate) e lo schema\_BILANCIATO (bilanciato) al posto di schema\_Corrente. Sostituire "dello schema corrente? nei comandi - setactive powercfg illustrati in precedenza con l'alias desiderato per abilitare il risparmio di energia.

Ad esempio, per modificare la modalità di aumento di priorità nel piano di risparmio di energia e rendere il risparmio di energia è il piano corrente, eseguire i comandi seguenti:

``` syntax
Powercfg -setacvalueindex scheme_max sub_processor PERFBOOSTMODE 1
Powercfg -setactive scheme_max
```

## <a name="minimum-and-maximum-processor-performance-state"></a>Lo stato delle prestazioni processore massimo e minimo

Processori cambiano tra gli stati prestazionali (P-state) molto rapidamente richiedono l'alimentazione corrispondenza, forniscono prestazioni laddove necessario e risparmiando energia quando possibile. Se il server ha requisiti specifici a prestazioni elevate o consumo di energia minimo, si potrebbe provare a configurare il **lo stato delle prestazioni del processore minima** parametro o **massimo le prestazioni del processore Stato** parametro.

I valori per il **lo stato delle prestazioni del processore minima** e **lo stato delle prestazioni processore massimo** i parametri sono espressi come percentuale della frequenza massima del processore, con un valore compreso nell'intervallo 0 – 100.

Se il server richiede una latenza estremamente bassa, invariante frequenza della CPU (ad esempio, per l'esecuzione di test ripetibili) o i livelli di prestazioni più elevati, è possibile evitare i processori di commutazione agli stati di prestazioni inferiore. Per un server, è possibile limitare lo stato delle prestazioni processore di almeno pari al 100% usando i comandi seguenti:

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor PROCTHROTTLEMIN 100
Powercfg -setactive scheme_current
```

Se il server richiede minore consumo di energia, si potrebbe voler limitare lo stato delle prestazioni del processore in percentuale del valore massimo. Ad esempio, è possibile limitare il processore al 75% della relativa frequenza massima usando i comandi seguenti:

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor PROCTHROTTLEMAX 75
Powercfg -setactive scheme_current
```

>[!Note]
> Limitando le prestazioni del processore in percentuale del valore massimo richiede il supporto del processore. Consultare la documentazione di processore per determinare la presenza di tale supporto o visualizzare il contatore di Performance Monitor **% della frequenza massima** nel **processore** gruppo per vedere se sono stati eventuali limiti di frequenza applicato.

## <a name="processor-performance-increase-and-decrease-of-thresholds-and-policies"></a>Le prestazioni del processore aumentata o ridotta di soglie e criteri

La velocità con cui uno stato prestazioni del processore aumenta o diminuisce è controllata dalle più parametri. I quattro parametri seguenti hanno l'impatto più visibile:

-   **Soglia di aumento delle prestazioni processore** definisce il valore di utilizzo precedente che aumenta lo stato delle prestazioni del processore. I valori maggiori rallentano la frequenza di aumento per lo stato delle prestazioni in risposta a una maggiore attività.

-   **Soglia di riduzione delle prestazioni processore** definisce il valore di utilizzo seguente questo modo verrà ridotto lo stato delle prestazioni del processore. I valori maggiori aumentano la velocità di diminuzione per lo stato delle prestazioni durante i periodi di inattività.

-   **Criteri di aumentare le prestazioni del processore e riduzione delle prestazioni processore** criteri determinano quali lo stato delle prestazioni deve essere impostato quando si verifica una modifica. "Singolo? Questo significa che sceglie lo stato successivo. "Rocket? indica lo stato delle prestazioni power massimo o minimo. "Ideale? prova a trovare un equilibrio tra potenza e le prestazioni.

Ad esempio, se il server richiede una latenza estremamente bassa pur volendo comunque possibile trarre vantaggio dal basso consumo durante i periodi di inattività, si potrebbe quicken l'aumento dello stato delle prestazioni per un aumento del carico e rallentare la diminuzione quando carico si arresta. I comandi seguenti impostano i criteri di aumento per "Rocket? per uno stato più veloce aumentare e diminuire l'impostazione "Single?. Le soglie di aumento e riduzione sono impostate rispettivamente su 10 e 8.

``` syntax
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFINCPOL 2
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFDECPOL 1
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFINCTHRESHOLD 10
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFDECTHRESHOLD 8
Powercfg.exe /setactive scheme_current
```

## <a name="processor-performance-core-parking-maximum-and-minimum-cores"></a>Processore a core prestazioni minimi e massimo di core di parcheggio

Parcheggio core è una funzionalità introdotta in Windows Server 2008 R2. Il motore di processore power management (PPM) e l'utilità di pianificazione interagiscono per adeguare dinamicamente il numero di core che è possibile eseguire i thread. Il motore PPM sceglie un numero minimo di core per i thread che verranno pianificati.

Core che sono parcheggiati in genere non dispongono di alcun thread pianificato e viene eliminato in stati di alimentazione molto bassa quando non elaborano gli interrupt, le DPC o eseguire altre operazioni rigorosamente creata. Core rimanenti sono responsabili per il resto del carico di lavoro. Parcheggio core può potenzialmente aumentare l'efficienza energetica durante minore utilizzo.

Per la maggior parte dei server, il comportamento di core-parcheggio predefinito offre un compromesso ottimale tra velocità effettiva e l'efficienza energetica. Nei processori in cui parcheggio core potrebbe non essere visualizzato come molti vantaggi dei carichi di lavoro generico può essere disabilitato per impostazione predefinita.

Se il server ha specifico core parking requisiti, è possibile controllare il numero di core disponibili di attesa usando il **prestazioni Core Parking massimo core del processore** parametro o **processore Prestazioni Core Parking numero minimo di core** parametro in Windows Server 2016.

Da sempre parcheggio core non è ottimale per uno scenario è quando sono presenti uno o più thread attivi affinità con un considerevole subset di CPU in un nodo NUMA (vale a dire, più di 1 CPU, ma minore l'intero set di CPU nel nodo). Quando l'algoritmo di parcheggio core preleva core da unpark (presupponendo che si verifica un aumento di intensità del carico di lavoro), i core all'interno del sottoinsieme ottimizzato attivo (o subset) a unpark potrebbe non sempre scegliere e pertanto può finisce unparking di core che non è in realtà utilizzata.

I valori per questi parametri sono percentuali nell'intervallo da 0 a 100. Il **prestazioni Core Parking massimo core del processore** parametro controlla la percentuale massima di core che possono essere unparked (disponibile per l'esecuzione di thread) in qualsiasi momento, mentre il **processore prestazioni Core Parking Numero minimo di core** parametro controlla la percentuale minima di core che possono essere unparked. Per disattivare il parcheggio core, impostare il **processore prestazioni Core Parking numero minimo di core** parametro fino al 100% usando i comandi seguenti:

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor CPMINCORES 100
Powercfg -setactive scheme_current
```

Per ridurre il numero di core pianificabili portandolo al 50% del numero massimo, impostare il **prestazioni Core Parking massimo core del processore** parametro fino a 50 come indicato di seguito:

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor CPMAXCORES 50
Powercfg -setactive scheme_current
```

## <a name="processor-performance-core-parking-utility-distribution"></a>Processore a core prestazioni distribuzione utilità di parcheggio

Utilità di distribuzione è un'ottimizzazione algoritmica in Windows Server 2016 che è progettato per migliorare l'efficienza energetica per alcuni carichi di lavoro. Tiene traccia di attività di CPU non spostabile (vale a dire le DPC, gli interrupt o thread rigorosamente affinità) e consente di prevedere le future attività di ogni processore basate sul presupposto che tutte le operazioni di Mobile possono essere distribuita ugualmente tra tutte le memorie centrali unparked.

Utilità di distribuzione è abilitata per impostazione predefinita per il piano di risparmio di energia bilanciata per alcuni processori. Riducendo le frequenze di CPU richieste dei carichi di lavoro che si trovano nello stato stazionario ragionevolmente può ridurre il consumo di energia del processore. Tuttavia, utilità di distribuzione non è necessariamente un'ottima scelta algoritmica per carichi di lavoro che sono soggetti a picchi di attività elevata o per i programmi in cui il carico di lavoro rapidamente e in modo casuale passa tra i processori.

Per questi carichi di lavoro, è consigliabile la disattivazione di utilità di distribuzione usando i comandi seguenti:

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor DISTRIBUTEUTIL 0
Powercfg -setactive scheme_current
```

## <a name="see-also"></a>Vedere anche
- [Considerazioni sulle prestazioni dell'Hardware di server](../index.md)
- [Considerazioni relative all'alimentazione Hardware server](../power.md)
- [Processore Power Management di ottimizzazione](processor-power-management-tuning.md)
- [Parametri bilanciato piano consigliato](recommended-balanced-plan-parameters.md)
