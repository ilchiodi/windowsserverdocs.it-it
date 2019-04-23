---
title: Gruppi di server RADIUS remoti
description: In questo argomento viene fornita una panoramica di rete criteri Server RADIUS gruppi di Server remoti in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d81678a7-be21-48f2-9b3f-5a75d6aef013
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9912927a7b75e4c9f04aa3d24eb7ed46c73a7dd2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855262"
---
# <a name="remote-radius-server-groups"></a>Gruppi di server RADIUS remoti

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Quando si configura Server dei criteri di rete (NPS) come proxy RADIUS Remote Authentication Dial-In User Service (), utilizzare criteri di rete per inoltrare le richieste di connessione al server RADIUS che sono in grado di elaborare le richieste di connessione perché possano eseguire autenticazione e autorizzazione nel dominio in cui si trova l'account utente o computer. Ad esempio, se si desidera inoltrare le richieste di connessione a uno o più server RADIUS in domini non trusted, è possibile configurare criteri di rete come proxy RADIUS per inoltrare le richieste al server RADIUS remoti nel dominio non trusted.

>[!NOTE]
>Gruppi di server RADIUS remoti sono correlate alle e separati dai gruppi di Windows.

Per configurare criteri di rete come proxy RADIUS, è necessario creare un criterio di richiesta di connessione che contiene tutte le informazioni necessarie per criteri di rete per valutare i messaggi da inoltrare e dove inviare i messaggi.

Quando si configura un gruppo di server RADIUS remoti in Criteri di rete e si configura un criterio di richiesta di connessione con il gruppo, si definisce la posizione in cui deve inoltrare le richieste di connessione NPS.

## <a name="configuring-radius-servers-for-a-group"></a>Configurazione dei server RADIUS per un gruppo

Un gruppo di server RADIUS remoti è un gruppo denominato che contiene uno o più server RADIUS. Se si configura più di un server, è possibile specificare le impostazioni per determinare l'ordine in cui i server vengono utilizzati dal proxy o distribuire il flusso dei messaggi RADIUS tra tutti i server del gruppo per impedire il sovraccarico di uno o più server di bilanciamento del carico con un numero eccessivo di richieste di connessione.

Ogni server del gruppo ha le seguenti impostazioni.

- **Nome o indirizzo**. Ogni membro del gruppo deve avere un nome univoco all'interno del gruppo. Il nome può essere un indirizzo IP o un nome che può essere risolto nel relativo indirizzo IP.

- **Autenticazione e accounting**. È possibile inoltrare le richieste di autenticazione, richieste di gestione o entrambi per ogni membro del gruppo di server RADIUS remoto.

- **Il bilanciamento del carico**. Un'impostazione di priorità viene usata per indicare quale membro del gruppo è il server primario (la priorità è impostata su 1). Per i membri del gruppo che hanno la stessa priorità, un'impostazione del peso viene utilizzata per calcolare con quale frequenza vengono inviati i messaggi RADIUS a ogni server. È possibile utilizzare le impostazioni aggiuntive per configurare il modo in cui i criteri di rete rileva quando un membro del gruppo non è più disponibile e quando diventa disponibile dopo che è stato determinato come non disponibile.

Dopo aver configurato un gruppo di Server RADIUS remoti, è possibile specificare il gruppo di nell'autenticazione e le impostazioni dell'accounting di un criterio di richiesta di connessione. Per questo motivo, è possibile configurare un gruppo di server RADIUS remoti. Successivamente, è possibile configurare i criteri di richiesta di connessione per utilizzare il gruppo di server RADIUS remoto appena configurato. In alternativa, è possibile utilizzare la creazione guidata nuovo criterio richiesta di connessione per creare un nuovo gruppo di server RADIUS remoto durante la creazione di criteri di richiesta di connessione.

Per altre informazioni sui criteri di rete, vedere [Strumentazione gestione Windows (NPS, Network Policy Server)](nps-top.md).
