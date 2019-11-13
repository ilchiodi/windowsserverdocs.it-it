---
title: Creare certificati radice per l'autenticazione VPN con Azure AD
description: Azure AD usa il certificato VPN per firmare i certificati rilasciati ai client Windows 10 durante l'autenticazione per Azure AD per la connettività VPN. Il certificato contrassegnato come primario è l'emittente usato da Azure AD.
services: active-directory
ms.prod: windows-server
ms.technology: networking-ras
ms.workload: identity
ms.topic: article
ms.date: 06/28/2019
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 41e98648ab963347f8370233c320f5e38b5d4d96
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388011"
---
# <a name="step-72-create-conditional-access-root-certificates-for-vpn-authentication-with-azure-ad"></a>Passaggio 7.2. Creare certificati radice di accesso condizionale per l'autenticazione VPN con Azure AD

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Precedente:** Passaggio 7,1. Configurare EAP-TLS per ignorare il controllo dell'elenco di revoche di certificati (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md)
- [Passaggio **successivo:** Passaggio 7,3. Configurare i criteri di accesso condizionale](vpn-config-conditional-access-policy.md)

In questo passaggio vengono configurati i certificati radice di accesso condizionale per l'autenticazione VPN con Azure AD, che consente di creare automaticamente un'app Cloud chiamata server VPN nel tenant. Per configurare l'accesso condizionale per la connettività VPN, è necessario:

1. Creare un certificato VPN nel portale di Azure.
2. Scaricare il certificato VPN.
3. Distribuire il certificato nei server VPN e server dei criteri di rete.

> [!IMPORTANT]
> Una volta creato un certificato VPN nel portale di Azure, Azure AD inizierà a usarlo immediatamente per emettere certificati di breve durata per il client VPN. È fondamentale che il certificato VPN venga distribuito immediatamente al server VPN per evitare problemi con la convalida delle credenziali del client VPN.

Quando un utente tenta una connessione VPN, il client VPN effettua una chiamata a gestione account Web (WAM) nel client Windows 10. WAM effettua una chiamata all'app Cloud Server VPN. Quando vengono soddisfatte le condizioni e i controlli dei criteri di accesso condizionale, Azure AD rilascia un token sotto forma di un certificato di breve durata (un'ora) al WAM. WAM inserisce il certificato nell'archivio certificati dell'utente e passa il controllo al client VPN.  

Il client VPN invia quindi i problemi del certificato Azure AD alla VPN per la convalida delle credenziali.  

> [!NOTE]
> Azure AD usa il certificato creato più di recente nel pannello connettività VPN come emittente.

**Procedura**

1. Accedere al [portale di Azure](https://portal.azure.com) come amministratore globale.
2. Nel menu a sinistra fare clic su **Azure Active Directory**.
3. Nella sezione **Gestisci** della pagina **Azure Active Directory** fare clic su **accesso condizionale**.
4. Nella sezione **Gestisci** della pagina **accesso condizionale** fare clic su **connettività VPN (anteprima)** .
5. Nella pagina **connettività VPN** fare clic su **nuovo certificato**.
6. Nella **nuova** pagina, seguire questa procedura: a. Per **Seleziona durata**selezionare 1, 2 o 3 anni.
   b. Seleziona **Crea**.

## <a name="next-steps"></a>Passaggi successivi

[Passaggio 7,3. Configurare i criteri di accesso condizionale](vpn-config-conditional-access-policy.md): in questo passaggio si configurano i criteri di accesso condizionale per la connettività VPN.
