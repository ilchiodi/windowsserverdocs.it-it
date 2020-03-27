---
title: Domande frequenti su vRSS
description: In questo argomento sono disponibili alcune domande frequenti e le relative risposte sull'uso di vRSS.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 61ae242e-82a8-430d-b07d-52b86c01e686
ms.localizationpriority: medium
manager: dougkim
ms.date: 09/05/2018
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 0ee9bf121d64eebe98798df907a2584747a00c7a
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315362"
---
# <a name="vrss-frequently-asked-questions"></a>Domande frequenti su vRSS

In questo argomento sono disponibili alcune domande frequenti e le relative risposte sull'uso di vRSS.

## <a name="what-are-the-requirements-for-the-physical-network-adapters-that-i-use-with-vrss"></a>Quali sono i requisiti per le schede di rete fisiche utilizzate con vRSS?

Le schede di rete devono essere compatibili con Coda macchine virtuali \(VMQ\) e devono avere una velocità di collegamento di 10 Gbps o superiore.

Per altre informazioni, vedere [pianificare l'uso di vRSS](vrss-plan.md).

## <a name="does-vrss-work-with-hyper-threaded-processor-cores"></a>VRSS funziona con i core del processore Hyper\-?

No. Sia vRSS che VMQ ignorano i core del processore con thread Hyper\-.

## <a name="does-vrss-work-for-host-virtual-nics-vnics"></a>VRSS funziona per le schede NIC virtuali host \(\)schede?

Sì. Usare il parametro **-managementos** anziché il nome della macchina virtuale \(VM\) nel comando **set-VMNetworkAdapter** di Windows PowerShell e **Enable-NetAdapterRss** nell'host vNIC.

Per ulteriori informazioni, vedere [comandi di Windows PowerShell per RSS e vRSS](vrss-wps.md).

## <a name="how-many-logical-processors-does-a-vm-need-to-use-vrss"></a>Quanti processori logici una macchina virtuale deve usare vRSS?

Le VM necessitano di due o più processori logici \(LPs\) per poter usare vRSS.

Per altre informazioni, vedere [pianificare l'uso di vRSS](vrss-plan.md).

## <a name="is-vrss-compatible-with-nic-teaming"></a>VRSS è compatibile con gruppo NIC?

Sì. Se si utilizza il gruppo NIC, è importante configurare correttamente VMQ per l'utilizzo con le impostazioni gruppo NIC. Per informazioni dettagliate sulla distribuzione e la gestione del gruppo NIC, vedere [Gruppo NIC](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming).

## <a name="vrss-is-enabled-but-how-do-i-know-if-it-is-working"></a>vRSS è abilitato, ma come è possibile verificare se funziona? 

Sarà possibile indicare a vRSS di funzionare aprendo Gestione attività nella macchina virtuale e visualizzando l'utilizzo del processore virtuale. Se sono state stabilite più connessioni alla macchina virtuale, è possibile visualizzare più di un core superiore allo 0% di utilizzo.

Poiché una singola sessione TCP non può essere sottoposta a bilanciamento del carico tra più core del processore logico, è necessario che la macchina virtuale riceva più sessioni TCP prima di poter osservare se il vRSS funziona o meno.

Se la macchina virtuale riceve più sessioni TCP, ma non è possibile visualizzare più di un core LP oltre lo 0% di utilizzo, assicurarsi di aver completato tutti i passaggi di preparazione nell'argomento [pianificare l'uso di vRSS](vrss-plan.md).

## <a name="im-looking-at-the-host-and-not-all-of-the-processors-are-being-used-it-looks-like-every-other-one-is-being-skipped"></a>Sto osservando l'host e non tutti i processori vengono usati. Sembra che tutti gli altri vengano ignorati.
  
Verificare se sia abilitato l'hyperthreading. Sia VMQ che vRSS sono progettati per ignorare i core con thread Hyper\-.

## <a name="are-there-different-windows-powershell-commands-for-rss-and-vrss"></a>Sono disponibili diversi comandi di Windows PowerShell per RSS e vRSS?

Sì e no. Quando si usano gli stessi comandi sia per RSS in host nativi che per RSS nelle VM, vRSS richiede anche l'abilitazione di VMQ nella scheda di interfaccia di rete fisica e per l'abilitazione della macchina virtuale e del vRSS sulla porta di commutazione.

Per ulteriori informazioni, vedere [comandi di Windows PowerShell per RSS e vRSS](vrss-wps.md).

---
