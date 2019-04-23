---
title: Configurare le reti locali virtuali per Hyper-V
description: Fornisce le istruzioni per la configurazione di una finestra di rete locale virtuale (VLAN) per l'uso dalle macchine virtuali in un host Hyper-V.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8510a709-001c-4eee-b6d6-c451e8a8a836
author: KBDAzure
ms.author: kathydav
ms.date: 10/11/2016
ms.openlocfilehash: 5b5eaf175e7c09124aaa3f7a33523e8b87a9ae84
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848462"
---
# <a name="configure-virtual-local-area-networks-for-hyper-v"></a>Configurare le reti locali virtuali per Hyper-V
Le reti locali virtuali \(VLAN\) offrono un modo per isolare il traffico di rete. Le VLAN vengono configurate nei commutatori e router che supportano 802.1q. Se si configurano più VLAN e si desidera che la comunicazione tra loro, è necessario configurare i dispositivi di rete per consentire l'associazione. 

È necessario quanto segue per configurare le VLAN:  
  
-   Una scheda di rete fisica e il driver che supporta 802.1q codifica VLAN.  
-   Un commutatore di rete fisica che supporta 802.1q codifica VLAN.  
  
Nell'host, si configurerà il commutatore virtuale per consentire il traffico di rete nella porta del commutatore fisico. Questo vale per gli ID VLAN che si desidera utilizzare internamente con macchine virtuali. Successivamente, si configura la macchina virtuale per specificare la VLAN che la macchina virtuale verrà utilizzato per tutte le comunicazioni di rete.  
  
#### <a name="to-allow-a-virtual-switch-to-use-a-vlan"></a>Per consentire un commutatore virtuale utilizzare una VLAN  
  
1.  Aprire Hyper\-Manager V.  
  
2.  Dal menu Azioni, fare clic su **gestione commutatori virtuali**.  
  
3.  Sotto **commutatori virtuali**, selezionare un commutatore virtuale connesso a una scheda di rete fisica che supporti le VLAN. 

4. Nel riquadro di destra, nell'ID di VLAN, selezionare **Abilita identificazione LAN virtuale** e quindi digitare un numero per la VLAN ID.  
  
    Tutto il traffico che passa attraverso la scheda di rete fisica connessa al commutatore virtuale verrà contrassegnato con l'ID VLAN è impostato.  
  
#### <a name="to-allow-a-virtual-machine-to-use-a-vlan"></a>Per consentire una macchina virtuale da utilizzare una VLAN  
  
1.  Aprire Hyper\-Manager V.  
  
2.  Nel riquadro dei risultati, sotto **macchine virtuali**, selezionare la macchina virtuale appropriata e quindi fare doppio clic su **impostazioni**.  

3.  Sotto **Hardware**, selezionare un commutatore virtuale configurato con una VLAN.
  
4.  Nel riquadro di destra, selezionare **Abilita identificazione LAN virtuale**, quindi digitare lo stesso ID VLAN a quello specificato per il commutatore virtuale. 

Se la macchina virtuale deve usare altre VLAN, eseguire una delle operazioni seguenti:  
  
-   Collegare altre schede di rete virtuale per appropriato commutatori virtuali e assegnare gli ID VLAN. Assicurarsi di configurare gli indirizzi IP in modo corretto e che il traffico che si desidera eseguire l'indirizzamento attraverso la VLAN anche utilizza l'indirizzo IP corretto.  
  
-   Configurare la scheda di rete virtuale word in modalità trunk utilizzando il [impostata\-VMNetworkAdapterVlan](https://technet.microsoft.com/library/hh848475.aspx) cmdlet.
  
## <a name="see-also"></a>Vedere anche  
 
[Hyper\-V Virtual Switch](https://technet.microsoft.com/windows-server-docs/networking/technologies/hyper-v-virtual-switch/hyper-v-virtual-switch)