---
title: Pianificare la rete Hyper-V in Windows Server
description: Vengono descritti gli elementi necessari per la rete di base in Hyper-V e vengono forniti i collegamenti alle istruzioni
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: KBDAzure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: f09bc82d0dd47d3393dd05dcf03913db11e4c335
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392511"
---
# <a name="plan-for-hyper-v-networking-in-windows-server"></a>Pianificare la rete Hyper-V in Windows Server

>Si applica a: Microsoft Hyper-V Server 2016, Windows Server 2016, Microsoft Hyper-V Server 2019, Windows Server 2019
  
Una conoscenza di base delle funzionalità di rete in Hyper-V consente di pianificare la rete per le macchine virtuali. Questo articolo illustra anche alcune considerazioni sulla rete quando si usa la migrazione in tempo reale e quando si usa Hyper-V con altre funzionalità e ruoli del server.  
  
## <a name="hyper-v-networking-basics"></a>Nozioni fondamentali sulla rete Hyper-V  
La rete di base in Hyper-V è piuttosto semplice. USA due parti: un commutire virtuale e una scheda di rete virtuale. È necessario almeno uno dei due per stabilire la rete per una macchina virtuale. Il Commuter virtuale si connette a qualsiasi rete basata su Ethernet. La scheda di rete virtuale si connette a una porta del Commuter virtuale, che consente a una macchina virtuale di usare una rete.  
  
Il modo più semplice per stabilire la rete di base consiste nel creare un commutire virtuale quando si installa Hyper-V. Quindi, quando si crea una macchina virtuale, è possibile connetterla al Commuter. La connessione al Commuter aggiunge automaticamente una scheda di rete virtuale alla macchina virtuale. Per istruzioni, vedere [creare un commutire virtuale per macchine virtuali Hyper-V](../get-started/Create-a-virtual-switch-for-Hyper-V-virtual-machines.md).  
  
Per gestire diversi tipi di rete, è possibile aggiungere commutatori virtuali e schede di rete virtuali. Tutti i commutatori fanno parte dell'host Hyper-V, ma ogni scheda di rete virtuale appartiene a una sola macchina virtuale.  
  
Il Commuter virtuale è uno switch di rete Ethernet di livello 2 Basato su software. Fornisce funzionalità predefinite per il monitoraggio, il controllo e la segmentazione del traffico, nonché per la sicurezza e la diagnostica.  È possibile aggiungere al set di funzionalità predefinite installando i plug-in, detti anche *estensioni*. Sono disponibili da fornitori di software indipendenti. Per ulteriori informazioni sulle opzioni e sulle estensioni, vedere [Commuter virtuale Hyper-V](../../hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md).  
  
### <a name="switch-and-network-adapter-choices"></a>Opzioni di commutazione e scheda di rete  
Hyper-V offre tre tipi di commutatori virtuali e due tipi di schede di rete virtuali. Si sceglierà quello che si vuole creare quando lo si crea. È possibile usare la console di gestione di Hyper-V o il modulo Hyper-V per Windows PowerShell per creare e gestire i commutatori virtuali e le schede di rete virtuali. Alcune funzionalità di rete avanzate, ad esempio elenchi di controllo di accesso (ACL) di porta estesi, possono essere gestite solo tramite i cmdlet nel modulo Hyper-V.  
  
È possibile apportare alcune modifiche a un commutatore virtuale o a una scheda di rete virtuale dopo averla creata. È ad esempio possibile modificare un'opzione esistente in un tipo diverso, ma ciò influisca sulle funzionalità di rete di tutte le macchine virtuali connesse a tale commutatore.  Quindi, è probabile che non si esegua questa operazione, a meno che non si sia verificato un errore o sia necessario testare qualcosa. Un altro esempio è la possibilità di connettere una scheda di rete virtuale a un'altra opzione, operazione che può essere eseguita se si desidera connettersi a una rete diversa. Tuttavia, non è possibile modificare una scheda di rete virtuale da un tipo a un altro. Anziché modificare il tipo, è necessario aggiungere un'altra scheda di rete virtuale e scegliere il tipo appropriato.  
  
I tipi di Commuter virtuali sono:  
  
-   **Commutatore virtuale esterno** : consente di connettersi a una rete fisica cablata mediante binding a una scheda di rete fisica.  
  
-   **Commuter virtuale interno** : consente di connettersi a una rete che può essere usata solo dalle macchine virtuali in esecuzione nell'host con il commutere virtuale e tra l'host e le macchine virtuali.  
  
-   **Switch virtuale privato** : si connette a una rete che può essere usata solo dalle macchine virtuali in esecuzione nell'host con il Commuter virtuale, ma non fornisce la rete tra l'host e le macchine virtuali.  
  
I tipi di scheda di rete virtuale sono:  
  
-   **Scheda di rete specifica di Hyper-V** : disponibile per le macchine virtuali di prima e seconda generazione. È progettato specificamente per Hyper-V e richiede un driver incluso nei servizi di integrazione Hyper-V. Questo tipo di scheda di rete è più veloce ed è la scelta consigliata, a meno che non sia necessario eseguire l'avvio sulla rete o non sia in esecuzione un sistema operativo guest non supportato. Il driver richiesto è disponibile solo per i sistemi operativi guest supportati. Si noti che nella console di gestione di Hyper-V e i cmdlet di rete, questo tipo è semplicemente denominato scheda di rete.  
  
-   **Scheda di rete legacy** : disponibile solo nelle macchine virtuali di prima generazione. Emula una scheda PCI Fast Ethernet basata su Intel 21140 e può essere usata per l'avvio in una rete, in modo da poter installare un sistema operativo da un servizio, ad esempio servizi di distribuzione Windows.  
  
## <a name="hyper-v-networking-and-related-technologies"></a>Rete Hyper-V e tecnologie correlate  
Nelle versioni recenti di Windows Server sono stati introdotti miglioramenti che offrono più opzioni per la configurazione della rete per Hyper-V. Windows Server 2012, ad esempio, ha introdotto il supporto per la rete convergente. Questo consente di instradare il traffico di rete attraverso un Commuter virtuale esterno. Windows Server 2016 si basa su questo consentendo l'accesso diretto a memoria remota (RDMA) sulle schede di rete associato a un Commuter virtuale Hyper-V. Questa configurazione può essere utilizzata con o senza switch Embedded Teaming (SET). Per informazioni dettagliate, [vedere &#40;accesso diretto a memoria&#41; remota RDMA e switch Embedded Teaming &#40;Set&#41; ](../../hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)  
  
Alcune funzionalità si basano su configurazioni di rete specifiche o migliorano in determinate configurazioni. Prendere in considerazione questi requisiti durante la pianificazione o l'aggiornamento dell'infrastruttura di rete.  
  
**Clustering di failover** : si tratta di una procedura consigliata per isolare il traffico del cluster e utilizzare la qualità del servizio (QoS) Hyper-V nel Commuter virtuale. Per informazioni dettagliate, vedere [consigli sulla rete per un cluster Hyper-V](https://technet.microsoft.com/library/dn550728.aspx)  
  
**Migrazione in tempo reale** : usare le opzioni per le prestazioni per ridurre l'utilizzo della rete e della CPU e il tempo necessario per completare una migrazione in tempo reale. Per istruzioni, vedere [configurare gli host per la migrazione in tempo reale senza clustering di failover](../deploy/set-up-hosts-for-live-migration-without-failover-clustering.md).  
  
**Spazi di archiviazione diretta** : questa funzionalità si basa sul protocollo di rete SMB 3.0 e RDMA. Per informazioni dettagliate, vedere [spazi di archiviazione diretta in Windows Server 2016](../../../storage/storage-spaces/storage-spaces-direct-overview.md).