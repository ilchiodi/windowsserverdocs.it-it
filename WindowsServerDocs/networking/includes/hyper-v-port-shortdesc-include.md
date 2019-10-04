---
author: shortpatti
ms.author: pashort
ms.date: 10/02/2018
ms.prod: windows-server
ms:topic: include
ms.openlocfilehash: 01f231ca730a19ac0e7e868bcb7180377830afe1
ms.sourcegitcommit: 73898afec450fb3c2f429ca373f6b48a74b19390
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/03/2019
ms.locfileid: "71935051"
---
Con la porta Hyper-V, i team NIC configurati in host Hyper-V assegnano agli indirizzi MAC indipendenti dalle macchine virtuali.  Per dividere il traffico di rete tra i membri del gruppo NIC, è possibile usare l'indirizzo MAC delle VM o la macchina virtuale connessa al commutire Hyper-V. Non è possibile configurare i team NIC creati all'interno delle VM con la modalità di bilanciamento del carico della porta Hyper-V. Usare invece la modalità hash indirizzo. 