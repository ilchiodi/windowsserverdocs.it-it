---
title: Creare certificati radice per l'autenticazione VPN con Azure AD
description: Azure AD utilizza il certificato VPN per firmare i certificati rilasciati ai client di Windows 10 durante l'autenticazione in Azure AD per la connessione VPN. Il certificato contrassegnato come principale è l'autorità di certificazione che usa Azure AD.
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
ms.openlocfilehash: 14ef17ab403cc4e7c9891f4ede48e41c25e8522d
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067293"
---
# Passaggio 7.2. Creare l'accesso condizionale i certificati radice per l'autenticazione VPN con Azure AD

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

& #171;  [ **Precedente:** passaggio 7.1. Configurare EAP-TLS per ignorare il controllo elenco di revoche di certificati (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md)<br>
& #187; [ **Successivo:** 7.3 passaggio. Configura il criterio di accesso condizionale](vpn-config-conditional-access-policy.md)

In questo passaggio, configurare i certificati radice di accesso condizionale per l'autenticazione VPN con Azure AD, che crea automaticamente un'app Cloud denominata Server VPN nel tenant. Per configurare l'accesso condizionale per la connettività VPN, è necessario:

1. Creare un certificato VPN nel portale di Azure (è possibile creare uno o più certificati).
2. Scarica il certificato VPN.
2. Distribuisci il certificato per i server dei criteri di rete e VPN.

Quando un utente tenta di una connessione VPN, il client VPN effettua una chiamata nel Web Account Manager (WAM) nel client Windows 10. WAM effettua una chiamata all'app cloud Server VPN. Quando sono soddisfatte le condizioni e controlli nei criteri di accesso condizionale, Azure AD emette un token in forma di un certificato di breve durata (1-ora) per il WAM. Il WAM posiziona il certificato nell'archivio certificati dell'utente e passa il controllo al client VPN.  

Il client VPN invia quindi i problemi di certificato da Azure AD per la connessione VPN per la convalida delle credenziali.Azure AD utilizza il certificato viene contrassegnato come**principale**nel pannello connettività VPN come autorità emittente. 

Nel portale di Azure, creare due certificati per gestire la transizione in cui un solo certificato sta per scadere. Quando si crea un certificato, puoi scegliere se è il certificato principale, che viene utilizzato per firmare il certificato per la connessione durante l'autenticazione.

**Procedura:**

1. Accedi al [portale di Azure](https://portal.azure.com) come amministratore globale.

2. Nel menu a sinistra, fai clic su **Azure Active Directory**. 

    ![Seleziona Azure Active Directory](../../media/Always-On-Vpn/01.png)

3. Nella pagina **Azure Active Directory** , nella sezione **Gestisci** , fai clic su **accesso condizionale**.

    ![Seleziona l'accesso condizionale](../../media/Always-On-Vpn/02.png)

4. Nella pagina di **accesso condizionale** , nella sezione **Gestisci** , fai clic su **connettività VPN (anteprima)**.

    ![Seleziona la connettività VPN](../../media/Always-On-Vpn/03.png)

5. Nella pagina **connettività VPN** , fai clic sul **nuovo certificato**.

    ![Seleziona nuovo certificato](../../media/Always-On-Vpn/04.png)

6. Nella pagina **New** , eseguire i passaggi seguenti:

    ![Seleziona la durata e primario](../../media/Always-On-Vpn/05.png)

    a. Per **Selezionare la durata**, seleziona 1 o 2 anni. Puoi aggiungere fino a due certificati per gestire le transizioni quando il certificato sta per scadere. Puoi scegliere quale è primario (quello usato durante l'autenticazione per firmare il certificato per la connettività).

    b. Per **primario**, seleziona **Sì**.

    c. Fai clic su **Crea**.

## Passaggio successivo
Passaggio [7.3. Configura il criterio di accesso condizionale](vpn-config-conditional-access-policy.md): In questo passaggio, configurare i criteri di accesso condizionale per la connessione VPN. 

---