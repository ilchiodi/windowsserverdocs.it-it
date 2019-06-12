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
ms.openlocfilehash: 466e76d01ca99a1e1ed72fa955ccd287ae63c5df
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749495"
---
# <a name="step-73-configure-the-conditional-access-policy"></a>Passaggio 7.3. Configurare i criteri di accesso condizionale

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Precedente:** Passaggio 7.2. Creare certificati radice per l'autenticazione VPN con Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md)
- [**prossimo:** Passaggio 7.4. Distribuire i certificati radice di accesso condizionale ai servizi locali Active Directory](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)

In questo passaggio si configura il criterio di accesso condizionale per la connettività VPN. Quando viene creato il primo certificato radice nel pannello 'Connettività VPN', viene creato automaticamente un'applicazione cloud 'Server VPN' nel tenant.

Creare un criterio di accesso condizionale assegnato al gruppo di utenti VPN e l'ambito di **app Cloud** al **Server VPN**:

- **Utenti**: Utenti VPN
- **App cloud**: Server VPN
- **Grant (controllo degli accessi)** : "Require multi-factor authentication". Se lo si desidera, è possono utilizzare altri controlli.

**Procedura:** Questo passaggio riguarda la creazione dei criteri di accesso condizionale di base.  Se si desidera, è possono utilizzare controlli e le condizioni aggiuntive.


1. Nel **accesso condizionale** pagina, nella barra degli strumenti in alto, selezionare **Add**.

    ![Selezionare Aggiungi nella pagina di accesso condizionale](../../media/Always-On-Vpn/07.png)

2. Nel **New** nella pagina il **nome** immettere un nome per il criterio. Ad esempio, immettere **criteri VPN**.

    ![Aggiungi nome criterio nella pagina di accesso condizionale](../../media/Always-On-Vpn/08.png)

3. Nel **assegnazione** sezione, selezionare **utenti e gruppi**.

    ![Selezionare utenti e gruppi](../../media/Always-On-Vpn/09.png)

4. Nel **utenti e gruppi** seguire i passaggi seguenti:

    ![Utente di test selezionare](../../media/Always-On-Vpn/10.png)

    a. Selezionare **Seleziona utenti e gruppi**.

    b. Selezionare **seleziona**.

    c. Nel **seleziona** pagina, selezionare il **gli utenti VPN** gruppo e quindi selezionare **selezionare**.

    d. Nel **utenti e gruppi** pagina, selezionare **eseguita**.

5. Nel **New** seguire i passaggi seguenti:

    ![Selezionare le app cloud](../../media/Always-On-Vpn/11.png)

    a. Nel **assegnazioni** sezione, selezionare **le app Cloud**.

    b. Nel **le app Cloud** pagina, selezionare **selezionare le app**.

    d. Selezionare **Server VPN**.

6.  Nel **New** pagina, aprire il **Grant** nella pagina il **controlli** selezionare **Grant**.

    ![Autorizzazione SELECT](../../media/Always-On-Vpn/13.png)

7.  Nel **Concedi** seguire i passaggi seguenti:

    ![Selezionare Richiedi autenticazione a più fattori](../../media/Always-On-Vpn/14.png)

    a. Selezionare **richiedono l'autenticazione a più fattori**.

    b. Selezionare **seleziona**.

8.  Nel **New** nella pagina **Abilita criteri**, selezionare **su**.

    ![Abilitare i criteri](../../media/Always-On-Vpn/15.png)

9.  Nel **New** pagina, selezionare **crea**.


## <a name="next-steps"></a>Passaggi successivi
[Passaggio 7.4. Distribuire i certificati radice di accesso condizionale ai servizi locali AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md): In questo passaggio si distribuisce il certificato radice di accesso condizionale come certificato radice attendibile per l'autenticazione VPN per on-premises AD.
