---
title: Disponibilità elevata del controller di rete
description: È possibile usare questo argomento per informazioni sulla disponibilità elevata del controller di rete per SDN (Software Defined Networking) in Windows Server 2016.
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 334b090d-bec4-4e67-8307-13831dbdd1d8
ms.author: anpaul
author: AnirbanPaul
ms.openlocfilehash: 3c6d18dcf1071eabaabe9acc29713a7b9a226a84
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859654"
---
# <a name="network-controller-high-availability"></a>Disponibilità elevata del controller di rete

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile usare questo argomento per informazioni sulla configurazione della disponibilità elevata e della scalabilità del controller di rete per Software Defined Networking \(SDN\).

Quando si distribuisce SDN nel Data Center, è possibile usare il controller di rete per distribuire, monitorare e gestire in modo centralizzato molti elementi di rete, inclusi i gateway RAS, i servizi di bilanciamento del carico software, i criteri di rete virtuale per la comunicazione dei tenant, i criteri del firewall dei Data Center, la qualità del servizio \(QoS\) per i criteri SDN, i criteri di rete ibrida

Poiché il controller di rete è l'elemento fondamentale della gestione SDN, è essenziale per le distribuzioni del controller di rete fornire disponibilità elevata e la possibilità di aumentare o ridurre facilmente i nodi del controller di rete con le esigenze del Data Center.

Sebbene sia possibile distribuire il controller di rete come cluster a computer singolo, per la disponibilità elevata e il failover è necessario distribuire il controller di rete in un cluster con più computer con un minimo di tre computer.

>[!NOTE]
>È possibile distribuire il controller di rete in computer server o in macchine virtuali \(VM\) che eseguono Windows Server 2016 Datacenter Edition. Se si distribuisce il controller di rete nelle macchine virtuali, le macchine virtuali devono essere in esecuzione in host Hyper-V che eseguono anche Datacenter Edition. Il controller di rete non è disponibile in Windows Server 2016 Standard Edition.

## <a name="network-controller-as-a-service-fabric-application"></a>Controller di rete come applicazione Service Fabric

Per ottenere la disponibilità elevata e la scalabilità, il controller di rete si basa su Service Fabric. Service Fabric offre una piattaforma di sistemi distribuiti per la creazione di applicazioni scalabili, affidabili e facilmente gestibili.

Come piattaforma, Service Fabric fornisce la funzionalità necessaria per la creazione di un sistema distribuito scalabile. Fornisce il servizio che ospita su più istanze del sistema operativo, la sincronizzazione delle informazioni sullo stato tra le istanze, la scelta di un leader, il rilevamento degli errori, il bilanciamento del carico e altro ancora.

>[!NOTE]
>Per informazioni sui Service Fabric in Azure, vedere [Panoramica di Service fabric di Azure](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview).

Quando si distribuisce il controller di rete in più computer, il controller di rete viene eseguito come singola applicazione Service Fabric in un cluster Service Fabric. È possibile creare un cluster Service Fabric connettendo un set di istanze del sistema operativo.

L'applicazione controller di rete è costituita da più servizi di Service Fabric con stato. Ogni servizio è responsabile di una funzione di rete, ad esempio la gestione della rete fisica, la gestione della rete virtuale, la gestione del firewall o la gestione del gateway. 

Ogni servizio Service Fabric ha una replica primaria e due repliche secondarie. La replica del servizio primaria elabora le richieste, mentre le due repliche del servizio secondarie forniscono una disponibilità elevata nei casi in cui la replica primaria è disabilitata o non disponibile per qualche motivo.

Nella figura seguente viene illustrato un controller di rete Service Fabric cluster con cinque computer. Quattro servizi vengono distribuiti tra i cinque computer: servizio firewall, servizio gateway, bilanciamento del carico software \(servizio di\) SLB e rete virtuale \(servizio\) vnet.  Ognuno dei quattro servizi include una replica del servizio primaria e due repliche del servizio secondarie.

![Cluster Service Fabric del controller di rete](../../../media/Network-Controller-HA/Network-Controller-HA.jpg)

## <a name="advantages-of-using-service-fabric"></a>Vantaggi dell'utilizzo di Service Fabric

Di seguito sono riportati i principali vantaggi per l'utilizzo di Service Fabric per i cluster di controller di rete.

### <a name="high-availability-and-scalability"></a>Disponibilità elevata e scalabilità

Poiché il controller di rete è il nucleo di una rete di Data Center, deve essere resiliente agli errori ed essere sufficientemente scalabile per consentire modifiche agili nelle reti dei Data Center nel tempo. Le funzionalità seguenti forniscono queste capacità: 

- **Failover veloce**. Service Fabric fornisce un failover estremamente veloce. Sono sempre disponibili più repliche del servizio secondario a caldo. Se un'istanza del sistema operativo non è più disponibile a causa di un errore hardware, una delle repliche secondarie viene immediatamente innalzata alla replica primaria. 
- **Agilità della scalabilità**. È possibile ridimensionare facilmente e rapidamente questi servizi affidabili da poche istanze fino a migliaia di istanze e quindi eseguire il backup in alcune istanze, a seconda delle esigenze delle risorse. 

### <a name="persistent-storage"></a>Archiviazione permanente

Per la configurazione e lo stato dell'applicazione controller di rete sono previsti requisiti di archiviazione di grandi dimensioni. L'applicazione deve essere inoltre utilizzabile nelle interruzioni pianificate e non pianificate. A tale scopo, Service Fabric fornisce un archivio chiave-valore \(KVS\) che è un archivio replicato, transazionale e permanente.

### <a name="modularity"></a>Modularità

Il controller di rete è progettato con un'architettura modulare, con ognuno dei servizi di rete, ad esempio il servizio di reti virtuali e il servizio firewall, compilati\-come singoli servizi. 

Questa architettura di applicazione offre i vantaggi seguenti.

1. La modularità del controller di rete consente lo sviluppo indipendente di ogni servizio supportato, in base alle esigenze. Il servizio di bilanciamento del carico software, ad esempio, può essere aggiornato senza influire sugli altri servizi o sul normale funzionamento del controller di rete.
2. La modularità del controller di rete consente l'aggiunta di nuovi servizi, man mano che la rete si evolve. I nuovi servizi possono essere aggiunti al controller di rete senza alcun effetto sui servizi esistenti.

>[!NOTE]
>In Windows Server 2016 l'aggiunta di servizi di terze parti al controller di rete non è supportata.

Service Fabric modularità utilizza gli schemi del modello di servizio per massimizzare la facilità di sviluppo, distribuzione e manutenzione di un'applicazione.

## <a name="network-controller-deployment-options"></a>Opzioni di distribuzione del controller di rete

Per distribuire il controller di rete usando System Center Virtual Machine Manager \(\)VMM, vedere [configurare un controller di rete Sdn nell'infrastruttura VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller).

Per distribuire il controller di rete usando gli script, vedere [distribuire un'infrastruttura software defined Network usando gli script](../../deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md).

Per distribuire il controller di rete con Windows PowerShell, vedere [distribuire un controller di rete con Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)

Per ulteriori informazioni sul controller di rete, vedere [controller di rete](Network-Controller.md).