---
title: Guida alla configurazione di rete convergente interfaccia scheda (NIC)
description: In questo argomento fa parte di convergenza NIC Configuration Guide per Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d7642338-9b33-4dce-8100-8b2c38d7127a
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 42f7ef674da1754253ad72ae30ad505f29ba6a7d
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="converged-network-interface-card-nic-configuration-guide"></a>Guida alla configurazione di rete convergente Interface Card \(NIC\)

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Scheda di interfaccia di rete convergente \(NIC\) consente di esporre RDMA tramite un \(vNIC\) NIC virtuale host\-partition in modo che i servizi di partizione host possano accedere \(RDMA\) Remote Direct Memory Access nelle stesse schede NIC che utilizzano il guest Hyper-V per il traffico TCP/IP.

Prima la funzionalità della scheda di rete convergente, servizi di gestione \(host partition\) che volevano usare RDMA erano necessario utilizzare schede di rete in grado di supportare RDMA\ dedicati, anche se la larghezza di banda era disponibile in schede di rete associata al commutatore virtuale Hyper-V.

Con convergenza NIC, i carichi di due lavoro \ (Gestione utenti della traffic\ RDMA e Guest) possono condividere le stesse schede NIC fisiche, che consente di installare un minor numero di schede di rete nel server.

Quando si distribuisce una scheda di rete convergente con gli host Windows Server 2016 Hyper-V e i commutatori virtuali Hyper-V, vNICs in host Hyper-V espongono RDMA servizi ai processi host tramite RDMA over qualsiasi tecnologia basata su Ethernet\ RDMA.

>[!NOTE]
>Per utilizzare la tecnologia NIC convergente, le schede di rete certificate nel server devono supportare RDMA.

Questa guida vengono fornite due set di istruzioni, uno per le distribuzioni in cui i server dispongono di una sola scheda di rete installata, che è una distribuzione di base della scheda di rete convergente; e un altro set di istruzioni in cui i server dispongono di due o più schede di rete installate, che è una distribuzione di scheda di rete convergente tramite un team Switch Embedded Teaming \(SET\) in grado di supportare RDMA\ delle schede di rete.

Questa guida contiene gli argomenti seguenti.

- [Configurazione di rete convergente con una sola scheda di rete](cnic-single.md)
- [Configurazione di rete convergente le NIC combinate](cnic-datacenter.md)
- [Configurazione di commutatore fisico per scheda di rete convergente](cnic-app-switch-config.md)
- [Risoluzione dei problemi convergente configurazioni NIC](cnic-app-troubleshoot.md)

## <a name="prerequisites"></a>Prerequisiti

Di seguito sono i prerequisiti per le distribuzioni di base e Datacenter di interfaccia di rete convergente.

>[!NOTE]
>Per esempi in questa Guida, viene utilizzata una scheda Ethernet Gbps 40 Pro Mellanox ConnectX-3, ma è possibile utilizzare uno dei certificati in grado di supportare RDMA\ schede di rete che supportano questa funzionalità Windows Server. Per ulteriori informazioni sulle schede di rete compatibili, vedere l'argomento Windows Server Catalog [schede LAN](https://www.windowsservercatalog.com/results.aspx?&bCatID=1468&cpID=0&avc=85&ava=0&avt=0&avq=46&OR=1).

### <a name="basic-converged-nic-prerequisites"></a>Prerequisiti di base convergente NIC

Per eseguire i passaggi in questa guida per la configurazione di base di scheda di rete convergente, è necessario disporre di quanto segue.

- Due server che eseguono Windows Server 2016 Datacenter edition o Windows Server 2016 Standard edition.
- Un supporto per RDMA, certificati scheda di rete installata in ogni server.
- Il ruolo server Hyper-V deve essere installato in ogni server.

### <a name="datacenter-converged-nic-prerequisites"></a>Prerequisiti di Data Center convergente NIC

Per eseguire i passaggi in questa guida per la configurazione Data Center convergente NIC, è necessario disporre di quanto segue.

- Due server che eseguono Windows Server 2016 Datacenter edition o Windows Server 2016 Standard edition.
- Due con supporto per RDMA, certificati schede di rete installate in ogni server.
- Il ruolo server Hyper-V deve essere installato in ogni server.
- Deve avere familiarità con Switch Embedded Teaming \(SET\), che costituisce un'alternativa soluzione gruppo NIC che è possibile utilizzare in ambienti che includono Hyper-V e lo stack di rete SDN (Software Defined) in Windows Server 2016. SET di alcune funzionalità gruppo NIC si integra il commutatore virtuale Hyper-V. Per ulteriori informazioni, vedere [diretta accesso memoria remota (RDMA) e Switch Embedded Teaming (SET)](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).

