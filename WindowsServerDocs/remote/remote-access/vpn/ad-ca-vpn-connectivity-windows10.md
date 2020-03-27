---
title: Accesso condizionale per la connettività VPN con Azure AD
description: In questo passaggio facoltativo è possibile ottimizzare il modo in cui gli utenti VPN autorizzati accedono alle risorse usando l'accesso condizionale Azure Active Directory (Azure AD).
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.localizationpriority: medium
ms.author: lizross
author: eross-msft
ms.date: 06/28/2019
ms.reviewer: deverette
ms.openlocfilehash: 1a26f19cee5c6b6faf551633fd1739b1103c6ddf
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80313388"
---
# <a name="step-7-optional-conditional-access-for-vpn-connectivity-using-azure-ad"></a>Passaggio 7. Opzionale Accesso condizionale per la connettività VPN con Azure AD

- [**Precedente:** Passaggio 6. Configurare le connessioni VPN Always On client Windows 10](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)
- [Passaggio **successivo:** Passaggio 7,1. Configurare EAP-TLS per ignorare il controllo dell'elenco di revoche di certificati (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md)

In questo passaggio facoltativo è possibile ottimizzare il modo in cui gli utenti VPN accedono alle risorse usando [l'accesso condizionale Azure Active Directory (Azure ad)](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). Con Azure AD l'accesso condizionale per la connettività di rete privata virtuale (VPN), è possibile proteggere le connessioni VPN. L'accesso condizionale è un motore di valutazione basato su criteri che ti consente di creare regole di accesso per qualsiasi applicazione connessa ad Azure Active Directory (Azure AD).

## <a name="prerequisites"></a>Prerequisiti

Si ha familiarità con gli argomenti seguenti:

- [Accesso condizionale in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)
- [VPN e accesso condizionale](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)

Per configurare Azure Active Directory l'accesso condizionale per la connettività VPN, è necessario che siano configurati gli elementi seguenti:

- [Infrastruttura server](always-on-vpn/deploy/vpn-deploy-server-infrastructure.md)
- [Server di accesso remoto per VPN Always On](always-on-vpn/deploy/vpn-deploy-ras.md)
- [Server dei criteri di rete](always-on-vpn/deploy/vpn-deploy-nps.md)
- [Impostazioni del firewall e DNS](always-on-vpn/deploy/vpn-deploy-dns-firewall.md)
- [Connessioni VPN client Always On Windows 10](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)

## <a name="step-71-configure-eap-tls-to-ignore-certificate-revocation-list-crl-checking"></a>[Passaggio 7,1. Configurare EAP-TLS per ignorare il controllo dell'elenco di revoche di certificati (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md)

In questo passaggio è possibile aggiungere **IgnoreNoRevocationCheck** e impostarlo in modo da consentire l'autenticazione dei client quando il certificato non include i punti di distribuzione CRL. Per impostazione predefinita, IgnoreNoRevocationCheck è impostato su 0 (disabilitato).

Un client EAP-TLS non è in grado di connettersi a meno che il server dei criteri di rete non completi un controllo di revoca della catena di certificati, incluso il certificato radice. I certificati cloud rilasciati all'utente da Azure AD non dispongono di un CRL perché sono certificati di breve durata con una durata di un'ora. EAP in NPS deve essere configurato per ignorare l'assenza di un CRL. Poiché il metodo di autenticazione è EAP-TLS, questo valore del registro di sistema è necessario solo in **EAP\13**. Se vengono usati altri metodi di autenticazione EAP, è necessario aggiungere anche il valore del registro di sistema.

## <a name="step-72-create-root-certificates-for-vpn-authentication-with-azure-ad"></a>[Passaggio 7,2. Creare certificati radice per l'autenticazione VPN con Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md)

In questo passaggio vengono configurati i certificati radice per l'autenticazione VPN con Azure AD, che consente di creare automaticamente un'app Cloud Server VPN nel tenant.  

Per configurare l'accesso condizionale per la connettività VPN, è necessario:

1. Creare un certificato VPN nel portale di Azure.
2. Scaricare il certificato VPN.
3. Distribuire il certificato nel server VPN.

> [!IMPORTANT]
> Una volta creato un certificato VPN nel portale di Azure, Azure AD inizierà a usarlo immediatamente per emettere certificati di breve durata per il client VPN. È fondamentale che il certificato VPN venga distribuito immediatamente al server VPN per evitare problemi con la convalida delle credenziali del client VPN.

## <a name="step-73-configure-the-conditional-access-policy"></a>[Passaggio 7,3. Configurare i criteri di accesso condizionale](vpn-config-conditional-access-policy.md)

In questo passaggio si configureranno i criteri di accesso condizionale per la connettività VPN.

Per configurare i criteri di accesso condizionale, è necessario:

1. Creare un criterio di accesso condizionale assegnato agli utenti VPN.
2. Impostare l'app Cloud sul **server VPN**.
3. Impostare la concessione (controllo di accesso) per **richiedere l'autenticazione**a più fattori.  Se necessario, è possibile usare altri controlli.

## <a name="step-74-deploy-conditional-access-root-certificates-to-on-premises-ad"></a>[Passaggio 7,4. Distribuire i certificati radice di accesso condizionale ad Active Directory locale](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)

In questo passaggio viene distribuito un certificato radice attendibile per l'autenticazione VPN in Active Directory locale.

Per distribuire il certificato radice attendibile, è necessario:

1. Aggiungere il certificato scaricato come *CA radice attendibile per l'autenticazione VPN*.
2. Importare il certificato radice nel server VPN e nel client VPN.
3. Verificare che i certificati siano presenti e visualizzati come attendibili.

## <a name="step-75-create-oma-dm-based-vpnv2-profiles-to-windows-10-devices"></a>[Passaggio 7,5. Creare profili VPNv2 basati su OMA-DM per i dispositivi Windows 10](vpn-create-oma-dm-based-vpnv2-profiles.md)

In questo passaggio è possibile creare profili VPNv2 basati su OMA usando Intune per distribuire i criteri di configurazione del dispositivo VPN. Se si vuole usare Configuration Manager o uno script di PowerShell per creare profili VPNv2, vedere [le impostazioni di VPNV2 CSP](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp) per altri dettagli.

## <a name="next-steps"></a>Passaggi successivi

[Passaggio 7,1. Configurare EAP-TLS per ignorare il controllo dell'elenco di revoche di certificati (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md): in questo passaggio è necessario aggiungere **IgnoreNoRevocationCheck** e impostarlo per consentire l'autenticazione dei client quando il certificato non include i punti di distribuzione CRL. Per impostazione predefinita, IgnoreNoRevocationCheck è impostato su 0 (disabilitato).

## <a name="related-topics"></a>Argomenti correlati

- [Configurare i profili VPNv2](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): il client VPN è ora in grado di integrarsi con la piattaforma di accesso condizionale basato sul cloud per fornire un'opzione di conformità del dispositivo per i client remoti. In questo passaggio i profili VPNv2 vengono configurati con **\<DeviceCompliance > \<abilitato > true\</Enabled >** .

- [Miglioramento dell'accesso remoto in Windows 10 con un profilo VPN automatico](https://www.microsoft.com/itshowcase/Article/Content/894/Enhancing-remote-access-in-Windows-10-with-an-automatic-VPN-profile): informazioni su come Microsoft implementa l'accesso condizionale per la connettività VPN. I profili VPN contengono tutte le informazioni richieste da un dispositivo per connettersi alla rete aziendale, inclusi i metodi di autenticazione supportati e il server VPN a cui il dispositivo deve connettersi. Le modifiche apportate all'aggiornamento dell'anniversario di Windows 10, tra cui l'accesso condizionale e la Single Sign-On, consentono di creare il profilo di connessione VPN always on.

- [Accesso condizionale in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal): la sicurezza è una delle principali preoccupazioni per le organizzazioni che usano il cloud. Un aspetto fondamentale della sicurezza del cloud è l'identità e l'accesso per la gestione delle risorse cloud. In un mondo per dispositivi mobili e cloud, gli utenti possono accedere alle risorse dell'organizzazione usando un'ampia gamma di dispositivi e app ovunque ci si trovi. Di conseguenza, concentrarsi solo su chi può accedere a una risorsa non è più sufficiente. Per padroneggiare il saldo tra sicurezza e produttività, i professionisti IT devono anche considerare il modo in cui viene eseguito l'accesso a una risorsa in una decisione di controllo di accesso.

- [VPN e accesso condizionale](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): il client VPN è ora in grado di integrarsi con la piattaforma di accesso condizionale basato sul cloud per fornire un'opzione di conformità del dispositivo per i client remoti. L'accesso condizionale è un motore di valutazione basato su criteri che ti consente di creare regole di accesso per qualsiasi applicazione connessa ad Azure Active Directory (Azure AD).
