---
title: Opzioni di selezione Subnet DHCP
description: Questo argomento fornisce informazioni sulle opzioni di selezione subnet DHCP per Dynamic Host Configuration Protocol (DHCP) in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: ca19e7d1-e445-48fc-8cf5-e4c45f561607
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 43bc3d165f895767ded921b41118ecaccf9734e8
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="dhcp-subnet-selection-options"></a>Opzioni di selezione Subnet DHCP

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per informazioni sulle nuove opzioni di selezione subnet DHCP.

DHCP supporta ora \(sub-option 5\) opzioni 118 e 82. È possibile utilizzare queste opzioni per consentire ai client proxy DHCP e gli agenti di inoltro richiedere un indirizzo IP per una subnet specifica e da un intervallo di indirizzi IP specifico e l'ambito.

Se si utilizza un client proxy DHCP configurato con l'opzione DHCP 118, ad esempio un server di rete privata virtuale (VPN) che esegue Windows Server 2016 e il ruolo server di accesso remoto, il server VPN può richiedere lease di indirizzi IP per i client VPN da un intervallo di indirizzi IP specifico.

Se si utilizza un agente di inoltro DHCP configurato con l'opzione DHCP 82, secondarie opzione 5, l'agente di inoltro può richiedere un lease di indirizzi IP per i client DHCP da un intervallo di indirizzi IP specifico.

Di seguito sono collegamenti a richiesta per argomenti commenti per queste opzioni.

- **Opzione 118**: [RFC 3011 IPv4 Subnet selezione opzione per DHCP](http://www.rfc-base.org/rfc-3011.html)
- **Opzione 82 Sub opzione 5**: [opzioni secondarie RFC 3527 collegamento selezione per l'opzione di informazioni di agente di inoltro per DHCPv4](https://tools.ietf.org/html/rfc3527)


## <a name="dhcp-option-118-client-subnet-and-relay-agent-link-selection"></a>DHCP opzione 118: Subnet del Client e collegamento-selezione agenti di inoltro

L'opzione di selezione subnet DHCP fornisce un meccanismo per i proxy DHCP specificare una subnet IP da cui il server DHCP deve assegnare indirizzi IP e le opzioni.

### <a name="use-case-scenario"></a>Scenario di caso di utilizzo

In questo scenario, un server di rete privata virtuale \(VPN\) assegna indirizzi IP ai client VPN. 

In questo caso, il server VPN è connesso a entrambe intranet in cui è installato il server DHCP e a Internet, in modo che i client VPN possono connettersi al server VPN da posizioni remote.

Prima che il server VPN può fornire lease di indirizzi IP ai client VPN, il server contatta il server DHCP in una subnet interna e riserva un blocco di indirizzi IP. Quindi, il server VPN gestisce gli indirizzi IP che sono ottenute dal server DHCP. Se il server VPN fornisce tutti gli indirizzi IP riservati in lease ai client VPN, il server VPN ottiene quindi altri indirizzi IP dal server DHCP.

Configurando il server VPN con l'opzione DHCP 118, è possibile specificare l'intervallo di indirizzi IP e un ambito che si desidera utilizzare per i client VPN. Dopo aver configurato l'opzione 118, il server VPN richiede gli indirizzi IP da un intervallo di indirizzi IP specifico e un ambito dal server DHCP.

### <a name="the-dhcp-subnet-selection-option-field"></a>Il campo di opzioni DHCP selezione subnet

Il campo di opzioni DHCP subnet selezione contiene un singolo IPv4 indirizzo utilizzato per rappresentare l'indirizzo di subnet di origine per una richiesta di lease DHCP.  Un server DHCP configurato per rispondere a questa opzione alloca l'indirizzo da uno:

1. La subnet specificato nell'opzione di selezione subnet.
2. Una subnet che si trova in stesso segmento di rete della subnet specificato nell'opzione di selezione subnet.

## <a name="option-82-sub-option-5-link-selection-sub-option"></a>82 secondario come opzione 5: Opzione di Sub selezione collegamento

L'opzione secondari selezione collegamento dell'agente di inoltro consente un agente di inoltro DHCP specificare una subnet IP da cui il server DHCP deve assegnare indirizzi IP e le opzioni.

In genere, gli agenti di inoltro DHCP si basano sul campo indirizzo IP del Gateway \(GIADDR\) per comunicare con i server DHCP. Tuttavia, GIADDR è limitata dalle funzioni operative due:

1. Per informare la subnet in cui risiede il client DHCP che richiede il lease dell'indirizzo IP del server DHCP.
2. Per informare il server DHCP dell'indirizzo IP da utilizzare per comunicare con l'agente di inoltro.

In alcuni casi, l'indirizzo IP che usa l'agente di inoltro per comunicare con il server DHCP potrebbe essere diverso da quello l'intervallo di indirizzi IP da cui deve essere allocata l'indirizzo IP client DHCP. 

Impossibile creare agenti di inoltro DHCP utilizzare dell'opzione 118, come le funzionalità sono limitate e scrive solo il campo GIADDR o \(option 82\) l'opzione di informazioni dell'agente di inoltro. 

L'opzione di collegamento secondario di selezione dell'opzione 82 è utile in questo caso, consentendo l'agente di inoltro in modo esplicito la subnet da cui desidera che l'indirizzo IP allocato sotto forma di opzione v4 DHCP opzione sub 82 5.

### <a name="use-case-scenario"></a>Scenario di caso di utilizzo

In questo scenario, rete di un'organizzazione include un server DHCP e un punto di accesso Wireless \(AP\) per gli utenti guest. Assegnazione di indirizzi IP di guest client dal server DHCP dell'organizzazione, tuttavia, a causa di restrizioni dei criteri firewall, il server DHCP non può accedere la rete senza fili guest o un client senza fili con messaggi broadcase.

Per risolvere questa restrizione, il punto di accesso è configurata con l'opzione 5 di collegamento selezione secondario per specificare la subnet da cui vuole l'indirizzo IP allocato per i guest client, mentre il GIADDR anche specificando l'indirizzo IP dell'interfaccia che porta alla rete aziendale interna.
