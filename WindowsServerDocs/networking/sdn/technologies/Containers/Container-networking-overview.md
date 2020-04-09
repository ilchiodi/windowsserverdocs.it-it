---
title: Panoramica della rete di contenitori
description: Questo argomento è una panoramica dello stack di rete per i contenitori di Windows e include collegamenti a indicazioni aggiuntive sulla creazione, la configurazione e la gestione di reti di contenitori.
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 318659e5-e4a5-4e46-99d6-211dfc46f6b8
ms.author: anpaul
author: AnirbanPaul
ms.date: 09/04/2018
ms.openlocfilehash: 8e2f10c7577f6f271c3766cff9a4fb1cc9d41140
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854314"
---
# <a name="container-networking-overview"></a>Panoramica della rete di contenitori

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Questo argomento fornisce una panoramica dello stack di rete per i contenitori di Windows e include collegamenti a indicazioni aggiuntive sulla creazione, la configurazione e la gestione di reti di contenitori.

I contenitori di Windows Server sono un metodo di virtualizzazione del sistema operativo semplice che separa le applicazioni o i servizi da altri servizi in esecuzione nello stesso host contenitore. I contenitori di Windows funzionano in modo analogo alle macchine virtuali. Se abilitata, ogni contenitore dispone di una visualizzazione separata del sistema operativo, dei processi, file system, del registro di sistema e degli indirizzi IP, che è possibile connettere alle reti virtuali. 

Un contenitore di Windows condivide un kernel con l'host contenitore e tutti i contenitori in esecuzione nell'host. Poiché lo spazio kernel è condiviso, è necessario che questi contenitori abbiano la stessa versione di kernel e la stessa configurazione. I contenitori forniscono l'isolamento delle applicazioni tramite la tecnologia di isolamento processo e spazio dei nomi.

>[!IMPORTANT]
>I contenitori di Windows non forniscono un limite di sicurezza ostile e non devono essere utilizzati per isolare il codice non attendibile. 

Con i contenitori di Windows, è possibile distribuire un host Hyper-V, in cui è possibile creare una o più macchine virtuali negli host VM. All'interno degli host di macchine virtuali, i contenitori vengono creati e l'accesso alla rete avviene tramite un commutire virtuale in esecuzione all'interno della macchina virtuale. È possibile usare le immagini riutilizzabili archiviate in un repository per distribuire il sistema operativo e i servizi in contenitori. Ogni contenitore dispone di una scheda di rete virtuale che si connette a un Commuter virtuale, che invia il traffico in ingresso e in uscita. È possibile alleghi gli endpoint del contenitore a una rete host locale (ad esempio NAT), alla rete fisica o alla rete virtuale sovrapposta creata tramite lo stack SDN.

Per applicare l'isolamento tra i contenitori nello stesso host, è necessario creare un raggruppamento di rete per ogni contenitore di Windows Server e Hyper-V. I contenitori di Windows Server usano una scheda vNIC host per il collegamento al commutatore virtuale. I contenitori di Hyper-V usano una scheda NIC VM sintetica, non visibile alla VM delle utilità, per il collegamento al commutatore virtuale. 

## <a name="related-topics"></a>Argomenti correlati 

- [Rete di contenitori di Windows](https://docs.microsoft.com/virtualization/windowscontainers/container-networking/architecture): informazioni su come creare e gestire le reti di contenitori per le distribuzioni non sovrapposte/Sdn.

- [Connettere gli endpoint del contenitore a una rete virtuale tenant](../../manage/Connect-container-endpoints-to-a-Tenant-Virtual-Network.md): informazioni su come creare e gestire le reti di contenitori per le reti virtuali sovrapposte con Sdn. 