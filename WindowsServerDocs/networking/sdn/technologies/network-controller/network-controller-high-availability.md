---
title: Disponibilità elevata del controller di rete
description: È possibile utilizzare questo argomento per informazioni sulla disponibilità elevata di Controller di rete per reti SDN (Software Defined) in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 334b090d-bec4-4e67-8307-13831dbdd1d8
ms.author: pashort
author: shortpatti
ms.openlocfilehash: dbd3ae9f4c1f1fc3035fae9ace880046312df2f0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813352"
---
# <a name="network-controller-high-availability"></a>Disponibilità elevata del controller di rete

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per informazioni sui Controller di rete ad alta disponibilità e scalabilità configurazione per Software Defined Networking \(SDN\).

Quando si distribuisce SDN nel tuo Data Center, è possibile usare Controller di rete per distribuire, monitorare e gestire molti elementi di rete, compresi i criteri di rete virtuali gateway RAS, servizi di bilanciamento del carico Software, per la comunicazione di tenant, in modo centralizzato Firewall del centro dati i criteri QoS \(QoS\) per SDN criteri, criteri di rete ibrida e altro ancora.

Poiché i Controller di rete è l'elemento fondamentale della gestione di rete SDN, è fondamentale per le distribuzioni di Controller di rete fornire la disponibilità elevata e la capacità per l'utente a facilmente scalabilità verso l'alto o verso il basso i nodi di Controller di rete adatta alle esigenze dei Data Center.

Sebbene sia possibile distribuire Controller di rete come cluster singolo computer, per il failover e la disponibilità elevata è necessario distribuire Controller di rete in un cluster a più computer con un minimo di tre macchine.

>[!NOTE]
>È possibile distribuire Controller di rete su computer server o nelle macchine virtuali \(macchine virtuali\) che eseguono Windows Server 2016 Datacenter edition. Se si distribuiscono Controller di rete nelle macchine virtuali, le macchine virtuali devono essere in esecuzione negli host Hyper-V che eseguono anche Datacenter edition. Controller di rete non è disponibile in Windows Server 2016 Standard edition.

## <a name="network-controller-as-a-service-fabric-application"></a>Controller di rete come un'applicazione di Service Fabric

Per ottenere scalabilità e disponibilità elevata, Controller di rete si basa su Service Fabric. Service Fabric offre una piattaforma di sistemi distribuiti per realizzare scalabili, affidabili e facilmente gestibili le applicazioni.

Come piattaforma di Service Fabric offre funzionalità che è necessario per la creazione di un sistema distribuito scalabile. Fornisce servizi di hosting su più istanze del sistema operativo, sincronizzazione delle informazioni di stato tra le istanze, designando come leader, rilevamento degli errori, bilanciamento del carico e altro ancora.

>[!NOTE]
>Per informazioni su Service Fabric in Azure, vedere [Panoramica di Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview).

Quando si distribuisce Controller di rete su più computer, Controller di rete viene eseguito come un'unica applicazione di Service Fabric in un cluster di Service Fabric. È possibile formare un cluster di Service Fabric tramite la connessione di un set di istanze del sistema operativo.

L'applicazione Controller di rete è costituita da più servizi di Service Fabric con stati. Ogni servizio è responsabile di una funzione di rete, ad esempio la gestione della rete fisica, la gestione della rete virtuale, la gestione del firewall o Gestione gateway. 

Ogni servizio di Service Fabric è una replica primaria e due repliche secondarie. La replica primaria del servizio elabora le richieste, anche se le due repliche secondarie del servizio forniscono la disponibilità elevata in circostanze in cui la replica primaria è disabilitato o non disponibile per qualche motivo.

La figura seguente illustra un cluster di Service Fabric Controller di rete con i cinque computer. Quattro servizi vengono distribuiti tra i cinque computer: Firewall del servizio, il servizio Gateway, Software Load Balancing \(SLB\) servizio e la rete virtuale \(Vnet\) servizio.  Ognuno dei quattro servizi include una replica di servizi primaria e due repliche secondarie del servizio.

![Cluster di Service Fabric di Controller di rete](../../../media/Network-Controller-HA/Network-Controller-HA.jpg)

## <a name="advantages-of-using-service-fabric"></a>Vantaggi dell'uso di Service Fabric

Di seguito sono illustrati i vantaggi principali per l'uso di Service Fabric per i cluster di Controller di rete.

### <a name="high-availability-and-scalability"></a>Disponibilità elevata e scalabilità

Perché il Controller di rete è alla base di una rete di Data Center, deve sia sia resiliente agli errori e deve essere sufficientemente scalabile per consentire le modifiche agile nelle reti di Data Center nel corso del tempo. Le funzionalità seguenti forniscono queste capacità: 

- **Failover rapido**. Service Fabric fornisce failover molto rapido. Più repliche dei servizi di secondario attivo sono sempre disponibili. Se un'istanza del sistema operativo non è più disponibile a causa di un errore hardware, una delle repliche secondarie immediatamente viene promosso alla replica primaria. 
- **Agilità di scala**. È possibile facilmente e rapidamente scalabilità questi servizi affidabili da poche istanze fino a migliaia di istanze e quindi nuovamente verso il basso in alcuni casi, a seconda delle esigenze di risorse. 

### <a name="persistent-storage"></a>Archivio permanente

L'applicazione Controller di rete presenta requisiti di archiviazione di grandi dimensioni per la configurazione e lo stato. L'applicazione deve inoltre essere utilizzabile tra interruzioni pianificate e non pianificate. A tale scopo, Service Fabric offre un Store chiave-valore \(KVS\) che è un archivio replicato, transazionale e persistente.

### <a name="modularity"></a>Modularità

Controller di rete è progettato con un'architettura modulare, con tutti i servizi di rete, ad esempio il servizio reti virtuali e firewall, compilato\-in come singoli servizi. 

Questa architettura di applicazione offre i vantaggi seguenti.

1. Controller di rete modularità consente lo sviluppo indipendente della ognuno dei servizi supportati, come deve evolversi. Ad esempio, il servizio di bilanciamento del carico Software può essere aggiornato senza influire su alcune di altri servizi o il normale funzionamento del Controller di rete.
2. La modularità di Controller di rete consente l'aggiunta di nuovi servizi, come l'evoluzione della rete. Nuovi servizi possono essere aggiunti al Controller di rete senza conseguenze per i servizi esistenti.

>[!NOTE]
>In Windows Server 2016, l'aggiunta di servizi di terze parti al Controller di rete non è supportato.

La modularità di Service Fabric Usa gli schemi del modello di servizio per massimizzare la facilità di sviluppo, distribuzione e manutenzione di un'applicazione.

## <a name="network-controller-deployment-options"></a>Opzioni di distribuzione di Controller di rete

Distribuire Controller di rete con System Center Virtual Machine Manager \(VMM\), vedere [configurare un controller di rete SDN nell'infrastruttura di VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller).

Per distribuire Controller di rete tramite script, vedere [distribuire un Software di rete dell'infrastruttura usando gli script definiti](../../deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md).

Per distribuire Controller di rete tramite Windows PowerShell, vedere [distribuire Controller di rete tramite Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)

Per altre informazioni sui Controller di rete, vedere [Controller di rete](Network-Controller.md).