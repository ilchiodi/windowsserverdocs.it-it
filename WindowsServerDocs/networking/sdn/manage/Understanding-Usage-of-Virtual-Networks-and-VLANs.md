---
title: Informazioni sull'utilizzo di reti virtuali e VLAN
description: Questo argomento fa parte della Guida alla rete definita dal Software su come gestire carichi di lavoro Tenant e reti virtuali in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 84ac2458-3fcf-4c4f-acfe-6105443dd83f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fcf84c5c1f0be2fa1c7524592f8d02e4b11d3a2b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="understanding-usage-of-virtual-networks-and-vlans"></a>Informazioni sull'utilizzo di reti virtuali e VLAN

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per informazioni sulle reti virtuali di Hyper-V rete virtualizzazione e come si differenziano da reti locali virtuali (VLAN).  
  
Rete SDN (software Defined) in Windows Server 2016 si basa sulla programmazione di criteri per reti virtuali sovrapposizione all'interno di un commutatore virtuale Hyper-V. È possibile creare reti virtuali sovrimpressione, definito anche come reti virtuali, con virtualizzazione rete Hyper-V.   
  
Quando si distribuisce virtualizzazione rete Hyper-V, vengono create reti sovrapposizione incapsulando frame Ethernet di livello 2 della macchina virtuale tenant originale con un overlay - o tunnel - intestazione (ad esempio, VXLAN o NVGRE) e livello 3 IP ed Ethernet di livello 2 rete intestazioni dal Metti sotto (o fisico). Le reti virtuali sovrimpressione sono identificate da un 24 bit virtuale rete identificatore (VNI) per mantenere l'isolamento del traffico tenant e per consentire di indirizzi IP sovrapposti. Il VNI è costituito da una subnet virtuale ID (VSID), l'ID di commutatore logico e ID del tunnel.  
  
Inoltre, ogni tenant viene assegnato un dominio di routing (simile a virtuali routing e inoltro - VRF) in modo che più prefissi di subnet virtuale (ogni rappresentato da un VNI) è possibile instradare direttamente tra loro. Cross-tenant (o tra domini di routing) di routing non è supportato senza passare attraverso un gateway.   
  
La rete fisica in cui viene effettuato il tunneling traffico incapsulato ogni tenant è rappresentata da una rete logica denominata la rete logica provider. Questa rete logica provider è costituita da uno o più subnet, ciascuna rappresentata da un prefisso IP ed eventualmente una VLAN 802.1 q tag.  
  
È possibile creare altre reti logiche e le subnet per motivi di infrastruttura eseguire il traffico di gestione, il traffico di archiviazione, live il traffico di migrazione e così via.  
  
Microsoft SDN non supporta l'isolamento delle reti tenant tramite VLAN. L'isolamento del tenant viene eseguita esclusivamente tramite virtualizzazione rete Hyper-V sovrimpressione reti virtuali e incapsulamento. 


