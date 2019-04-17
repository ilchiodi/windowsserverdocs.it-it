---
title: Configurare gruppi di Server RADIUS remoti
description: In questo argomento fornisce informazioni su come configurare i gruppi di Server RADIUS remoti in Server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ca125e57-249c-4d97-85d1-2929cbf871f1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f293fd18176115365e5e243a90a034676b3262f9
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="configure-remote-radius-server-groups"></a>Configurare gruppi di Server RADIUS remoti

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per configurare i gruppi di server RADIUS remoti quando si desidera configurare come un server proxy e le richieste di connessione inoltrare ad altri server dei criteri di rete per l'elaborazione dei criteri di rete.

## <a name="add-a-remote-radius-server-group"></a>Aggiungere un gruppo di Server RADIUS remoti

È possibile utilizzare questa procedura per aggiungere un nuovo gruppo di server RADIUS remoto nello snap-in Server dei criteri di rete (NPS).

Quando si configura criteri di rete come proxy RADIUS, si crea un nuovo criterio di richiesta di connessione che utilizza criteri di rete per determinare le richieste di connessione inoltrare ad altri server RADIUS. Inoltre, il criterio di richiesta di connessione è configurato, specificando un gruppo server RADIUS remoto che contiene uno o più server RADIUS, che indica la posizione in cui inviare le richieste di connessione che corrispondono al criterio di richiesta di connessione di criteri di rete.

>[!NOTE]
>È inoltre possibile configurare un nuovo gruppo di server RADIUS remoto durante il processo di creazione di un nuovo criterio di richiesta di connessione.

Appartenenza al gruppo **Domain Admins**, o equivalente è il requisito minimo necessario per completare questa procedura.

### <a name="to-add-a-remote-radius-server-group"></a>Per aggiungere un gruppo di server RADIUS remoto 

1. In Server Manager, fare clic su **strumenti**, quindi fare clic su **Server dei criteri di rete** per aprire la console dei criteri di rete.
2. Nell'albero della console, fare doppio clic su **client e server RADIUS**, fare doppio clic su **gruppi di Server RADIUS remoti**, quindi fare clic su **New**.
3. Il **nuovo gruppo di Server RADIUS remoti** apre la finestra di dialogo. In **nome gruppo**, digitare un nome per il gruppo di server RADIUS remoto.
4. **In server RADIUS**, fare clic su **Aggiungi**. Il **Aggiungi server RADIUS** apre la finestra di dialogo. Digitare l'indirizzo IP del server RADIUS che si desidera aggiungere al gruppo o digitare \(FQDN\) il nome di dominio completo del server RADIUS e quindi fare clic su **verifica**.
5. In **Aggiungi server RADIUS**, fare clic su di **l'Accounting o autenticazione** scheda. In **segreto condiviso** e **Conferma segreto condiviso**, digitare il segreto condiviso. Quando si configura il computer locale come client RADIUS nel server RADIUS remoti, è necessario utilizzare lo stesso segreto condiviso.
6. Se non si utilizza protocollo EAP (Extensible Authentication) per l'autenticazione, fare clic su **richiesta deve contenere l'attributo autenticatore messaggio**. EAP utilizza l'attributo autenticatore messaggio per impostazione predefinita.
7. Verificare che i numeri di porta di autenticazione e accounting siano corretti per la distribuzione.
8. Se si utilizza un segreto condiviso diverso per l'accounting, in **Accounting**, deselezionare il **utilizzare lo stesso segreto condiviso per l'autenticazione e accounting** casella di controllo e quindi digitare il segreto condiviso di accounting in **segreto condiviso** e **Conferma segreto condiviso**.
9. Se non si desidera inoltrare l'avvio di server di accesso rete e arrestare i messaggi per il server RADIUS remoto, deselezionare il **inoltrare l'avvio di server di accesso rete e interrompere le notifiche a questo server** casella di controllo.

Per ulteriori informazioni sulla gestione dei criteri di rete, vedere [gestire Server dei criteri di rete](nps-manage-top.md).

Per ulteriori informazioni sui criteri di rete, vedere [Server dei criteri di rete (NPS)](nps-top.md).

