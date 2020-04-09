---
title: Parametri di combinazione per il risparmio di energia consigliati per tempi di risposta rapidi
description: Parametri di combinazione per il risparmio di energia consigliati per tempi di risposta rapidi
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: qizha;tristanb
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 288746b5361c550e167f64886a929c96c81ff8d0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851964"
---
# <a name="recommended-balanced-power-plan-parameters-for-workloads-requiring-quick-response-times"></a>Parametri di combinazione per il risparmio di energia consigliati per i carichi di lavoro che richiedono tempi di risposta rapidi

La combinazione per il risparmio di energia predefinita **bilancia** usa la **velocità effettiva** come misurazione delle prestazioni per l'ottimizzazione. Durante lo stato stazionario, la **velocità effettiva** non cambia con diversi utilizzi fino a quando il sistema non viene completamente sovraccaricato (~ 100% di utilizzo).  Di conseguenza, il piano di risparmio energia **bilanciato** favorisce un notevole risparmio di energia riducendo la frequenza del processore e ottimizzando l'utilizzo.

Tuttavia, il **tempo di risposta** potrebbe aumentare in modo esponenziale con un aumento dell'utilizzo. Attualmente, il requisito del tempo di risposta rapido è aumentato significativamente. Anche se Microsoft ha suggerito agli utenti di passare alla combinazione per il risparmio di energia a **prestazioni elevate** quando necessitano di tempi di risposta rapidi, alcuni utenti non vogliono perdere il vantaggio energetico durante i livelli di carico leggero e medio. Di conseguenza, Microsoft fornisce il seguente set di modifiche dei parametri suggerite per i carichi di lavoro che richiedono tempi di risposta rapidi.


| Parametro | Descrizione | Valore predefinito | Valore proposto |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Soglia di aumento delle prestazioni del processore | Soglia di utilizzo oltre la quale è necessario aumentare la frequenza | 90 | 60 |
| Soglia di riduzione delle prestazioni del processore | Soglia di utilizzo al di sotto della quale la frequenza diminuisce | 80 | 40 |
| Tempo di aumento delle prestazioni del processore | Numero di finestre di controllo PPM prima che la frequenza aumenti | 3 | 1 |
| Criteri di aumento delle prestazioni del processore | Velocità di aumento della frequenza | Single | Ideale |

Per impostare i valori proposti, gli utenti possono eseguire i comandi seguenti in una finestra con amministratore:

``` syntax
Powercfg -setacvalueindex scheme_balanced sub_processor PERFINCTHRESHOLD 60
Powercfg -setacvalueindex scheme_balanced sub_processor PERFDECTHRESHOLD 40
Powercfg -setacvalueindex scheme_balanced sub_processor PERFINCTIME 1
Powercfg -setacvalueindex scheme_balanced sub_processor PERFINCPOL 0
Powercfg -setactive scheme_balanced
```

Questa modifica è basata sull'analisi delle prestazioni e del compromesso di alimentazione usando i carichi di lavoro seguenti. Per gli utenti che desiderano ottimizzare ulteriormente l'efficienza energetica con determinati requisiti del contratto di assistenza, vedere [considerazioni sulle prestazioni dell'hardware del server](../power.md).

>[!Note]
> Per consigli aggiuntivi e informazioni dettagliate sull'uso delle combinazioni per il risparmio di energia per ottimizzare i carichi di lavoro virtualizzati, vedere [configurazione di Hyper-v](../../role/hyper-v-server/configuration.md)

## <a name="specpower--java-workload"></a>SPECpower: carico di lavoro JAVA

[SPECpower\_ssj2008](http://spec.org/power_ssj2008/), il benchmark delle specifiche standard del settore più diffuso per le caratteristiche di potenza e prestazioni del server, viene usato per controllare l'effetto sull'alimentazione. Poiché usa la **velocità effettiva** solo come metrica delle prestazioni, la combinazione per il risparmio di energia predefinita **equilibrata** offre la migliore efficienza energetica.

La modifica proposta del parametro usa una potenza leggermente superiore alla luce (ad esempio, < = 20%) livelli di carico. Tuttavia, con il livello di carico più elevato, la differenza aumenta e inizia a consumare la stessa potenza della combinazione per il risparmio di energia a **prestazioni elevate** dopo il livello di carico del 60%. Per usare i parametri di modifica proposti, è necessario che gli utenti siano a conoscenza del costo dell'energia a livelli di carico medio-alto durante la pianificazione della capacità del rack.

## <a name="geekbench-3"></a>GeekBench 3

[Geekbench 3](http://www.geekbench.com/geekbench3/) è un benchmark del processore multipiattaforma che separa i punteggi per le prestazioni single core e multicore. Simula un set di carichi di lavoro, inclusi carichi di lavoro di tipo Integer (crittografia, compressioni, elaborazione di immagini e così via), carichi di lavoro a virgola mobile (modellazione, frattale, nitidezza delle immagini, sfocatura delle immagini e così via) e carichi di lavoro di memoria (streaming).

Il **tempo di risposta** è una misura principale nel calcolo del punteggio. Nel nostro sistema testato, la combinazione per il risparmio di energia predefinita **bilanciata** ha una regressione di ~ 18% nei test single core e la regressione di ~ 40% nei test multicore rispetto alla combinazione per il risparmio di energia a **prestazioni elevate** . La modifica proposta rimuove queste regressioni.

## <a name="diskspd"></a>DiskSpd

[Diskspd](https://en.wikipedia.org/wiki/Diskspd) è uno strumento da riga di comando per il benchmarking di archiviazione sviluppato da Microsoft. Viene ampiamente usato per generare una serie di richieste nei sistemi di archiviazione per l'analisi delle prestazioni di archiviazione.

Viene configurato un [cluster di failover] e sono stati usati Diskspd per generare casualmente e sequenziali e leggere e scrivere IOs nei sistemi di archiviazione locali e remoti con diverse dimensioni di i/o. I test indicano che il tempo di risposta IO è molto sensibile alla frequenza del processore in combinazioni per il risparmio di energia diverse. Il piano di risparmio energia **bilanciato** potrebbe raddoppiare il tempo di risposta di tale combinazione dalla combinazione per il risparmio di energia a **prestazioni elevate** in determinati carichi La modifica proposta rimuove la maggior parte delle regressioni.

>[!Important]
>A partire da processori Intel [Broadwell] che eseguono Windows Server 2016, la maggior parte delle decisioni relative al risparmio di energia del processore vengono apportate al processore anziché al livello del sistema operativo per ottenere un adattamento più rapido alle modifiche del carico di lavoro. I parametri PPM legacy usati dal sistema operativo hanno un effetto minimo sulle decisioni relative alla frequenza effettiva, tranne per indicare al processore se è opportuno ottimizzare il consumo di energia o le prestazioni o la limitazione delle frequenze minime e massime. Di conseguenza, la modifica del parametro PPM proposto è destinata solo ai sistemi pre-Broadwell.

## <a name="see-also"></a>Vedi anche
- [Considerazioni sulle prestazioni dell'hardware del server](../index.md)
- [Considerazioni sull'alimentazione dell'hardware del server](../power.md)
- [Risparmio energia e ottimizzazione delle prestazioni](power-performance-tuning.md)
- [Ottimizzazione di Risparmio energia del processore](processor-power-management-tuning.md)
- [Cluster di failover](https://technet.microsoft.com/library/cc725923.aspx)
