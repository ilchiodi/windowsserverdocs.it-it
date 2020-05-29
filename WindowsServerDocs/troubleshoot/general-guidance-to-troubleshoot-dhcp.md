---
title: Indicazioni generali per la risoluzione dei problemi relativi a DHCP
description: Questo artilce introduce indicazioni generali per la risoluzione dei problemi relativi a DHCP.
ms.prod: windows-server
ms.service: na
manager: dcscontentpm
ms.technology: server-general
ms.date: 5/26/2020
ms.topic: article
author: Deland-Han
ms.author: delhan
ms.openlocfilehash: c0460791fef2451722af09e8bbe08b51a605f01b
ms.sourcegitcommit: ef089864980a1d4793a35cbf4cbdd02ce1962054
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/28/2020
ms.locfileid: "84150159"
---
# <a name="general-guidance-to-troubleshoot-dhcp"></a>Indicazioni generali per la risoluzione dei problemi relativi a DHCP

Prima di iniziare a risolvere i problemi, controllare gli elementi seguenti. Questi possono aiutare a individuare la causa radice del problema.

## <a name="checklist"></a>Elenco di controllo

  - Quando è iniziato il problema?

  - Sono presenti messaggi di errore?

  - Il server DHCP funzionava in precedenza o non ha mai funzionato?  
    Se funzionava in precedenza, è stato modificato prima dell'avvio del problema. Ad esempio, è stato installato un aggiornamento? È stata apportata una modifica all'infrastruttura?

  - Il problema persiste o intermittente? Se è intermittente, quando è stata eseguita l'ultima volta?

  - Gli errori di lease degli indirizzi si verificano per tutti i client o solo per client specifici, ad esempio una subnet con ambito singolo?

  - Ci sono client nella stessa subnet di rete del server DHCP?

  - Se i client si trovano nella stessa subnet di rete, possono ottenere indirizzi IP?

  - Se i client non si trovano nella stessa subnet di rete, i router o i commutatori VLAN sono configurati correttamente per avere agenti di inoltro DHCP (noti anche come helper IP)?

  - Il server DHCP è autonomo o è configurato per la disponibilità elevata, ad esempio il failover con ambito diviso o DHCP?

  - Controllare i dispositivi intermedi per le funzionalità, ad esempio VRRP/HSRP, l'ispezione dinamica ARP o il snooping DHCP, che potrebbero causare problemi.
