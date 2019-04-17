---
title: Architettura dei criteri QoS
description: In questo argomento viene fornita una panoramica di qualità del servizio (QoS), che consente di usare criteri di gruppo per stabilire le priorità del traffico larghezza di banda di specifiche applicazioni e servizi in Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 25097cb8-b9b1-41c9-b3c7-3610a032e0d8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 00d36604c57add6bf9f45b0166b08c1fb15be467
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="qos-policy-architecture"></a>Architettura dei criteri QoS

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per informazioni sull'architettura dei criteri QoS.

Nella figura seguente è illustrata l'architettura di QoS basata su criteri.

![Architettura dei criteri QoS](../../media/QoS/QoS-Policy-Architecture.jpg)

L'architettura di QoS basata su criteri include i componenti seguenti:

- **Servizio Client di criteri di gruppo**. Un servizio di Windows che gestisce le impostazioni di criteri di gruppo Configurazione utente e computer.

- **Modulo criteri di gruppo**. Un componente del servizio Client di criteri di gruppo che recupera le impostazioni di criteri di gruppo Configurazione utente e computer di Active Directory all'avvio e periodicamente verifiche \ (per impostazione predefinita, ogni 90 minutes\). Se vengono rilevate le modifiche, il motore dei criteri di gruppo consente di recuperare le nuove impostazioni di criteri di gruppo. Il motore dei criteri di gruppo Elabora oggetti Criteri di gruppo in ingresso e informa l'estensione lato Client di QoS quando vengono aggiornati i criteri QoS.

- **Estensione lato Client QoS**. Un componente del servizio Client di criteri di gruppo che attende un'indicazione del motore di criteri di gruppo che sono stati modificati i criteri QoS e informa il modulo di ispezione QoS.

- **Stack TCP/IP**. Lo stack TCP/IP che include il supporto integrato per IPv4 e IPv6 e supporta la piattaforma filtro Windows. 

- **Ispezione QoS**. Un modulo componente nello stack TCP/IP per indicazioni di modifiche a criteri di QoS dall'estensione lato Client, QoS attesa recupera le impostazioni dei criteri QoS e interagisce con il Layer di trasporto e Pacer.sys internamente contrassegnare il traffico che soddisfa i criteri QoS.

- **NDIS 6. x**. Un'interfaccia standard tra i driver di rete in modalità kernel e il sistema operativo nei sistemi operativi Windows Server e Client. NDIS 6. x supporta leggeri filtri, ovvero un modello di driver semplificata per i driver NDIS intermedi e i driver miniport che fornisce prestazioni migliori.

- **Interfaccia di rete QoS Provider \(NPI\)**. Un'interfaccia per driver in modalità kernel interagire con Pacer.sys.

- **Pacer. sys**. Un driver di filtro leggero 6. x NDIS che controlla la pianificazione dei pacchetti per QoS basata su criteri e per il traffico di applicazioni che utilizzano il \(GQoS\) QoS generico e il controllo del traffico \(TC\) API. Pacer.sys sostituito Psched.sys in Windows Server 2003 e Windows XP. Pacer.sys viene installato con il componente utilità di pianificazione pacchetti QoS dalle proprietà di una connessione di rete o scheda.

Per l'argomento successivo in questa Guida, vedere [scenari dei criteri QoS](qos-policy-scenarios.md).

Per il primo argomento in questa Guida, vedere [criteri Quality of Service (QoS)](qos-policy-top.md).

