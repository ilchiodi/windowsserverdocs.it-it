---
title: Creare certificati radice per l'autenticazione VPN con Azure AD
description: Azure AD usa il certificato della rete VPN per firmare i certificati rilasciati ai client di Windows 10 durante l'autenticazione ad Azure AD per la connettività VPN. Il certificato contrassegnato come primario è l'emittente usato da Azure AD.
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.date: 06/28/2019
ms.author: v-tea
author: Teresa-MOTIV
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 5058095fa4bd6f7ba769fd274f46bc8b96878158
ms.sourcegitcommit: 430c6564c18f89eecb5bbc39cfee1a6f1d8ff85b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/26/2020
ms.locfileid: "83855675"
---
# <a name="step-72-create-conditional-access-root-certificates-for-vpn-authentication-with-azure-ad"></a>Passaggio 7.2. Creare certificati radice di accesso condizionale per l'autenticazione VPN con Azure AD

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Precedente:** Passaggio 7,1. Configurare EAP-TLS per ignorare il controllo dell'elenco di revoche di certificati (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md)
- [Passaggio **successivo:** Passaggio 7,3. Configurare i criteri di accesso condizionale](vpn-config-conditional-access-policy.md)

In questo passaggio vengono configurati i certificati radice di accesso condizionale per l'autenticazione VPN con Azure AD, che consente di creare automaticamente un'app Cloud chiamata server VPN nel tenant. Per configurare l'accesso condizionale per la connettività VPN è necessario eseguire queste operazioni:

1. Creare un certificato della rete VPN nel portale di Azure.
2. Scaricare il certificato della rete VPN.
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
3. Nella sezione **Gestisci** della pagina **Azure Active Directory** fare clic su **sicurezza**.
4. Nella sezione **Proteggi** della pagina **sicurezza** fare clic su **accesso condizionale**.
5. Nell' **accesso condizionale | Pagina Criteri** , nella sezione **Gestisci** , fare clic su **connettività VPN**.
5. Nella pagina della **connettività VPN** fare clic su **Nuovo certificato**.
6. Nella **nuova** pagina, seguire questa procedura: a. Per **Seleziona durata**selezionare 1, 2 o 3 anni.
   b. Selezionare **Create** (Crea).

## <a name="next-steps"></a>Passaggi successivi

[Passaggio 7,3. Configurare i criteri di accesso condizionale](vpn-config-conditional-access-policy.md): in questo passaggio si configurano i criteri di accesso condizionale per la connettività VPN.
