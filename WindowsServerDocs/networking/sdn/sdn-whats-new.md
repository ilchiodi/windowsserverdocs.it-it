---
title: Novità di SDN per Windows Server
description: Questo argomento fornisce informazioni sulle nuove funzionalità di Software Defined Networking per Windows Server 1709
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: efad919b-e9e7-4a0c-b373-e68a092f93b5
ms.author: pashort
author: shortpatti
ms.date: 10/02/2018
ms.openlocfilehash: 803ca27ca138281cbea1a93aca7e5a8b799bd862
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870163"
---
# <a name="whats-new-in-sdn-for-windows-server-2019"></a>Novità di SDN per Windows Server 2019

>Si applica a Windows Server (Canale semestrale)


|                         **Funzionalità**                          |                                                                                                                                                                                         **Descrizione**                                                                                                                                                                                         | **Nuovo/aggiornato** |
|--------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|
| [Reti crittografate](vnet-encryption/sdn-vnet-encryption.md) | La crittografia della rete virtuale consente la crittografia del traffico di rete virtuale tra macchine virtuali che comunicano tra loro all'interno delle subnet contrassegnate come "crittografia abilitata". Utilizza anche Datagram Transport Layer Security (DTLS) nella subnet virtuale per codificare pacchetti. DTLS impedisce intercettazioni, manomissioni e contraffazioni da parte di chiunque abbia accesso alla rete fisica. |       Nuova       |
|    [Controllo del firewall](security/sdn-firewall-auditing.md)    |                                                                                            Il controllo del firewall è una nuova funzionalità per il firewall SDN in Windows Server 2019. Quando si Abilita il firewall SDN, viene registrato qualsiasi flusso elaborato dalle regole del firewall SDN (ACL) con la registrazione abilitata.                                                                                            |       Nuova       |
| [Peering di rete virtuale](vnet-peering/sdn-vnet-peering.md)  |                                                                                                                      Il peering di rete virtuale consente di connettere facilmente due reti virtuali. Una volta eseguito il peering, per finalità di connettività, le reti virtuali vengono visualizzate come una sola.                                                                                                                      |       Nuova       |
|           [Misurazione in uscita](manage/sdn-egress.md)            |                  Questa nuova funzionalità di Windows Server 2019 consente a SDN di offrire contatori di utilizzo per i trasferimenti di dati in uscita. Con questa funzionalità aggiunta, il controller di rete mantiene un elenco di elementi consentiti per ogni rete virtuale di tutti gli intervalli IP usati in SDN e considera i pacchetti associati a una destinazione non inclusa in uno di questi intervalli per la fatturazione dei trasferimenti di dati in uscita.                   |       Nuova       |

---



