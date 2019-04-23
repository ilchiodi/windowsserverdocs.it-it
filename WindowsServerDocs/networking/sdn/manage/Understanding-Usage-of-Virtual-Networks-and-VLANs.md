---
title: Comprendere l'utilizzo di reti virtuali e VLAN
description: In questo argomento informazioni sulle reti virtuali basate su virtualizzazione rete Hyper-V e le differenze da reti locali virtuali (VLAN). Con virtualizzazione rete Hyper-V, per creare le reti virtuali di sovrapposizione, definito anche come reti virtuali.
manager: dougkim
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
ms.date: 08/26/2018
ms.openlocfilehash: d126e97a91e4c61ecff00cc2b5a527618b2d4d0f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875532"
---
# <a name="understand-the-usage-of-virtual-networks-and-vlans"></a>Comprendere l'utilizzo di reti virtuali e VLAN

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo argomento informazioni sulle reti virtuali basate su virtualizzazione rete Hyper-V e le differenze da reti locali virtuali (VLAN). Con virtualizzazione rete Hyper-V, per creare le reti virtuali di sovrapposizione, definito anche come reti virtuali.



  
Reti SDN (software Defined) in Windows Server 2016 si basa sulla programmazione dei criteri di overlay della rete virtuale all'interno di un commutatore virtuale Hyper-V. È possibile creare le reti virtuali di sovrapposizione, definito anche come reti virtuali, con virtualizzazione rete Hyper-V. 
  
Quando si distribuisce virtualizzazione rete Hyper-V, l'overlay della rete vengono creati tramite l'incapsulamento frame Ethernet di livello 2 della macchina virtuale tenant originale con un'intestazione di sovrapposizione - o tunnel - (ad esempio, VXLAN o NVGRE) e IP livello 3 ed Ethernet di livello 2 rete di intestazioni dal Metti sotto (o fisici). L'overlay della rete virtuale sono identificati da una a 24 bit virtuale rete identificatore (VNI) per mantenere l'isolamento del traffico tenant e per consentire gli indirizzi IP sovrapposti. Il VNI è costituito da una subnet virtuale ID (VSID), l'ID di commutatore logico e ID del tunnel.  
  
Inoltre, ogni tenant viene assegnato un dominio di routing (simile a routing virtuale e l'inoltro - VRF) in modo che più prefissi di subnet virtuale (ognuno rappresentato da un VNI) possono essere indirizzati direttamente tra loro. Tra i tenant (o cross-domain routing) routing non è supportato senza passare attraverso un gateway.   
  
La rete fisica in cui il traffico incapsulato di ogni tenant è sottoposto a tunneling è rappresentata da una rete logica denominata rete logica del provider. Questa rete logica provider è costituito da una o più subnet, ognuno rappresentato da un prefisso IP ed eventualmente una VLAN 802.1q tag.  
  
È possibile creare reti logiche aggiuntive e le subnet per motivi di infrastruttura supportare il traffico di gestione, il traffico di archiviazione, live il traffico di migrazione e così via.  
  
SDN Microsoft non supporta l'isolamento delle reti tenant usando reti VLAN. Isolamento dei tenant viene eseguita esclusivamente utilizzando sovrapposizione di virtualizzazione rete Hyper-V reti virtuali e incapsulamento. 


