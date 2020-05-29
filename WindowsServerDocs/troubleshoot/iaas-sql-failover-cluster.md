---
title: Modificare la soglia di rete della linea di base di failover
description: Questo articolo presenta le soluzioni per la regolazione della soglia delle reti di cluster di failover.
ms.prod: windows-server
ms.technology: server-general
ms.date: 05/28/2020
author: Deland-Han
ms.author: delhan
ms.openlocfilehash: c0e2f0309049f0271a223c2a23012eb2efa8d843
ms.sourcegitcommit: ef089864980a1d4793a35cbf4cbdd02ce1962054
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/28/2020
ms.locfileid: "84150169"
---
# <a name="iaas-with-sql-alwayson---tuning-failover-cluster-network-thresholds"></a>IaaS con SQL AlwaysOn - Ottimizzazione delle soglie di rete del cluster di failover

Questo articolo presenta le soluzioni per la regolazione della soglia delle reti di cluster di failover.

## <a name="symptom"></a>Sintomo

Quando si eseguono nodi del cluster di failover Windows in IaaS con SQL Server AlwaysOn, è consigliabile modificare l'impostazione del cluster su uno stato di monitoraggio più rilassato. Le impostazioni del cluster predefinite sono restrittive e possono causare interruzioni non necessarie. Le impostazioni predefinite sono progettate per le reti locali altamente ottimizzate e non prendono in considerazione la possibilità di latenza indotta causata da un ambiente multi-tenant, ad esempio Windows Azure (IaaS).

Windows Server failover clustering monitora costantemente le connessioni di rete e l'integrità dei nodi in un cluster Windows.  Se un nodo non è raggiungibile in rete, viene eseguita un'azione per ripristinare e portare online le applicazioni e i servizi su un altro nodo del cluster. La latenza di comunicazione tra i nodi del cluster può causare l'errore seguente:  

> Errore 1135 (registro eventi di sistema)

Il nodo del cluster **node1** è stato rimosso dall'appartenenza al cluster di failover attivo. È possibile che l'Servizio cluster in questo nodo sia stata arrestata. Questo problema può essere dovuto anche al fatto che il nodo ha perso la comunicazione con altri nodi attivi nel cluster di failover. Eseguire la convalida guidata configurazione per verificare la configurazione di rete. Se la condizione è permanente, verificare la presenza di errori hardware o software relativi alle schede di rete in questo nodo. Controllare anche la presenza di errori in qualsiasi altro componente di rete a cui è connesso il nodo, ad esempio hub, commutatori o Bridge.

Esempio di cluster. log:

```console
0000ab34.00004e64::2014/06/10-07:54:34.099 DBG   [NETFTAPI] Signaled NetftRemoteUnreachable event, local address 10.xx.x.xxx:3343 remote address 10.x.xx.xx:3343
0000ab34.00004b38::2014/06/10-07:54:34.099 INFO  [IM] got event: Remote endpoint 10.xx.xx.xxx:~3343~ unreachable from 10.xx.x.xx:~3343~
0000ab34.00004b38::2014/06/10-07:54:34.099 INFO  [IM] Marking Route from 10.xxx.xxx.xxxx:~3343~ to 10.xxx.xx.xxxx:~3343~ as down
0000ab34.00004b38::2014/06/10-07:54:34.099 INFO  [NDP] Checking to see if all routes for route (virtual) local fexx::xxx:5dxx:xxxx:3xxx:~0~ to remote xxx::cxxx:xxxd:xxx:dxxx:~0~ are down
0000ab34.00004b38::2014/06/10-07:54:34.099 INFO  [NDP] All routes for route (virtual) local fxxx::xxxx:5xxx:xxxx:3xxx:~0~ to remote fexx::xxxx:xxxx:xxxx:xxxx:~0~ are down
0000ab34.00007328::2014/06/10-07:54:34.099 INFO  [CORE] Node 8: executing node 12 failed handlers on a dedicated thread
0000ab34.00007328::2014/06/10-07:54:34.099 INFO  [NODE] Node 8: Cleaning up connections for n12.
0000ab34.00007328::2014/06/10-07:54:34.099 INFO  [Nodename] Clearing 0 unsent and 15 unacknowledged messages.
0000ab34.00007328::2014/06/10-07:54:34.099 INFO  [NODE] Node 8: n12 node object is closing its connections
0000ab34.00008b68::2014/06/10-07:54:34.099 INFO  [DCM] HandleNetftRemoteRouteChange
0000ab34.00004b38::2014/06/10-07:54:34.099 INFO  [IM] Route history 1: Old: 05.936, Message: Response, Route sequence: 150415, Received sequence: 150415, Heartbeats counter/threshold: 5/5, Error: Success, NtStatus: 0 Timestamp: 2014/06/10-07:54:28.000, Ticks since last sending: 4
0000ab34.00007328::2014/06/10-07:54:34.099 INFO  [NODE] Node 8: closing n12 node object channels
0000ab34.00004b38::2014/06/10-07:54:34.099 INFO  [IM] Route history 2: Old: 06.434, Message: Request, Route sequence: 150414, Received sequence: 150402, Heartbeats counter/threshold: 5/5, Error: Success, NtStatus: 0 Timestamp: 2014/06/10-07:54:27.665, Ticks since last sending: 36
0000ab34.0000a8ac::2014/06/10-07:54:34.099 INFO  [DCM] HandleRequest: dcm/netftRouteChange
0000ab34.00004b38::2014/06/10-07:54:34.099 INFO  [IM] Route history 3: Old: 06.934, Message: Response, Route sequence: 150414, Received sequence: 150414, Heartbeats counter/threshold: 5/5, Error: Success, NtStatus: 0 Timestamp: 2014/06/10-07:54:27.165, Ticks since last sending: 4
0000ab34.00004b38::2014/06/10-07:54:34.099 INFO  [IM] Route history 4: Old: 07.434, Message: Request, Route sequence: 150413, Received sequence: 150401, Heartbeats counter/threshold: 5/5, Error: Success, NtStatus: 0 Timestamp: 2014/06/10-07:54:26.664, Ticks since last sending: 36
```

```console
0000ab34.00007328::2014/06/10-07:54:34.100 INFO    <realLocal>10.xxx.xx.xxx:~3343~</realLocal>
0000ab34.00007328::2014/06/10-07:54:34.100 INFO    <realRemote>10.xxx.xx.xxx:~3343~</realRemote>
0000ab34.00007328::2014/06/10-07:54:34.100 INFO    <virtualLocal>fexx::xxxx:xxxx:xxxx:xxxx:~0~</virtualLocal>
0000ab34.00007328::2014/06/10-07:54:34.100 INFO    <virtualRemote>fexx::xxxx:xxxx:xxxx:xxxx:~0~</virtualRemote>
0000ab34.00007328::2014/06/10-07:54:34.100 INFO    <Delay>1000</Delay>
0000ab34.00007328::2014/06/10-07:54:34.100 INFO    <Threshold>5</Threshold>
0000ab34.00007328::2014/06/10-07:54:34.100 INFO    <Priority>140481</Priority>
0000ab34.00007328::2014/06/10-07:54:34.100 INFO    <Attributes>2147483649</Attributes>
0000ab34.00007328::2014/06/10-07:54:34.100 INFO  </struct mscs::FaultTolerantRoute>
0000ab34.00007328::2014/06/10-07:54:34.100 INFO   removed
```

```console
0000ab34.0000a7c0::2014/06/10-07:54:38.433 ERR   [QUORUM] Node 8: Lost quorum (3 4 5 6 7 8)
0000ab34.0000a7c0::2014/06/10-07:54:38.433 ERR   [QUORUM] Node 8: goingAway: 0, core.IsServiceShutdown: 0
0000ab34.0000a7c0::2014/06/10-07:54:38.433 ERR   lost quorum (status = 5925)
```

## <a name="cause"></a>Causa

Sono disponibili due impostazioni che consentono di configurare lo stato di connettività del cluster.

**Delay** : definisce la frequenza con cui vengono inviati gli heartbeat del cluster tra i nodi.  Il ritardo indica il numero di secondi prima dell'invio dell'heartbeat successivo.  All'interno dello stesso cluster possono esserci diversi ritardi tra i nodi nella stessa subnet e tra i nodi che si trovano in subnet diverse.

**Threshold** : definisce il numero di heartbeat mancanti prima che il cluster prenda un'azione di ripristino.  La soglia è costituita da un numero di heartbeat.  All'interno dello stesso cluster possono essere presenti soglie diverse tra i nodi nella stessa subnet e tra i nodi che si trovano in subnet diverse.

Per impostazione predefinita, Windows Server 2016 imposta **SameSubnetThreshold** su 10 e **SameSubnetDelay** su 1000 ms. Se, ad esempio, il monitoraggio della connettività ha esito negativo per 10 secondi, viene raggiunta la soglia di failover che determina la rimozione del nodo non raggiungibile dall'appartenenza al cluster. Ciò comporta lo spostamento delle risorse in un altro nodo disponibile nel cluster. Verranno segnalati gli errori del cluster, incluso l'errore del cluster 1135 (sopra).

## <a name="resolution"></a>Soluzione

In un ambiente IaaS, attenuare le impostazioni di configurazione della rete cluster.

### <a name="steps-to-verify-current-configuration"></a>Passaggi per verificare la configurazione corrente

Controllare le impostazioni di configurazione della rete cluster correnti usare il comando Get-cluster:

```powershell
C:\Windows\system32> get-cluster | fl *subnet*
```

Valori predefiniti, minimi, massimi e consigliati per ogni sistema operativo di supporto

|   |OS|Min|Max|Predefinito|Consigliato|
|---|---|---|---|---|---|
|CrossSubnetThreshold|2008 R2|3|20|5|20|
|Soglia CrossSubnet|2012|3|120|5|20|
|Soglia CrossSubnet|2012 R2|3|120|5|20|
|Soglia CrossSubnet|2016|3|120|20|20|
|Soglia SameSubnet|2008 R2|3|10|5|10|
|Soglia SameSubnet|2012|3|120|5|10
|Soglia SameSubnet|2012 R2|3|120|5|10|
|SameSubnetThreshold|2016|3|120|10|10|
|||||||

I valori per threshold riflettono le raccomandazioni correnti relative all'ambito della distribuzione, come descritto nell'articolo seguente:

[Ottimizzazione delle soglie di rete del cluster di failover in Windows Server 2012 R2](https://support.microsoft.com/en-us/help/3153887/fine-tuning-failover-cluster-network-thresholds-in-windows-server-2012)

La **soglia** definisce il numero di heartbeat mancanti prima che il cluster prenda un'azione di ripristino.  La soglia è costituita da un numero di heartbeat.  All'interno dello stesso cluster possono essere presenti soglie diverse tra i nodi nella stessa subnet e tra i nodi che si trovano in subnet diverse.

## <a name="recommendations-for-changing-to-more-relaxed-settings-for-multi-tenant-environments-like-azure-iaas"></a>Suggerimenti per la modifica di impostazioni più flessibili per ambienti multi-tenant come Azure (IaaS)

> [!NOTE]
> L'aumento della resilienza dell'ambiente cluster mediante la modifica delle impostazioni di configurazione della rete cluster può comportare un aumento del tempo di inattività. Per ulteriori informazioni, vedere [impostazione delle soglie di rete del cluster di failover](https://techcommunity.microsoft.com/t5/failover-clustering/tuning-failover-cluster-network-thresholds/ba-p/371834).

1. Modificare in impostazioni più flessibili:

    > [!NOTE]
    > La modifica della soglia del cluster diventa effettiva immediatamente, non è necessario riavviare il cluster o le risorse.

    Per le distribuzioni di gruppi di disponibilità AlwaysOn sono consigliate le impostazioni seguenti per la stessa subnet e per le distribuzioni tra aree diverse.

    ```powershell
    C:\Windows\system32> (get-cluster).SameSubnetThreshold = 20
    ```

    ```powershell
    C:\Windows\system32> (get-cluster).CrossSubnetThreshold = 20
    ```

2. Verificare le modifiche:

    ```powershell
    C:\Windows\system32> get-cluster | fl *subnet*
    ```

    :::image type="content" source="media/iaas-sql-failover-cluster/cmd.png" alt-text="cmd" border="false":::

## <a name="references"></a>Riferimenti

Per ulteriori informazioni sull'ottimizzazione delle impostazioni di configurazione della rete cluster Windows, vedere [ottimizzazione delle soglie di rete del cluster di failover](https://techcommunity.microsoft.com/t5/failover-clustering/tuning-failover-cluster-network-thresholds/ba-p/371834).

Per informazioni sull'utilizzo di cluster. exe per ottimizzare le impostazioni di configurazione della rete cluster Windows, vedere [How to configure cluster Networks for a failover cluster](/previous-versions/office/exchange-server-2007/bb690953(v=exchg.80)?redirectedfrom=MSDN).
