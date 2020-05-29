---
title: Guida alla risoluzione dei problemi per Dynamic Host Configuration Protocol (DHCP)
description: Questo artilce introduce una guida alla risoluzione dei problemi relativi a DHCP.
ms.prod: windows-server
ms.service: na
manager: dcscontentpm
ms.technology: server-general
ms.date: 5/26/2020
ms.topic: article
author: Deland-Han
ms.author: delhan
ms.openlocfilehash: 4bded9f879061903b1e925632a3cb4c83dfaac08
ms.sourcegitcommit: ef089864980a1d4793a35cbf4cbdd02ce1962054
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/28/2020
ms.locfileid: "84150319"
---
# <a name="troubleshooting-guide-for-dynamic-host-configuration-protocol-dhcp"></a>Guida alla risoluzione dei problemi per Dynamic Host Configuration Protocol (DHCP)

Per qualsiasi dispositivo (ad esempio un computer o un telefono) per poter funzionare in una rete, è necessario che sia assegnato un indirizzo IP. È possibile assegnare un indirizzo IP manualmente o automaticamente. L'assegnazione automatica viene gestita dal servizio DHCP (server Microsoft o di terze parti).

In questo articolo verranno illustrate le procedure generali per la risoluzione dei problemi per il client e il server DHCP IPv4 Microsoft.

## <a name="more-information"></a>Ulteriori informazioni

La procedura per l'assegnazione di indirizzi IPv4 prevede in genere tre componenti principali:

- Un dispositivo client DHCP che deve ottenere un indirizzo IP

- Un servizio DHCP che fornisce indirizzi IP al client in base a impostazioni specifiche

- Un agente di inoltro DHCP/IP Helper per inviare richieste di trasmissione DHCP a un segmento di rete diverso

Una comunicazione da client a server DHCP è costituita da tre tipi di interazione tra i due peer:

- **Dora basata su broadcast** (individuazione, offerta, richiesta, riconoscimento). Questo processo comprende i passaggi seguenti:
  
    - Il client DHCP invia una richiesta di trasmissione di individuazione DHCP a tutti i server DHCP disponibili all'interno di un intervallo.
  
    - Viene ricevuta una risposta di trasmissione dell'offerta DHCP dal server DHCP, che offre un lease di indirizzo IP disponibile.
  
    - La richiesta di trasmissione del client DHCP richiede il lease dell'indirizzo IP offerto e la conferma della trasmissione DHCP alla fine.
  
    - Se il client e il server DHCP si trovano in segmenti di rete logica diversi, un agente di inoltro DHCP funge da server di inoltro, inviando i pacchetti di trasmissione DHCP tra i peer.

- **Richieste di rinnovo unicast DHCP**: vengono inviate direttamente al server DHCP dal client DHCP per rinnovare l'assegnazione di indirizzi IP dopo il 50% del tempo di lease degli indirizzi IP.

- **Riassocia richieste broadcast DHCP**: vengono effettuate a qualsiasi server DHCP compreso nell'intervallo del client. Questi vengono inviati dopo il 87,5% della durata del lease di indirizzi IP, perché questo indica che la richiesta unicast diretta non funziona. Per quanto riguarda il processo di DORA, questo processo implica la comunicazione di un agente di inoltro DHCP.

Se un client Microsoft DHCP non riceve un indirizzo IPv4 DHCP valido, è probabile che il client sia configurato per l'utilizzo di un indirizzo APIPA. Per ulteriori informazioni, vedere l'articolo della Knowledge Base seguente:  
[220874](https://support.microsoft.com/help/220874) come usare l'indirizzamento TCP/IP automatico senza un server DHCP

Tutte le comunicazioni vengono eseguite sulle porte UDP 67 e 68. Per ulteriori informazioni, vedere l'articolo della Knowledge Base seguente:  
[169289](https://support.microsoft.com/help/169289) nozioni fondamentali su DHCP (Dynamic Host Configuration Protocol).