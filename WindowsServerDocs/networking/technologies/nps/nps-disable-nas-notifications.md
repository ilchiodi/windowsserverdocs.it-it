---
title: Disabilitare l'inoltro di notifica NAS in Criteri di rete
description: Questo argomento fornisce istruzioni sulla configurazione delle autenticazioni simultanee di Server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: a09bfb03-95fc-4534-bf3c-97078ef6b07e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bc4c6afdcb02eb2bbab1f0373a5b3a28236269bf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882262"
---
# <a name="disable-nas-notification-forwarding-in-nps"></a>Disabilitare l'inoltro di notifica NAS in Criteri di rete

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questa procedura per disabilitare l'inoltro di avviare e arrestare i messaggi dal server di accesso di rete (NAS) ai membri di un gruppo di server RADIUS remoti configurato in Criteri di rete.

Dopo aver configurato un gruppi di server RADIUS remoti e, in NPS **criteri di richiesta di connessione**, si deseleziona il **inoltrare le richieste di accounting a questo gruppo di server RADIUS remoti** casella di controllo, questi gruppi sono NAS ancora inviato avviare e arrestare i messaggi di notifica. 

Ciò consente di creare traffico di rete superfluo. Per eliminare questo tipo di traffico, disabilitare la notifica NAS di inoltro per i singoli server in ogni gruppo di server RADIUS remoto.

Per completare questa procedura, è necessario essere un membro del **amministratori** gruppo.

### <a name="to-disable-nas-notification-forwarding"></a>Per disabilitare l'inoltro di notifica di NAS

1. In Server Manager, fare clic su **strumenti**, quindi fare clic su **Server dei criteri di rete**. Apre la console Criteri di rete.

2. Nella console Criteri di rete, fare doppio clic su **client e server RADIUS**, fare clic su **gruppi di Server RADIUS remoti**e quindi fare doppio clic sul gruppo di server RADIUS remoto che si desidera configurare. Il gruppo di server RADIUS remoto **proprietà** verrà visualizzata la finestra di dialogo.

3. Fare doppio clic sul membro del gruppo che si desidera configurare e quindi scegliere il **Autenticazione/Accounting** scheda.

4. Nelle **Accounting**, deselezionare il **inoltrare l'avvio del server Accesso rete e arrestare le notifiche per questo server** casella di controllo e quindi fare clic su **OK**.

5. Ripetere i passaggi 3 e 4 per tutti i membri del gruppo che si desidera configurare.

Per altre informazioni sulla gestione dei criteri di rete, vedere [gestire i Server dei criteri di rete](nps-manage-top.md).

Per altre informazioni sui criteri di rete, vedere [Strumentazione gestione Windows (NPS, Network Policy Server)](nps-top.md).
