---
title: Ottimizzazione dell'alimentazione e delle prestazioni
description: Ottimizzazione del risparmio energia del processore (PPM) per il piano di risparmio energia bilanciato di Windows Server
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: qizha;tristanb
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 1457328a151c87d2d4cb41c4ee91b4759f4fb8e2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851994"
---
# <a name="power-and-performance-tuning"></a>Ottimizzazione dell'alimentazione e delle prestazioni

L'efficienza energetica è sempre più importante negli ambienti aziendali e data center e aggiunge un altro set di compromessi alla combinazione di opzioni di configurazione.

Windows Server 2016 è ottimizzato per ottimizzare l'efficienza energetica, con un effetto minimo sulle prestazioni in un'ampia gamma di carichi di lavoro dei clienti. [Ottimizzazione del risparmio energia del processore (ppm) per il piano di risparmio energia bilanciato di Windows Server](processor-power-management-tuning.md) descrive i carichi di lavoro usati per ottimizzare i parametri predefiniti in Windows Server 2016 e fornisce suggerimenti per le ottimizzazioni personalizzate.

Questa sezione espande i compromessi di efficienza energetica che consentono di prendere decisioni informate se è necessario modificare le impostazioni di risparmio energia predefinite nel server. Tuttavia, la maggior parte dell'hardware e dei carichi di lavoro del server non deve richiedere l'ottimizzazione dell'alimentazione dell'amministratore quando si esegue Windows Server 2016.

## <a name="calculating-server-energy-efficiency"></a>Calcolo dell'efficienza energetica del server

Quando si ottimizza il server per ottenere risparmi energetici, è necessario prendere in considerazione anche le prestazioni. L'ottimizzazione influiscono sulle prestazioni e sulla potenza, a volte in quantità sproporzionate. Per ogni possibile regolazione, prendere in considerazione gli obiettivi di risparmio energia e prestazioni per determinare se il compromesso è accettabile.

È possibile calcolare il rapporto di efficienza energetica del server per una metrica utile che incorpora informazioni sul risparmio di energia e sulle prestazioni. L'efficienza energetica è il rapporto tra le operazioni eseguite e la potenza media necessaria per un periodo di tempo specificato.

![Formula efficienza energetica](../../media/perftune-guide-power-formula.png)

È possibile usare questa metrica per impostare obiettivi pratici che rispettano il compromesso tra potenza e prestazioni. Al contrario, l'obiettivo del 10% di risparmi energetici nell'data center non riesce a catturare gli effetti corrispondenti sulle prestazioni e viceversa.

Analogamente, se si ottimizza il server per migliorare le prestazioni del 5% e si ottiene un consumo di energia superiore del 10%, il risultato totale potrebbe o non essere accettabile per gli obiettivi aziendali. La metrica relativa all'efficienza energetica consente di prendere decisioni più informate rispetto alle metriche di potenza o prestazioni.

## <a name="measuring-system-energy-consumption"></a>Misurazione del consumo di energia del sistema

Prima di ottimizzare il server per l'efficienza energetica, è necessario stabilire una misura di alimentazione di base.

Se il server dispone del supporto necessario, è possibile usare le funzionalità di misurazione dell'energia e di budget in Windows Server 2016 per visualizzare il consumo di energia a livello di sistema tramite Performance Monitor.

Un modo per determinare se il server dispone del supporto per la misurazione e il budget consiste nel rivedere il [Catalogo di Windows Server](https://www.windowsservercatalog.com). Se il modello server è idoneo per la nuova qualificazione avanzata di risparmio energia nel programma di certificazione hardware di Windows, è garantito il supporto della funzionalità di misurazione e budget.

Un altro modo per verificare il supporto di misurazione consiste nel cercare manualmente i contatori in Performance Monitor. Aprire Performance Monitor, selezionare **Aggiungi contatori**e quindi individuare il gruppo di contatori del contatore di **energia elettrica** .

Se le istanze denominate di contatori di energia vengono visualizzate nella casella **istanze dell'oggetto selezionato**, la piattaforma supporta la misurazione. Il contatore di **alimentazione** che mostra l'alimentazione in watt viene visualizzato nel gruppo di contatori selezionato. La derivazione esatta del valore dei dati di risparmio energia non è specificata. Ad esempio, potrebbe trattarsi di un Power-on istantaneo o di un progetto di alimentazione medio in un intervallo di tempo.

Se la piattaforma server non supporta la misurazione, è possibile usare un dispositivo di misurazione fisico connesso all'input dell'alimentatore per misurare l'alimentazione del sistema o il consumo di energia.

Per stabilire una linea di base, è necessario misurare la potenza media necessaria in diversi punti di carico del sistema, da inattività a 100% (velocità effettiva massima) per generare una riga di carico. Nella figura seguente sono illustrate le righe di caricamento per tre configurazioni di esempio:

![righe di carico di esempio](../../media/perftune-guide-sample-loadlines.png)

È possibile usare le linee di carico per valutare e confrontare le prestazioni e il consumo di energia delle configurazioni in tutti i punti di carico. In questo esempio specifico, è facile vedere qual è la configurazione migliore. Tuttavia, è possibile che si verifichino facilmente scenari in cui una configurazione funziona meglio per carichi di lavoro intensi e una soluzione ottimale per i carichi di lavoro leggeri.

È necessario comprendere accuratamente i requisiti del carico di lavoro per scegliere una configurazione ottimale. Non presupporre che, quando si individua una configurazione corretta, rimarrà sempre ottimale. È necessario misurare l'utilizzo del sistema e il consumo di energia a intervalli regolari e dopo le modifiche ai carichi di lavoro, ai livelli del carico di lavoro o all'hardware del server.

## <a name="diagnosing-energy-efficiency-issues"></a>Diagnosi dei problemi di efficienza energetica

**Powercfg. exe** supporta un'opzione della riga di comando che è possibile utilizzare per analizzare l'efficienza energetica inattiva del server. Quando si esegue PowerCfg. exe con l'opzione **/Energy** , lo strumento esegue un test di 60 secondi per rilevare potenziali problemi di efficienza energetica. Lo strumento genera un report HTML semplice nella directory corrente.

> [!Important]
> Per garantire un'analisi accurata, assicurarsi che tutte le app locali siano chiuse prima di eseguire **powercfg. exe**. 

Frequenza dei cicli di frequenza del timer abbreviata, driver che non dispongono del supporto per il risparmio di energia e un utilizzo eccessivo della CPU sono alcuni dei problemi di comportamento rilevati dal comando **powercfg/Energy** . Questo strumento fornisce un modo semplice per identificare e correggere i problemi di risparmio energia, causando potenzialmente risparmi significativi in un Data Center di grandi dimensioni.

Per ulteriori informazioni su PowerCfg. exe, vedere [utilizzo di Powercfg per valutare l'efficienza energetica del sistema](https://msdn.microsoft.com/windows/hardware/gg463250.aspx).

## <a name="using-power-plans-in-windows-server"></a>Uso di combinazioni per il risparmio di energia in Windows Server

Windows Server 2016 prevede tre combinazioni per il risparmio di energia predefinite progettate per soddisfare diversi insiemi di esigenze aziendali. Questi piani forniscono un modo semplice per personalizzare un server per soddisfare gli obiettivi di potenza o prestazioni. La tabella seguente descrive i piani, elenca gli scenari comuni in cui usare ogni piano e fornisce alcuni dettagli di implementazione per ogni piano.

| **Pianificazione** | **Descrizione** | **Scenari comuni applicabili** | **Caratteristiche principali dell'implementazione** |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Bilanciato (scelta consigliata) | Impostazione predefinita. Ha come obiettivo un'efficienza energetica efficace con un effetto minimo sulle prestazioni. | Calcolo generale | Corrisponde alla capacità da richiedere. Le funzionalità di risparmio energia ribilanciano la potenza e le prestazioni. |
| Prestazioni elevate | Aumenta le prestazioni a costo di un consumo di energia elevato. Si applicano le limitazioni di energia elettrica e termica, le spese operative e l'affidabilità. | App a bassa latenza e codice dell'app sensibili alle modifiche delle prestazioni del processore | I processori sono sempre bloccati con lo stato delle prestazioni più elevato (incluse le frequenze "Turbo"). Tutti i core sono non parcheggiati. L'output termico può essere significativo. |
| Risparmio energia | Limita le prestazioni per risparmiare energia e ridurre i costi operativi. Non consigliato senza test completi per assicurarsi che le prestazioni siano adeguate. | Distribuzioni con allocazione di energia elettrica limitata e vincoli termici | Frequenza del processore di caps a una percentuale del valore massimo (se supportato) e Abilita altre funzionalità di risparmio energetico. |


Queste combinazioni per il risparmio di energia sono disponibili in Windows per sistemi ad alte correnti (CA) e correnti diretti (DC), ma si presuppone che i server utilizzino sempre una fonte di alimentazione AC.

Per altre informazioni sulle combinazioni per il risparmio di energia e le configurazioni dei criteri di alimentazione, vedere [configurazione e distribuzione dei criteri di risparmio energia in Windows](https://msdn.microsoft.com/windows/hardware/gg463243.aspx).

> [!Note]
> Alcuni produttori di server hanno le proprie opzioni di risparmio energia disponibili tramite le impostazioni del BIOS. Se il sistema operativo non ha il controllo del risparmio energia, la modifica delle combinazioni per il risparmio di energia in Windows non influirà sulle prestazioni e sull'alimentazione del sistema.

## <a name="tuning-processor-power-management-parameters"></a>Ottimizzazione dei parametri di risparmio energia del processore

Ogni combinazione per il risparmio di energia rappresenta una combinazione di numerosi parametri di risparmio energia sottostanti. I piani predefiniti sono tre raccolte di impostazioni consigliate che coprono un'ampia gamma di carichi di lavoro e scenari. Tuttavia, si riconosce che questi piani non soddisfano le esigenze di ogni cliente.

Le sezioni seguenti descrivono i modi per ottimizzare alcuni parametri specifici di risparmio energia del processore per soddisfare gli obiettivi non risolti dai tre piani predefiniti. Se è necessario comprendere una matrice più ampia di parametri di alimentazione, vedere [configurazione e distribuzione dei criteri di risparmio energia in Windows](https://msdn.microsoft.com/windows/hardware/gg463243.aspx).

## <a name="processor-performance-boost-mode"></a>Modalità di aumento delle prestazioni del processore

Le tecnologie Intel Turbo Boost e AMD Turbo CORE sono funzionalità che consentono ai processori di ottenere prestazioni aggiuntive quando sono particolarmente utili, ovvero a carichi di sistema elevati. Questa funzionalità, tuttavia, aumenta il consumo di energia di base della CPU, quindi Windows Server 2016 configura le tecnologie Turbo in base ai criteri di risparmio energia in uso e all'implementazione specifica del processore.

Turbo è abilitato per le combinazioni per il risparmio di energia a prestazioni elevate su tutti i processori Intel e AMD ed è disabilitato per le combinazioni per il risparmio di energia. Per le combinazioni di energia bilanciate sui sistemi che si basano sulla gestione tradizionale della frequenza basata sullo stato P, Turbo è abilitato per impostazione predefinita solo se la piattaforma supporta il registro EPB.

> [!Note]
> Il registro EPB è supportato solo nei processori Intel Westmere e versioni successive.

Per i processori Intel Nehalem e AMD, Turbo è disabilitato per impostazione predefinita nelle piattaforme basate sullo stato P. Tuttavia, se un sistema supporta il controllo delle prestazioni del processore di collaborazione (CPPC), che è una nuova modalità alternativa di comunicazione delle prestazioni tra il sistema operativo e l'hardware (definito in ACPI 5,0), è possibile che il sistema operativo Windows richieda in modo dinamico l'hardware per offrire i massimi livelli di prestazioni possibili.

Per abilitare o disabilitare la funzionalità Turbo Boost, il parametro della modalità di boosting delle prestazioni del processore deve essere configurato dall'amministratore o dalle impostazioni predefinite dei parametri per la combinazione per il risparmio di energia scelta. La modalità di potenziamento delle prestazioni del processore ha cinque valori consentiti, come illustrato nella tabella 5.

Per il controllo basato sullo stato P, le scelte sono disabilitate, abilitate (Turbo è disponibile per l'hardware ogni volta che è richiesta una prestazione nominale) ed efficiente (Turbo è disponibile solo se il registro EPB è implementato).

Per il controllo basato su CPPC, le opzioni sono disabilitate, efficienza abilitata (Windows specifica la quantità esatta di Turbo da fornire) e aggressiva (Windows chiede "prestazioni massime" per abilitare turbo).

In Windows Server 2016, il valore predefinito per la modalità Boost è 3.

| **Nome** | **Comportamento basato sullo stato P** | **Comportamento di CPPC** |
|--------------------------|------------------------|-------------------|
| 0 (disabilitato) | Disabled | Disabled |
| 1 (abilitato) | Abilitato | Efficienza abilitata |
| 2 (aggressivo) | Abilitato | Aggressive |
| 3 (efficienza abilitata) | Efficiente | Efficienza abilitata |
| 4 (efficace aggressivo) | Efficiente | Aggressive |

 
I comandi seguenti abilitano la modalità di potenziamento delle prestazioni del processore nella combinazione per il risparmio di energia corrente (specificare il criterio usando un alias GUID):

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor PERFBOOSTMODE 1
Powercfg -setactive scheme_current
```

> [!Important]
> Per abilitare le nuove impostazioni, è necessario eseguire il comando **powercfg-seactive** . Non è necessario riavviare il server.

Per impostare questo valore per le combinazioni per il risparmio di energia diverso dal piano attualmente selezionato, è possibile usare alias come lo schema\_MAX (risparmio energia), lo schema\_MIN (prestazioni elevate) e lo schema\_bilanciato (bilanciato) al posto dello schema\_corrente. Sostituire "schema Current" nei comandi powercfg-seactive indicati in precedenza con l'alias desiderato per abilitare la combinazione per il risparmio di energia.

Per modificare, ad esempio, la modalità Boost nel piano di risparmio energia e fare in modo che Power Saver sia il piano corrente, eseguire i comandi seguenti:

``` syntax
Powercfg -setacvalueindex scheme_max sub_processor PERFBOOSTMODE 1
Powercfg -setactive scheme_max
```

## <a name="minimum-and-maximum-processor-performance-state"></a>Stato prestazioni minimo e massimo processore

I processori cambiano molto rapidamente tra gli Stati delle prestazioni (P-Stati) per trovare una corrispondenza tra la fornitura e la richiesta, fornendo prestazioni laddove necessario e risparmiando energia quando possibile. Se il server dispone di requisiti specifici ad alte prestazioni o a consumo di energia minima, è possibile prendere in considerazione la configurazione del parametro di **stato delle prestazioni del processore minimo** o del parametro di **stato delle prestazioni massimo del processore** .

I valori per lo **stato delle prestazioni del processore minimo** e i parametri **massimi dello stato delle prestazioni** del processore sono espressi come percentuale della frequenza massima del processore, con un valore compreso nell'intervallo da 0 a 100.

Se il server richiede una latenza ultra-bassa, frequenza CPU invariante, ad esempio per i test ripetibili, o i livelli di prestazioni più elevati, è possibile che non si desideri che i processori cambino in Stati con prestazioni inferiori. Per un server di questo tipo, è possibile limitare lo stato delle prestazioni del processore minimo al 100% usando i comandi seguenti:

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor PROCTHROTTLEMIN 100
Powercfg -setactive scheme_current
```

Se il server richiede un consumo di energia inferiore, potrebbe essere necessario limitare lo stato delle prestazioni del processore a una percentuale del valore massimo. Ad esempio, è possibile limitare il processore al 75% della relativa frequenza massima usando i comandi seguenti:

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor PROCTHROTTLEMAX 75
Powercfg -setactive scheme_current
```

> [!Note]
> La limitazione delle prestazioni del processore a una percentuale del valore massimo richiede il supporto del processore. Controllare la documentazione del processore per determinare se tale supporto esiste o visualizzare il contatore delle prestazioni **del contatore% di frequenza massima** del gruppo di **processori** per verificare se sono stati applicati i limiti di frequenza.

## <a name="processor-performance-increase-and-decrease-of-thresholds-and-policies"></a>Aumento delle prestazioni del processore e riduzione delle soglie e dei criteri

La velocità con cui lo stato delle prestazioni di un processore aumenta o diminuisce è controllato da più parametri. I quattro parametri seguenti hanno un effetto più evidente:

-   **Soglia di aumento delle prestazioni del processore** definisce il valore di utilizzo oltre il quale aumenterà lo stato delle prestazioni di un processore. I valori più grandi rallentano la frequenza di aumento dello stato delle prestazioni in risposta a attività più elevate.

-   **Soglia di riduzione delle prestazioni del processore** definisce il valore di utilizzo al di sotto del quale diminuirà lo stato delle prestazioni di un processore. I valori più grandi aumentano la velocità di riduzione per lo stato delle prestazioni durante i periodi di inattività.

-   **Aumento delle prestazioni dei criteri e riduzione delle prestazioni del processore** I criteri determinano quale stato delle prestazioni deve essere impostato quando si verifica una modifica. Il criterio "singolo" indica che sceglie lo stato successivo. "Rocket" indica lo stato delle prestazioni di alimentazione massimo o minimo. "Ideal" prova a trovare un equilibrio tra potenza e prestazioni.

Se, ad esempio, il server richiede una latenza ultra bassa e si desidera comunque trarre vantaggio dalla bassa potenza durante i periodi di inattività, è possibile velocizzare l'aumento dello stato delle prestazioni per qualsiasi aumento del carico e rallentare il calo quando il carico diventa inattivo. I comandi seguenti impostano i criteri di aumento su "Rocket" per un aumento più rapido dello stato e impostano i criteri di riduzione su "single". Le soglie di aumento e riduzione sono impostate rispettivamente su 10 e 8.

``` syntax
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFINCPOL 2
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFDECPOL 1
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFINCTHRESHOLD 10
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFDECTHRESHOLD 8
Powercfg.exe /setactive scheme_current
```

## <a name="processor-performance-core-parking-maximum-and-minimum-cores"></a>Dimensioni massime e minime Core di parcheggio per le prestazioni del processore

Il parcheggio di base è una funzionalità introdotta in Windows Server 2008 R2. Il motore di risparmio energia del processore (PPM) e l'utilità di pianificazione interagiscono per modificare dinamicamente il numero di core disponibili per l'esecuzione dei thread. Il motore PPM sceglie un numero minimo di core per i thread che verranno pianificati.

I core che sono parcheggiati in genere non hanno thread pianificati e verranno rilasciati in Stati di alimentazione molto bassi quando non elaborano gli interrupt, DPC o altri rigorosamente creata un'affinità lavoro. I core rimanenti sono responsabili del resto del carico di lavoro. Il parcheggio di base può potenzialmente aumentare l'efficienza energetica in caso di utilizzo ridotto.

Per la maggior parte dei server, il comportamento predefinito per il parcheggio di base offre un ragionevole equilibrio tra velocità effettiva e efficienza energetica. Nei processori in cui il parcheggio di base potrebbe non essere molto vantaggioso per i carichi di lavoro generici, può essere disabilitato per impostazione predefinita.

Se il server dispone di requisiti specifici per il parcheggio di base, è possibile controllare il numero di core disponibili per il parcheggio usando il parametro core per le **prestazioni del processore** per il numero massimo di core, o il parametro core per le prestazioni del processore Core di **parcheggio** in Windows Server 2016.

Uno scenario in cui il parcheggio principale non è sempre ottimale per è quando uno o più thread attivi creata un'affinità a un subset non semplice di CPU in un nodo NUMA, ovvero più di una CPU, ma minore dell'intero set di CPU nel nodo. Quando l'algoritmo di parcheggio principale sta raccogliendo core da unpark (presupponendo che si verifichi un aumento dell'intensità del carico di lavoro), è possibile che non vengano sempre scelti i core all'interno del sottoinsieme di creata un'affinità attivo (o subset) per eseguire il unparking e, di conseguenza, i core che non verranno effettivamente utilizzati.

I valori per questi parametri sono percentuali nell'intervallo compreso tra 0 e 100. Il parametro del numero massimo di core per il **parcheggio delle prestazioni del processore** controlla la percentuale massima di core che possono essere annullati (disponibile per l'esecuzione di thread) in qualsiasi momento, mentre il parametro di core minimo per le **prestazioni del processore** controlla la percentuale minima di core che possono essere non parcheggiati. Per disattivare il parcheggio dei componenti di base, impostare il parametro di core minimo per le **prestazioni del processore** al 100% usando i comandi seguenti:

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor CPMINCORES 100
Powercfg -setactive scheme_current
```

Per ridurre il numero di core pianificabili al 50% del conteggio massimo, impostare il parametro massimo di core per il parcheggio per le **prestazioni del processore** su 50 come indicato di seguito:

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor CPMAXCORES 50
Powercfg -setactive scheme_current
```

## <a name="processor-performance-core-parking-utility-distribution"></a>Distribuzione dell'utilità di base per le prestazioni del processore

La distribuzione dell'utilità è un'ottimizzazione algoritmica di Windows Server 2016 progettata per migliorare l'efficienza energetica per alcuni carichi di lavoro. Consente di tenere traccia dell'attività della CPU inamovibile (ovvero DPC, interrupt o thread strettamente creata un'affinità) e di prevedere il lavoro futuro su ogni processore, in base al presupposto che qualsiasi lavoro movibile possa essere distribuito equamente tra tutti i core non parcheggiati.

La distribuzione dell'utilità è abilitata per impostazione predefinita per la combinazione per il risparmio di energia per alcuni processori. Può ridurre il consumo di energia del processore abbassando le frequenze di CPU richieste dei carichi di lavoro in uno stato ragionevolmente stabile. Tuttavia, la distribuzione dell'utilità non è necessariamente una buona scelta algoritmica per i carichi di lavoro soggetti a picchi di attività elevati o per i programmi in cui il carico di lavoro passa in modo rapido e casuale tra i processori.

Per questi carichi di lavoro, è consigliabile disabilitare la distribuzione dell'utilità usando i comandi seguenti:

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor DISTRIBUTEUTIL 0
Powercfg -setactive scheme_current
```

## <a name="see-also"></a>Vedi anche
- [Considerazioni sulle prestazioni dell'hardware del server](../index.md)
- [Considerazioni sull'alimentazione dell'hardware del server](../power.md)
- [Ottimizzazione di Risparmio energia del processore](processor-power-management-tuning.md)
- [Parametri della combinazione per il risparmio di energia Bilanciato](recommended-balanced-plan-parameters.md)
