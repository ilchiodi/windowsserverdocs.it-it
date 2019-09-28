---
title: Linee guida per la configurazione della scheda di interfaccia di rete (NIC) convergenti
description: Scheda di interfaccia di rete (NIC) convergente consente di esporre RDMA tramite una NIC virtuale a partizione host (vNIC) in modo che i servizi di partizione host possano accedere a accesso diretto a memoria remota (RDMA) sulle stesse schede di interfaccia di rete usate dagli utenti guest Hyper-V per il traffico TCP/IP.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: d7642338-9b33-4dce-8100-8b2c38d7127a
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: d791e0d51278d1f83f344250d38b1c7005c1a14a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355443"
---
# <a name="converged-network-interface-card-nic-configuration-guidance"></a>Scheda di interfaccia di rete convergente \(NIC @ no__t-1 Guida alla configurazione

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Scheda di interfaccia di rete convergente \(NIC @ no__t-1 consente di esporre RDMA tramite un host @ no__t-2partition virtuali NIC \(vNIC @ no__t-4 in modo che i servizi di partizione host possano accedere a accesso diretto a memoria remota \(RDMA @ no__t-6 sulle stesse schede NIC che i guest Hyper-V utilizzino per il traffico TCP/IP.

Prima della funzionalità NIC convergente, Management \(host Partition @ no__t-1 Services che volevano usare RDMA era necessario usare le schede di interfaccia di rete RDMA @ no__t-2capable dedicate, anche se la larghezza di banda era disponibile sulle schede di interfaccia di rete associate al Commuter virtuale Hyper-V.

Con la scheda di interfaccia di rete convergente, i due carichi di lavoro @no__t 0management gli utenti di RDMA e il traffico Guest @ no__t-1 possono condividere le stesse schede di interfaccia di rete fisiche, consentendo di installare meno schede di interfaccia di rete nei server.

Quando si distribuisce una scheda di interfaccia di rete convergente con gli host Hyper-v di Windows Server 2016 e i commutatori virtuali Hyper-V, schede negli host Hyper-V espongono i servizi RDMA per ospitare i processi usando RDMA su qualsiasi tecnologia Ethernet @ no__t-0based RDMA.

>[!NOTE]
>Per utilizzare la tecnologia NIC convergente, le schede di rete certificate nei server devono supportare RDMA.

In questa guida sono disponibili due set di istruzioni, una per le distribuzioni in cui i server dispongono di una singola scheda di rete installata, ovvero una distribuzione di base di una scheda di interfaccia di rete convergente. e un altro set di istruzioni in cui i server dispongono di due o più schede di rete installate, ovvero una distribuzione di una scheda di interfaccia di rete convergente tramite un switch Embedded Teaming \(SET @ no__t-1 team delle schede di rete RDMA @ no__t-2capable.


## <a name="prerequisites"></a>Prerequisiti

Di seguito sono riportati i prerequisiti per le distribuzioni di base e Datacenter di una scheda di interfaccia di rete convergente.

>[!NOTE]
>Per gli esempi forniti, viene usata una scheda Ethernet Mellanox ConnectX-3 Pro 40 Gbps, ma è possibile usare una delle schede di rete RDMA @ no__t-0capable certificate di Windows Server che supportano questa funzionalità. Per ulteriori informazioni sulle schede di rete compatibili, vedere [l'argomento relativo](https://www.windowsservercatalog.com/results.aspx?&bCatID=1468&cpID=0&avc=85&ava=0&avt=0&avq=46&OR=1)al catalogo di Windows Server.

### <a name="basic-converged-nic-prerequisites"></a>Prerequisiti di base per NIC convergenti

Per eseguire i passaggi descritti in questa guida per la configurazione di base della scheda di interfaccia di rete convergente, è necessario disporre di quanto segue.

- Due server che eseguono Windows Server 2016 Datacenter Edition o Windows Server 2016 Standard Edition.
- Una scheda di rete certificata con supporto per RDMA installata in ogni server.
- Ruolo server Hyper-V installato in ogni server.

### <a name="datacenter-converged-nic-prerequisites"></a>Prerequisiti NIC convergenti per il Data Center

Per eseguire i passaggi illustrati in questa guida per la configurazione NIC convergente di datacenter, è necessario disporre di quanto segue.

- Due server che eseguono Windows Server 2016 Datacenter Edition o Windows Server 2016 Standard Edition.
- Due schede di rete certificate con supporto per RDMA installate in ogni server.
- Ruolo server Hyper-V installato in ogni server.
- È necessario avere familiarità con switch Embedded Teaming \(SET @ no__t-1, una soluzione di gruppo NIC alternativa usata in ambienti che includono Hyper-V e lo stack SDN (Software Defined Networking) in Windows Server 2016. SET integra alcune funzionalità di gruppo NIC nel Commuter virtuale Hyper-V. Per ulteriori informazioni, vedere [accesso diretto a memoria remota (RDMA) e switch Embedded Teaming (set)](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).

## <a name="related-topics"></a>Argomenti correlati
- [Configurazione NIC convergente con una singola scheda di rete](cnic-single.md)
- [Configurazione NIC convergente con gruppo NIC](cnic-datacenter.md)
- [Configurazione del Commuter fisico per NIC convergente](cnic-app-switch-config.md)
- [Risoluzione dei problemi relativi alle configurazioni NIC convergenti](cnic-app-troubleshoot.md)

---