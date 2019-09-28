---
ms.assetid: eeb919de-e21e-48d8-8186-e42adec6933f
title: Progettazione della topologia di un sito
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: d3ebc3bd764a8ed44e201d0fca5f85b06df8be9d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408891"
---
# <a name="designing-the-site-topology"></a>Progettazione della topologia di un sito

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Una topologia del sito del servizio directory è una rappresentazione logica della rete fisica. La progettazione di una topologia del sito per Active Directory Domain Services (AD DS) prevede la pianificazione del posizionamento dei controller di dominio e la progettazione di siti, subnet, collegamenti di sito e ponti di collegamenti di sito per garantire un routing efficiente del traffico di query e di replica.  
  
La progettazione di una topologia del sito consente di instradare in modo efficiente le query client e Active Directory il traffico Una topologia del sito ben progettata consente all'organizzazione di ottenere i vantaggi seguenti:  
  
-   Ridurre al minimo il costo della replica dei dati Active Directory.  
  
-   Ridurre al minimo le attività amministrative necessarie per gestire la topologia del sito.  
  
-   Pianificare la replica che consente percorsi con collegamenti di rete lenti o remote per replicare i dati Active Directory durante le fasce orarie non di punta.  
  
-   Ottimizzare la capacità dei computer client di individuare le risorse più vicine, ad esempio i controller di dominio e i server file system distribuito (DFS). Questo consente di ridurre il traffico di rete su collegamenti Wide Area Network lenti (WAN), migliorare i processi di accesso e disconnessione e velocizzare le operazioni di download dei file.  
  
Prima di iniziare a progettare la topologia del sito, è necessario comprendere la struttura di rete fisica. Inoltre, è necessario innanzitutto progettare la struttura logica di Active Directory, tra cui la gerarchia amministrativa, il piano della foresta e il piano di dominio per ogni foresta. È inoltre necessario completare la progettazione dell'infrastruttura di Domain Name System (DNS) per servizi di dominio Active Directory. Per ulteriori informazioni sulla progettazione della struttura logica Active Directory e dell'infrastruttura DNS, vedere [progettazione della struttura logica per servizi di dominio Active Directory di Windows Server 2008](https://technet.microsoft.com/library/cc770806.aspx).  
  
Dopo aver completato la progettazione della topologia del sito, è necessario verificare che i controller di dominio soddisfino i requisiti hardware per Windows Server 2008 standard, Windows Server 2008 Enterprise e Windows Server 2008 Datacenter.  
  
## <a name="in-this-guide"></a>Contenuto della guida  
  
-   [Informazioni sulla topologia del sito di Active Directory](../../ad-ds/plan/Understanding-Active-Directory-Site-Topology.md)  
  
-   [Raccolta di informazioni di rete](../../ad-ds/plan/Collecting-Network-Information.md)  
  
-   [Pianificazione del posizionamento del controller di dominio](../../ad-ds/plan/Planning-Domain-Controller-Placement.md)  
  
-   [Creazione di un progetto di sito](../../ad-ds/plan/Creating-a-Site-Design.md)  
  
-   [Creazione di un progetto di collegamenti di sito](../../ad-ds/plan/Creating-a-Site-Link-Design.md)  
  
-   [Creazione di un progetto di ponte di collegamenti di sito](../../ad-ds/plan/Creating-a-Site-Link-Bridge-Design.md)  
  
-   [Ricerca di risorse aggiuntive per la progettazione della topologia del sito di Windows Server 2008 Active Directory](../../ad-ds/plan/Finding-Additional-Resources-for-Windows-Server-2008-Active-Directory-Site-Topology-Design.md)  
  


