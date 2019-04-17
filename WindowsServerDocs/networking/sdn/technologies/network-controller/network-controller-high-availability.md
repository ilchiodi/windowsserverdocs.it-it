---
title: Disponibilità elevata del Controller di rete
description: È possibile utilizzare questo argomento per apprendere la disponibilità elevata di Controller di rete per reti SDN (Software Defined) in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 334b090d-bec4-4e67-8307-13831dbdd1d8
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f260f3e4d8ca5fcd998824327478c2fbe3c81875
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="network-controller-high-availability"></a>Disponibilità elevata del Controller di rete

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per informazioni sulla configurazione di scalabilità per la rete definita dal Software \(SDN\) e la disponibilità elevata di Controller di rete.

Quando si distribuisce SDN nel centro dati, è possibile utilizzare Controller di rete per distribuire centralmente, monitorare e gestire diversi elementi di rete, inclusi gateway RAS, servizi di bilanciamento del carico Software, criteri di rete virtuali per la comunicazione di tenant, i criteri Firewall del centro dati, Quality of Service \(QoS\) per SDN criteri di rete ibrida, criteri e altro.

Poiché i Controller di rete è la base di gestione di SDN, è fondamentale per le distribuzioni di Controller di rete fornire elevata disponibilità e la possibilità per te scalare facilmente verso l'alto o verso il basso nodi di Controller di rete con le esigenze di Data Center.

Sebbene sia possibile distribuire Controller di rete come un cluster singolo computer, per la disponibilità elevata e il failover è necessario distribuire Controller di rete in un cluster a più computer con un minimo di tre computer.

>[!NOTE]
>È possibile distribuire Controller di rete in uno dei due computer server o in \(VMs\) macchine virtuali che eseguono Windows Server 2016 Datacenter edition. Se si distribuiscono Controller di rete nelle macchine virtuali, le macchine virtuali devono essere in esecuzione in host Hyper-V che eseguono anche Datacenter edition. Controller di rete non è disponibile in Windows Server 2016 Standard edition.

## <a name="network-controller-as-a-service-fabric-application"></a>Controller di rete come un'applicazione di infrastruttura del servizio

Per ottenere disponibilità e scalabilità elevate, Controller di rete si basa sull'infrastruttura del servizio. Service Fabric fornisce una piattaforma di sistemi distribuiti per compilare scalabili e affidabili e facile da gestire le applicazioni.

Come piattaforma, Service Fabric fornisce funzionalità che è necessario per la creazione di un sistema distribuito scalabile. Fornisce l'hosting di servizi per più istanze del sistema operativo, sincronizzazione delle informazioni di stato tra le istanze, possibilità di scegliere una linea guida, il rilevamento degli errori, il bilanciamento del carico e altro.

>[!NOTE]
>Per informazioni su Service Fabric in Azure, vedere [Panoramica di Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview).

Quando si distribuita Controller di rete su più computer, Controller di rete viene eseguito come una singola applicazione Service Fabric in un cluster di infrastruttura del servizio. Si può costituire un cluster di infrastruttura servizio tramite la connessione a un gruppo di istanze del sistema operativo.

L'applicazione Controller di rete è costituita da più servizi di infrastruttura servizio informazioni sullo stato. Ogni servizio è responsabile per una funzione di rete, ad esempio gestione di rete fisica, gestione di rete virtuale, dei firewall o Gestione gateway. 

Ogni servizio Service Fabric dispone di una replica primaria e due repliche secondarie. La replica primaria servizio elabora le richieste, mentre le repliche del servizio secondario due garantire un'elevata disponibilità nei casi in cui la replica primaria è disabilitato o non disponibile per qualche motivo.

L'illustrazione seguente mostra un cluster di infrastruttura servizio Controller di rete con cinque computer. Quattro servizi distribuiti in cinque macchine: servizio Firewall, il servizio Gateway, il servizio di bilanciamento del carico Software \(SLB\) e servizio di rete virtuale \(Vnet\).  Ognuno dei quattro servizi include una replica servizio primario e due repliche servizio secondario.

![Cluster di infrastruttura servizio Controller di rete](../../../media/Network-Controller-HA/Network-Controller-HA.jpg)

## <a name="advantages-of-using-service-fabric"></a>Vantaggi dell'utilizzo dell'infrastruttura del servizio

Di seguito sono illustrati i vantaggi principali per l'uso dell'infrastruttura del servizio per i cluster di Controller di rete.

### <a name="high-availability-and-scalability"></a>Disponibilità elevata e scalabilità

Poiché i Controller di rete è alla base di una rete di centri dati, deve sia resilienza agli errori e scalabile sufficiente per consentire le modifiche agile in reti di Data Center nel tempo. Le funzionalità seguenti offrono questa capacità: 

- **Failover veloce**. Service Fabric fornisce il failover estremamente veloce. Più repliche a caldo servizio secondari sono sempre disponibili. Se un'istanza del sistema operativo non diventa disponibile a causa di errore hardware, una delle repliche secondarie viene promosso immediatamente a replica primaria. 
- **Flessibilità di scala**. Puoi facilmente e velocemente scalare tali servizi affidabile da alcune istanze fino a migliaia di istanze e quindi nuovamente verso il basso in alcuni casi, a seconda delle esigenze di risorse. 

### <a name="persistent-storage"></a>Archivio permanente

L'applicazione Controller di rete ha requisiti di archiviazione di grandi dimensioni per la configurazione e dello stato. L'applicazione deve essere utilizzabile tra le interruzioni pianificate e non pianificate. A tale scopo, Service Fabric fornisce un \(KVS\) Store chiave-valore che è un archivio persistente e transazionale replicato.

### <a name="modularity"></a>Modularità

Controller di rete è stato progettato con un'architettura modulare, con ognuno dei servizi di rete, ad esempio il servizio di reti virtuali e firewall, predefinita-in come singoli servizi. 

Questa architettura applicativa offre i vantaggi seguenti.

1. Controller di rete modularità consente lo sviluppo indipendente di ognuno dei servizi supportati, come deve evolversi. Ad esempio, il servizio di bilanciamento del carico Software può essere aggiornato senza influire su altri servizi o il normale funzionamento del Controller di rete.
2. La modularità Controller di rete consente l'aggiunta di nuovi servizi, come si evolve della rete. Nuovi servizi possono essere aggiunti al Controller di rete senza alcun impatto servizi esistenti.

>[!NOTE]
>In Windows Server 2016, non è supportata l'aggiunta di servizi di terze parti al Controller di rete.

La modularità Service Fabric Usa gli schemi di modello di servizio per ottimizzare la facilità di sviluppo, la distribuzione e manutenzione di un'applicazione.

## <a name="network-controller-deployment-options"></a>Opzioni di distribuzione di Controller di rete

Per distribuire Controller di rete utilizzando \(VMM\) System Center Virtual Machine Manager, vedere [configurare un controller di rete SDN nell'infrastruttura VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller).

Per distribuire Controller di rete tramite script, vedere [distribuire un Software definito dell'infrastruttura utilizzando script di rete](../../deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md).

Per distribuire Controller di rete tramite Windows PowerShell, vedere [distribuire Controller di rete tramite Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)

Per ulteriori informazioni sui Controller di rete, vedere [Controller di rete](Network-Controller.md).