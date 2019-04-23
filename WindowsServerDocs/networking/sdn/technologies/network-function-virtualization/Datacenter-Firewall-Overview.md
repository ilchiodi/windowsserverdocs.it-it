---
title: Informazioni generali sul firewall del centro dati
description: È possibile utilizzare questo argomento per apprendere Firewall del centro dati, ovvero un livello di rete o di un firewall con stato, multi-tenant in Windows Server 2016, 5 tuple (protocollo, origine e destinazione numeri di porta, gli indirizzi IP di origine e destinazione).
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 67576533-206b-428a-956c-ed8c53218d9b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f1de50dc61639f4985c9d28fdde6072af650f42e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890832"
---
# <a name="datacenter-firewall-overview"></a>Informazioni generali sul firewall del centro dati

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Firewall del centro dati è un nuovo servizio incluso in Windows Server 2016. È un livello di rete, 5 tuple (protocollo, numeri di porta di origine e di destinazione, indirizzi IP di origine e destinazione), con stato e multi-tenant. Quando distribuito e disponibile come servizio dal provider di servizi, gli amministratori tenant possono installare e configurare i criteri firewall per proteggere le reti virtuali dal traffico indesiderato provenienti da Internet e reti intranet.  
  
![Firewall del centro dati nello stack di rete](../../../media/Datacenter-Firewall-Overview/MultitenantFirewallOverview2.png)  
  
L'amministratore di provider del servizio o l'amministratore tenant può gestire i criteri Firewall del centro dati tramite l'API northbound e il controller di rete.  
  
Per i provider di servizi cloud, il Firewall del centro dati offre i vantaggi seguenti:  
  
-   Una soluzione altamente scalabile, gestibile e diagnosable firewall basati su software che può essere offerti ai tenant  
  
-   Possibilità di spostare le macchine virtuali tenant agli host di calcolo diverse senza violare i criteri firewall di tenant  
  
    -   Distribuito come un firewall dell'agente host porta vSwitch  
  
    -   Le macchine virtuali tenant ottenere i criteri assegnati al proprio firewall dell'agente host di commutatore virtuale  
  
    -   Le regole del firewall sono configurate in ogni porta vSwitch, indipendente dall'host effettivo in esecuzione la macchina virtuale  
  
-   Offre protezione per le macchine virtuali indipendente dal sistema operativo guest tenant del tenant  
  
Il Firewall del centro dati offre i vantaggi seguenti per i tenant:  
  
-   Possibilità di definire le regole del firewall per proteggere i carichi di lavoro nelle reti virtuali è connessa a Internet  
  
-   Possibilità di definire le regole del firewall per proteggere il traffico tra macchine virtuali sulla stessa L2 subnet virtuale anche tra le macchine virtuali in subnet distinte L2  
  
-   Possibilità di definire le regole del firewall per proteggere e isolare il traffico di rete tra tenant on-premises reti e le proprie reti virtuali nel provider di servizi  
  


