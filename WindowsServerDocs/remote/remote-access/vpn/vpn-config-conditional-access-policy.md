---
title: Configurare i criteri di accesso condizionale
description: Dopo aver creato un certificato radice, connettività' VPN' attiva la creazione dell'applicazione cloud 'Server VPN' nel tenant del cliente.
services: active-directory
ms.prod: windows-server-threshold
ms.technology: networking-ras
documentationcenter: ''
ms.assetid: ''
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 8c00855c50de79efa1b48c7b8762e1b679db4a87
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870282"
---
# <a name="step-73-configure-the-conditional-access-policy"></a>Passaggio 7.3. Configurare i criteri di accesso condizionale

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

&#171;  [**Precedente:** Passaggio 7.2. Creare i certificati radice per l'autenticazione VPN con Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md)<br>
&#187; [ **Next:** Passaggio 7.4. Distribuire i certificati radice di accesso condizionale ai servizi locali Active Directory](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)

In questo passaggio si configura il criterio di accesso condizionale per la connettività VPN. Quando viene creato il primo certificato radice nel pannello 'Connettività VPN', viene creato automaticamente un'applicazione cloud 'Server VPN' nel tenant. 

Creare un criterio di accesso condizionale assegnato al gruppo di utenti VPN e l'ambito di **app Cloud** al **Server VPN**: 

- **Utenti**: Utenti VPN
- **App cloud**: Server VPN
- **Grant (controllo degli accessi)**: "Require multi-factor authentication". Se lo si desidera, è possono utilizzare altri controlli.

**Procedura:** Questo passaggio riguarda la creazione dei criteri di accesso condizionale di base.  Se si desidera, è possono utilizzare controlli e le condizioni aggiuntive.


1. Nel **accesso condizionale** sulla barra degli strumenti in alto fare clic su **Add**.

    ![Selezionare Aggiungi nella pagina di accesso condizionale](../../media/Always-On-Vpn/07.png)

2. Nel **New** nella pagina il **nome** , digitare un nome per il criterio. Ad esempio, digitare **criteri VPN**.

    ![Aggiungi nome criterio nella pagina di accesso condizionale](../../media/Always-On-Vpn/08.png)

3. Nel **assegnazione** fare clic su **utenti e gruppi**.

    ![Selezionare utenti e gruppi](../../media/Always-On-Vpn/09.png)

4. Nel **utenti e gruppi** seguire i passaggi seguenti:

    ![Utente di test selezionare](../../media/Always-On-Vpn/10.png)

    a. Fare clic su **Seleziona utenti e gruppi**.

    b. Fare clic su **Seleziona**.

    c. Nel **seleziona** pagina, selezionare il **gli utenti VPN** gruppo e quindi fare clic su **selezionare**.

    d. Nel **utenti e gruppi** pagina, fare clic su **eseguita**.

5. Nel **New** seguire i passaggi seguenti:

    ![Selezionare le app cloud](../../media/Always-On-Vpn/11.png)

    a. Nel **assegnazioni** fare clic su **le app Cloud**.

    b. Nel **le app Cloud** pagina, fare clic su **selezionare le app**.

    d. Selezionare **Server VPN**.

13. Nel **New** pagina, aprire il **Grant** nella pagina il **controlli** fare clic su **Grant**.

    ![Autorizzazione SELECT](../../media/Always-On-Vpn/13.png)

14. Nel **Concedi** seguire i passaggi seguenti:

    ![Selezionare Richiedi autenticazione a più fattori](../../media/Always-On-Vpn/14.png)

    a. Selezionare **richiedono l'autenticazione a più fattori**.

    b. Fare clic su **Seleziona**.

15. Nel **New** nella pagina **Abilita criterio**, fare clic su **in**.

    ![Abilitare i criteri](../../media/Always-On-Vpn/15.png)

16. Nel **New** pagina, fare clic su **crea**.


## <a name="next-step"></a>Passaggio successivo
[Passaggio 7.4. Distribuire i certificati radice di accesso condizionale ai servizi locali AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md): In questo passaggio si distribuisce il certificato radice di accesso condizionale come certificato radice attendibile per l'autenticazione VPN per on-premises AD.

---