---
title: Gruppi di Server RADIUS remoti
description: In questo argomento viene fornita una panoramica di rete criteri di Server RADIUS gruppi di Server remoti in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d81678a7-be21-48f2-9b3f-5a75d6aef013
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1f27b5e501f110a038264cd54d75c8b8f9566a64
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="remote-radius-server-groups"></a>Gruppi di Server RADIUS remoti

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Quando si configura Server dei criteri di rete (NPS) come proxy RADIUS Remote Authentication Dial-In User Service (), utilizzare criteri di rete per inoltrare le richieste di connessione al server RADIUS che sono in grado di elaborare le richieste di connessione, perché possono eseguire l'autenticazione e autorizzazione nel dominio in cui si trova l'account utente o computer. Ad esempio, se si desidera inoltrare le richieste di connessione a uno o più server RADIUS in domini non trusted, è possibile configurare criteri di rete come proxy RADIUS per inoltrare le richieste inviate al server RADIUS remoti nel dominio non trusted.

>[!NOTE]
>Gruppi di server RADIUS remoti sono correlati e separato da gruppi di Windows.

Per configurare criteri di rete come proxy RADIUS, è necessario creare un criterio di richiesta di connessione che contiene tutte le informazioni necessarie per criteri di rete per valutare i messaggi per l'inoltro e inviare i messaggi.

Quando si configura un gruppo di server RADIUS remoto in Criteri di rete e si configura un criterio di richiesta di connessione con il gruppo, si designa la posizione in cui i criteri di rete è per inoltrare le richieste di connessione.

## <a name="configuring-radius-servers-for-a-group"></a>Configurazione dei server RADIUS per un gruppo

Un gruppo di server RADIUS remoto è un gruppo che contiene uno o più server RADIUS. Se si configura uno o più server, è possibile specificare le impostazioni per determinare l'ordine in cui vengono utilizzati i server da proxy o per distribuire il flusso dei messaggi RADIUS in tutti i server del gruppo per impedire il sovraccarico di uno o più server con un numero eccessivo di richieste di connessione di bilanciamento del carico.

Ogni server del gruppo presenta le impostazioni seguenti.

- **Nome o l'indirizzo**. Ogni membro del gruppo deve avere un nome univoco all'interno del gruppo. Il nome può essere un indirizzo IP o un nome che può essere risolto nel relativo indirizzo IP.

- **Autenticazione e accounting**. È possibile inoltrare le richieste di autenticazione, le richieste di accounting o entrambi per ogni membro del gruppo di server RADIUS remoto.

- **Bilanciamento del carico**. Un'impostazione di priorità viene utilizzata per indicare quale membro del gruppo è il server primario (la priorità è impostata su 1). Per i membri del gruppo che hanno la stessa priorità, un'impostazione peso viene usata per calcolare la frequenza con cui RADIUS vengono inviati a ogni server. È possibile utilizzare impostazioni aggiuntive per configurare il modo in cui il server dei criteri di rete rileva quando un membro del gruppo diventa non disponibile e quando diventa disponibile dopo aver determinato di non essere disponibili.

Dopo aver configurato un gruppo di Server RADIUS remoti, è possibile specificare il gruppo di autenticazione e le impostazioni di accounting di un criterio di richiesta di connessione. Per questo motivo, è possibile configurare un gruppo di server RADIUS remoto. Successivamente, è possibile configurare il criterio di richiesta di connessione per utilizzare il gruppo di server RADIUS remoto appena configurato. In alternativa, è possibile utilizzare la creazione guidata nuovo criterio di richiesta di connessione per creare un nuovo gruppo di server RADIUS remoto, anche se si sta creando il criterio di richiesta di connessione.

Per ulteriori informazioni sui criteri di rete, vedere [Server dei criteri di rete (NPS)](nps-top.md).
