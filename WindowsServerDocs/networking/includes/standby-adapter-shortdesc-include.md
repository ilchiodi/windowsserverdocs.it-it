---
author: shortpatti
ms.author: pashort
ms.date: 10/02/2018
ms.prod: windows-server-threshold
ms:topic: include
ms.openlocfilehash: d1bfc74c4daa751e3081b26c87dd0d2c88f5f095
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879852"
---
Le opzioni per Adapter di Standby **Nessuno (tutte le schede attive)** o la selezione di una scheda di rete specifici nel gruppo NIC che agisce come una scheda di Standby. Quando si configura una scheda di rete come scheda di Standby, tutti gli altri membri del team non selezionato sono attivi e nessun traffico di rete viene inviato o elaborato dall'adapter fino a quando non si verifica un errore di un'interfaccia di rete attiva. Dopo che un'interfaccia di rete attivi non riesce, l'interfaccia di rete di Standby diventa attiva e il traffico di rete dei processi. Quando vengono ripristinati tutti i membri del team al servizio, il membro del team standby viene ripristinato lo stato standby.  