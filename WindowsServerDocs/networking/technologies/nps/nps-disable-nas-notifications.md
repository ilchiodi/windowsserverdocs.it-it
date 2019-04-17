---
title: Disabilitare l'inoltro di notifica NAS in Criteri di rete
description: In questo argomento vengono fornite istruzioni sulla configurazione di autenticazioni simultanee Server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: a09bfb03-95fc-4534-bf3c-97078ef6b07e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c25f3b5a94624a35099e84ede3296f7ab860da7c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="disable-nas-notification-forwarding-in-nps"></a>Disabilitare l'inoltro di notifica NAS in Criteri di rete

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questa procedura per disabilitare l'inoltro della schermata start e stop messaggi dal server di accesso di rete (NAS) per i membri di un gruppo di server RADIUS remoti configurati in Criteri di rete.

Quando si dispone di gruppi di server RADIUS remoti configurati e, in Criteri di rete **criteri richiesta di connessione**, cancellare il **inoltrare le richieste di accounting a questo gruppo di server RADIUS remoti** casella di controllo, questi gruppi vengono inviati i NAS avviare e arrestare i messaggi di notifica. 

Verrà creato il traffico di rete non necessari. Per eliminare il traffico, disabilitare la notifica NAS inoltro per i server singoli in ogni gruppo di server RADIUS remoto.

Per completare questa procedura, è necessario essere un membro del **amministratori** gruppo.

### <a name="to-disable-nas-notification-forwarding"></a>Per disabilitare l'inoltro delle notifiche NAS

1. In Server Manager, fare clic su **strumenti**, quindi fare clic su **Server dei criteri di rete**. Apre la console dei criteri di rete.

2. Nella console di criteri di rete, fare doppio clic su **client e server RADIUS**, fare clic su **gruppi di Server RADIUS remoti**, quindi fare doppio clic sul gruppo di server RADIUS remoto che si desidera configurare. Gruppo di server RADIUS remoti **proprietà** apre la finestra di dialogo.

3. Fare doppio clic il membro del gruppo che si desidera configurare, quindi fare clic il **l'Accounting o autenticazione** scheda.

4. In **Accounting**, deselezionare il **inoltrare l'avvio di server di accesso rete e interrompere le notifiche a questo server** casella di controllo, quindi fare clic su **OK**.

5. Ripetere i passaggi 3 e 4 per tutti i membri del gruppo che si desidera configurare.

Per ulteriori informazioni sulla gestione dei criteri di rete, vedere [gestire Server dei criteri di rete](nps-manage-top.md).

Per ulteriori informazioni sui criteri di rete, vedere [Server dei criteri di rete (NPS)](nps-top.md).
