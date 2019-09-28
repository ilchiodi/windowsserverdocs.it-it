---
title: Annullare la registrazione di un server di Server dei criteri di rete da un Dominio di Active Directory
description: È possibile utilizzare questo argomento per registrare un server che esegue Server dei criteri di rete in Windows Server 2016 nel dominio predefinito server dei criteri di rete o in un altro dominio.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 68a94616-3c29-45bd-bd33-e4c578f119e1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3225c42ab14e2ea1bc283f520b14c09ebc2254c4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396085"
---
# <a name="unregister-an-nps-from-an-active-directory-domain"></a>Annullare la registrazione di un server di Server dei criteri di rete da un Dominio di Active Directory

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Nel processo di gestione della distribuzione del server dei criteri di dominio, può risultare utile spostare un server dei criteri di dominio in un altro dominio, sostituire un server dei criteri di server o ritirare un server dei criteri di dominio. 

Quando si sposta o si ritira un server dei criteri di servizio, è possibile annullare la registrazione del server dei criteri di dominio nei domini Active Directory in cui il server dei criteri di dominio dispone dell'autorizzazione per leggere le proprietà degli account utente nel Active Directory.

L'appartenenza a **amministratori**, o equivalente è il requisito minimo necessario per eseguire queste procedure.

## <a name="to-unregister-an-nps"></a>Per annullare la registrazione di un server dei criteri di server

1. Nel controller di dominio, in Server Manager, fare clic su **strumenti**, quindi fare clic su **Active Directory utenti e computer**. Verrà aperta la console Utenti e computer di Active Directory.

2. Fare clic su **utenti**, quindi fare doppio clic su **server RAS e IAS**.

3. Fare clic sulla scheda **membri** , quindi selezionare il server dei criteri di server di cui si desidera annullare la registrazione.

4. Fare clic su **Rimuovi**, fare clic su **Sì**, quindi su **OK**.

