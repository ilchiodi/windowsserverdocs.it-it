---
title: Panoramica della rete di contenitori
description: In questo argomento viene fornita una panoramica dello stack di rete per i contenitori Windows e include collegamenti a indicazioni aggiuntive sulla creazione, configurazione e gestione delle reti di contenitori.
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
ms.date: 09/04/2018
ms.openlocfilehash: 72b1ac739d9012ac7b90e97abe22e5f321ddba63
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886142"
---
# <a name="container-networking-overview"></a>Panoramica della rete di contenitori

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo argomento, Microsoft fornisce una panoramica dello stack di rete per i contenitori Windows e sono inclusi collegamenti a indicazioni aggiuntive sulla creazione, configurazione e gestione delle reti di contenitori.

Contenitori di Windows Server sono un metodo di virtualizzazione del sistema operativo leggero che separa le applicazioni o servizi da altri servizi eseguiti nello stesso host contenitore. Funzione di contenitori Windows in modo analogo alle macchine virtuali. Quando abilitata, ogni contenitore dispone di una visualizzazione separata del sistema operativo, processi, file system, Registro di sistema e indirizzi IP, che è possibile connettersi alle reti virtuali. 

Un contenitore Windows condivide un kernel con l'host contenitore e tutti i contenitori in esecuzione nell'host. Poiché lo spazio kernel è condiviso, è necessario che questi contenitori abbiano la stessa versione di kernel e la stessa configurazione. I contenitori offrono l'isolamento dell'applicazione tramite la tecnologia di isolamento dei processi e dello spazio dei nomi.

>[!IMPORTANT]
>I contenitori di Windows non forniscono un limite di sicurezza potenzialmente ostile e non devono essere utilizzati per isolare il codice non attendibile. 

Con i contenitori di Windows, è possibile distribuire un host Hyper-V, in cui si creano uno o più macchine virtuali negli host macchina virtuale. All'interno di host macchina virtuale, vengono creati i contenitori e l'accesso di rete avviene tramite un commutatore virtuale in esecuzione nella macchina virtuale. È possibile utilizzare riutilizzabili immagini archiviate in un repository per distribuire il sistema operativo e i servizi in contenitori. Ogni contenitore dispone di una scheda di rete virtuale che si connette a un commutatore virtuale, inoltrare il traffico in ingresso e in uscita. È possibile collegare gli endpoint del contenitore a una rete dell'host locale (ad esempio NAT), la rete fisica o rete virtuale di sovrapposizione create tramite lo stack SDN.

Per applicare l'isolamento tra contenitori nello stesso host, si crea un raggruppamento di rete per ogni contenitore di Windows Server e Hyper-V. I contenitori di Windows Server usano una scheda vNIC host per il collegamento al commutatore virtuale. I contenitori di Hyper-V usano una scheda NIC VM sintetica, non visibile alla VM delle utilità, per il collegamento al commutatore virtuale. 

## <a name="related-topics"></a>Argomenti correlati 

- [Rete di contenitori Windows](https://docs.microsoft.com/virtualization/windowscontainers/container-networking/architecture): Informazioni su come creare e gestire le reti di contenitori per le distribuzioni non SDN-overlay /.

- [Connettere gli endpoint del contenitore a una rete virtuale tenant](../../manage/Connect-container-endpoints-to-a-Tenant-Virtual-Network.md): Informazioni su come creare e gestire le reti di contenitori per le reti virtuali di sovrapposizione con SDN. 