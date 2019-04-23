---
title: Annullare la registrazione di un server di Server dei criteri di rete da un Dominio di Active Directory
description: È possibile utilizzare questo argomento per registrare un server che esegue Server dei criteri di rete in Windows Server 2016 nel dominio predefinito dei criteri di rete o in un altro dominio.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 68a94616-3c29-45bd-bd33-e4c578f119e1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8fe4773efd89aeb413b3793f874ad6a1b030294a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864352"
---
# <a name="unregister-an-nps-from-an-active-directory-domain"></a>Annullare la registrazione di un server di Server dei criteri di rete da un Dominio di Active Directory

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Nel processo di gestione della distribuzione dei criteri di rete, si potrebbe risultare utile per spostare un NPS in un altro dominio, per sostituire un criteri di rete, o per disattivare un NPS. 

Quando si sposta o rimuovere le autorizzazioni di un NPS, è possibile annullare la registrazione di NPS in domini di Active Directory in cui i criteri di rete disponga dell'autorizzazione per leggere le proprietà degli account utente in Active Directory.

L'appartenenza a **amministratori**, o equivalente è il requisito minimo necessario per eseguire queste procedure.

## <a name="to-unregister-an-nps"></a>Per annullare la registrazione di un criteri di rete

1. Nel controller di dominio, in Server Manager fare clic su **degli strumenti**, quindi fare clic su **Active Directory Users and Computers**. Verrà aperta la console Utenti e computer di Active Directory.

2. Fare clic su **gli utenti**, quindi fare doppio clic su **server RAS e IAS**.

3. Scegliere il **membri** scheda e quindi selezionare i criteri di rete che si desidera annullare la registrazione.

4. Fare clic su **rimuovere**, fare clic su **Yes**, quindi fare clic su **OK**.

