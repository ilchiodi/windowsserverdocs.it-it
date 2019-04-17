---
title: vRSS domande frequenti
description: In questo argomento, puoi trovare che alcune comunemente domande frequenti sull'uso di vRSS.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 61ae242e-82a8-430d-b07d-52b86c01e686
ms.localizationpriority: medium
manager: dougkim
ms.date: 09/05/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3fafe6c39285e65a9d39a76cc6b652dac5c3efbd
ms.sourcegitcommit: e84e328c13a701e8039b16a4824a6e58a6e59b0b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/22/2018
ms.locfileid: "4133817"
---
# vRSS domande frequenti

In questo argomento, puoi trovare che alcune comunemente domande frequenti sull'uso di vRSS.

## Quali sono i requisiti per le schede di rete fisica usate con vRSS?

Le schede di rete devono essere compatibili con \(VMQ\) coda di macchina virtuale e devono avere una velocità di collegamento di 10 Gbps o più.

Per altre informazioni, vedi [prevede l'uso di vRSS](vrss-plan.md).

## VRSS funziona con core processore hyper\-v?

No. VRSS sia VMQ ignorare core processore hyper\-v.

## VRSS funziona per \(vNICs\) schede di rete virtuale host?

Sì. Usa il parametro **- ManagementOS** anziché il nome \(VM\) macchina virtuale nel comando Windows PowerShell **Set-VMNetworkAdapter** e **Abilitare NetAdapterRss** su vNIC host.

Per altre informazioni, vedi [I comandi di Windows PowerShell per RSS e vRSS](vrss-wps.md).

## Quanti processori logici una macchina virtuale è necessario usare vRSS?

Le macchine virtuali devono \(LPs\) processori logici due o più essere in grado di usare vRSS.

Per altre informazioni, vedi [prevede l'uso di vRSS](vrss-plan.md).

## È compatibile con gruppo NIC vRSS?

Sì. Se usi gruppo NIC, è importante che configurare correttamente VMQ per lavorare con le impostazioni di gruppo NIC. Per informazioni dettagliate sulla gestione e distribuzione gruppo NIC, vedi [Gruppo NIC](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming).

## vRSS è abilitato, ma come faccio a sapere se funziona? 

Sarai in grado di stabilire vRSS sta aprendo Gestione attività nella tua macchina virtuale e l'utilizzo del processore virtuale di visualizzazione. Se sono presenti più connessioni stabilite alla macchina virtuale, è possibile visualizzare uno o più core di sopra di utilizzo di 0%.

Una singola sessione TCP non è possibile carico bilanciato tra più core logici, la macchina virtuale deve essere ricezione TCP più sessioni prima di esaminare o meno vRSS funziona.

Se la macchina virtuale riceve più sessioni TCP, ma non viene visualizzato uno o più core LP di sopra di utilizzo di 0%, assicurati di avere completato tutti i passaggi di preparazione nell'argomento [prevede l'uso di vRSS](vrss-plan.md).

## Ricerca di host e non tutti i processori sono in uso. Sembra che ogni altro viene ignorato.
  
Verificare se il threading hyper è abilitato. VMQ sia vRSS sono progettati per ignorare il thread hyper\-v Core.

## Esistono diversi comandi di Windows PowerShell per RSS e vRSS?

Sì e no. Anche se puoi utilizzare gli stessi comandi per RSS nell'host nativo sia RSS nelle macchine virtuali, vRSS richiede anche VMQ sia abilitato nella scheda di rete fisica - e per la macchina virtuale e vRSS sia abilitato sulla porta switch.

Per altre informazioni, vedi [I comandi di Windows PowerShell per RSS e vRSS](vrss-wps.md).

---
