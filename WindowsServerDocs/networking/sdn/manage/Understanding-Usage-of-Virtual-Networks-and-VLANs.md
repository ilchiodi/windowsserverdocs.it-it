---
title: Informazioni sull'utilizzo delle reti virtuali e delle VLAN
description: In questo argomento vengono fornite informazioni sulle reti virtuali di virtualizzazione rete Hyper-V e su come sono diverse dalle reti locali virtuali (VLAN). Con la virtualizzazione rete Hyper-V è possibile creare reti virtuali sovrapposte, denominate anche reti virtuali.
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 84ac2458-3fcf-4c4f-acfe-6105443dd83f
ms.author: lizross
author: eross-msft
ms.date: 08/26/2018
ms.openlocfilehash: 1faf0112953146959ce2ec587c083c9ae9fed9c5
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80317618"
---
# <a name="understand-the-usage-of-virtual-networks-and-vlans"></a>Informazioni sull'utilizzo delle reti virtuali e delle VLAN

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In questo argomento vengono fornite informazioni sulle reti virtuali di virtualizzazione rete Hyper-V e su come sono diverse dalle reti locali virtuali (VLAN). Con la virtualizzazione rete Hyper-V è possibile creare reti virtuali sovrapposte, denominate anche reti virtuali.



  
SDN (Software Defined Networking) in Windows Server 2016 si basa sui criteri di programmazione per le reti virtuali sovrapposte all'interno di un Commuter virtuale Hyper-V. È possibile creare reti virtuali sovrapposte, denominate anche reti virtuali, con virtualizzazione rete Hyper-V. 
  
Quando si distribuisce virtualizzazione rete Hyper-V, le reti sovrapposte vengono create incapsulando il frame Ethernet di livello 2 della macchina virtuale del tenant originale con un'intestazione overlay o tunnel (ad esempio, VXLAN o NVGRE) e l'IP di livello 3 e Ethernet di livello 2 intestazioni dalla rete sottostante (o fisica). Le reti virtuali sovrapposte sono identificate da un identificatore di rete virtuale a 24 bit (VNI) per mantenere l'isolamento del traffico del tenant e per consentire gli indirizzi IP sovrapposti. VNI è costituito da un ID di subnet virtuale (VSID), da un ID di Commuter logico e da un ID di tunneling.  
  
A ogni tenant viene inoltre assegnato un dominio di routing (simile a routing virtuale e inoltro-VRF), in modo che più prefissi di subnet virtuali (ognuno rappresentato da un VNI) possano essere indirizzati direttamente tra loro. Il routing tra tenant (o tra domini di routing incrociato) non è supportato senza passare attraverso un gateway.   
  
La rete fisica in cui si trova il traffico incapsulato di ogni tenant viene rappresentata da una rete logica chiamata rete logica del provider. Questa rete logica del provider è costituita da una o più subnet, ciascuna rappresentata da un prefisso IP e, facoltativamente, da un tag 802.1 q VLAN.  
  
È possibile creare reti logiche e subnet aggiuntive per scopi di infrastruttura per il traffico di gestione, il traffico di archiviazione, il traffico di migrazione in tempo reale e così via.  
  
Microsoft SDN non supporta l'isolamento delle reti tenant tramite VLAN. L'isolamento dei tenant viene eseguito esclusivamente usando le reti virtuali e l'incapsulamento della virtualizzazione rete Hyper-V. 


