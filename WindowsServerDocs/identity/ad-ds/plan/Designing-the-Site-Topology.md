---
ms.assetid: eeb919de-e21e-48d8-8186-e42adec6933f
title: Progettazione della topologia di un sito
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e1d9323ceda478369973f959687d46c9ca3cb88f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888492"
---
# <a name="designing-the-site-topology"></a>Progettazione della topologia di un sito

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Una topologia del sito del servizio directory è una rappresentazione logica della rete fisica. Progettazione di una topologia del sito per Active Directory Domain Services (AD DS) implica la pianificazione per la selezione del controller di dominio e progettare siti, subnet, i collegamenti di sito e ponti di collegamenti di sito per garantire un routing efficiente di traffico di replica e query.  
  
Progettazione di una topologia del sito consente di instradare in modo efficiente le query dei client e il traffico di replica di Active Directory. Una topologia del sito ben progettata consente all'organizzazione di ottenere i vantaggi seguenti:  
  
-   Ridurre al minimo il costo della replica dei dati di Active Directory.  
  
-   Ridurre al minimo le attività amministrative che sono tenute a mantenere la topologia del sito.  
  
-   Pianificare la replica che consente di percorsi con collegamenti di rete lenta o remota per replicare i dati di Active Directory durante gli orari.  
  
-   Ottimizzare la capacità dei computer client per individuare le risorse più vicina, ad esempio i controller di dominio e server di File System distribuito (DFS). Questo monitoraggio consente di ridurre il traffico di rete tramite WAN lenta (WAN) i collegamenti della rete, migliorare i processi di accesso e disconnessione e velocizzare le operazioni di download di file.  
  
Prima di iniziare a progettare la topologia del sito, è necessario comprendere la struttura di rete fisica. Inoltre, è necessario innanzitutto progettare la struttura logica di Active Directory, tra cui la gerarchia amministrativo, la pianificazione della foresta e piano di dominio per ogni foresta. È inoltre necessario completare il progetto di infrastruttura Domain Name System (DNS) per Active Directory Domain Services. Per altre informazioni sulla progettazione di struttura logica di Active Directory e infrastruttura DNS, vedere [progettare la logica struttura per Windows Server 2008 AD DS](https://technet.microsoft.com/library/cc770806.aspx).  
  
Dopo aver completato la progettazione di topologia del sito, è necessario verificare che i controller di dominio soddisfino i requisiti hardware per Windows Server 2008 Standard, Windows Server 2008 Enterprise e Windows Server 2008 Datacenter.  
  
## <a name="in-this-guide"></a>Contenuto della guida  
  
-   [Informazioni sulla topologia del sito Active Directory](../../ad-ds/plan/Understanding-Active-Directory-Site-Topology.md)  
  
-   [Raccolta delle informazioni di rete](../../ad-ds/plan/Collecting-Network-Information.md)  
  
-   [Pianificazione della selezione del Controller di dominio](../../ad-ds/plan/Planning-Domain-Controller-Placement.md)  
  
-   [Creazione di una progettazione di sito](../../ad-ds/plan/Creating-a-Site-Design.md)  
  
-   [Creazione di una progettazione di collegamento di sito](../../ad-ds/plan/Creating-a-Site-Link-Design.md)  
  
-   [Creazione di una progettazione di ponte di collegamenti di sito](../../ad-ds/plan/Creating-a-Site-Link-Bridge-Design.md)  
  
-   [Ricerca di risorse aggiuntive per la progettazione di topologia del sito di Windows Server 2008 Active Directory](../../ad-ds/plan/Finding-Additional-Resources-for-Windows-Server-2008-Active-Directory-Site-Topology-Design.md)  
  


