---
title: Pianificare la rete di Hyper-V in Windows Server
description: Descrive che cos'è necessaria per la rete di base in Hyper-V e vengono forniti collegamenti alle istruzioni
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: KBDAzure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 83c014b917566a78796d061dd88962966ec0c9ab
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829872"
---
# <a name="plan-for-hyper-v-networking-in-windows-server"></a>Pianificare la rete di Hyper-V in Windows Server

>Si applica a: Microsoft Hyper-V Server 2016, Windows Server 2016, Microsoft Hyper-V Server 2019, Windows Server 2019
  
Una conoscenza di base di rete di Hyper-V consente di pianificare la rete per le macchine virtuali. Questo articolo illustra anche alcune considerazioni di rete quando si usa la migrazione in tempo reale e quando si usa Hyper-V con altri ruoli e funzionalità di server.  
  
## <a name="hyper-v-networking-basics"></a>Nozioni fondamentali sulla rete di Hyper-V  
Concetti fondamentali sulle reti in Hyper-V è piuttosto semplice. Usa due parti: un commutatore virtuale e una scheda di rete virtuale. È necessario almeno uno di ciascuno per stabilire la rete per una macchina virtuale. Il commutatore virtuale si connette a qualsiasi rete basata su Ethernet. La scheda di rete virtuale si connette a una porta del commutatore virtuale, che rende possibile per una macchina virtuale usi una rete.  
  
Il modo più semplice per stabilire la rete di base consiste nel creare un commutatore virtuale quando si installa Hyper-V. Quindi, quando si crea una macchina virtuale, è possibile collegarlo al commutatore. Ci si connette automaticamente al commutatore aggiunge una scheda di rete virtuale alla macchina virtuale. Per istruzioni, vedere [creare un commutatore virtuale per le macchine virtuali Hyper-V](../get-started/Create-a-virtual-switch-for-Hyper-V-virtual-machines.md).  
  
Per gestire diversi tipi di rete, è possibile aggiungere commutatori virtuali e schede di rete virtuale. Tutte le opzioni fanno parte dell'host Hyper-V, ma ogni scheda di rete virtuale a cui appartiene una sola macchina virtuale.  
  
Il commutatore virtuale è un commutatore di rete Ethernet di livello 2 di basata sul software. Fornisce funzionalità incorporate per il monitoraggio, controllo e segmentando traffico, nonché protezione e la diagnostica.  È possibile aggiungere al set di funzionalità predefinite mediante l'installazione di plug-in, chiamato anche *estensioni*. Sono disponibili da fornitori di software indipendenti. Per altre informazioni sul commutatore e le estensioni, vedere [commutatore virtuale Hyper-V](../../hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md).  
  
### <a name="switch-and-network-adapter-choices"></a>Opzioni di adapter switch e rete  
Hyper-V offre tre tipi di commutatori virtuali e due tipi di schede di rete virtuale. È possibile scegliere quale della ognuno desiderato durante la creazione. È possibile utilizzare Gestione Hyper-V o il modulo Hyper-V per Windows PowerShell per creare e gestire i commutatori virtuali e schede di rete virtuale. Alcune funzionalità di rete avanzate, ad esempio elenchi di controllo di accesso estesi della porta (ACL), possono essere gestite solo usando i cmdlet nel modulo Hyper-V.  
  
È possibile apportare alcune modifiche a un commutatore virtuale o una scheda di rete virtuale dopo averla creata. Ad esempio, è possibile modificare un commutatore esistente in un tipo diverso, ma questa operazione interessa la funzionalità di rete di tutte le macchine virtuali connesse al commutatore.  Pertanto, è probabilmente non questa procedura solo se si commette un errore o necessitano di testare qualcosa. Come ulteriore esempio, è possibile connettere una scheda di rete virtuale a un commutatore diverso, che potrebbe accadere se si desidera connettersi a una rete diversa. Tuttavia, non è possibile modificare una scheda di rete virtuale da un tipo a altro. Anziché modificare il tipo, di aggiungere un'altra scheda di rete virtuale e scegliere il tipo appropriato.  
  
Tipi di commutatore virtuale sono:  
  
-   **Commutatore virtuale esterno** -si connette a una rete cablata, fisica creando un'associazione a una scheda di rete fisica.  
  
-   **Commutatore virtuale interno** -si connette a una rete che può essere utilizzata solo per le macchine virtuali in esecuzione nell'host con il commutatore virtuale e tra l'host e macchine virtuali.  
  
-   **Commutatore virtuale privato** -si connette a una rete che può essere utilizzata solo per le macchine virtuali in esecuzione sull'host che ha il commutatore virtuale, ma non fornisce funzionalità di rete tra l'host e macchine virtuali.  
  
Tipi di scheda di rete virtuale sono:  
  
-   **Scheda di rete specifici di Hyper-V** : disponibile per la generazione 1 e macchine virtuali di generazione 2. È progettato espressamente per Hyper-V e richiede un driver che è incluso in servizi di integrazione Hyper-V. Questo tipo di scheda di rete più veloce e rappresenta la scelta consigliata a meno che non è necessario eseguire l'avvio di rete o sono in esecuzione un sistema operativo guest non supportati. Il driver richiesto viene fornito solo per i sistemi operativi supportati. Si noti che nella console di gestione Hyper-V e i cmdlet di rete, questo tipo viene definito solo come una scheda di rete.  
  
-   **Scheda di rete legacy** : disponibile solo nelle macchine virtuali di generazione 1. Emula un basati su Intel 21140 PCI Fast Ethernet Adapter e può essere usato per l'avvio di una rete, pertanto è possibile installare un sistema operativo da un servizio, ad esempio servizi di distribuzione Windows.  
  
## <a name="hyper-v-networking-and-related-technologies"></a>Rete Hyper-V e le tecnologie correlate  
Versioni recenti di Windows Server introdotti miglioramenti che offrono ulteriori opzioni per la configurazione di rete per Hyper-V. Ad esempio, Windows Server 2012 introdotto il supporto per convergente rete. Ciò consente di instradare il traffico di rete tramite un commutatore virtuale esterno. Windows Server 2016 si basa su questo, consentendo a Direct accesso memoria remota (RDMA) nelle schede di rete associate a un commutatore virtuale Hyper-V. È possibile usare questa configurazione con o senza Switch Embedded Teaming (SET). Per informazioni dettagliate, vedere [Remote Direct Memory Access &#40;RDMA&#41; e Switch Embedded Teaming &#40;SET&#41;](../../hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)  
  
Alcune funzionalità si basano su specifiche configurazioni di rete o fare di meglio in determinate configurazioni. Considerare queste durante la pianificazione o l'aggiornamento dell'infrastruttura di rete.  
  
**Clustering di failover** -è consigliabile isolare il traffico del cluster e la qualità del servizio (QoS) di Hyper-V nel commutatore virtuale. Per informazioni dettagliate, vedere [consigli sulla rete per un Cluster Hyper-V](https://technet.microsoft.com/library/dn550728.aspx)  
  
**Migrazione in tempo reale** -usare le opzioni di prestazioni per ridurre la rete e l'utilizzo della CPU e il tempo necessario per completare una migrazione in tempo reale. Per istruzioni, vedere [configurare gli host per live migration senza Clustering di Failover](../deploy/set-up-hosts-for-live-migration-without-failover-clustering.md).  
  
**Spazi di archiviazione diretta** -questa funzionalità si basa sul protocollo di rete SMB3.0 e RDMA. Per informazioni dettagliate, vedere [spazi di archiviazione diretta in Windows Server 2016](../../../storage/storage-spaces/storage-spaces-direct-overview.md).