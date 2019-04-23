---
title: Tecnologie di ottimizzazione e offload di rete
description: In questo argomento viene fornita una panoramica delle tecnologie di ottimizzazione in Windows Server 2016 e l'Offload e include collegamenti a indicazioni aggiuntive su queste tecnologie.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/20/2018
ms.openlocfilehash: 9cd63ae557eab6e8e4f69fedd20619d4e7af9184
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847852"
---
# <a name="network-offload-and-optimization-technologies"></a>Tecnologie di ottimizzazione e offload di rete

In questo argomento è fornire una panoramica delle rete diversa offload e ottimizzazione di funzionalità disponibili in Windows Server 2016 e discutere su come contribuiscono a rendere più efficienti funzionalità di rete. Queste tecnologie includono funzionalità solo Software (SO) e tecnologie, Software e Hardware (SH) integrato di funzionalità e tecnologie, tecnologie e funzionalità solo Hardware (host).

Le tre categorie di funzionalità di rete disponibili in Windows Server 2016 sono: 

1.  [Solo software (SO) funzionalità e tecnologie](hpn-software-only-features.md): Queste funzionalità vengono implementate come parte del sistema operativo e sono indipendenti tra le schede di rete sottostante. A volte queste funzionalità richiederà alcuni ottimizzazione dell'interfaccia di rete per un funzionamento ottimale. Esempi di queste funzionalità di Hyper-v, ad esempio vmQoS, ACL e le funzionalità di Hyper-V come gruppo NIC.   

2.  [Software e Hardware (SH) integrate funzionalità e tecnologie](hpn-software-hardware-features.md): Queste funzionalità includono componenti hardware e software. Il software è strettamente a funzionalità hardware necessarie per usare la funzionalità. Esempi includono VMMQ VMQ, Offload Checksum IPv4 lato di trasmissione e RSS.   

3.  [Funzionalità hardware solo (host) e le tecnologie](hpn-hardware-only-features.md): Le accelerazioni hardware migliorare le prestazioni di rete in combinazione con il software ma non sono strettamente parte di qualsiasi funzionalità del software. Esempi includono regolazione di Interrupt, controllo di flusso e Receive-side dell'Offload Checksum IPv4. 

4. [Interfaccia di rete le proprietà avanzate](hpn-nic-advanced-properties.md): È possibile gestire tutte le funzionalità tramite Windows PowerShell usando il cmdlet NetAdapter e schede di rete.  È anche possibile gestire interfacce di rete e tutte le funzionalità usando il pannello di controllo di rete (ncpa. cpl). 

>[!TIP]
>- Quindi, tecnologie e funzionalità sono disponibili in tutte le architetture hardware, indipendentemente dalla velocità di interfaccia di rete o la funzionalità di interfaccia di rete.
>
>- SH e HO funzionalità sono disponibili solo quando la scheda di rete supporta le funzionalità o tecnologie.

---