---
title: Informazioni generali sul firewall del centro dati
description: È possibile utilizzare questo argomento per informazioni sul firewall del Data Center, ovvero un livello di rete, 5 Tuple (protocollo, numeri di porta di origine e destinazione, indirizzi IP di origine e di destinazione), il firewall con stato e multi-tenant in Windows Server 2016.
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 67576533-206b-428a-956c-ed8c53218d9b
ms.author: anpaul
author: AnirbanPaul
ms.openlocfilehash: 2f50ee45d64f6888306a5fb5efc8c9b801a1c3d0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859644"
---
# <a name="datacenter-firewall-overview"></a>Informazioni generali sul firewall del centro dati

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Il firewall del Data Center è un nuovo servizio incluso in Windows Server 2016. Si tratta di un livello di rete, a 5 Tuple (protocollo, numeri di porta di origine e destinazione, indirizzi IP di origine e di destinazione), a un firewall multi-tenant con stato. Quando vengono distribuiti e offerti come servizio dal provider di servizi, gli amministratori tenant possono installare e configurare i criteri del firewall per proteggere le reti virtuali da traffico indesiderato originato da reti Internet e Intranet.  
  
![Firewall del Data Center nello stack di rete](../../../media/Datacenter-Firewall-Overview/MultitenantFirewallOverview2.png)  
  
L'amministratore del provider di servizi o l'amministratore tenant può gestire i criteri del firewall del Data Center tramite il controller di rete e le API in direzione nord.  
  
Il firewall del Data Center offre i vantaggi seguenti per i provider di servizi cloud:  
  
-   Soluzione firewall basata su software altamente scalabile, gestibile e diagnosticabile che può essere offerta ai tenant  
  
-   Libertà di spostare le macchine virtuali tenant in host di calcolo diversi senza suddividere i criteri del firewall del tenant  
  
    -   Distribuito come firewall dell'agente host della porta vSwitch  
  
    -   Le macchine virtuali tenant ottengono i criteri assegnati al firewall dell'agente host vSwitch  
  
    -   Le regole del firewall vengono configurate in ogni porta vSwitch, indipendentemente dall'host effettivo che esegue la macchina virtuale  
  
-   Offre protezione alle macchine virtuali tenant indipendenti dal sistema operativo guest tenant  
  
Il firewall del Data Center offre i vantaggi seguenti per i tenant:  
  
-   Possibilità di definire regole del firewall per proteggere i carichi di lavoro con connessione Internet nelle reti virtuali  
  
-   Possibilità di definire regole del firewall per proteggere il traffico tra macchine virtuali nella stessa subnet virtuale L2 e tra macchine virtuali in subnet virtuali L2 diverse  
  
-   Possibilità di definire le regole del firewall per proteggere e isolare il traffico di rete tra le reti locali tenant e le relative reti virtuali presso il provider di servizi  
  


