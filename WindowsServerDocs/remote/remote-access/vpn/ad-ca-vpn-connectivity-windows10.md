---
title: Accesso condizionale per la connettività VPN con Azure AD
description: In questo passaggio facoltativo, è possibile ottimizzare la modalità VPN gli utenti autorizzati accedono le risorse usando l'accesso condizionale di Azure Active Directory (Azure AD).
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 07/13/2018
ms.reviewer: deverette
ms.openlocfilehash: c9104a5d9ae3069e753b8b771270502c4264db96
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885272"
---
# <a name="step-7-optional-conditional-access-for-vpn-connectivity-using-azure-ad"></a>Passaggio 7. (Facoltativo) Accesso condizionale per la connettività VPN con Azure AD

&#171;  [**Precedente:** Passaggio 6. Configurare sempre i Client Windows 10 su connessioni VPN](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)<br>
&#187; [ **Next:** Passaggio 7.1. Configurare EAP-TLS per ignorare il controllo elenco di revoche di certificati (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md)

In questo passaggio facoltativo, è possibile ottimizzare come gli utenti VPN accedono alle risorse usando [accesso condizionale di Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). Con accesso condizionale di Azure AD per la connettività di rete privata virtuale (VPN), è possibile proteggere le connessioni VPN. L'accesso condizionale è un motore di valutazione basato su criteri che ti consente di creare regole di accesso per qualsiasi applicazione connessa ad Azure Active Directory (Azure AD). 

## <a name="prerequisites"></a>Prerequisiti

Si ha familiarità con gli argomenti seguenti:
- [Accesso condizionale in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)
- [VPN e l'accesso condizionale](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)

Per configurare l'accesso condizionale di Azure Active Directory per la connettività VPN, è necessario configurare gli aspetti seguenti:
- [Infrastruttura server](always-on-vpn/deploy/vpn-deploy-server-infrastructure.md)
- [Server di accesso remoto VPN Always On](always-on-vpn/deploy/vpn-deploy-ras.md)
- [Server dei criteri di rete](always-on-vpn/deploy/vpn-deploy-nps.md)
- [DNS e le impostazioni del Firewall](always-on-vpn/deploy/vpn-deploy-dns-firewall.md)
- [Windows 10 Client sempre su connessioni VPN](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)

## <a name="step-71-configure-eap-tls-to-ignore-certificate-revocation-list-crl-checkingvpn-config-eap-tls-to-ignore-crl-checkingmd"></a>[Passaggio 7.1. Configurare EAP-TLS per ignorare il controllo elenco di revoche di certificati (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md)

In questo passaggio, è possibile aggiungere **IgnoreNoRevocationCheck** e impostarlo per consentire l'autenticazione dei client quando il certificato non include i punti di distribuzione CRL. Per impostazione predefinita, IgnoreNoRevocationCheck è impostata su 0 (disabilitato).

Non può connettersi un client di EAP-TLS, a meno che il server dei criteri di rete ha completato un controllo delle revoche della catena di certificati (incluso il certificato radice). Certificati cloud rilasciati all'utente da Azure AD non è un elenco CRL perché sono i certificati di breve durati con una durata di un'ora. EAP in Criteri di rete deve essere configurato per ignorare l'assenza di un CRL. Poiché il metodo di autenticazione EAP-TLS, questo valore del Registro di sistema è necessario solo in **EAP\13**. Se vengono utilizzati altri metodi di autenticazione EAP, quindi il valore del Registro di sistema deve essere aggiunti nelle quelli anche. 




## <a name="step-72-create-root-certificates-for-vpn-authentication-with-azure-advpn-create-root-cert-for-vpn-auth-azure-admd"></a>[Passaggio 7.2. Creare i certificati radice per l'autenticazione VPN con Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md)

In questo passaggio si configura i certificati radice per l'autenticazione VPN con Azure AD, che crea automaticamente un'app cloud di server VPN nel tenant.  

Per configurare l'accesso condizionale per la connettività VPN, è necessario:
1. Creare un certificato VPN nel portale di Azure (è possibile creare più di un certificato).
2. Scaricare il certificato VPN.
3. Distribuire il certificato nel server VPN.

## <a name="step-73-configure-the-conditional-access-policyvpn-config-conditional-access-policymd"></a>[Passaggio 7.3. Configurare i criteri di accesso condizionale](vpn-config-conditional-access-policy.md)

In questo passaggio si configura il criterio di accesso condizionale per la connettività VPN. 

Per configurare i criteri di accesso condizionale, è necessario:
1. Creare un criterio di accesso condizionale che viene assegnato agli utenti VPN.
2. Impostare l'app Cloud **Server VPN**.
3. Impostare la concessione (controllo degli accessi) su **richiedono l'autenticazione a più fattori**.  È possibile usare altri controlli in base alle esigenze.

## <a name="step-74-deploy-conditional-access-root-certificates-to-on-premises-advpn-deploy-cond-access-root-cert-to-on-premise-admd"></a>[Passaggio 7.4. Distribuire i certificati radice di accesso condizionale ai servizi locali Active Directory](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)

In questo passaggio si distribuisce un certificato radice attendibile per l'autenticazione VPN per on-premises AD.

Per distribuire il certificato radice attendibile, è necessario:
1. Aggiungere il certificato scaricato come una *CA radice attendibile per l'autenticazione VPN*.
2. Importare il certificato radice per il server VPN e il client VPN.
3. Verificare che i certificati siano presenti e mostrano come attendibile.


## <a name="step-75-create-oma-dm-based-vpnv2-profiles-to-windows-10-devicesvpn-create-oma-dm-based-vpnv2-profilesmd"></a>[Passaggio 7.5. Creare OMA-DM basati su profili VPNv2 per i dispositivi Windows 10](vpn-create-oma-dm-based-vpnv2-profiles.md)

In questo passaggio, è possibile creare OMA-DM basati su profili VPNv2 usando Intune per distribuire un criterio di configurazione del dispositivo VPN. Se si desidera utilizzare SCCM o Script di PowerShell per creare i profili di VPNv2, vedere [impostazioni VPNv2 CSP](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp) per altri dettagli. 


## <a name="next-step"></a>Passaggio successivo
[Passaggio 7.1. Configurare EAP-TLS per ignorare il controllo elenco di revoche di certificati (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md): In questo passaggio, è necessario aggiungere **IgnoreNoRevocationCheck** e impostarlo per consentire l'autenticazione dei client quando il certificato non include i punti di distribuzione CRL. Per impostazione predefinita, IgnoreNoRevocationCheck è impostata su 0 (disabilitato).

---

## <a name="related-topics"></a>Argomenti correlati
- [Configurare i profili di VPNv2](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): Il client VPN può ora essere integrato con la piattaforma di accesso condizionale basato sul cloud per fornire un'opzione di conformità dei dispositivi per i client remoti. In questo passaggio si configura i profili di VPNv2 con  **\<DeviceCompliance > \<abilitato > true\<o abilitati >**. 
 
- [Miglioramento dell'accesso remoto in Windows 10 con un profilo VPN automatico](https://www.microsoft.com/itshowcase/Article/Content/894/Enhancing-remote-access-in-Windows-10-with-an-automatic-VPN-profile): Informazioni su come Microsoft implementa l'accesso condizionale per la connettività VPN. I profili VPN contengono tutte le informazioni di che un dispositivo richiede per la connessione alla rete aziendale, inclusi i metodi di autenticazione supportati e il server VPN a cui il dispositivo deve connettersi. Le modifiche nell'aggiornamento dell'anniversario di Windows 10, tra cui l'accesso condizionale e single sign-on, effettuate a Microsoft per creare il profilo di connessione VPN Always-on. È stato creato il profilo di connessione per dominio e i dispositivi gestiti da Intune: Microsoft utilizzando la console di System Center Configuration Manager. 

- [Accesso condizionale in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal): La sicurezza è una priorità assoluta per le organizzazioni che usano il cloud. Un aspetto fondamentale della sicurezza nel cloud è identità e degli accessi, se si desidera gestire le risorse cloud. In un mondo incentrato su dispositivi mobili e cloud-first, gli utenti possono accedere alle risorse aziendali usando un'ampia gamma di dispositivi e App ovunque. Di conseguenza, concentrarsi solo su chi può accedere a una risorsa non è più sufficiente. Per poter equilibrio ideale tra produttività e sicurezza, i professionisti IT devono anche a fattori come una risorsa è a cui si accede a una decisione di controllo di accesso.

- [VPN e l'accesso condizionale](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): Il client VPN può ora essere integrato con la piattaforma di accesso condizionale basato sul cloud per fornire un'opzione di conformità dei dispositivi per i client remoti. L'accesso condizionale è un motore di valutazione basato su criteri che ti consente di creare regole di accesso per qualsiasi applicazione connessa ad Azure Active Directory (Azure AD). 

---