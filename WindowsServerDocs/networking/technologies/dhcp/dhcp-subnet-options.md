---
title: Opzioni di selezione subnet DHCP
description: In questo argomento vengono fornite informazioni sulle opzioni di selezione della subnet DHCP per Dynamic Host Configuration Protocol (DHCP) in Windows Server 2016.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: ca19e7d1-e445-48fc-8cf5-e4c45f561607
ms.author: pashort
author: shortpatti
ms.date: 08/17/2018
ms.openlocfilehash: 4718204fad49b23c84cc73b67164f34a803ddd86
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405756"
---
# <a name="dhcp-subnet-selection-options"></a>Opzioni di selezione subnet DHCP

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per informazioni sulle nuove opzioni di selezione della subnet DHCP.

DHCP supporta ora l'opzione 82 \(opzione secondaria 5\). È possibile utilizzare queste opzioni per consentire ai client proxy DHCP e agli agenti di inoltro di richiedere un indirizzo IP per una subnet specifica e da un intervallo di indirizzi IP e un ambito specifici.  Per altri dettagli, vedere **opzione 82 sub option 5**: [RFC 3527 link Selection sub-option for the Relay Agent Information option for estensione DHCPv4](https://tools.ietf.org/html/rfc3527).

Se si usa un agente di inoltro DHCP configurato con l'opzione DHCP 82, l'opzione secondaria 5, l'agente di inoltro può richiedere un lease di indirizzi IP per i client DHCP da un intervallo di indirizzi IP specifico.


## <a name="option-82-sub-option-5-link-selection-sub-option"></a>Opzione 82 sub option 5: link Selection sub option

L'opzione secondaria selezione collegamento agente di inoltro consente a un agente di inoltro DHCP di specificare una subnet IP da cui il server DHCP deve assegnare gli indirizzi IP e le opzioni.

In genere, gli agenti di inoltro DHCP si basano sull'indirizzo IP del gateway \(campo\) GIADDR per comunicare con i server DHCP. Tuttavia, GIADDR è limitato dalle due funzioni operative:

1. Per informare il server DHCP sulla subnet su cui risiede il client DHCP che richiede il lease di indirizzi IP.
2. Per informare il server DHCP dell'indirizzo IP da utilizzare per comunicare con l'agente di inoltro.

In alcuni casi, l'indirizzo IP usato dall'agente di inoltro per comunicare con il server DHCP potrebbe essere diverso dall'intervallo di indirizzi IP da cui deve essere allocato l'indirizzo IP del client DHCP. 

L'opzione di selezione dei collegamenti dell'opzione 82 è utile in questa situazione, consentendo all'agente di inoltro di dichiarare in modo esplicito la subnet da cui si desidera che l'indirizzo IP venga allocato sotto forma di opzione DHCP V4 82 sub option 5.

> [!NOTE]
>
> Tutti gli indirizzi IP dell'agente di inoltro (GIADDR) devono far parte di un intervallo di indirizzi IP di ambito DHCP attivo. Qualsiasi GIADDR al di fuori degli intervalli di indirizzi IP di ambito DHCP è considerato un inoltro non autorizzato e il server DHCP Windows non rileverà le richieste del client DHCP da tali agenti di inoltro.
>
> È possibile creare un ambito speciale per "autorizzare" gli agenti di inoltro. Creare un ambito con GIADDR (o multiple se gli GIADDR sono indirizzi IP sequenziali), escludere gli indirizzi GIADDR dalla distribuzione e quindi attivare l'ambito. Gli agenti di inoltro saranno autorizzati a impedire l'assegnazione degli indirizzi GIADDR.


### <a name="use-case-scenario"></a>Scenario caso d'uso

In questo scenario, in una rete dell'organizzazione sono inclusi sia un server DHCP sia un punto di accesso wireless \(AP\) per gli utenti guest. Gli indirizzi IP dei client guest vengono assegnati dal server DHCP dell'organizzazione. Tuttavia, a causa delle restrizioni dei criteri del firewall, il server DHCP non può accedere alla rete wireless guest o ai client wireless con messaggi broadcase.

Per risolvere questa restrizione, il punto di accesso viene configurato con l'opzione di selezione del collegamento Sub 5 per specificare la subnet da cui si desidera che l'indirizzo IP sia allocato per i client Guest, mentre in GIADDR specifica anche l'indirizzo IP dell'interfaccia interna che conduce al rete aziendale.
