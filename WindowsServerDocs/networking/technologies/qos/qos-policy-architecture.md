---
title: Architettura dei criteri QoS
description: Questo argomento fornisce una panoramica dei criteri di qualità del servizio (QoS), che consente di usare Criteri di gruppo per assegnare priorità alla larghezza di banda del traffico di rete di applicazioni e servizi specifici in Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 25097cb8-b9b1-41c9-b3c7-3610a032e0d8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 609d3f28465380b7d15648cfeb73070a39b9362f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395979"
---
# <a name="qos-policy-architecture"></a>Architettura dei criteri QoS

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per informazioni sull'architettura dei criteri QoS.

Nella figura seguente viene illustrata l'architettura di QoS basata su criteri.

![Architettura dei criteri QoS](../../media/QoS/QoS-Policy-Architecture.jpg)

L'architettura di QoS basata su criteri è costituita dai componenti seguenti:

- **Criteri di gruppo servizio client**. Un servizio di Windows che gestisce la configurazione di utenti e computer Criteri di gruppo impostazioni.

- **Motore criteri di gruppo**. Componente del servizio client di Criteri di gruppo che recupera le impostazioni di configurazione di utenti e computer Criteri di gruppo da Active Directory all'avvio e controlla periodicamente la presenza di modifiche \(per impostazione predefinita, ogni 90 minuti\). Se vengono rilevate modifiche, il motore di Criteri di gruppo Recupera le nuove impostazioni Criteri di gruppo. Il motore di Criteri di gruppo elabora gli oggetti Criteri di gruppo in ingresso e informa l'estensione del lato client QoS quando vengono aggiornati i criteri QoS.

- **Estensione lato client QoS**. Componente del servizio client di Criteri di gruppo che attende un'indicazione dal motore di Criteri di gruppo che i criteri QoS sono stati modificati e informa il modulo di ispezione QoS.

- **Stack TCP/IP**. Lo stack TCP/IP che include il supporto integrato per IPv4 e IPv6 e supporta la piattaforma filtro Windows. 

- **Controllo QoS**. Eseguire il modulo di un componente nello stack TCP/IP che attende le indicazioni relative alle modifiche dei criteri QoS dall'estensione lato client QoS, recupera le impostazioni dei criteri QoS e interagisce con il livello trasporto e Pacer. sys per contrassegnare internamente il traffico che corrisponde ai criteri QoS.

- **NDIS 6. x**. Interfaccia standard tra i driver di rete in modalità kernel e il sistema operativo nei sistemi operativi Windows Server e client. NDIS 6. x supporta i filtri leggeri, un modello di driver semplificato per i driver di NDIS intermedio e i driver miniport che offrono prestazioni migliori.

- **Interfaccia del provider di rete QoS \(NPI\)** . Interfaccia per i driver in modalità kernel che interagiscono con Pacer. sys.

- **Pacer. sys**. Un driver di filtro Lightweight NDIS 6. x che controlla la pianificazione dei pacchetti per QoS basata su criteri e per il traffico di applicazioni che usano il QoS generico \(GQoS\) e il controllo del traffico \(le API\) TC. Pacer. sys ha sostituito psched. sys in Windows Server 2003 e Windows XP. Pacer. sys viene installato con il componente utilità di pianificazione pacchetti QoS dalle proprietà di una connessione di rete o di una scheda.

Per l'argomento successivo di questa guida, vedere [QoS Policy Scenarios](qos-policy-scenarios.md).

Per il primo argomento di questa guida, vedere [criteri di qualità del servizio (QoS)](qos-policy-top.md).

