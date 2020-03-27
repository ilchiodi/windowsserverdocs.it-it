---
title: Configurare i gruppi di server RADIUS remoti
description: In questo argomento vengono fornite informazioni su come configurare gruppi di server RADIUS remoti in server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: ca125e57-249c-4d97-85d1-2929cbf871f1
ms.author: lizross
author: eross-msft
ms.openlocfilehash: d9e1afd9505d3bbf1383d174cac6a2f543fcaae2
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316187"
---
# <a name="configure-remote-radius-server-groups"></a>Configurare i gruppi di server RADIUS remoti

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per configurare gruppi di server RADIUS remoti quando si desidera configurare il server dei criteri di gruppo in modo che funga da server proxy e inoltri richieste di connessione ad altri NPSs per l'elaborazione.

## <a name="add-a-remote-radius-server-group"></a>Aggiungere un gruppo di server RADIUS remoti

È possibile utilizzare questa procedura per aggiungere un nuovo gruppo di server RADIUS remoti nello snap-in server dei criteri di rete (NPS).

Quando si configura NPS come proxy RADIUS, si crea un nuovo criterio di richiesta di connessione che server dei criteri di connessione utilizza per determinare quali richieste di connessione inviare ad altri server RADIUS. Inoltre, il criterio di richiesta di connessione viene configurato specificando un gruppo di server RADIUS remoto che contiene uno o più server RADIUS, che indica a NPS dove inviare le richieste di connessione che corrispondono ai criteri di richiesta di connessione.

>[!NOTE]
>È inoltre possibile configurare un nuovo gruppo di server RADIUS remoto durante il processo di creazione di un nuovo criterio di richiesta di connessione.

L'appartenenza al gruppo **Domain Admins** oppure a un gruppo equivalente è il requisito minimo necessario per completare questa procedura.

### <a name="to-add-a-remote-radius-server-group"></a>Per aggiungere un gruppo di server RADIUS remoti 

1. In Server Manager fare clic su **strumenti**e quindi su **Server dei criteri di rete** per aprire la console server dei criteri di rete.
2. Nell'albero della console fare doppio clic su **client e server RADIUS**, fare clic con il pulsante destro del mouse su **gruppi di server RADIUS remoti**, quindi scegliere **nuovo**.
3. Verrà visualizzata la finestra di dialogo **nuovo gruppo di server RADIUS remoti** . In **nome gruppo**Digitare un nome per il gruppo di server RADIUS remoti.
4. **In server RADIUS**fare clic su **Aggiungi**. Verrà visualizzata la finestra di dialogo **Aggiungi server RADIUS** . Digitare l'indirizzo IP del server RADIUS che si desidera aggiungere al gruppo oppure digitare il nome di dominio completo \(\) FQDN del server RADIUS, quindi fare clic su **Verifica**.
5. In **Aggiungi server RADIUS**fare clic sulla scheda **autenticazione/accounting** . In **segreto condiviso** e **Conferma segreto**condiviso digitare il segreto condiviso. È necessario utilizzare lo stesso segreto condiviso quando si configura il computer locale come client RADIUS sul server RADIUS remoto.
6. Se non si utilizza il protocollo EAP (Extensible Authentication Protocol) per l'autenticazione, fare clic su **richiesta deve contenere l'attributo dell'autenticatore del messaggio**. Per impostazione predefinita, EAP utilizza l'attributo Message-Authenticator.
7. Verificare che i numeri di porta di autenticazione e accounting siano corretti per la distribuzione.
8. Se si usa un segreto condiviso diverso per l'accounting, in **Contabilità**deselezionare la casella di controllo **Usa la stessa chiave privata condivisa per l'autenticazione e l'accounting** , quindi digitare il segreto condiviso contabilità in **segreto** condiviso e **confermare il segreto**condiviso.
9. Se non si desidera che i messaggi di avvio e arresto del server di accesso alla rete vengano inviati al server RADIUS remoto, deselezionare la casella **di controllo Invia notifiche di avvio e arresto del server di accesso alla rete al** server.

Per ulteriori informazioni sulla gestione dei server dei criteri di rete, vedere [Manage Network Policy Server](nps-manage-top.md).

Per ulteriori informazioni su NPS, vedere [Server dei criteri di rete (NPS)](nps-top.md).

