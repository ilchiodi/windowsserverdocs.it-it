---
title: Nuove funzionalità di SDN in Windows Server
description: In questo argomento vengono fornite informazioni sulle nuove funzionalità di Software Defined Networking per Windows Server 1709
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: efad919b-e9e7-4a0c-b373-e68a092f93b5
ms.author: pashort
author: shortpatti
ms.date: 10/02/2018
ms.openlocfilehash: aef2bc32f249550d4e8d33d4b871ca98010e2f49
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446297"
---
# <a name="whats-new-in-sdn-for-windows-server-2019"></a>Novità di SDN per Windows Server 2019

>Si applica a: Windows Server (Canale semestrale)


|                         **Funzionalità**                          |                                                                                                                                                                                         **Descrizione**                                                                                                                                                                                         | **New/updated** |
|--------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|
| [Reti crittografate](vnet-encryption/sdn-vnet-encryption.md) | Crittografia di rete virtuale consente la crittografia del traffico della rete virtuale tra le macchine virtuali che comunicano tra loro all'interno di subnet contrassegnata come 'Crittografia Enabled'. Utilizza anche Datagram Transport Layer Security (DTLS) nella subnet virtuale per codificare pacchetti. DTLS impedisce intercettazioni, manomissioni e contraffazioni da parte di chiunque abbia accesso alla rete fisica. |       Nuova       |
|    [Il controllo del firewall](security/sdn-firewall-auditing.md)    |                                                                                            Il controllo firewall è una nuova funzionalità per il firewall di rete SDN in Windows Server 2019. Quando si abilita il firewall di rete SDN, ottiene registrato qualsiasi flusso elaborata dalle regole di firewall di rete SDN (ACL) che hanno abilitata la registrazione.                                                                                            |       Nuova       |
| [Peering di rete virtuale](vnet-peering/sdn-vnet-peering.md)  |                                                                                                                      Peering reti virtuali consente di connettere due reti virtuali senza problemi. Una volta eseguito il peering, per motivi di connettività, le reti virtuali vengono visualizzate come una.                                                                                                                      |       Nuova       |
|           [Dati in uscita di misurazione](manage/sdn-egress.md)            |                  Questa nuova funzionalità di Windows Server 2019 Abilita SDN offrire i contatori di utilizzo per i trasferimenti di dati in uscita. Con questa funzionalità aggiunta, mantiene di Controller di rete, un elenco elementi consentiti per ogni rete virtuale di tutti gli intervalli IP usati all'interno di SDN e prendere in considerazione qualsiasi pacchetto associato per una destinazione che non è incluso in uno di questi intervalli di essere fatturato trasferimenti dati in uscita.                   |       Nuova       |

---



