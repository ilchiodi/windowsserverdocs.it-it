---
author: eross-msft
ms.author: lizross
ms.date: 10/02/2018
ms.prod: windows-server
ms:topic: include
ms.openlocfilehash: 47a91c86ac75aedf532289055c94b34899fa01df
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316474"
---
Con la porta Hyper-V, i team NIC configurati in host Hyper-V assegnano agli indirizzi MAC indipendenti dalle macchine virtuali.  Per dividere il traffico di rete tra i membri del gruppo NIC, è possibile usare l'indirizzo MAC delle VM o la macchina virtuale connessa al commutire Hyper-V. Non è possibile configurare i team NIC creati all'interno delle VM con la modalità di bilanciamento del carico della porta Hyper-V. Usare invece la modalità hash indirizzo. 