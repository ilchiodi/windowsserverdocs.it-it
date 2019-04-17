---
title: Configurare server registrazione automatica dei certificati
description: Questo argomento fa parte della Guida di certificati del Server di distribuzione per le distribuzioni Wireless e cablate 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: c81e85cb-ecb8-442a-ad27-442c2f9e40df
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ddf8a905fdb68bbc474b10f526b32f3d8b83af46
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="configure-certificate-auto-enrollment"></a>Configurare la registrazione automatica dei certificati

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

> [!NOTE]
> Prima di eseguire questa procedura, è necessario configurare un modello di certificato server utilizzando lo snap-in Console di gestione Microsoft di modelli di certificato in una CA che esegue Servizi certificati Active Directory.
L'appartenenza a entrambi **Enterprise Admins** e del dominio radice **Domain Admins** gruppo è il requisito minimo necessario per completare questa procedura.

## <a name="configure-server-certificate-auto-enrollment"></a>Configurare server registrazione automatica dei certificati

1. Nel computer in cui è installato Active Directory, aprire Windows PowerShell&reg;, tipo **mmc**, quindi premere INVIO. Verrà visualizzata la finestra di Microsoft Management Console.
2. Nel **File** menu, fare clic su **Aggiungi/Rimuovi Snap-in**. Il **Aggiungi o Rimuovi Snap-in** apre la finestra di dialogo.
3. In **snap-in disponibili**, scorrere verso il basso e fare doppio clic su **Editor Gestione criteri di gruppo**. Il **Selezione oggetto Criteri di gruppo** apre la finestra di dialogo.

     > [!IMPORTANT]
     > Assicurarsi di selezionare **Editor Gestione criteri di gruppo** e non **Gestione criteri di gruppo**. Se si seleziona **Gestione criteri di gruppo**, la configurazione utilizzando queste istruzioni avrà esito negativo e un certificato server non sarà eseguita la registrazione automatica dei server dei criteri di rete.

4. In **oggetto Criteri di gruppo**, fare clic su **Sfoglia**. Il **cercare un oggetto Criteri di gruppo** apre la finestra di dialogo.
5. In **domini, unità organizzative e oggetti Criteri di gruppo collegati,** fare clic su **criterio dominio predefinito**, quindi fare clic su **OK**.
6. Fare clic su **fine**, quindi fare clic su **OK**.
7. Fare doppio clic su **criterio dominio predefinito**. Nella console, espandere il percorso seguente: **configurazione Computer**, **criteri**, **le impostazioni di Windows**, **le impostazioni di sicurezza**e quindi **criteri chiave pubblica**.
8. Fare clic su **criteri chiave pubblica**. Nel riquadro dei dettagli fare doppio clic su **Client di servizi certificati - registrazione automatica**. Il **proprietà** apre la finestra di dialogo. Configurare gli elementi seguenti e quindi fare clic su **OK**:

     1. In **modello configurazione**selezionare **abilitato**.
     2. Selezionare il **Rinnova i certificati scaduti, aggiorna quelli in sospeso, e Rimuovi i certificati revocati** casella di controllo.
     3. Selezionare il **Aggiorna i certificati che utilizzano modelli di certificato** casella di controllo.

9. Fare clic su **OK**.

## <a name="configure-user-certificate-auto-enrollment"></a>Configurare l'utente registrazione automatica dei certificati

1. Nel computer in cui è installato Active Directory, aprire Windows PowerShell&reg;, tipo **mmc**, quindi premere INVIO. Verrà visualizzata la finestra di Microsoft Management Console.
2. Nel **File** menu, fare clic su **Aggiungi/Rimuovi Snap-in**. Il **Aggiungi o Rimuovi Snap-in** apre la finestra di dialogo.
3. In **snap-in disponibili**, scorrere verso il basso e fare doppio clic su **Editor Gestione criteri di gruppo**. Il **Selezione oggetto Criteri di gruppo** apre la finestra di dialogo.

     > [!IMPORTANT]
     > Assicurarsi di selezionare **Editor Gestione criteri di gruppo** e non **Gestione criteri di gruppo**. Se si seleziona **Gestione criteri di gruppo**, la configurazione utilizzando queste istruzioni avrà esito negativo e un certificato server non sarà eseguita la registrazione automatica dei server dei criteri di rete.

4. In **oggetto Criteri di gruppo**, fare clic su **Sfoglia**. Il **cercare un oggetto Criteri di gruppo** apre la finestra di dialogo.
5. In **domini, unità organizzative e oggetti Criteri di gruppo collegati,** fare clic su **criterio dominio predefinito**, quindi fare clic su **OK**.
6. Fare clic su **fine**, quindi fare clic su **OK**.
7. Fare doppio clic su **criterio dominio predefinito**. Nella console, espandere il percorso seguente: **configurazione utente**, **criteri**, **le impostazioni di Windows**, **le impostazioni di sicurezza**e quindi **criteri chiave pubblica**.
8. Fare clic su **criteri chiave pubblica**. Nel riquadro dei dettagli fare doppio clic su **Client di servizi certificati - registrazione automatica**. Il **proprietà** apre la finestra di dialogo. Configurare gli elementi seguenti e quindi fare clic su **OK**:

     1. In **modello configurazione**selezionare **abilitato**.
     2. Selezionare il **Rinnova i certificati scaduti, aggiorna quelli in sospeso, e Rimuovi i certificati revocati** casella di controllo.
     3. Selezionare il **Aggiorna i certificati che utilizzano modelli di certificato** casella di controllo.

9. Fare clic su **OK**.

## <a name="next-steps"></a>Passaggi successivi

[Aggiornare criteri di gruppo](refresh-group-policy.md)