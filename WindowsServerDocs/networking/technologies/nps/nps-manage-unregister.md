---
title: Annullare la registrazione di un Server dei criteri di rete da un dominio Active Directory
description: È possibile utilizzare questo argomento per registrare un server che esegue Server dei criteri di rete in Windows Server 2016 nel dominio predefinito server dei criteri di rete o in un altro dominio.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 68a94616-3c29-45bd-bd33-e4c578f119e1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 55c3b00146706831351ce63d1e5b74f45d7b9be1
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="unregister-an-nps-server-from-an-active-directory-domain"></a>Annullare la registrazione di un Server dei criteri di rete da un dominio Active Directory

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Nel processo di gestione della distribuzione di server dei criteri di rete, potrebbe essere utile per spostare un server dei criteri di rete in un altro dominio, per sostituire un server dei criteri di rete o per rimuovere un server dei criteri di rete. 

Quando si sposta o rimuovere le autorizzazioni di un server dei criteri di rete, è possibile annullare la registrazione di server dei criteri di rete in domini di Active Directory in cui il server dei criteri di rete dispone dell'autorizzazione per leggere le proprietà dell'account utente in Active Directory.

Appartenenza al gruppo **amministratori**, o equivalente è il requisito minimo necessario per eseguire queste procedure.

## <a name="to-unregister-an-nps-server"></a>Per annullare la registrazione di un server dei criteri di rete

1. Nel controller di dominio, in Server Manager, fare clic su **strumenti**, quindi fare clic su **Active Directory Users and Computers**. Apre la console Active Directory Users and Computers.

2. Fare clic su **utenti**e quindi fare doppio clic su **server RAS e IAS**.

3. Fare clic su di **membri** scheda e quindi selezionare il server dei criteri di rete che si desidera annullare la registrazione.

4. Fare clic su **rimuovere**, fare clic su **Sì**, quindi fare clic su **OK**.

