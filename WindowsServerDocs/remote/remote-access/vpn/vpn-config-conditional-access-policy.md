---
title: Configurare i criteri di accesso condizionale
description: Dopo la creazione di un certificato radice, la "connettività VPN" attiva la creazione dell'applicazione cloud "server VPN" nel tenant del cliente.
services: active-directory
ms.prod: windows-server
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
ms.openlocfilehash: 22983c085f2b9d9e7e16810e25c6fa50111f9fa6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404350"
---
# <a name="step-73-configure-the-conditional-access-policy"></a>Passaggio 7.3. Configurare i criteri di accesso condizionale

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Precedente** Passaggio 7.2. Creare certificati radice per l'autenticazione VPN con Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md)
- [**Prossimo** Passaggio 7.4. Distribuire i certificati radice di accesso condizionale in AD locale @ no__t-0

In questo passaggio si configureranno i criteri di accesso condizionale per la connettività VPN. Quando viene creato il primo certificato radice nel pannello "connettività VPN", viene creata automaticamente un'applicazione cloud "server VPN" nel tenant.

Creare un criterio di accesso condizionale assegnato al gruppo utenti VPN e definire l'ambito dell' **app Cloud** nel **server VPN**:

- **Utenti**: Utenti VPN
- **App Cloud**: Server VPN
- **Grant (controllo di accesso)** : "Richiedi autenticazione a più fattori". Se lo si desidera, è possibile utilizzare altri controlli.

**Procedura** Questo passaggio illustra la creazione dei criteri di accesso condizionale di base.  Se lo si desidera, è possibile utilizzare condizioni e controlli aggiuntivi.


1. Nella pagina **accesso condizionale** fare clic su **Aggiungi**nella barra degli strumenti in alto.

    ![Selezionare Aggiungi nella pagina accesso condizionale](../../media/Always-On-Vpn/07.png)

2. Nella casella **nome** della **nuova** pagina immettere un nome per il criterio. Ad esempio, immettere **criteri VPN**.

    ![Aggiungere il nome per i criteri nella pagina accesso condizionale](../../media/Always-On-Vpn/08.png)

3. Nella sezione **assegnazione** selezionare **utenti e gruppi**.

    ![Seleziona utenti e gruppi](../../media/Always-On-Vpn/09.png)

4. Nella pagina **utenti e gruppi** seguire questa procedura:

    ![Seleziona utente test](../../media/Always-On-Vpn/10.png)

    a. Selezionare **Seleziona utenti e gruppi**.

    b. Selezionare **Seleziona**.

    c. Nella pagina **Seleziona** selezionare il gruppo **utenti VPN** e quindi selezionare **Seleziona**.

    d. Nella pagina **utenti e gruppi** selezionare **fine**.

5. Nella **nuova** pagina, seguire questa procedura:

    ![Selezionare le app Cloud](../../media/Always-On-Vpn/11.png)

    a. Nella sezione **assegnazioni** selezionare **app Cloud**.

    b. Nella pagina **app Cloud** selezionare **Seleziona app**.

    d. Selezionare **server VPN**.

6.  Nella pagina **nuovo** , per aprire la pagina **Concedi** , nella sezione **controlli** selezionare **Concedi**.

    ![Selezionare Concedi](../../media/Always-On-Vpn/13.png)

7.  Nella pagina **Concedi** seguire questa procedura:

    ![Selezionare Richiedi autenticazione a più fattori](../../media/Always-On-Vpn/14.png)

    a. Selezionare **Richiedi autenticazione a più fattori**.

    b. Selezionare **Seleziona**.

8.  Nella pagina **nuovo** , in **Abilita criterio**, selezionare **on**.

    ![Abilita criteri](../../media/Always-On-Vpn/15.png)

9.  Nella pagina **nuovo** selezionare **Crea**.


## <a name="next-steps"></a>Passaggi successivi
[Passaggio 7.4. Distribuire i certificati radice di accesso condizionale in AD locale @ no__t-0: In questo passaggio si distribuisce il certificato radice di accesso condizionale come certificato radice attendibile per l'autenticazione VPN in Active Directory locale.
