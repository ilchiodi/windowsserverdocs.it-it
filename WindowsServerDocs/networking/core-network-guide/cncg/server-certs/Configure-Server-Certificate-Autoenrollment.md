---
title: Configurare la registrazione automatica del certificato server
description: Questo argomento fa parte della Guida alla distribuzione di un Server dei certificati per le distribuzioni Wireless e cablate 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: c81e85cb-ecb8-442a-ad27-442c2f9e40df
ms.prod: windows-server
ms.technology: networking
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 9c85c4fd9026155fe1ca880ecb05f4c9358a2309
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318419"
---
# <a name="configure-certificate-auto-enrollment"></a>Configurare la registrazione automatica del certificato

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

> [!NOTE]
> Prima di eseguire questa procedura, è necessario configurare un modello di certificato server utilizzando lo snap-in Console di gestione Microsoft di modelli di certificato in una CA che esegue Servizi certificati Active Directory.
Per completare questa procedura, è necessaria almeno l'appartenenza al gruppo **Enterprise Admins** e al gruppo **Domain Admins** del dominio radice.

## <a name="configure-server-certificate-auto-enrollment"></a>Configurare la registrazione automatica del certificato server

1. Nel computer in cui è installato Active Directory, aprire Windows PowerShell&reg;, tipo **mmc**, quindi premere INVIO. Verrà visualizzata la finestra di Microsoft Management Console.
2. Nel menu **File** fare clic su **Aggiungi/Rimuovi snap-in**. Verrà visualizzata la finestra di dialogo **Aggiungi o rimuovi snap-in**.
3. In **snap-in disponibili**, scorrere verso il basso e fare doppio clic su **Editor Gestione criteri di gruppo**. Il **Selezione oggetto Criteri di gruppo** verrà visualizzata la finestra di dialogo.

     > [!IMPORTANT]
     > Assicurarsi di selezionare **Editor Gestione criteri di gruppo** e non **Gestione criteri di gruppo**. Se si seleziona **criteri di gruppo Management**, la configurazione con queste istruzioni avrà esito negativo e un certificato server non verrà registrato automaticamente nella NPSs.

4. In **Oggetto Criteri di gruppo** fare clic su **Sfoglia**. Verrà visualizzata la finestra di dialogo **Ricerca oggetto Criteri di gruppo**.
5. In **Domini, unità organizzative e oggetti Criteri di gruppo** fare clic su **Criterio dominio predefinito** e quindi su **OK**.
6. Fare clic su **Fine** e quindi su **OK**.
7. Fare doppio clic su **Criterio dominio predefinito**. Nella console, espandere il percorso seguente: **Configurazione Computer**, **criteri**, **le impostazioni di Windows**, **le impostazioni di sicurezza**, e quindi **Criteri chiave pubblica**.
8. Fare clic su **Criteri chiave pubblica**. Nel riquadro dei dettagli fare doppio clic su **Client Servizi certificati - Registrazione automatica**. Il **proprietà** verrà visualizzata la finestra di dialogo. Configurare gli elementi seguenti e quindi fare clic su **OK**.

     1. In **Modello configurazione** selezionare **Abilitato**.
     2. Selezionare la casella di controllo **Rinnova i certificati scaduti, aggiorna quelli in sospeso e rimuovi i certificati revocati**.
     3. Selezionare la casella di controllo **Aggiorna i certificati che utilizzano modelli di certificato**.

9. Fare clic su **OK**.

## <a name="configure-user-certificate-auto-enrollment"></a>Configurare la registrazione automatica del certificato utente

1. Nel computer in cui è installato Active Directory, aprire Windows PowerShell&reg;, tipo **mmc**, quindi premere INVIO. Verrà visualizzata la finestra di Microsoft Management Console.
2. Nel menu **File** fare clic su **Aggiungi/Rimuovi snap-in**. Verrà visualizzata la finestra di dialogo **Aggiungi o rimuovi snap-in**.
3. In **snap-in disponibili**, scorrere verso il basso e fare doppio clic su **Editor Gestione criteri di gruppo**. Il **Selezione oggetto Criteri di gruppo** verrà visualizzata la finestra di dialogo.

     > [!IMPORTANT]
     > Assicurarsi di selezionare **Editor Gestione criteri di gruppo** e non **Gestione criteri di gruppo**. Se si seleziona **criteri di gruppo Management**, la configurazione con queste istruzioni avrà esito negativo e un certificato server non verrà registrato automaticamente nella NPSs.

4. In **Oggetto Criteri di gruppo** fare clic su **Sfoglia**. Verrà visualizzata la finestra di dialogo **Ricerca oggetto Criteri di gruppo**.
5. In **Domini, unità organizzative e oggetti Criteri di gruppo** fare clic su **Criterio dominio predefinito** e quindi su **OK**.
6. Fare clic su **Fine** e quindi su **OK**.
7. Fare doppio clic su **Criterio dominio predefinito**. Nella console di espandere il percorso seguente: **Configurazione utente**, **criteri**, **impostazioni di Windows**, **impostazioni di sicurezza**.
8. Fare clic su **Criteri chiave pubblica**. Nel riquadro dei dettagli fare doppio clic su **Client Servizi certificati - Registrazione automatica**. Il **proprietà** verrà visualizzata la finestra di dialogo. Configurare gli elementi seguenti e quindi fare clic su **OK**.

     1. In **Modello configurazione** selezionare **Abilitato**.
     2. Selezionare la casella di controllo **Rinnova i certificati scaduti, aggiorna quelli in sospeso e rimuovi i certificati revocati**.
     3. Selezionare la casella di controllo **Aggiorna i certificati che utilizzano modelli di certificato**.

9. Fare clic su **OK**.

## <a name="next-steps"></a>Passaggi successivi

[Aggiorna Criteri di gruppo](refresh-group-policy.md)
