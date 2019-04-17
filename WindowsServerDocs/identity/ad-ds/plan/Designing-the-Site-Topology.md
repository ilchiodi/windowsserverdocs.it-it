---
ms.assetid: eeb919de-e21e-48d8-8186-e42adec6933f
title: Progettazione della topologia del sito
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 3f16b5b941ef9c3bd8f4bf742d432afc1b3f559a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="designing-the-site-topology"></a>Progettazione della topologia del sito

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Una topologia del sito del servizio directory è una rappresentazione logica della rete fisica. Progettazione di una topologia del sito per servizi di dominio Active Directory (AD DS) implica la pianificazione di posizionamento del controller di dominio e la progettazione siti, subnet, collegamenti di sito e ponti di collegamenti di sito per assicurare il routing efficiente di traffico di replica e di query.  
  
Progettazione di una topologia del sito consente di instradare in modo efficace le query client e il traffico di replica di Active Directory. Una topologia del sito ben progettata consente all'organizzazione di ottenere i vantaggi seguenti:  
  
-   Ridurre il costo della replica dei dati di Active Directory.  
  
-   Ridurre al minimo gli sforzi amministrativi necessari per gestire la topologia del sito.  
  
-   Pianificare la replica che consente di percorsi con collegamenti di rete lenta o remota per replicare i dati di Active Directory durante gli orari.  
  
-   Ottimizzare la capacità del computer client di individuare le risorse più vicina, ad esempio i controller di dominio e server di File System distribuito (DFS). Questo contribuisce a ridurre il traffico di rete tramite WAN lenta (WAN) collegamenti di rete, migliorano i processi di accesso e disconnessione e velocizzare le operazioni di download di file.  
  
Prima di iniziare a progettare la topologia del sito, è necessario conoscere la struttura di rete fisica. Inoltre, è prima necessario progettare la struttura logica di Active Directory, tra cui la gerarchia amministrativa, la pianificazione della foresta e dominio piano per ogni foresta. È inoltre necessario completare il progetto di infrastruttura del sistema DNS (Domain Name) per Active Directory. Per ulteriori informazioni sulla progettazione dell'infrastruttura DNS e la struttura logica di Active Directory, vedere [progettare la logica struttura per Windows Server 2008 Active Directory](https://technet.microsoft.com/library/cc770806.aspx).  
  
Dopo aver completato la progettazione della topologia del sito, è necessario verificare che i controller di dominio soddisfino i requisiti hardware per Windows Server 2008 Standard, Windows Server 2008 Enterprise e Windows Server 2008 Datacenter.  
  
## <a name="in-this-guide"></a>In questa Guida  
  
-   [Informazioni sulla topologia del sito di Active Directory](../../ad-ds/plan/Understanding-Active-Directory-Site-Topology.md)  
  
-   [Raccolta di informazioni di rete](../../ad-ds/plan/Collecting-Network-Information.md)  
  
-   [Pianificazione del posizionamento di Controller di dominio](../../ad-ds/plan/Planning-Domain-Controller-Placement.md)  
  
-   [Creazione di una progettazione di sito](../../ad-ds/plan/Creating-a-Site-Design.md)  
  
-   [Creazione di una progettazione di collegamento di sito](../../ad-ds/plan/Creating-a-Site-Link-Design.md)  
  
-   [Creazione di una progettazione di ponte di collegamenti di sito](../../ad-ds/plan/Creating-a-Site-Link-Bridge-Design.md)  
  
-   [Risorse aggiuntive per la progettazione della topologia del sito di Windows Server 2008 Active Directory](../../ad-ds/plan/Finding-Additional-Resources-for-Windows-Server-2008-Active-Directory-Site-Topology-Design.md)  
  


