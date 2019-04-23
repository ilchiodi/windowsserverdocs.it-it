---
title: vRSS domande frequenti
description: In questo argomento è trovare che alcune delle domande domande e risposte con vRSS.
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840242"
---
# <a name="vrss-frequently-asked-questions"></a>vRSS domande frequenti

In questo argomento è trovare che alcune delle domande domande e risposte con vRSS.

## <a name="what-are-the-requirements-for-the-physical-network-adapters-that-i-use-with-vrss"></a>Quali sono i requisiti per le schede di rete fisica utilizzati con vRSS?

Le schede di rete devono essere compatibile con coda macchine virtuali \(VMQ\) e deve avere una velocità di collegamento pari a 10 Gbps o più.

Per altre informazioni, vedere [prevede l'uso di vRSS](vrss-plan.md).

## <a name="does-vrss-work-with-hyper-threaded-processor-cores"></a>VRSS funziona con hyper\-processore Core con hyperthreading?

No. VRSS sia VMQ ignorare hyper\-processore Core con hyperthreading.

## <a name="does-vrss-work-for-host-virtual-nics-vnics"></a>VRSS funziona per host virtuale NIC \(Vnic\)?

Sì. Usare la **- ManagementOS** parametro anziché la macchina virtuale \(VM\) name nella **Set-VMNetworkAdapter** comando di Windows PowerShell, e  **Enable-NetAdapterRss** su vNIC host.

Per altre informazioni, vedere [i comandi di Windows PowerShell per RSS e vRSS](vrss-wps.md).

## <a name="how-many-logical-processors-does-a-vm-need-to-use-vrss"></a>Il numero di processori logico è necessario che una macchina virtuale da usare vRSS?

Le macchine virtuali necessarie due o più processori logici \(LPs\) per poter usare vRSS.

Per altre informazioni, vedere [prevede l'uso di vRSS](vrss-plan.md).

## <a name="is-vrss-compatible-with-nic-teaming"></a>È compatibile con gruppo NIC vRSS?

Sì. Se si Usa gruppo NIC, è importante configurare correttamente VMQ per lavorare con le impostazioni del gruppo NIC. Per informazioni dettagliate sulla gestione e distribuzione di gruppo NIC, vedere [NIC Teaming](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming).

## <a name="vrss-is-enabled-but-how-do-i-know-if-it-is-working"></a>abilitazione di vRSS è, ma come sapere se funziona? 

Sarà in grado di indicare vRSS sta aprendo Gestione attività nella macchina virtuale e visualizzando l'utilizzo del processore virtuale. Se sono presenti più connessioni stabilite alla macchina virtuale, è possibile visualizzare più di un core sopra 0% di utilizzo.

Poiché una singola sessione TCP non può essere con carico bilanciato tra più core del processore logico, la macchina virtuale deve ricevere TCP più sessioni prima che è possibile osservare se vRSS funzioni.

Se la macchina virtuale sta ricevendo più sessioni TCP, ma non viene visualizzato più di un core LP sopra 0% di utilizzo, assicurarsi di aver completato tutti i passaggi di preparazione nell'argomento [prevede l'uso di vRSS](vrss-plan.md).

## <a name="im-looking-at-the-host-and-not-all-of-the-processors-are-being-used-it-looks-like-every-other-one-is-being-skipped"></a>Osservando l'host, non tutti i processori risultano in uso. Sembra che tutti gli altri vengano ignorati.
  
Verificare se sia abilitato l'hyperthreading. Coda macchine Virtuali sia vRSS sono progettati per ignorare\-core con hyperthreading.

## <a name="are-there-different-windows-powershell-commands-for-rss-and-vrss"></a>Sono disponibili diversi comandi di Windows PowerShell per RSS e vRSS?

Sì e no. Mentre si usano gli stessi comandi per RSS negli host nativo e RSS nelle macchine virtuali, vRSS richiede inoltre coda macchine Virtuali da abilitare l'interfaccia di rete fisica - e per la macchina virtuale e vRSS deve essere abilitata nella porta del commutatore.

Per altre informazioni, vedere [i comandi di Windows PowerShell per RSS e vRSS](vrss-wps.md).

---
