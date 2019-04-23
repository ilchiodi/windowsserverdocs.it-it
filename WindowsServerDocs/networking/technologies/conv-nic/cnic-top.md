---
title: Linee guida di configurazione scheda di interfaccia di rete (NIC) convergente
description: Scheda di interfaccia di rete convergenti (NIC) consente di esporre RDMA tramite una partizione host scheda di rete virtuale (vNIC) in modo che il partizionamento dei servizi host possa accedere a Direct accesso memoria remota (RDMA) sulle schede di rete stesso che i guest Hyper-V Usa per il traffico TCP/IP.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d7642338-9b33-4dce-8100-8b2c38d7127a
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: e9f5180285dda790e11cec543a109d0cb58edd2d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838842"
---
# <a name="converged-network-interface-card-nic-configuration-guidance"></a>Scheda di interfaccia di rete convergente \(NIC\) indicazioni di configurazione

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Scheda di interfaccia di rete convergente \(NIC\) consente di esporre RDMA tramite un host\-interfaccia di rete virtuale di partizione \(vNIC\) in modo che il partizionamento dei servizi host può accedere Remote Direct Memory Access \(RDMA\) sulle schede di rete stesso che i guest Hyper-V Usa per il traffico TCP/IP.

Prima la funzionalità di interfaccia di rete convergente, management \(ospitano partizione\) i servizi che vogliono usare RDMA dovevano usare RDMA dedicata\-schede di rete in grado di supportare, anche se la larghezza di banda era disponibile le schede di rete che sono stati associati per il ruolo Hyper-V Commutatore virtuale.

Con convergente scheda di rete, i due carichi di lavoro \(agli utenti di gestione del traffico RDMA e Guest\) possono condividere le stesse schede NIC fisiche, che consente di installare un minor numero di schede di rete nei server.

Quando si distribuisce l'interfaccia di rete convergente con gli host Windows Server 2016 Hyper-V e i commutatori virtuali Hyper-V, le schede negli host Hyper-V espongono servizi RDMA a processi host usando RDMA over qualsiasi Ethernet\-basato su tecnologia RDMA.

>[!NOTE]
>Per usare la tecnologia di interfaccia di rete convergente, le schede di rete Certificate nei server devono supportare RDMA.

Questa guida offre due set di istruzioni, uno per le distribuzioni in cui i server hanno una sola scheda di rete installata, che è una distribuzione di base dell'interfaccia di rete convergente; e un altro set di istruzioni in cui i server hanno due o più schede di rete installate, che è una distribuzione di interfaccia di rete convergente tramite un Switch Embedded Teaming \(impostata\) team RDMA\-schede di rete in grado di supportare.


## <a name="prerequisites"></a>Prerequisiti

Di seguito sono i prerequisiti per le distribuzioni di base e Data Center di interfaccia di rete convergente.

>[!NOTE]
>Per gli esempi forniti, usiamo una scheda di Ethernet Mellanox ConnectX-3 Pro 40 Gbps, ma è possibile usare uno qualsiasi di Windows Server certificata RDMA\-schede di rete che supportano questa funzionalità è supportata. Per altre informazioni sulle schede di rete compatibili, vedere l'argomento Windows Server Catalog [schede di rete LAN](https://www.windowsservercatalog.com/results.aspx?&bCatID=1468&cpID=0&avc=85&ava=0&avt=0&avq=46&OR=1).

### <a name="basic-converged-nic-prerequisites"></a>Prerequisiti di interfaccia di rete convergente base

Per eseguire i passaggi descritti in questa guida per la configurazione di interfaccia di rete convergente base, è necessario disporre di quanto segue.

- Due server che eseguono Windows Server 2016 Datacenter edition o Windows Server 2016 Standard edition.
- Uno con supporto per RDMA, certified scheda di rete installato in ogni server.
- Ruolo server Hyper-V installato in ogni server.

### <a name="datacenter-converged-nic-prerequisites"></a>Prerequisiti di interfaccia di rete convergente datacenter

Per eseguire i passaggi descritti in questa guida per la configurazione di interfaccia di rete convergente Data Center, è necessario disporre di quanto segue.

- Due server che eseguono Windows Server 2016 Datacenter edition o Windows Server 2016 Standard edition.
- Due con supporto per RDMA, certified schede di rete installate in ogni server.
- Ruolo server Hyper-V installato in ogni server.
- È necessario avere familiarità con Switch Embedded Teaming \(impostare\), una soluzione usata in ambienti che includono Hyper-V e lo stack di rete SDN (Software Defined) in Windows Server 2016 alternativa gruppo NIC. SET integra alcune funzionalità gruppo NIC nel commutatore virtuale Hyper-V. Per altre informazioni, vedere [diretta accesso memoria remota (RDMA) e Switch Embedded Teaming (SET)](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).

## <a name="related-topics"></a>Argomenti correlati
- [Configurazione scheda di rete convergente con una sola scheda di rete](cnic-single.md)
- [Configurazione di interfaccia di rete convergente le NIC](cnic-datacenter.md)
- [Configurazione di commutatore fisico per la scheda di rete convergente](cnic-app-switch-config.md)
- [Risoluzione dei problemi di configurazioni di interfaccia di rete convergente](cnic-app-troubleshoot.md)

---