---
title: Opzioni di selezione Subnet DHCP
description: In questo argomento vengono fornite informazioni sulle opzioni di selezione subnet DHCP per Dynamic Host Configuration Protocol (DHCP) in Windows Server 2016.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: ca19e7d1-e445-48fc-8cf5-e4c45f561607
ms.author: pashort
author: shortpatti
ms.date: 08/17/2018
ms.openlocfilehash: 034ca48ef13a6bdac63ca99ac753fc9826460922
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870812"
---
# <a name="dhcp-subnet-selection-options"></a>Opzioni di selezione Subnet DHCP

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per informazioni sulle nuove opzioni di selezione subnet DHCP.

DHCP ora supporta l'opzione 82 \(Sub-opzione 5\). È possibile usare queste opzioni per consentire i client proxy DHCP e gli agenti di inoltro richiedere un indirizzo IP per una subnet specifica e da un intervallo di indirizzi IP specifico e un ambito.  Per altre informazioni, vedere **opzione 82 Sub opzione 5**: [Opzioni secondarie RFC 3527 collegamento selezione per l'opzione di informazioni dell'agente di inoltro per DHCPv4](https://tools.ietf.org/html/rfc3527).

Se si usa un agente di inoltro DHCP configurato con l'opzione DHCP 82, Sub-opzione 5, l'agente di inoltro può richiedere un lease di indirizzi IP per i client DHCP da un intervallo di indirizzi IP specifico.


## <a name="option-82-sub-option-5-link-selection-sub-option"></a>Opzione 82 Sub opzione 5: Opzione di collegamento selezione secondaria

L'opzione secondaria di selezione di collegamento dell'agente di inoltro consente a un agente di inoltro DHCP specificare una subnet IP da cui il server DHCP deve assegnare indirizzi IP e le opzioni.

In genere, gli agenti di inoltro DHCP si basano sull'indirizzo IP Gateway \(GIADDR\) campo per comunicare con i server DHCP. Tuttavia, GIADDR è limitato dalle due funzioni operative:

1. In modo da informare il server DHCP nella subnet su cui risiede il client DHCP che richiede il lease di indirizzi IP.
2. Per informare il server DHCP dell'indirizzo IP da usare per comunicare con l'agente di inoltro.

In alcuni casi, l'indirizzo IP utilizzato dall'agente di inoltro per comunicare con il server DHCP potrebbe essere diverso rispetto all'intervallo di indirizzi IP da cui deve essere allocato l'indirizzo IP client DHCP. 

L'opzione di collegamento selezione secondaria dell'opzione 82 è utile in questo caso, consentendo all'agente di inoltro in modo esplicito la subnet da cui vuole che l'indirizzo IP allocato sotto forma di opzione DHCP v4 82 opzione secondaria di 5.

> [!NOTE]
>
> Tutti inoltro agente gli indirizzi IP (GIADDR) devono far parte di un intervallo di indirizzi IP di ambito DHCP attivo. Qualsiasi GIADDR di fuori di intervalli di indirizzi IP di ambito DHCP viene considerato un inoltro rogue e Server DHCP di Windows non lo riconoscerà le richieste client DHCP da tali agenti di inoltro.
>
> Un ambito speciale può essere creato per gli agenti di inoltro "autorizza". Creare un ambito con la GIADDR (o più se il GIADDR sono sequenziali gli indirizzi IP), escludere gli indirizzi GIADDR dalla distribuzione e quindi attivare l'ambito. Questo autorizza gli agenti di inoltro impedendo che gli indirizzi GIADDR assegnato.


### <a name="use-case-scenario"></a>Scenari dei casi d'uso

In questo scenario, rete di un'organizzazione include un server DHCP sia un punto di accesso Wireless \(Asia Pacifico\) per gli utenti guest. Indirizzi IP client gli utenti guest assegnati dal server DHCP dell'organizzazione, tuttavia, a causa di restrizioni dei criteri firewall, il server DHCP non può accedere la rete wireless guest o i computer client wireless con broadcase messaggi.

Per risolvere questa limitazione, il punto di accesso è configurato con 5 collegamento selezione Sub opzione per specificare la subnet da cui vuole che l'indirizzo IP allocato per i guest client, mentre il GIADDR anche specificando l'indirizzo IP dell'interfaccia interno che fa sì che la rete aziendale.
