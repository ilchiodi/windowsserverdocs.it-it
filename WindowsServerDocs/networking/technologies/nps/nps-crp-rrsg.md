---
title: Gruppi di server RADIUS remoti
description: Questo argomento fornisce una panoramica dei gruppi di server RADIUS remoti del server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: d81678a7-be21-48f2-9b3f-5a75d6aef013
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 63a6eb5f0f78ed8dbcc0144602f16274fd6ec213
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396318"
---
# <a name="remote-radius-server-groups"></a>Gruppi di server RADIUS remoti

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Quando si configura server dei criteri di rete come proxy RADIUS (Remote Authentication Dial-In User Service), si utilizza NPS per inviare le richieste di connessione ai server RADIUS in grado di elaborare le richieste di connessione in quanto possono eseguire autenticazione e autorizzazione nel dominio in cui si trova l'account utente o computer. Se ad esempio si desidera inviare le richieste di connessione a uno o più server RADIUS in domini non trusted, è possibile configurare NPS come proxy RADIUS per l'inoltro delle richieste ai server RADIUS remoti nel dominio non trusted.

>[!NOTE]
>I gruppi di server RADIUS remoti non sono correlati a e separati dai gruppi di Windows.

Per configurare NPS come proxy RADIUS, è necessario creare un criterio di richiesta di connessione contenente tutte le informazioni necessarie per la valutazione dei messaggi da inoltrare e la posizione in cui inviare i messaggi.

Quando si configura un gruppo di server RADIUS remoti in NPS e si configura un criterio di richiesta di connessione con il gruppo, si designa la posizione in cui NPS deve inviare le richieste di connessione.

## <a name="configuring-radius-servers-for-a-group"></a>Configurazione dei server RADIUS per un gruppo

Un gruppo di server RADIUS remoto è un gruppo denominato che contiene uno o più server RADIUS. Se si configurano più di un server, è possibile specificare le impostazioni di bilanciamento del carico per determinare l'ordine in cui i server vengono utilizzati dal proxy o per distribuire il flusso di messaggi RADIUS tra tutti i server del gruppo per evitare l'overload di uno o più server con troppe richieste di connessione.

Per ogni server del gruppo sono disponibili le impostazioni seguenti.

- **Nome o indirizzo**. Ogni membro del gruppo deve avere un nome univoco all'interno del gruppo. Il nome può essere un indirizzo IP o un nome che può essere risolto nel relativo indirizzo IP.

- **Autenticazione e accounting**. È possibile inviare richieste di autenticazione, richieste di Accounting o entrambe a ciascun membro del gruppo di server RADIUS remoti.

- **Bilanciamento del carico**. Viene utilizzata un'impostazione di priorità per indicare quale membro del gruppo è il server primario (la priorità è impostata su 1). Per i membri del gruppo con la stessa priorità, viene usata un'impostazione del peso per calcolare la frequenza con cui vengono inviati i messaggi RADIUS a ogni server. È possibile utilizzare impostazioni aggiuntive per configurare il modo in cui il server dei criteri di gruppo rileva quando un membro del gruppo diventa disponibile per la prima volta e diventa disponibile dopo che è stato determinato che non è disponibile.

Dopo aver configurato un gruppo di server RADIUS remoti, è possibile specificare il gruppo nelle impostazioni di autenticazione e accounting di un criterio di richiesta di connessione. Per questo motivo, è possibile configurare prima un gruppo di server RADIUS remoto. Successivamente, è possibile configurare i criteri di richiesta di connessione per l'uso del gruppo di server RADIUS remoto appena configurato. In alternativa, è possibile utilizzare la creazione guidata nuovo criterio di richiesta di connessione per creare un nuovo gruppo di server RADIUS remoto durante la creazione dei criteri di richiesta di connessione.

Per ulteriori informazioni su NPS, vedere [Server dei criteri di rete (NPS)](nps-top.md).
