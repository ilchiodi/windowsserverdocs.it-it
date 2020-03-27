---
title: Disabilitare l'invio di notifiche NAS in NPS
description: Questo argomento fornisce istruzioni sulla configurazione di autenticazioni simultanee del server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: a09bfb03-95fc-4534-bf3c-97078ef6b07e
ms.author: lizross
author: eross-msft
ms.openlocfilehash: dd56dfd4db9dd41c98141e2239efcca544a364fe
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316164"
---
# <a name="disable-nas-notification-forwarding-in-nps"></a>Disabilitare l'invio di notifiche NAS in NPS

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questa procedura per disabilitare l'invio di messaggi di avvio e di arresto da server di accesso alla rete (NAS) ai membri di un gruppo di server RADIUS remoti configurato in NPS.

Quando sono configurati gruppi di server RADIUS remoti e, in **criteri di richiesta di connessione**NPS, deselezionare la casella di controllo **inoltra richieste di contabilità a questo gruppo di server RADIUS remoti** . questi gruppi continuano a inviare i messaggi di notifica di avvio e arresto del server NAS. 

Viene creato un traffico di rete non necessario. Per eliminare il traffico, disabilitare l'invio di notifiche NAS per i singoli server in ogni gruppo di server RADIUS remoti.

Per completare questa procedura è necessaria l'appartenenza al gruppo **Administrators**.

### <a name="to-disable-nas-notification-forwarding"></a>Per disabilitare l'invio di notifiche NAS

1. In Server Manager, fare clic su **strumenti**, quindi fare clic su **Server dei criteri di rete**. Si apre la console NPS.

2. Nella console server dei criteri di gruppo fare doppio clic su **client e server RADIUS**, fare clic su **gruppi di server RADIUS remoti**, quindi fare doppio clic sul gruppo di server RADIUS remoti che si desidera configurare. Verrà visualizzata la finestra di dialogo **Proprietà** gruppo di server RADIUS remoti.

3. Fare doppio clic sul membro del gruppo che si desidera configurare, quindi fare clic sulla scheda **autenticazione/accounting** .

4. In **Contabilità**deselezionare la casella **di controllo Invia notifiche di avvio e arresto del server di accesso alla rete al server** e quindi fare clic su **OK**.

5. Ripetere i passaggi 3 e 4 per tutti i membri del gruppo che si desidera configurare.

Per ulteriori informazioni sulla gestione dei server dei criteri di rete, vedere [Manage Network Policy Server](nps-manage-top.md).

Per ulteriori informazioni su NPS, vedere [Server dei criteri di rete (NPS)](nps-top.md).
