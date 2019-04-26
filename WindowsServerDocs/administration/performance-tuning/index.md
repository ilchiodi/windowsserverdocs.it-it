---
title: Linee guida di ottimizzazione delle prestazioni per Windows Server 2016
description: Linee guida per l'ottimizzazione delle prestazioni per Windows Server 2016
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: landing-page
ms.author: phstee
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 1445ae40428bc8626f8e2a12cbfc2a1e6f41592f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59891962"
---
# <a name="performance-tuning-guidelines-for-windows-server-2016"></a>Linee guida di ottimizzazione delle prestazioni per Windows Server 2016

Quando si esegue un sistema server nella propria organizzazione, è possibile che le impostazioni predefinite del server non soddisfino le proprie esigenze aziendali. Potrebbe essere necessario, ad esempio, impostare il consumo di energia più basso possibile, la latenza minima possibile o la massima velocità effettiva possibile sul server. Questo documento offre linee guida utili per ottimizzare le impostazioni del server in Windows Server 2016 e ottenere un miglioramento incrementale in termini di prestazioni ed efficienza energetica, soprattutto quando la natura del carico di lavoro varia leggermente nel tempo.

È importante che le modifiche di ottimizzazione tengano conto dell'hardware, del carico di lavoro, dell'allocazione dell'energia elettrica e degli obiettivi di prestazioni del server. Questa guida descrive ogni impostazione con i potenziali effetti che può avere per consentire di prendere una decisione consapevole sulla relativa pertinenza per il sistema, il carico di lavoro, le prestazioni e gli obiettivi di utilizzo dell'energia.

> [!warning]
> Le impostazioni del Registro di sistema e i parametri di ottimizzazione sono cambiati in modo significativo da una versione di Windows Server all'altra. Assicurarsi di usare le linee guida di ottimizzazione più recenti per evitare risultati imprevisti.

## <a name="in-this-guide"></a>Contenuto della guida
In questo documento le linee guida sulle prestazioni e sull'ottimizzazione per Windows Server 2016 sono organizzate in tre categorie di ottimizzazione:

|Hardware del server | Ruolo server | Sottosistema del server |
|:---:|:---:|:---:|
|[Considerazioni sulle prestazioni dell'hardware](hardware/index.md) |[Server Active Directory](role/active-directory-server/index.md) |[Gestione della cache e della memoria](subsystem/cache-memory-management/index.md)|
|[Considerazioni sull'alimentazione hardware](hardware/power.md)|[File server](role/file-server/index.md)|[Sottosistema di rete](../../networking/technologies/network-subsystem/net-sub-performance-top.md)|
||[Server Hyper-V](role/hyper-v-server/index.md)|[Spazi di archiviazione diretta](subsystem/storage-spaces-direct/index.md)|
||[Servizi Desktop remoto](role/remote-desktop/session-hosts.md)|[SDN (Software Defined Networking)](subsystem/software-defined-networking/index.md)|
||[Server Web](role/web-server/index.md)||
||[Contenitori di Windows Server](role/windows-server-container/index.md)||


## <a name="changes-in-this-version"></a>Modifiche in questa versione

### <a name="sections-added"></a>Sezioni aggiunte
- [Considerazioni sulla configurazione del tipo di installazione Nano Server](../../get-started/getting-started-with-nano-server.md)


- [Software Defined Networking](subsystem/software-defined-networking/index.md), tra cui [HNV](subsystem/software-defined-networking/hnv-gateway-performance.md) e [indicazioni per la configurazione di gateway di bilanciamento del carico software](subsystem/software-defined-networking/slb-gateway-performance.md)

- [Spazi di archiviazione diretta](subsystem/storage-spaces-direct/index.md)

- [HTTP1.1 e HTTP2](role/web-server/http-performance.md)

- [Contenitori di Windows Server](role/windows-server-container/index.md)

### <a name="sections-changed"></a>Sezioni modificate

- Aggiornamenti apportati alla sezione relativa alle [indicazioni per Active Directory](role/active-directory-server/index.md)

- Aggiornamenti apportati alla sezione relativa alle [indicazioni per file server](role/file-server/index.md)

- Aggiornamenti apportati alla sezione relativa alle [indicazioni per server Web](role/web-server/index.md)

- Aggiornamenti apportati alla sezione relativa alle [indicazioni sull'alimentazione hardware](hardware/power.md)

- Aggiornamenti apportati alla sezione relativa alle [indicazioni per l'ottimizzazione di PowerShell](powershell/index.md)

- Aggiornamenti significativi apportati alla sezione relativa alle [indicazioni per Hyper-V](role/hyper-v-server/index.md)

- *Rimozione della sezione relativa all'ottimizzazione delle prestazioni per carichi di lavoro*, aggiunta di puntatori a risorse pertinenti nell'[articolo Risorse di ottimizzazione aggiuntive](additional-resources.md)

- *Rimozione delle sezioni relative all'archiviazione* a favore di una nuova sezione [Spazi di archiviazione diretta](subsystem/storage-spaces-direct/index.md) e di contenuto Technet canonico

- *Rimozione della sezione relativa alle reti* a favore di contenuto Technet canonico  
