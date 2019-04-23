---
title: Consiglia di parametri di piano di risparmio di energia bilanciata per tempi di risposta rapidi
description: Consiglia di parametri di piano di risparmio di energia bilanciata per tempi di risposta rapidi
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Qizha;TristanB
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 134e868e1400729f754039fc8120cea0c73945bf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878792"
---
# <a name="recommended-balanced-power-plan-parameters-for-workloads-requiring-quick-response-times"></a>Consiglia di parametri di piano di risparmio di energia bilanciata per carichi di lavoro che richiedono tempi di risposta rapidi

Il valore predefinito **bilanciato** power Usa piano **velocità effettiva** come metrica delle prestazioni per l'ottimizzazione. Durante lo stato stazionario **velocità effettiva** non cambia con degli utilizzi diversi fino a quando il sistema è totalmente overload (~ 100% di utilizzo).  Di conseguenza, il **bilanciato** risparmio di energia predilige power un bel po' con riducendo al minimo la frequenza del processore e utilizzo di ottimizzazione.

Tuttavia **tempi di risposta** potrebbe aumentare in modo esponenziale con incrementi di utilizzo. Al giorno d'oggi, il requisito di tempi di risposta rapidi è notevolmente aumentate. Anche se Microsoft suggerito agli utenti di passare per il **ad alte prestazioni** risparmio di energia quando sono necessari tempi di risposta rapidi, alcuni utenti non si desiderano perdere il vantaggio power durante chiaro ai livelli di carico medio. Di conseguenza, Microsoft offre il seguente set di modifiche di parametro suggeriti per i carichi di lavoro che richiedono tempi di risposta rapidi.


| Parametro | Descrizione | Valore predefinito | Valore proposto |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Soglia di aumento delle prestazioni processore | Soglia di utilizzo precedente che è l'aumento della frequenza | 90 | 60 |
| Soglia di riduzione delle prestazioni processore | Soglia di utilizzo seguente che la frequenza è quello di ridurre | 80 | 40 |
| Tempo di aumento delle prestazioni processore | Numero di finestre di PPM prima è l'aumento della frequenza | 3 | 1 |
| Criteri di aumento delle prestazioni del processore | Velocità con cui la frequenza è aumentare | singolo | Ideale |

Per impostare i valori proposti, gli utenti possono eseguire i comandi seguenti in una finestra con l'amministratore:

``` syntax
Powercfg -setacvalueindex scheme_balanced sub_processor PERFINCTHRESHOLD 60
Powercfg -setacvalueindex scheme_balanced sub_processor PERFDECTHRESHOLD 40
Powercfg -setacvalueindex scheme_balanced sub_processor PERFINCTIME 1
Powercfg -setacvalueindex scheme_balanced sub_processor PERFINCPOL 0
Powercfg -setactive scheme_balanced
```

Questa modifica si basa sulle prestazioni e analisi di compromesso power con carichi di lavoro seguenti. Per gli utenti che desiderano ulteriormente per ottimizzare l'efficienza di risparmio energia con determinati requisiti del contratto di servizio, consultare [considerazioni sulle prestazioni Hardware Server](../power.md).

>[!Note]
> Per altre indicazioni e informazioni dettagliate sull'utilizzo di risparmio di energia per l'ottimizzazione di carichi di lavoro virtualizzati, leggere [configurazione Hyper-v](../../role/hyper-v-server/configuration.md)

## <a name="specpower--java-workload"></a>SPECpower: carico di lavoro JAVA

[SPECpower\_ssj2008](http://spec.org/power_ssj2008/), più popolari benchmark specifiche tecniche standard di settore per le caratteristiche di potenza e prestazioni del server, viene usato per controllare l'impatto di risparmio energia. Dal momento che utilizza solo **velocità effettiva** come metrica di prestazioni, il valore predefinito **bilanciato** risparmio di energia fornisce il migliore risparmio energetico.

La modifica di proposta parametro consuma energia leggermente superiore alla luce (ad esempio, < = % 20) livelli di carico. Ma con il maggiore aumenta il carico di livello, il differenza e inizia a consumare energia stesso come il **ad alte prestazioni** risparmio di energia dopo il livello di carico il 60%. Per usare i parametri della modifica proposta, gli utenti devono tenere il costo dell'energia media per i livelli di carico elevato durante la pianificazione della capacità per i rack.

## <a name="geekbench-3"></a>GeekBench 3

[3 GeekBench](http://www.geekbench.com/geekbench3/) è un benchmark di processore multipiattaforma che separa i punteggi per garantire prestazioni core singolo e multi-core. Simula una serie di carichi di lavoro inclusi interi carichi di lavoro (crittografare i dati, la compressione, l'elaborazione di immagini e così via), carichi di lavoro di punto a virgola mobile (modellazione, frattale, ottimizzazione delle immagini, immagine sfocatura, e così via) e carichi di lavoro di memoria (streaming).

**Tempo di risposta** è una misura principali per il calcolo del punteggio. Nel nostro sistema testato, il valore predefinito **bilanciato** risparmio di energia ha regressione di circa 18% nella regressione di test e ~ 40% single core nei test di multi-core rispetto al **ad alte prestazioni** risparmio di energia. La modifica proposta rimuove le regressioni.

## <a name="diskspd"></a>DiskSpd

[Diskspd](https://en.wikipedia.org/wiki/Diskspd) è uno strumento da riga di comando per archiviazione di benchmarking sviluppato da Microsoft. Viene ampiamente utilizzato per generare una serie di richieste nei sistemi di archiviazione per l'analisi delle prestazioni di archiviazione.

Si configura un [Cluster di Failover] e utilizzate Diskspd per generare casuale e sequenziale e lettura e scrittura per i sistemi di archiviazione locale e remota con diverse dimensioni dei / o. I test indicano che il tempo di risposta dei / o è molto sensibile alla frequenza del processore in risparmio di energia diverse. Il **bilanciato** risparmio di energia può raddoppiare il tempo di risposta di queste informazioni dalle **ad alte prestazioni** risparmio di energia in determinati carichi di lavoro. La modifica proposta rimuove la maggior parte delle regressioni.

>[!Important]
>A partire da processori Intel [Broadwell] che esegue Windows Server 2016, la maggior parte delle decisioni processor power management vengono effettuata nel processore anziché a livello del sistema operativo per ottenere la personalizzazione più rapido alle modifiche del carico di lavoro. I parametri PPM legacy utilizzati dal sistema operativo hanno un impatto minimo sulle decisioni frequenza effettiva, eccetto che informa il processore se deve favorire la riduzione dell'alimentazione o prestazioni o limitando le frequenze minimo e massime. Di conseguenza, la modifica di proposta PPM parametro è destinato solo per i sistemi di pre-Broadwell.

## <a name="see-also"></a>Vedere anche
- [Considerazioni sulle prestazioni dell'Hardware di server](../index.md)
- [Considerazioni relative all'alimentazione Hardware server](../power.md)
- [Risparmio energia e ottimizzazione delle prestazioni](power-performance-tuning.md)
- [Processore Power Management di ottimizzazione](processor-power-management-tuning.md)
- [Cluster di failover](https://technet.microsoft.com/library/cc725923.aspx)
