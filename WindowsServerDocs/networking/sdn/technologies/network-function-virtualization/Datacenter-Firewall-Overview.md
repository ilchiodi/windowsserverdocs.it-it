---
title: Cenni preliminari sul datacenter Firewall
description: È possibile utilizzare questo argomento per apprendere Firewall del centro dati, ovvero un livello di rete, 5 tuple (protocollo, l'origine e destinazione numeri di porta, gli indirizzi IP di origine e destinazione), multi-tenant, con stato firewall in Windows Server 2016.
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
ms.openlocfilehash: 0c9b9fb5b0fb9aa09ed783b2b66a8ad370a627c3
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="datacenter-firewall-overview"></a>Cenni preliminari sul datacenter Firewall

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Firewall del centro dati è un nuovo servizio incluso in Windows Server 2016. È un livello di rete, 5 tuple (protocollo, numeri di porta di origine e di destinazione, indirizzi IP di origine e destinazione), con stato e multi-tenant. Quando distribuito e offerto come servizio dal provider di servizi, gli amministratori tenant possono installare e configurare criteri del firewall per proteggere le reti virtuali dal traffico indesiderato provenienti da Internet e reti intranet.  
  
![Firewall del centro dati nello stack di rete](../../../media/Datacenter-Firewall-Overview/MultitenantFirewallOverview2.png)  
  
L'amministratore di provider del servizio o dell'amministratore tenant può gestire i criteri Firewall del centro dati tramite il controller di rete e le API northbound.  
  
Per i provider di servizi cloud, il Firewall del centro dati offre i vantaggi seguenti:  
  
-   Una soluzione altamente scalabile, gestibili e diagnosable firewall basato su software che può essere fornita ai tenant  
  
-   Consente di spostare le macchine virtuali tenant agli host di calcolo differente senza interrompere i criteri firewall tenant  
  
    -   Distribuito come un firewall di agente host porta vSwitch  
  
    -   Macchine virtuali tenant di ottenere i criteri assegnati ai loro firewall di agente host vSwitch  
  
    -   Vengono configurate regole firewall in ogni porta vSwitch, indipendentemente dall'host effettivo in esecuzione la macchina virtuale  
  
-   Offre protezione ai tenant di macchine virtuali indipendente dal sistema operativo guest di tenant  
  
Per i tenant, il Firewall del centro dati offre i vantaggi seguenti:  
  
-   Possibilità di definire le regole del firewall per proteggere i carichi di lavoro in reti virtuali è connessa a Internet  
  
-   Possibilità di definire le regole del firewall per proteggere il traffico tra le macchine virtuali nella stessa L2 subnet virtuale nonché per ciò che concerne le macchine virtuali in diverse subnet virtuale L2  
  
-   Possibilità di definire le regole del firewall per proteggere e isolare il traffico di rete tra tenant locale reti e le reti virtuali presso il provider di servizi  
  


