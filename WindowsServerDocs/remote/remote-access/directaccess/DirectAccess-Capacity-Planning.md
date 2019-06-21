---
title: Pianificazione della capacità di DirectAccess
description: È possibile utilizzare questo argomento per un report sulle prestazioni del server DirectAccess di Windows Server 2012 semplificano la pianificazione della capacità per DirectAccess in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 456e5971-3aa7-4a24-bc5d-0c21fec7687e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d6f0f4a089dd8e99bb9f9815f0900a3c53c9d1ba
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281989"
---
# <a name="directaccess-capacity-planning"></a>Pianificazione della capacità di DirectAccess

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Questo documento è un rapporto sulle prestazioni del server DirectAccess di Windows Server 2012. I test sono stati eseguiti per determinare la velocità effettiva dell'hardware di fascia alta e dell'hardware di fascia bassa. Le prestazioni dei computer di fascia alta e di fascia bassa dipendono dalla velocità effettiva del traffico di rete e dai tipi di client utilizzati. Come base per questi test è stata utilizzata la distribuzione tipica di DirectAccess che consiste nell'uso di 1/3 (30%) di client IPHTTPS e di 2/3 (70%) di client Teredo. Le prestazioni dei client Teredo superano in parte quelle dei client IPHTTPS perché Windows Server 2012 utilizza Receive Side Scaling (RSS) che consente l'uso di tutte le core CPU. In questi test, dal momento che RSS è abilitato, è stato disabilitato il threading Hyper. TCP/IP in Windows Server 2012 supporta inoltre il traffico UDP consentendo ai client Teredo di bilanciare il carico tra le CPU.  
  
I dati sono stati raccolti da un server di fascia bassa (4 core, 4 Gig) e da hardware utilizzato per lo più in server di fascia alta (8 core, 8 Gig).  Di seguito è riportata una schermata di Gestione attività Windows 8 di nuovo hardware di fascia bassa con 750 client (562 Teredo e 188 IPHTTPS) in esecuzione a circa 77 Mbits/sec. Si tratta di simulare gli utenti che non dispongono di credenziali della smart card.  
  
I risultati di questo test indicano che le prestazioni di Teredo sono migliori di quelle di IPHTTPS in Windows 8, ma che l'utilizzo della larghezza di banda è migliorato sia per Teredo che per IPHTTPS se confrontato con quello di Windows 7.  
  
![Risultati del test](../../media/DirectAccess-Capacity-Planning/DACapacityPlanning1.gif)  
  
## <a name="high-end-hardware-test-environment"></a>Ambiente di test di fascia alta  
Il grafico seguente illustra i risultati dell'ambiente di test delle prestazioni dell'hardware di fascia alta. Tutti i risultati dei test e le analisi sono disponibili in questo documento.  
  
||||  
|-|-|-|  
|Configurazione - Hardware|Hardware di fascia bassa (4 GB di ram, 4 core)|Hardware di fascia alta (8 GB, 8 core)|  
|Tunnel doppio<br /><br />-PKI<br /><br />-Con DNS64/NAT64|750 connessioni simultanee al 50% della CPU, 50% della memoria con velocità effettiva di Corpnet NIC pari a 75 Mbps. Il target esteso è di 1000 utenti al 50% della CPU.|1500 connessioni simultanee al 50% della CPU, 50% della memoria con velocità effettiva di Corpnet NIC 150 Mbps.|  
## <a name="test-environment"></a>Ambiente di test

**Topologia banco di test delle prestazioni**  
  
![Ambiente di test](../../media/DirectAccess-Capacity-Planning/DACapacityPlanning2.gif)  
  
L'ambiente del test delle prestazioni è costituito da un banco di test di 5 computer. Per il test dell'hardware di fascia bassa è stato utilizzato un server DirectAccess a 4 core e 4 Gig, mentre per il test dell'hardware di fascia alta è stato utilizzato un server DirectAccess a 8 core e 16 Gig. Per gli ambienti di test per l'hardware di fascia alta e di fascia bassa sono stati utilizzati un server back-end (il mittente) e due computer client (i destinatari).  I destinatari sono suddivisi nei due computer client. In caso contrario, i destinatari sarebbero vincolati dalla CPU e il numero di client e la larghezza di banda risulterebbero limitati. Nella parte di destinazione è stato utilizzato un simulatore per simulare centinaia di client (HTTPS o Teredo). Sono stati configurati IPsec e DOSp. Nel server DirectAccess è abilitato RSS. Le dimensioni della coda di RSS sono impostate su 8.  Se non si configurasse RSS, un solo processore verrebbe utilizzato al massimo mentre gli altri core verrebbero sottoutilizzati. Si noti inoltre che il server DirectAccess è un computer a 4 core con il threading Hyper disattivato.  Il threading Hyper è disattivato perché RSS funziona solo su core fisici e l'utilizzo del threading Hyper produce risultati distorti. Ciò significa che non tutti i core verranno caricati in modo uniforme.  
  
## <a name="testing-results-for-low-end-hardware"></a>Risultati del test per l'hardware di fascia bassa:

Il test è stato eseguito sia con 1000 che con 750 client.  In tutti i casi il traffico è stato così suddiviso: 70% Teredo e 30% IPHTTPS.  Per tutti i test è stato utilizzato traffico TCP su Nat64 con 2 tunnel IPsec per client.  In tutti i test l'utilizzo della memoria è stato leggero e l'utilizzo della CPU accettabile.  
  
**Risultati dei Test individuali:**  
  
Nelle sezioni seguenti vengono illustrati i test individuali. Il titolo di ogni sezione evidenzia gli elementi chiave di ogni test ed è seguito da una descrizione riepilogativa dei risultati, nonché da una tabella che include i dati dei risultati dettagliati.  
  
**Prestazioni fascia bassa:  750 client, 70/30 suddivisione, velocità effettiva di 84,17 Mbit/sec:**  
  
I tre test seguenti rappresentano l'hardware di fascia bassa.  Nelle esecuzioni di test illustrate sotto erano presenti 750 client con una velocità effettiva di 84,17 Mbit/sec e una suddivisione del traffico tra 562 Teredo e 188 IPHTTPS. Il valore MTU di Teredo è stato impostato su 1472 ed è stato abilitato Teredo Shunt. L'utilizzo medio della CPU è pari al 46,42% nei tre test e l'utilizzo medio della memoria, espresso come percentuale dei byte impegnati sul totale di memoria disponibile di 4 GB è pari al 25,95%.  
  
||||||||  
|-|-|-|-|-|-|-|  
|**Scenario**|**Media CPU (dal contatore)**|**Mbit/s (lato azienda)**|**Mbit/sec (internet Side)**|**QMSA attivo**|**MMSA attivo**|**Utilizzo memoria (sistema a 4 Gig)**|  
|**HW fascia bassa.  562 client Teredo.  188 client IPHTTPS.**|47.7472542|84.3|119.13|1502.05|1502.1|26,27%|  
|**HW fascia bassa.  562 client Teredo.  188 client IPHTTPS.**|46.3889778|84.146|118.73|1501.25|1501.2|25.90%|  
|**HW fascia bassa.  562 client Teredo.  188 client IPHTTPS.**|45.113082|84.0494|118.43|1546.14|1546.1|25.68%|  
  
**1000 client, 70/30 suddivisione, velocità effettiva di 78 Mbit/sec:**  
  
I tre test seguenti rappresentano le prestazioni dell'hardware di fascia bassa. Nelle esecuzioni di test illustrate sotto erano presenti 1000 client con una velocità effettiva di 78,64 Mbit/sec e una suddivisione del traffico tra 700 Teredo e 300 IPHTTPS.  Il valore MTU di Teredo è stato impostato su 1472 ed è stato abilitato Teredo Shunt.  L'utilizzo medio della CPU è pari circa al 50,7% e l'utilizzo medio della memoria, espresso come percentuale dei byte impegnati sul totale di memoria disponibile di 4 GB è pari circa al 28,7%.  
  
||||||||  
|-|-|-|-|-|-|-|  
|**Scenario**|**Media CPU (dal contatore)**|**Mbit/s (lato azienda)**|**Mbit/sec (internet Side)**|**QMSA attivo**|**MMSA attivo**|**Utilizzo memoria (sistema a 4 Gig)**|  
|**HW fascia bassa.  Client teredo 700.  300 client IPHTTPS.**|51.28406247|78.6432|113.19|2002.42|1502.1|25.59%|  
|**HW fascia bassa.  Client teredo 700.  300 client IPHTTPS.**|51.06993128|78.6402|113.22|2001.4|1501.2|30.87%|  
|**HW fascia bassa.  Client teredo 700.  300 client IPHTTPS.**|49.75235617|78.6387|113.2|2002.6|1546.1|30.66%|  
  
**1000 client, 70/30 suddivisione, velocità effettiva di 109 Mbit/sec:**  
  
Nelle tre esecuzioni di test illustrate sotto erano presenti 1000 client con una velocità effettiva di circa 109,2 Mbit/sec e una suddivisione del traffico tra 700 Teredo e 300 IPHTTPS. Il valore MTU di Teredo è stato impostato su 1472 ed è stato abilitato Teredo Shunt. L'utilizzo medio della CPU è pari circa al 59,06% e l'utilizzo medio della memoria, espresso come percentuale dei byte impegnati sul totale di memoria disponibile di 4 GB è pari circa al 27,34%.  
  
||||||||  
|-|-|-|-|-|-|-|  
|**Scenario**|**Media CPU (dal contatore)**|**Mbit/s (lato azienda)**|**Mbit/sec (internet Side)**|**QMSA attivo**|**MMSA attivo**|**Utilizzo memoria (sistema a 4 Gig)**|  
|**HW fascia bassa.  Client teredo 700.  300 client IPHTTPS.**|59.81640675|108.305|153.14|2001.64|2001.6|24.38%|  
|**HW fascia bassa.  Client teredo 700.  300 client IPHTTPS.**|59.46473798|110.969|157.53|2005.22|2005.2|28.72%|  
|**HW fascia bassa.  Client teredo 700.  300 client IPHTTPS.**|57.89089768|108.305|153.14|1999.53|2018.3|24.38%|  
  
## <a name="testing-results-for-high-end-hardware"></a>Risultati del test per l'hardware di fascia alta:  
Il test è stato eseguito con 1500 client. Il traffico è stato così suddiviso: 70% Teredo e 30% IPHTTPS. Per tutti i test è stato utilizzato traffico TCP su Nat64 con 2 tunnel IPsec per client. In tutti i test l'utilizzo della memoria è stato leggero e l'utilizzo della CPU accettabile.  
  
**Risultati dei Test individuali:**  
  
Nelle sezioni seguenti vengono illustrati i test individuali. Il titolo di ogni sezione evidenzia gli elementi chiave di ogni test ed è seguito da una descrizione riepilogativa dei risultati, nonché da una tabella che include i dati dei risultati dettagliati.  
  
**1500 client, 70/30 suddivisione, velocità effettiva di 153,2 Mbit/sec**  
  
I cinque test seguenti rappresentano l'hardware di fascia alta. Nelle esecuzioni di test illustrate sotto erano presenti 1500 client con una velocità effettiva media di 153,2 Mbit/sec e una suddivisione del traffico tra 1050 Teredo e 450 IPHTTPS. L'utilizzo medio della CPU è pari circa al 50,68% e l'utilizzo medio della memoria, espresso come percentuale dei byte impegnati sul totale di memoria disponibile di 8 GB è pari circa al 22,25%.  
  
||||||||  
|-|-|-|-|-|-|-|  
|**Scenario**|**Media CPU (dal contatore)**|**Mbit/s (lato azienda)**|**Mbit/sec (internet Side)**|**QMSA attivo**|**MMSA attivo**|**Utilizzo memoria (sistema a 4 Gig)**|  
|**HW fascia alta.  Client teredo 1050.  Client di 450 IPHTTPS.**|51.712437|157.029|216.29|3000.31|3046|21.58%|  
|**HW fascia alta.  Client teredo 1050.  Client di 450 IPHTTPS.**|48.86020205|151.012|206.53|3002.86|3045.3|21.15%|  
|**HW fascia alta.  Client teredo 1050.  Client di 450 IPHTTPS.**|52.23979519|155.511|213.45|3001.15|3002.9|22.90%|  
|**HW fascia alta.  Client teredo 1050.  Client di 450 IPHTTPS.**|51.26269767|155.09|212.92|3000.74|3002.4|22.91%|  
|**HW fascia alta.  Client teredo 1050.  Client di 450 IPHTTPS.**|50.15751307|154.772|211.92|3000.9|3002.1|22.93%|  
|**HW fascia alta.  Client teredo 1050.  Client di 450 IPHTTPS.**|49.83665607|145.994|201.92|3000.51|3006|22.03%|  
  
![Risultati dei test hardware di fascia alta](../../media/DirectAccess-Capacity-Planning/DACapacityPlanning3.gif)  
  


