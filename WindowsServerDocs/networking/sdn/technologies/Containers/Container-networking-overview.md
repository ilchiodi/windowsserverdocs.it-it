---
title: Panoramica delle reti contenitore
description: In questo argomento viene fornita una panoramica dello stack di rete per i contenitori di Windows e include collegamenti a ulteriori istruzioni sulla creazione, configurazione e la gestione delle reti di contenitore.
manager: ravirao
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 318659e5-e4a5-4e46-99d6-211dfc46f6b8
ms.author: pashort
author: jmesser81
ms.openlocfilehash: fd2f022948208d4aacce2994ff053e77384b28fc
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="container-networking-overview"></a>Panoramica delle reti contenitore

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

In questo argomento viene fornita una panoramica dello stack di rete per i contenitori di Windows e include collegamenti a ulteriori istruzioni sulla creazione, configurazione e la gestione delle reti di contenitore.

Contenitori di Windows Server sono un metodo di virtualizzazione del sistema operativo leggero usato per separare applicazioni o servizi da altri servizi in esecuzione nello stesso host contenitore. A tale scopo, ogni contenitore dispone di una propria vista del sistema operativo, i processi, file system, del Registro di sistema e gli indirizzi IP.

Contenitori di Windows funzionano in modo analogo alle macchine virtuali in merito alla rete. Ogni contenitore dispone di una scheda di rete virtuale connessa a un commutatore virtuale, in cui viene inoltrato il traffico in entrata e in uscita. Per applicare l'isolamento tra i contenitori nell'host stesso, viene creato un raggruppamento di rete per ogni Server di Windows e il contenitore di Hyper-V in cui è installata la scheda di rete per il contenitore. Contenitori di Windows Server utilizzano una scheda di rete Host per associare al commutatore virtuale. Contenitori di Hyper-V, utilizzare una scheda di rete VM sintetica (non esposte alla macchina virtuale di utilità) per associare al commutatore virtuale. 

Gli endpoint del contenitore possono essere collegati a una rete dell'host locale (ad esempio NAT), la rete fisica o una rete virtuale sovrimpressione creato attraverso lo stack di Microsoft di rete SDN (Software Defined). 

Per ulteriori informazioni sulla creazione e la gestione delle reti contenitore per le distribuzioni sovrimpressione/SDN, vedere il [rete contenitore Windows](https://msdn.microsoft.com/en-us/virtualization/windowscontainers/management/container_networking) Guida su MSDN.

Per ulteriori informazioni sulla creazione e la gestione delle reti contenitore per le reti virtuali con SDN sovrimpressione, fare riferimento a [connettere gli endpoint del contenitore a una rete virtuale tenant ](../../manage/Connect-container-endpoints-to-a-Tenant-Virtual-Network.md). 