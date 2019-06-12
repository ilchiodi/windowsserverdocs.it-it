---
title: Creare certificati radice per l'autenticazione VPN con Azure AD
description: Azure AD Usa il certificato della rete VPN per firmare i certificati rilasciati ai client di Windows 10 durante l'autenticazione ad Azure AD per la connettività VPN. Il certificato contrassegnato come primario è l'autorità di certificazione usati da Azure AD.
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
ms.openlocfilehash: dc24f0275e8639ffd972ae24550d0ada38eff4f1
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749635"
---
# <a name="step-72-create-conditional-access-root-certificates-for-vpn-authentication-with-azure-ad"></a>Passaggio 7.2. Creare i certificati radice per l'autenticazione VPN di accesso condizionale con Azure AD

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Precedente:** Passaggio 7.1. Configurare EAP-TLS per ignorare il controllo dell'elenco di revoche di certificati (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md)
- [**prossimo:** Passaggio 7.3. Configurare i criteri di accesso condizionale](vpn-config-conditional-access-policy.md)

In questo passaggio si configura i certificati radice di accesso condizionale per l'autenticazione VPN con Azure AD, che crea automaticamente un'app Cloud denominata Server VPN nel tenant. Per configurare l'accesso condizionale per la connettività VPN, è necessario:

1. Creare un certificato VPN nel portale di Azure (è possibile creare più di un certificato).
2. Scaricare il certificato VPN.
3. Distribuire il certificato per i server dei criteri di rete e VPN.

Quando si tenta una connessione VPN, il client VPN effettua una chiamata nel Web Account Manager Web sul client Windows 10. WAM effettua una chiamata all'app cloud Server VPN. Quando vengono soddisfatte le condizioni e i controlli nei criteri di accesso condizionale, Azure AD rilascia un token sotto forma di un certificato (1 ora) di breve durata per il Web. Il WAM inserisce il certificato nell'archivio certificati dell'utente e passa il controllo al client VPN.  

Il client VPN invia quindi i problemi di certificato da Azure AD per la connessione VPN per la convalida delle credenziali.  Azure AD Usa il certificato che è contrassegnato come **primari** nel pannello della connettività VPN come autorità di certificazione. 

Nel portale di Azure, è creare due certificati per gestire la transizione quando un certificato sta per scadere. Quando si crea un certificato, si sceglie se è il certificato primario, che viene usato durante l'autenticazione per firmare il certificato per la connessione.

**Procedura:**

1. Accedi per i [portale di Azure](https://portal.azure.com) come amministratore globale.

2. Nel menu a sinistra, fare clic su **Azure Active Directory**. 

    ![Selezionare Azure Active Directory](../../media/Always-On-Vpn/01.png)

3. Nel **Azure Active Directory** nella pagina il **Gestisci** fare clic su **accesso condizionale**.

    ![Selezionare accesso condizionale](../../media/Always-On-Vpn/02.png)

4. Nel **accesso condizionale** nella pagina il **Gestisci** fare clic su **connettività VPN (anteprima)** .

    ![Selezionare connettività VPN](../../media/Always-On-Vpn/03.png)

5. Nel **connettività VPN** pagina, fare clic su **nuovo certificato**.

    ![Selezionare nuovo certificato](../../media/Always-On-Vpn/04.png)

6. Nel **New** seguire i passaggi seguenti:

    ![Seleziona durata e primario](../../media/Always-On-Vpn/05.png)

    a. Per la **Seleziona durata**, selezionare 1 o 2 anni. È possibile aggiungere fino a due certificati per gestire le transizioni quando il certificato sta per scadere. È possibile scegliere quale di essi è la replica primaria (quello utilizzato durante l'autenticazione per firmare il certificato per la connettività).

    b. Per la **primari**, selezionare **Yes**.

    c. Selezionare **Create**.

## <a name="next-steps"></a>Passaggi successivi

[Passaggio 7.3. Configurare i criteri di accesso condizionale](vpn-config-conditional-access-policy.md): In questo passaggio si configura il criterio di accesso condizionale per la connettività VPN. 
