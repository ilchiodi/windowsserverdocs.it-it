---
title: Ottimizzazione del risparmio energia del processore (PPM) per il piano di risparmio energia bilanciato di Windows Server
description: Ottimizzazione del risparmio energia del processore (PPM) per il piano di risparmio energia bilanciato di Windows Server
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Qizha;TristanB
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: f98e8f3b64bd91837b6cc9b62777bebd57c0ec00
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866762"
---
# <a name="processor-power-management-ppm-tuning-for-the-windows-server-balanced-power-plan"></a>Ottimizzazione del risparmio energia del processore (PPM) per il piano di risparmio energia bilanciato di Windows Server

A partire da Windows Server 2008, Windows Server offre tre combinazioni per il risparmio di energia: **Bilanciamento**, **prestazioni elevate**e **risparmio di energia**. La combinazione per il risparmio di energia **bilanciata** è la scelta predefinita che mira a garantire la migliore efficienza energetica per un set di carichi di lavoro del server tipici. In questo argomento vengono descritti i carichi di lavoro utilizzati per determinare le impostazioni predefinite per lo schema **bilanciato** per le diverse versioni di Windows precedenti.

Se si esegue un sistema server con caratteristiche del carico di lavoro notevolmente diverse o requisiti di prestazioni e alimentazione rispetto a questi carichi di lavoro, è consigliabile provare a ottimizzare le impostazioni di risparmio energia predefinite (ovvero creare una combinazione per il risparmio di energia personalizzata). Una fonte di informazioni utili sull'ottimizzazione sono le [considerazioni sull'alimentazione hardware del server](../power.md). In alternativa, è possibile decidere che la combinazione per il risparmio di energia a **prestazioni elevate** è la scelta ideale per l'ambiente in uso, riconoscendo che è probabile che si verifichi un notevole calo di energia in cambio di un livello di velocità di risposta maggiore.

> [!IMPORTANT]
> È consigliabile usare i criteri di risparmio energia inclusi in Windows Server, a meno che non si abbia una necessità specifica di crearne uno personalizzato e si abbia una buona consapevolezza che i risultati variano a seconda delle caratteristiche del carico di lavoro.

## <a name="windows-processor-power-tuning-methodology"></a>Metodologia di ottimizzazione dell'alimentazione di Windows Processor


### <a name="tested-workloads"></a>Carichi di lavoro testati

I carichi di lavoro sono selezionati per coprire un insieme di carichi di lavoro di Windows Server "tipici". Ovviamente questo set non è destinato a essere rappresentativo dell'intera gamma di ambienti server reali.

L'ottimizzazione in ogni criterio di risparmio energia è basata sui dati dei cinque carichi di lavoro seguenti:

-   **Carico di lavoro server Web IIS**

    Un benchmark interno Microsoft denominato fundamentals Web viene usato per ottimizzare l'efficienza energetica delle piattaforme che eseguono il server Web IIS. Il programma di installazione contiene un server Web e più client che simulano il traffico di accesso Web. La distribuzione di pagine Web dinamiche, statiche a caldo (in memoria) e statiche a freddo (accesso al disco necessarie) si basa su studi statistici di server di produzione. Per eseguire il push dei core CPU del server a un utilizzo completo (un'estremità dello spettro testato), il programma di installazione deve disporre di risorse di rete e disco sufficientemente veloci.

-   **Carico di lavoro del database SQL Server**

    Il benchmark [TPC-E](http://www.tpc.org/tpce/default.asp) è un benchmark comune per l'analisi delle prestazioni del database. Viene usato per generare un carico di lavoro OLTP per le ottimizzazioni dell'ottimizzazione PPM. Questo carico di lavoro dispone di un I/O del disco significativo e pertanto presenta un requisito di prestazioni elevate per il sistema di archiviazione e le dimensioni della memoria.

-   **Carico di lavoro file server**

    Un benchmark sviluppato da Microsoft denominato [FSCT](http://www.snia.org/sites/default/files2/sdc_archives/2009_presentations/tuesday/BartoszNyczkowski-JianYan_FileServerCapacityTool.pdf) viene usato per generare un carico di lavoro SMB file server. Viene creato un set di file di grandi dimensioni nel server e vengono utilizzati molti sistemi client (effettivi o virtualizzati) per generare operazioni di apertura, chiusura, lettura e scrittura dei file. La combinazione di operazioni si basa sugli studi statistici dei server di produzione. Viene sottolineata la CPU, il disco e le risorse di rete.

-   **SPECpower: carico di lavoro JAVA**

    [SPECpower\_ssj2008](http://spec.org/power_ssj2008/) è il primo benchmark di specifiche di settore che valuta congiuntamente le caratteristiche di potenza e prestazioni. Si tratta di un carico di lavoro Java lato server con diversi livelli di carico della CPU. Non richiede molte risorse disco o di rete, ma presenta determinati requisiti per la larghezza di banda della memoria. Quasi tutte le attività della CPU vengono eseguite in modalità utente; l'attività in modalità kernel non ha un notevole effetto sulle caratteristiche di potenza e prestazioni dei benchmark, tranne per le decisioni relative al risparmio energia.

-   **Carico di lavoro del server applicazioni**

    Il benchmark [SAP-SD](http://global.sap.com/campaigns/benchmark/index.epx) viene usato per generare un carico di lavoro del server applicazioni. Viene utilizzata una configurazione a due livelli, con il database e il server applicazioni nello stesso host server. Questo carico di lavoro usa anche il tempo di risposta come metrica delle prestazioni, che differisce da altri carichi di lavoro sottoposti a test. Viene quindi usato per verificare l'effetto dei parametri PPM sulla velocità di risposta. Tuttavia, non è destinato a essere rappresentativo di tutti i carichi di lavoro di produzione sensibili alla latenza.

Tutti i benchmark ad eccezione di SPECpower sono stati originariamente progettati per l'analisi delle prestazioni e pertanto sono stati creati per l'esecuzione a livelli di carico di picco. Tuttavia, i livelli di carico medio-chiaro sono più comuni per i server di produzione reali e sono più interessanti per le ottimizzazioni del piano **bilanciato** . I benchmark vengono eseguiti intenzionalmente a livelli di carico diversi dal 100% fino al 10% (nel 10% dei passaggi) usando vari metodi di limitazione (ad esempio, riducendo il numero di utenti/client attivi).

### <a name="hardware-configurations"></a>Configurazioni hardware

Per ogni versione di Windows, i server di produzione più recenti vengono usati nel processo di ottimizzazione e analisi della combinazione per il risparmio di energia. In alcuni casi, i test sono stati eseguiti in sistemi di pre-produzione la cui pianificazione della versione corrisponde a quella della versione successiva di Windows.

Dato che la maggior parte dei server viene venduta con 1 a 4 socket del processore e poiché i server con scalabilità verticale hanno meno probabilità di avere un'efficienza energetica come preoccupazione principale, i test di ottimizzazione per la combinazione per il risparmio di energia vengono eseguiti principalmente su sistemi a 2 socket e a 4 socket. La quantità di RAM, disco e risorse di rete per ogni test viene scelta per consentire a ogni sistema di funzionare fino alla sua capacità completa, tenendo conto delle restrizioni di costo che verrebbero normalmente applicate per gli ambienti server reali, ad esempio mantenendo il configurazioni ragionevoli.

> [!IMPORTANT]
> Anche se il sistema può essere eseguito con il picco di carico, in genere viene ottimizzato per i livelli di carico inferiori, perché i server che vengono eseguiti in modo coerente con i livelli di carico di picco dovrebbero essere ben consigliati per usare la combinazione per il risparmio di energia a **prestazioni elevate** , a meno che l'efficienza priorità.

### <a name="metrics"></a>metrics

Tutti i benchmark testati usano la velocità effettiva come metrica delle prestazioni. Il tempo di risposta viene considerato un requisito del contratto di contratto per questi carichi di lavoro (ad eccezione di SAP, dove si tratta di una metrica primaria). Ad esempio, un'esecuzione di benchmark viene considerata "valida" Se il tempo di risposta medio o massimo è inferiore a un determinato valore.

Pertanto, l'analisi di ottimizzazione PPM usa anche la velocità effettiva come misurazione delle prestazioni.  Al livello di carico più elevato (100% di utilizzo della CPU), il nostro obiettivo è che la velocità effettiva non dovrebbe ridursi più di una percentuale a causa delle ottimizzazioni del risparmio energia. Tuttavia, la considerazione principale è la massimizzazione dell'efficienza energetica (come definito di seguito) a livelli di carico medio e basso.

![formula di risparmio energia](../../media/serverperf-ppm-formula.jpg)

L'esecuzione di core CPU a frequenze inferiori riduce il consumo di energia. Tuttavia, le frequenze inferiori in genere riducono la velocità effettiva e aumentano il tempo di risposta. Per la combinazione per il risparmio di energia **bilanciata** , c'è un compromesso intenzionale di velocità di risposta e efficienza energetica. I test del carico di lavoro SAP, così come i contratti di tempo di risposta per gli altri carichi di lavoro, assicurano che l'aumento del tempo di risposta non superi una determinata soglia (5% come esempio) per questi carichi di lavoro specifici.

> [!NOTE]
> Se il carico di lavoro usa il tempo di risposta come misurazione delle prestazioni, il sistema deve passare alla combinazione per il risparmio di energia a **prestazioni elevate** o modificare la combinazione per il risparmio di energia **bilanciata** , come suggerito nei [parametri di combinazione per il risparmio Ora](recommended-balanced-plan-parameters.md).

### <a name="tuning-results"></a>Risultati dell'ottimizzazione

A partire da Windows Server 2008, Microsoft ha collaborato con Intel e AMD per ottimizzare i parametri PPM per i processori server più aggiornati per ogni versione di Windows. Un numero enorme di combinazioni di parametri PPM è stato testato su ognuno dei carichi di lavoro descritti in precedenza per trovare la migliore efficienza energetica a diversi livelli di carico. Poiché gli algoritmi software sono stati perfezionati e le architetture per il risparmio di energia hardware si sono evolute, ogni nuova versione di Windows Server disponeva sempre di efficienza energetica migliore o uguale rispetto alle versioni precedenti nell'ambito dei carichi di lavoro testati.

Nella figura seguente viene illustrato un esempio di efficienza energetica in diversi livelli di carico TPC-E in un server di produzione a 4 socket che esegue Windows Server 2008 R2. Mostra un miglioramento dell'8% a livelli di carico medio rispetto a Windows Server 2008.

![confronto risparmio energia](../../media/serverperf-ppm-figure1.jpg)

## <a name="customized-tuning-suggestions"></a>Suggerimenti per l'ottimizzazione personalizzati

Se le caratteristiche del carico di lavoro principale differiscono significativamente dai cinque carichi di lavoro usati per l' **ottimizzazione predefinita del** piano di risparmio energia ppm, è possibile provare a modificare uno o più parametri ppm per trovare la soluzione migliore per l'ambiente.

A causa del numero e della complessità dei parametri, può trattarsi di un'attività complessa, ma se si sta cercando il migliore compromesso tra il consumo di energia e l'efficacia del carico di lavoro per un ambiente specifico, potrebbe valere la pena.

 Il set completo di parametri PPM ottimizzabili è disponibile nell' [ottimizzazione del risparmio energia del processore](https://msdn.microsoft.com/windows/hardware/gg566941.aspx). Alcuni dei più semplici parametri di alimentazione da cui iniziare possono essere:

-   **Soglia di aumento delle prestazioni del processore e aumento delle prestazioni del processore** : i valori più grandi rallentano la risposta delle prestazioni a un'attività aumentata

-   **Soglia di riduzione delle prestazioni del processore** : i valori di grandi dimensioni velocizzano la risposta al risparmio energia

-   **Riduzione del tempo del processore** : i valori più grandi diminuiscono gradualmente le prestazioni durante i periodi di inattività

-   **Criteri di aumento delle prestazioni del processore** : il criterio "singolo" rallenta la risposta delle prestazioni a un'attività aumentata e sostenuta; i criteri "Rocket" reagiscono rapidamente all'aumento delle attività

-   **Criteri di riduzione delle prestazioni del processore** : i criteri "singoli" diminuiscono gradualmente le prestazioni nei periodi di inattività più lunghi; il criterio "Rocket" si abbassa molto rapidamente quando si immette un periodo di inattività

>[!Important]
> Prima di iniziare qualsiasi esperimento, è necessario comprendere i carichi di lavoro, che consentiranno di prendere le scelte appropriate per i parametri PPM e di ridurre il lavoro di ottimizzazione.

### <a name="understand-high-level-performance-and-power-requirements"></a>Informazioni sulle prestazioni di alto livello e sui requisiti di alimentazione

Se il carico di lavoro è "in tempo reale" (ad esempio, soggetto a glitch o ad altri effetti visivi dell'utente finale) o ha un requisito di velocità di risposta molto elevato (ad esempio, un brokeraggio azionario) e se il consumo di energia non è un criterio primario per l'ambiente, è necessario probabilmente è sufficiente passare alla combinazione per il risparmio di energia a **prestazioni elevate** . In caso contrario, è necessario comprendere i requisiti del tempo di risposta dei carichi di lavoro e quindi ottimizzare i parametri PPM per ottenere una migliore efficienza energetica che soddisfi tali requisiti.

### <a name="understand-underlying-workload-characteristics"></a>Informazioni sulle caratteristiche del carico di lavoro sottostanti

È necessario essere a conoscenza dei carichi di lavoro e progettare i set di parametri dell'esperimento per l'ottimizzazione. Se, ad esempio, le frequenze dei core della CPU devono essere molto veloci (probabilmente si ha un carico di lavoro molto elevato con periodi di inattività significativi, ma è necessaria una velocità di risposta molto rapida quando viene rilevata una nuova transazione), i criteri di aumento delle prestazioni del processore potrebbe essere necessario impostare su "Rocket" (che, come suggerisce il nome, risale la frequenza di core della CPU al valore massimo anziché eseguire il rollup in un periodo di tempo).

Se il carico di lavoro è molto grande, l'intervallo di controllo PPM può essere ridotto per fare in modo che la frequenza della CPU venga avviata prima dell'arrivo di un impeto. Se il carico di lavoro non dispone di una concorrenza di thread elevata, è possibile abilitare il parcheggio di base per forzare l'esecuzione del carico di lavoro su un numero minore di core, che potrebbero anche migliorare la percentuale di riscontri nella cache del processore.

Se si desidera aumentare le frequenze della CPU a livelli di utilizzo medio (ad esempio, i livelli di carico di lavoro non sono semplici), le soglie di aumento/riduzione delle prestazioni del processore possono essere modificate in modo da non rispondere fino a quando non si osservano determinati livelli di attività.

### <a name="understand-periodic-behaviors"></a>Informazioni sui comportamenti periodici

Potrebbero essere previsti requisiti di prestazioni diversi per giorno e notte o nei fine settimana oppure possono essere presenti carichi di lavoro diversi eseguiti in momenti diversi. In questo caso, un set di parametri PPM potrebbe non essere ottimale per tutti i periodi di tempo. Poiché è possibile definire più combinazioni per il risparmio di energia personalizzate, è possibile eseguire l'ottimizzazione anche per diversi periodi di tempo e passare tra combinazioni per il risparmio di energia tramite script o altri metodi di configurazione dinamica del sistema.

Anche in questo caso, ciò consente di aumentare la complessità del processo di ottimizzazione, quindi si tratta di una questione della quantità di valore che verrà ottenuta da questo tipo di ottimizzazione, che probabilmente dovrà essere ripetuta in caso di aggiornamenti hardware significativi o di modifiche del carico di lavoro.

Questo è il motivo per cui Windows offre una combinazione per il risparmio di energia **bilanciata** , perché in molti casi probabilmente non vale la pena di effettuare l'ottimizzazione manuale per un carico di lavoro specifico in un server specifico.

## <a name="see-also"></a>Vedere anche
- [Considerazioni sulle prestazioni dell'hardware del server](../index.md)
- [Considerazioni sull'alimentazione dell'hardware del server](../power.md)
- [Risparmio energia e ottimizzazione delle prestazioni](power-performance-tuning.md)
- [Ottimizzazione di Risparmio energia del processore](processor-power-management-tuning.md)
- [Parametri della combinazione per il risparmio di energia Bilanciato](recommended-balanced-plan-parameters.md)
