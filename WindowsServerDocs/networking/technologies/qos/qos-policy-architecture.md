---
title: Architettura dei criteri QoS
description: In questo argomento viene fornita una panoramica dei criteri di qualità del servizio (QoS), che consentono di usare criteri di gruppo per definire le priorità della larghezza di banda di rete del traffico di specifiche applicazioni e servizi in Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 25097cb8-b9b1-41c9-b3c7-3610a032e0d8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bad37ba3558137b02ae495fe8dd9be2c903cdd97
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843142"
---
# <a name="qos-policy-architecture"></a>Architettura dei criteri QoS

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per informazioni sull'architettura dei criteri QoS.

La figura seguente mostra l'architettura della QoS basata su criteri.

![Architettura dei criteri QoS](../../media/QoS/QoS-Policy-Architecture.jpg)

L'architettura della QoS basata su criteri include i componenti seguenti:

- **Servizio Client di criteri di gruppo**. Un servizio Windows che gestisce le impostazioni di criteri di gruppo Configurazione utente e computer.

- **Modulo criteri di gruppo**. Un componente del servizio Client di criteri di gruppo che recupera le impostazioni di criteri di gruppo Configurazione utente e computer di Active Directory dopo l'avvio e periodicamente controlli le modifiche apportate \(per impostazione predefinita, ogni 90 minuti\). Se vengono rilevate modifiche, il motore di criteri di gruppo consente di recuperare le nuove impostazioni di criteri di gruppo. Il motore di criteri di gruppo Elabora oggetti Criteri di gruppo in ingresso e informa l'estensione lato Client di QoS quando vengono aggiornati i criteri QoS.

- **Estensione lato Client QoS**. Un componente del servizio Client di criteri di gruppo che attende un'indicazione del motore di criteri di gruppo che sono stati modificati i criteri QoS e informa il modulo di ispezione di QoS.

- **Stack TCP/IP**. Lo stack TCP/IP che include il supporto integrato per IPv4 e IPv6 e supporta Windows Filtering Platform. 

- **Ispezione QoS**. Componente di un modulo nello stack TCP/IP di attesa per le indicazioni di modifiche ai criteri di QoS dell'estensione lato Client QoS, recupera le impostazioni dei criteri di QoS e interagisce con il livello di trasporto e Pacer. sys per contrassegnare internamente il traffico che soddisfa i criteri QoS criteri.

- **NDIS 6.x**. Un'interfaccia standard tra driver di rete in modalità kernel e il sistema operativo nei sistemi operativi Windows Server e Client. NDIS 6.x supporta leggeri filtri, ovvero un modello di driver semplificata per i driver NDIS intermedi e driver miniport che offre prestazioni migliori.

- **Interfaccia del Provider di rete QoS \(NPI\)**. Interfaccia per i driver in modalità kernel interagire con Pacer.

- **Pacer**. Un driver filtro leggeri 6.x NDIS che controlla la pianificazione dei pacchetti per QoS basata su criteri e per il traffico delle applicazioni che usano il Generic QoS \(GQoS\) e il controllo del traffico \(TC\) API. Pacer. sys sostituito Psched. sys in Windows Server 2003 e Windows XP. Pacer. sys è installato con il componente utilità di pianificazione pacchetti QoS dalle proprietà di una connessione di rete o una scheda.

Per l'argomento successivo in questa Guida, vedere [scenari relativi ai criteri di QoS](qos-policy-scenarios.md).

Per il primo argomento in questa Guida, vedere [dei criteri di qualità del servizio (QoS)](qos-policy-top.md).

