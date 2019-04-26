---
title: Risparmio energia dei processori (PPM) di ottimizzazione per il piano di risparmio di energia bilanciata di Windows Server
description: Risparmio energia dei processori (PPM) di ottimizzazione per il piano di risparmio di energia bilanciata di Windows Server
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Qizha;TristanB
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 8a2ef4fd39554446aaac686e142ad24f53b4efaa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863052"
---
# <a name="processor-power-management-ppm-tuning-for-the-windows-server-balanced-power-plan"></a>Risparmio energia dei processori (PPM) di ottimizzazione per il piano di risparmio di energia bilanciata di Windows Server

A partire da Windows Server 2008, Windows Server sono disponibili tre combinazioni risparmio di energia: **Bilanciato**, **prestazioni elevate**, e **Power Saver**. Il **bilanciato** risparmio di energia è la scelta predefinita che mira a offrire la migliore efficienza energetica per una serie di carichi di lavoro server tipico. In questo argomento descrive i carichi di lavoro che sono stati utilizzati per determinare le impostazioni predefinite per il **bilanciato** schema per le diverse versioni precedenti di Windows.

Se si esegue un sistema server che include caratteristiche del carico di lavoro notevolmente differente o le prestazioni e requisiti di risparmio energia rispetto a questi carichi di lavoro, si potrebbe voler provare a ottimizzare le impostazioni di risparmio energia predefinite (ad esempio, creare un piano di risparmio di energia personalizzata). Un'unica fonte di informazioni utili di ottimizzazione è la [considerazioni relative all'alimentazione Hardware Server](../power.md). In alternativa, è possibile decidere che la **ad alte prestazioni** risparmio di energia è la scelta giusta per l'ambiente, riconoscendo che probabilmente si avrà una significativa energia raggiunto in cambio un certo livello di aumento della velocità di risposta.

>[!Important]
> È consigliabile usare i criteri di risparmio energia che sono inclusi in Windows Server a meno che non si dispone di un'esigenza specifica per crearne uno personalizzato e avere una conoscenza molto buona che i risultati variano a seconda delle caratteristiche del carico di lavoro.

## <a name="windows-processor-power-tuning-methodology"></a>Metodologia di ottimizzazione di Windows processore Power


### <a name="tested-workloads"></a>Carichi di lavoro testati

I carichi di lavoro sono selezionati per coprire un set di sforzo di tipiche"? Carichi di lavoro di Windows Server. Ovviamente questo set non intende essere rappresentativi dell'intera gamma di ambienti server reale.

L'ottimizzazione in ogni criterio di risparmio energia è data dai carichi di cinque lavoro seguenti:

-   **Carico di lavoro di Server Web IIS**

    Un benchmark interno Microsoft denominato nozioni fondamentali su Web consente di ottimizzare l'efficienza energetica di piattaforme che esegue Server Web IIS. Il programma di installazione contiene un server web e più client che simulano il traffico di accesso web. La distribuzione di dinamici e statici a caldo (in memoria) e a freddo statico (accesso al disco necessaria) pagine web è basata su statistici studi di server di produzione. Per eseguire il push di core CPU del server completo utilizzo (un'estremità dello spettro testato), il programma di installazione deve in modo abbastanza rapido risorse di rete e disco.

-   **Carico di lavoro di Database di SQL Server**

    Il [TPC-E](http://www.tpc.org/tpce/default.asp) benchmark è un benchmark più diffusi per l'analisi delle prestazioni di database. Viene utilizzato per generare un carico di lavoro OLTP per le ottimizzazioni di ottimizzazione PPM. Questo carico di lavoro è i/o disco significativa e pertanto dispone di un requisito ad alte prestazioni per le dimensioni di memoria e sistema di archiviazione.

-   **Carico di lavoro di file Server**

    Chiamata di un benchmark di Microsoft ha sviluppato [FSCT](http://www.snia.org/sites/default/files2/sdc_archives/2009_presentations/tuesday/BartoszNyczkowski-JianYan_FileServerCapacityTool.pdf) viene usato per generare un carico di lavoro server file SMB. Crea un file di grandi dimensioni impostate sul server e Usa molti sistemi client (effettivi o virtualizzati) per generare file apertura, chiusura, leggere e operazioni di scrittura. La combinazione di operazione si basa su statistici studi di server di produzione. Un aumento delle dimensioni della CPU, disco e le risorse di rete.

-   **SPECpower: carico di lavoro JAVA**

    [SPECpower\_ssj2008](http://spec.org/power_ssj2008/) è il primo benchmark SPEC standard del settore che congiuntamente restituisce le caratteristiche di potenza e le prestazioni. Si tratta di un carico di lavoro Java lato server con diversi livelli di carico della CPU. Non richiede molte risorse di rete o del disco, ma siano soddisfatti i requisiti per la larghezza di banda di memoria. Quasi tutte le attività della CPU viene eseguita in modalità utente. attività in modalità kernel non abbiano molto impatto su risparmio energia dei benchmark e caratteristiche di prestazioni, ad eccezione di decisioni sulla gestione dell'alimentazione.

-   **Carico di lavoro di applicazioni Server**

    Il [SAP-SD](http://global.sap.com/campaigns/benchmark/index.epx) benchmark viene usato per generare un carico di lavoro dell'applicazione server. Viene usato un programma di installazione a due livelli, con il database e il server applicazioni nello stesso host server. Questo carico di lavoro usa anche il tempo di risposta come una misurazione delle prestazioni, che differisce da altri carichi di lavoro testati. In questo modo viene utilizzato per verificare l'impatto dei parametri PPM sulla velocità di risposta. Tuttavia, non destinato a essere rappresentativo di tutti i carichi di lavoro di produzione sensibili alla latenza.

Tutti i benchmark tranne SPECpower sono stati originariamente progettati per l'analisi delle prestazioni e di conseguenza sono stati creati per l'esecuzione a livelli di carico di picco. Tuttavia, i livelli di medie e carico ridotto sono più comune per i server di produzione reali e sono più interessante per **bilanciato** prevede ottimizzazioni. Intenzionalmente eseguiamo il benchmark con diversi livelli di carico dal 100%, fino a 10% (nei passaggi del 10%) utilizzando diversi metodi di limitazione (ad esempio, riducendo il numero di utenti attivi/client).

### <a name="hardware-configurations"></a>Configurazioni hardware

Per ogni versione di Windows, server di produzione più recente vengono usati nel processo di analisi e ottimizzazione piano di risparmio energia. In alcuni casi, i test sono stati eseguiti in sistemi di pre-produzione corrispondenza cui pianificazione del rilascio della prossima versione di Windows.

Dato che la maggior parte dei server vengono vendute con i socket di processore da 1 a 4 e poiché il server di scalabilità verticale sono meno soggette a efficienza energetica come un problema primario, i test di ottimizzazione dell'alimentazione piano vengono eseguiti principalmente su sistemi a 2 e 4 socket. La quantità di RAM, disco e le risorse di rete per ogni test vengono scelti per consentire ogni sistema per l'esecuzione fino relativo piena capacità, mentre tenendo in considerazione le restrizioni di costi che sarebbero normalmente in atto per ambienti di server nel mondo reale, ad esempio mantenendo il configurazioni ragionevole.

>[!Important]
> Anche se il sistema può eseguire al relativo carico di picco, sarà in genere ottimizzato per i livelli di carico inferiore, poiché i server eseguiti in modo coerente con i livelli di carico di picco sarebbe particolarmente consigliabile usare la **ad alte prestazioni** il risparmio di energia a meno che non energia l'efficienza è una priorità alta.

### <a name="metrics"></a>Metriche

Tutti i benchmark testati utilizzano velocità effettiva come la metrica di prestazioni. Tempo di risposta viene considerato come un requisito di contratto di servizio per questi carichi di lavoro (ad eccezione di SAP, in cui è una metrica primaria). Ad esempio, un'esecuzione del benchmark è considerato "valido? Se il valore medio o tempo di risposta massimo è minore di certo valore.

Pertanto, il PPM anche analisi di ottimizzazione Usa velocità effettiva come metrica delle prestazioni.  Al livello di carico massimo (utilizzo della CPU del 100%), il nostro obiettivo è che la velocità effettiva non deve diminuire più di una piccola percentuale a causa delle ottimizzazioni di gestione dell'alimentazione. Ma la considerazione principale consiste nel migliorare l'efficienza di risparmio energia (come definita di seguito) a livelli di carico medio e basso.

![formula di efficienza di risparmio energia](../../media/serverperf-ppm-formula.jpg)

In esecuzione le memorie centrali CPU con frequenze più basse riduce il consumo di energia. Tuttavia, in genere le frequenze inferiori diminuire la velocità effettiva e aumentare il tempo di risposta. Per il **bilanciato** risparmio di energia, un compromesso intenzionale di velocità di risposta e risparmio energetico. I test di carico di lavoro SAP, nonché il tempo di risposta i contratti di servizio su altri carichi di lavoro, assicurarsi che l'aumento di tempo di risposta non superi una determinata soglia (5% come esempio) per questi carichi di lavoro specifici.

>[!Note]
> Se il carico di lavoro utilizza tempo di risposta come la metrica di prestazioni, il sistema deve si passa al **High Performance** risparmio di energia o modifiche **bilanciato** risparmio di energia come suggerito in [ Consiglia di parametri di piano di risparmio di energia bilanciata per tempi di risposta rapidi](recommended-balanced-plan-parameters.md).

### <a name="tuning-results"></a>Risultati dell'ottimizzazione

A partire da Windows Server 2008, Microsoft ha collaborato con Intel e AMD a ottimizzare i parametri PPM per i processori server più aggiornati di ogni versione di Windows. Un numero enorme di combinazioni di parametri PPM sono stato testato in ognuno dei carichi di lavoro descritto in precedenza per individuare il migliore risparmio energetico a livelli di carico diversi. Come software sono stati ottimizzati algoritmi e come architetture hardware power evoluto, ogni nuovo Server Windows sempre aveva una migliore o uguale a risparmio energetico rispetto alle versioni precedenti tutta la gamma di carichi di lavoro testati.

Nella figura seguente illustra un esempio di risparmio energetico in diversi livelli di carico TPC-E in un server di produzione di 4 socket che esegue Windows Server 2008 R2. Mostra un miglioramento di % 8 livelli di carico medio rispetto a Windows Server 2008.

![confronto tra l'efficienza di risparmio energia](../../media/serverperf-ppm-figure1.jpg)

## <a name="customized-tuning-suggestions"></a>I suggerimenti di ottimizzazione personalizzati

Se le caratteristiche del carico di lavoro primaria notevolmente diverse da quelle cinque carichi di lavoro utilizzati per il valore predefinito **bilanciato** risparmio di energia PPM ottimizzazione, è possibile sperimentare mediante la modifica di uno o più parametri PPM per trovare la soluzione ottimale per il ambiente.

A causa di un numero e la complessità dei parametri, potrebbe trattarsi di un'attività complessa, ma se si sta cercando il miglior compromesso tra l'efficacia di carico di lavoro e il consumo energetico a seconda dell'ambiente, potrebbe essere la pena.

 Il set completo di parametri configurabili PPM è reperibile nel [ottimizzazione del risparmio energia del processore](https://msdn.microsoft.com/windows/hardware/gg566941.aspx). Alcuni dei parametri di risparmio energia più semplici per iniziare potrebbe essere:

-   **Aumentare la soglia delle prestazioni del processore e tempo di aumentare le prestazioni del processore** – valori più grandi rallentamento della risposta di prestazioni per un incremento dell'attività

-   **Soglia di riduzione delle prestazioni processore** : valori di grandi dimensioni quicken la risposta di risparmio energia per i periodi di inattività

-   **Tempo di riduzione delle prestazioni processore** – valori più grandi più gradualmente riducono le prestazioni durante i periodi di inattività

-   **Criteri di aumento delle prestazioni del processore** : "singolo? criteri rallenta la risposta prestazioni maggiori e prolungate attività. "Rocket? criteri reagisce rapidamente a un incremento dell'attività

-   **Criteri di riduzione delle prestazioni del processore** : "singolo? criteri più diminuiscono le prestazioni i periodi di inattività più; "Rocket? criteri scende di potenza molto rapidamente quando si immette un periodo di inattività

>[!Important]
> Prima di iniziare qualsiasi esperimenti, è necessario innanzitutto comprendere i carichi di lavoro, che consentono di operare delle scelte PPM parametro e ridurre le operazioni di ottimizzazione.

### <a name="understand-high-level-performance-and-power-requirements"></a>Determinare i requisiti di risparmio energia e dettagliato delle prestazioni

Se il carico di lavoro "in tempo reale? (ad esempio, soggetto ad anomalie al o altro visibili per l'utente finale influisce su) o ha il requisito di velocità di risposta molto ridotto (ad esempio, una di intermediazione) e se il consumo di energia non è un criterio principale per l'ambiente, è probabile che deve passare al **Ad alte prestazioni** risparmio di energia. In caso contrario, si dovrebbe comprendere i requisiti di tempo di risposta dei carichi di lavoro e ottimizzare i parametri PPM per la migliore efficienza power possibili che continui a soddisfare tali requisiti.

### <a name="understand-underlying-workload-characteristics"></a>Comprendere le caratteristiche del carico di lavoro sottostante

È necessario conoscere i carichi di lavoro e i set di parametri esperimento per l'ottimizzazione di progettazione. Ad esempio, se le frequenze dei core della CPU necessario utilizzare il molto veloce (ad esempio si dispone di un carico di lavoro bursty con periodi di inattività significativi, ma necessaria velocità di risposta molto rapido quando subentra una nuova transazione), quindi le prestazioni del processore aumentare dei criteri potrebbe essere necessario sia impostato su "rocket? (che, come suggerisce il nome, emette la frequenza di core della CPU per il valore massimo consentito, piuttosto che l'esecuzione un periodo di tempo).

Se il carico di lavoro è molto bursty, è possibile ridurre l'intervallo di controllo PPM per rendere la frequenza della CPU di avviare l'esecuzione di istruzioni subito dopo l'arrivo di un burst. Se il carico di lavoro non dispone di concorrenza di thread elevato, quindi parcheggio core può essere abilitata per forzare il carico di lavoro da eseguire in un minor numero di memorie centrali, che potrebbe potenzialmente migliorare riscontri cache del processore rapporti.

Se si desidera semplicemente aumentare le frequenze di CPU a livelli di utilizzo medio (ad esempio, i livelli di carico di lavoro non ridotto), quindi aumentare o ridurre le soglie delle prestazioni processore possono essere regolate per non rispondere fino a quando non vengono osservate determinati livelli di attività.

### <a name="understand-periodic-behaviors"></a>Comprendere i comportamenti periodici

Potrebbero essere presenti i requisiti di prestazioni diverso per ore del giorno e notte o nel fine settimana oppure potrebbero esserci diversi carichi di lavoro eseguiti in momenti diversi. In questo caso, un set di parametri PPM potrebbe non essere ottima per tutti i periodi di tempo. Poiché è possibile individuare più piani di risparmio di energia personalizzata, è possibile anche ottimizzare per periodi di tempo diversi e spostarsi tra i piani di risparmio energia tramite script o altri metodi di configurazione dinamica del sistema.

Anche in questo caso, ciò aumenta la complessità del processo di ottimizzazione, pertanto è una questione di alcuna informazione da questo tipo di ottimizzazione, la quantità di valore che probabilmente dovrà essere ripetuta quando sono presenti aggiornamenti significativi hardware o modifiche del carico di lavoro.

Questo è il motivo per cui Windows fornisce un **bilanciato** il risparmio di energia in primo luogo, perché in molti casi è probabile che non la pena di intervento manuale per un carico di lavoro specifico in un server specifico.

## <a name="see-also"></a>Vedere anche
- [Considerazioni sulle prestazioni dell'Hardware di server](../index.md)
- [Considerazioni relative all'alimentazione Hardware server](../power.md)
- [Risparmio energia e ottimizzazione delle prestazioni](power-performance-tuning.md)
- [Processore Power Management di ottimizzazione](processor-power-management-tuning.md)
- [Parametri bilanciato piano consigliato](recommended-balanced-plan-parameters.md)
