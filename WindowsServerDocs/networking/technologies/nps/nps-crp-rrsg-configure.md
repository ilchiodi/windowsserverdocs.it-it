---
title: Configurare i gruppi di server RADIUS remoti
description: In questo argomento vengono fornite informazioni su come configurare gruppi di Server RADIUS remoti in Server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ca125e57-249c-4d97-85d1-2929cbf871f1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 02088a35196c0bfadeb65e8971a47fdcc741258d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818762"
---
# <a name="configure-remote-radius-server-groups"></a>Configurare i gruppi di server RADIUS remoti

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per configurare gruppi di server RADIUS remoti quando si vuole configurare criteri di rete per agire come un server proxy e le richieste di connessione diretta a altri NPSs per l'elaborazione.

## <a name="add-a-remote-radius-server-group"></a>Aggiungere un gruppo di Server RADIUS remoto

È possibile utilizzare questa procedura per aggiungere un nuovo gruppo di server RADIUS remoto nello snap-in Strumentazione gestione Windows (NPS, Network Policy Server).

Quando si configura criteri di rete come proxy RADIUS, si crea un nuovo criterio richiesta di connessione che utilizza criteri di rete per determinare quali richieste di connessione inoltrare ad altri server RADIUS. Inoltre, il criterio di richiesta di connessione viene configurato specificando un remoto gruppo di server RADIUS che contiene uno o più server RADIUS, che indica dove inviare le richieste di connessione che corrispondono al criterio di richiesta di connessione NPS.

>[!NOTE]
>È anche possibile configurare un nuovo gruppo di server RADIUS remoto durante il processo di creazione di un nuovo criterio richiesta di connessione.

Per eseguire questa procedura, è necessaria almeno l'appartenenza al gruppo **Domain Admins** o a un gruppo equivalente.

### <a name="to-add-a-remote-radius-server-group"></a>Per aggiungere un gruppo di server RADIUS remoti 

1. In Server Manager fare clic su **degli strumenti**, quindi fare clic su **Server dei criteri di rete** per aprire la console Criteri di rete.
2. Nell'albero della console, fare doppio clic su **client e server RADIUS**, fare doppio clic su **gruppi di Server RADIUS remoti**, quindi fare clic su **New**.
3. Il **nuovo gruppo di Server RADIUS remoti** verrà visualizzata la finestra di dialogo. Nelle **nome gruppo**, digitare un nome per il gruppo di server RADIUS remoto.
4. **Nel server RADIUS**, fare clic su **Add**. Il **Aggiungi server RADIUS** verrà visualizzata la finestra di dialogo. Digitare l'indirizzo IP del server RADIUS che si desidera aggiungere al gruppo, oppure digitare il nome di dominio completo \(FQDN\) del server RADIUS e quindi fare clic su **Verify**.
5. In **Aggiungi server RADIUS** fare clic sulla scheda **Autenticazione/Accounting**. Nelle **segreto condiviso** e **Conferma segreto condiviso**, digitare il segreto condiviso. Quando si configura il computer locale come client RADIUS del server RADIUS remoti, è necessario usare lo stesso segreto condiviso.
6. Se non si usa Extensible Authentication Protocol (EAP) per l'autenticazione, fare clic su **richiesta deve contenere l'attributo autenticatore messaggio**. EAP utilizza l'attributo autenticatore messaggio per impostazione predefinita.
7. Verificare che i numeri di porta di autenticazione e accounting siano corretti per la distribuzione.
8. Se si usa un segreto condiviso diverso per l'accounting, in **Accounting**deselezionare la **usano lo stesso segreto condiviso per l'autenticazione e accounting** casella di controllo e quindi digitare il segreto condiviso di accounting in  **Segreto condiviso** e **Conferma segreto condiviso**.
9. Se non si desidera inoltrare l'avvio del server Accesso rete e arrestare i messaggi al server RADIUS remoti, deselezionare il **inoltrare l'avvio del server Accesso rete e arrestare le notifiche per questo server** casella di controllo.

Per altre informazioni sulla gestione dei criteri di rete, vedere [gestire i Server dei criteri di rete](nps-manage-top.md).

Per altre informazioni sui criteri di rete, vedere [Strumentazione gestione Windows (NPS, Network Policy Server)](nps-top.md).

