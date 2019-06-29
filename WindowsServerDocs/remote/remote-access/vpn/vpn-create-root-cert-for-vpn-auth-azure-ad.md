---
title: Creare certificati radice per l'autenticazione VPN con Azure AD
description: Azure AD Usa il certificato della rete VPN per firmare i certificati rilasciati ai client di Windows 10 durante l'autenticazione ad Azure AD per la connettività VPN. Il certificato contrassegnato come primario è l'autorità di certificazione usati da Azure AD.
services: active-directory
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.workload: identity
ms.topic: article
ms.date: 06/28/2019
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 40403f6be65c84cc1e2506a222a009400fcdaacc
ms.sourcegitcommit: 34232723f15c7b4d6a32ca38b7348a417ba30ae0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/28/2019
ms.locfileid: "67464952"
---
# <a name="step-72-create-conditional-access-root-certificates-for-vpn-authentication-with-azure-ad"></a>Passaggio 7.2. Creare i certificati radice per l'autenticazione VPN di accesso condizionale con Azure AD

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Precedente:** Passaggio 7.1. Configurare EAP-TLS per ignorare il controllo dell'elenco di revoche di certificati (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md)
- [**prossimo:** Passaggio 7.3. Configurare i criteri di accesso condizionale](vpn-config-conditional-access-policy.md)

In questo passaggio si configura i certificati radice di accesso condizionale per l'autenticazione VPN con Azure AD, che crea automaticamente un'app Cloud denominata Server VPN nel tenant. Per configurare l'accesso condizionale per la connettività VPN, è necessario:

1. Creare un certificato VPN nel portale di Azure.
2. Scaricare il certificato VPN.
3. Distribuire il certificato per i server dei criteri di rete e VPN.

> [!IMPORTANT]
> Dopo aver creato un certificato della rete VPN nel portale di Azure, Azure AD inizierà a usare immediatamente per emettere certificati durati breve per il client VPN. È fondamentale che il certificato VPN distribuito immediatamente al server VPN per evitare eventuali problemi con la convalida delle credenziali del client VPN.

Quando si tenta una connessione VPN, il client VPN effettua una chiamata nel Web Account Manager Web sul client Windows 10. WAM effettua una chiamata all'app cloud Server VPN. Quando vengono soddisfatte le condizioni e i controlli nei criteri di accesso condizionale, Azure AD rilascia un token sotto forma di un certificato (1 ora) di breve durata per il Web. Il WAM inserisce il certificato nell'archivio certificati dell'utente e passa il controllo al client VPN.  

Il client VPN invia quindi i problemi di certificato da Azure AD per la connessione VPN per la convalida delle credenziali.  

> [!NOTE]
> Azure AD Usa il certificato creato più di recente nel pannello della connettività VPN come autorità di certificazione.

**Procedura:**

1. Accedi per i [portale di Azure](https://portal.azure.com) come amministratore globale.
2. Nel menu a sinistra, fare clic su **Azure Active Directory**.
3. Nel **Azure Active Directory** nella pagina il **Gestisci** fare clic su **accesso condizionale**.
4. Nel **accesso condizionale** nella pagina il **Gestisci** fare clic su **connettività VPN (anteprima)** .
5. Nel **connettività VPN** pagina, fare clic su **nuovo certificato**.
6. Nel **New** seguire i passaggi seguenti: una. Per la **Seleziona durata**, selezionare 1, 2 o 3 anni.
   b. Selezionare **Create**.

## <a name="next-steps"></a>Passaggi successivi

[Passaggio 7.3. Configurare i criteri di accesso condizionale](vpn-config-conditional-access-policy.md): In questo passaggio si configura il criterio di accesso condizionale per la connettività VPN.
