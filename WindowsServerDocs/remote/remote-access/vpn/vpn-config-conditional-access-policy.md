---
title: Configurare i criteri di accesso condizionale
description: Dopo aver creato un certificato radice, la connettività VPN' ' attiva la creazione dell'applicazione cloud 'Server VPN' nel tenant del cliente.
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
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067294"
---
# Passaggio 7.3. Configura il criterio di accesso condizionale

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

& #171;  [ **Precedente:** passaggio 7.2. Creare certificati radice per l'autenticazione VPN con Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md)<br>
& #187; [ **Successivo:** passaggio 7.4. Distribuire certificati radice di accesso condizionale locale AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)

In questo passaggio, configurare i criteri di accesso condizionale per la connessione VPN. Quando il certificato radice primo viene creato nel blade 'Connettività VPN', viene creato automaticamente un'applicazione cloud 'Server VPN' nel tenant. 

Creare un criterio di accesso condizionale assegnato al gruppo di utenti VPN e l'ambito all' **app Cloud** di **Server VPN**: 

- **Gli utenti**: gli utenti della VPN
- **App cloud**: Server VPN
- **Concedere (controllo di accesso)**: 'Richiedono l'autenticazione a più fattori'. Altri controlli possono essere usati se lo si desidera.

**Procedura:** Questo passaggio illustra la creazione del criterio di accesso condizionale più semplice.Se desiderato, controlli e condizioni aggiuntive possono essere usati.


1. Nella pagina di **Accesso condizionale** , nella barra degli strumenti nella parte superiore, fai clic su **Aggiungi**.

    ![Selezionare Aggiungi nella pagina di accesso condizionale](../../media/Always-On-Vpn/07.png)

2. Nella pagina **New** , nella casella **nome** , digita un nome per il criterio. Ad esempio, digita i **criteri VPN**.

    ![Aggiungere nome criterio nella pagina di accesso condizionale](../../media/Always-On-Vpn/08.png)

3. Nella sezione **assegnazione** , fai clic su **utenti e gruppi**.

    ![Seleziona gli utenti e gruppi](../../media/Always-On-Vpn/09.png)

4. Nella pagina **utenti e gruppi** , eseguire i passaggi seguenti:

    ![Utente seleziona test](../../media/Always-On-Vpn/10.png)

    a. Fai clic su **Seleziona utenti e gruppi**.

    b. Fai clic su **Seleziona**.

    c. Nella pagina **Seleziona** , seleziona il gruppo di **utenti VPN** e quindi fai clic su **Seleziona**.

    d. Nella pagina **utenti e gruppi** , fai clic sul **fatto**.

5. Nella pagina **New** , eseguire i passaggi seguenti:

    ![Seleziona le app cloud](../../media/Always-On-Vpn/11.png)

    a. Nella sezione **assegnazioni** , fai clic su **App Cloud**.

    b. Nella pagina **delle App Cloud** , fai clic su **Seleziona le app**.

    d. Selezionare **il Server VPN**.

13. Nella pagina di **Nuovo** , per aprire la pagina **Grant** , nella sezione **controlli** , fai clic su **Grant**.

    ![Seleziona grant](../../media/Always-On-Vpn/13.png)

14. Nella pagina **Grant** , eseguire i passaggi seguenti:

    ![Selezionare Richiedi autenticazione a più fattori](../../media/Always-On-Vpn/14.png)

    a. Seleziona **richiedono l'autenticazione a più fattori**.

    b. Fai clic su **Seleziona**.

15. Nella pagina **New** , sotto **abilitare i criteri**, fai clic **su**.

    ![Abilita criteri](../../media/Always-On-Vpn/15.png)

16. Nella pagina di **Nuovo** , fai clic su **Crea**.


## Passaggio successivo
Passaggio [7.4. Distribuire certificati radice di accesso condizionale locale AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md): In questo passaggio, distribuire il certificato radice di accesso condizionale come certificato radice attendibile per l'autenticazione VPN per locale AD.

---