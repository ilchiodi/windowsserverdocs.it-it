---
title: Accesso condizionale per la connettività VPN con Azure AD
description: In questo passaggio facoltativo, è possibile ottimizzare l'accesso agli utenti VPN come autorizzato le risorse con l'accesso condizionale di Azure Active Directory (Azure AD).
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
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067301"
---
# Passaggio 7. (Facoltativo) Accesso condizionale per la connettività VPN con Azure AD

& #171;  [ **Precedente:** passaggio 6. Configurare i Client di Windows 10 sempre per le connessioni VPN](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)<br>
& #187; [ **Successivo:** passaggio 7.1. Configurare EAP-TLS per ignorare il controllo elenco di revoche di certificati (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md)

In questo passaggio facoltativo, è possibile ottimizzare come gli utenti della VPN accedere alle risorse con [l'accesso condizionale di Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). Con l'accesso condizionale di Azure AD per la connettività di rete privata virtuale (VPN), puoi proteggere le connessioni VPN. L'accesso condizionale è un motore di valutazione basato su criteri che ti consente di creare regole di accesso per qualsiasi applicazione connessa ad Azure Active Directory (Azure AD). 

## Prerequisiti

Hai familiarità con gli argomenti seguenti:
- [Accesso condizionale in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)
- [VPN e accesso condizionale](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)

Per configurare l'accesso condizionale di Azure Active Directory per la connettività VPN, devi configurare gli aspetti seguenti:
- [Infrastruttura server](always-on-vpn/deploy/vpn-deploy-server-infrastructure.md)
- [Server di accesso remoto per Always On VPN](always-on-vpn/deploy/vpn-deploy-ras.md)
- [Server dei criteri di rete](always-on-vpn/deploy/vpn-deploy-nps.md)
- [DNS e le impostazioni del Firewall](always-on-vpn/deploy/vpn-deploy-dns-firewall.md)
- [Client di Windows 10 Always On connessioni VPN](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)

## [Passaggio 7.1. Configurare EAP-TLS per ignorare il controllo dell'elenco di revoche di certificati (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md)

In questo passaggio, puoi aggiungere **IgnoreNoRevocationCheck** e impostarla per consentire l'autenticazione dei client quando il certificato non includere punti di distribuzione CRL. Per impostazione predefinita, IgnoreNoRevocationCheck è impostato su 0 (disabilitato).

Un client EAP-TLS non riesce a connettersi a meno che il server dei criteri di rete viene completato un controllo di revoca della catena di certificati (incluso il certificato radice). I certificati di cloud rilasciati da Azure AD all'utente non hanno un CRL quanto sono inclusi i certificati di breve durati con una durata di un'ora. EAP in Criteri di rete deve essere configurato per ignorare l'assenza di un CRL. Poiché il metodo di autenticazione EAP-TLS, questo valore del Registro di sistema è necessaria solo in **EAP\13**. Se vengono utilizzati altri metodi di autenticazione EAP, quindi il valore del Registro di sistema deve essere aggiunto in quelli anche. 




## [Passaggio 7.2. Creare certificati radice per l'autenticazione VPN con Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md)

In questo passaggio, configurare i certificati radice per l'autenticazione VPN con Azure AD, che crea automaticamente un'app di cloud server VPN nel tenant.  

Per configurare l'accesso condizionale per la connettività VPN, è necessario:
1. Creare un certificato VPN nel portale di Azure (è possibile creare uno o più certificati).
2. Scarica il certificato VPN.
3. Distribuisci il certificato al server VPN.

## [Passaggio 7.3. Configurare i criteri di accesso condizionale](vpn-config-conditional-access-policy.md)

In questo passaggio, configurare i criteri di accesso condizionale per la connessione VPN. 

Per configurare i criteri di accesso condizionale, è necessario:
1. Creare un criterio di accesso condizionale che è stato assegnato agli utenti VPN.
2. Impostare l'app Cloud al **Server VPN**.
3. Imposta la concessione (controllo di accesso) per **richiedere l'autenticazione a più fattori**.  Puoi usare altri controlli in base alle esigenze.

## [Passaggio 7.4. Distribuire certificati radice di accesso condizionale locale AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)

In questo passaggio, distribuire un certificato radice attendibile per l'autenticazione VPN in locale AD.

Per distribuire il certificato radice attendibili, è necessario:
1. Aggiungi il certificato scaricato come *per l'autenticazione VPN CA radice attendibile*.
2. Importa il certificato radice per il client VPN e il server VPN.
3. Verifica che i certificati sono presenti e mostrano come attendibile.


## [Passaggio 7.5. Creare OMA-DM sulla base di profili VPNv2 in dispositivi Windows 10](vpn-create-oma-dm-based-vpnv2-profiles.md)

In questo passaggio, è possibile creare OMA-DM in base i profili VPNv2 tramite Intune per distribuire un criterio di configurazione dei dispositivi VPN. Se si desidera utilizzare SCCM o Script di PowerShell per creare i profili VPNv2, vedere [le impostazioni CSP VPNv2](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp) per altri dettagli. 


## Passaggio successivo
Passaggio [7.1. Configurare EAP-TLS per ignorare il controllo elenco di revoche di certificati (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md): In questo passaggio, è necessario aggiungere **IgnoreNoRevocationCheck** e impostarla per consentire l'autenticazione dei client quando il certificato non includere punti di distribuzione CRL. Per impostazione predefinita, IgnoreNoRevocationCheck è impostato su 0 (disabilitato).

---

## Argomenti correlati
- [Configurare i profili VPNv2](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): client VPN può ora essere integrato con la basata sul cloud piattaforma di accesso condizionale per fornire un'opzione di conformità dei dispositivi per i client remoti. In questo passaggio, configurare i profili VPNv2 **\ < devicecompliance del > \ < Enabled > true\ < / abilitata >**. 
 
- [Miglioramento di accesso remoto in Windows 10 con un profilo VPN automatico](https://www.microsoft.com/itshowcase/Article/Content/894/Enhancing-remote-access-in-Windows-10-with-an-automatic-VPN-profile): informazioni su come Microsoft implementa l'accesso condizionale per la connessione VPN. I profili VPN contengono tutte le informazioni che richiede un dispositivo di connettersi alla rete aziendale, inclusi i metodi di autenticazione che sono supportati e il server VPN che deve connettersi al dispositivo. Le modifiche nell'aggiornamento dell'anniversario di Windows 10, incluso l'accesso condizionale e single sign-on, reso possibile consentono di creare il profilo di connessione VPN Always-On. Abbiamo creato il profilo di connessione per appartenenti al dominio e dispositivi con Microsoft Intune: Gestione console di System Center Configuration Manager. 

- [Istruzione condizionale accedere in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal): sicurezza è una preoccupazione superiore per le organizzazioni che usano il cloud. Un aspetto chiave della sicurezza cloud è identità e dell'accesso quando si tratta di gestire le risorse cloud. In un mondo mobile-first, cloud-first, gli utenti possono accedere alle risorse dell'organizzazione con un'ampia gamma di dispositivi e le app ovunque. A seguito di questa operazione, semplicemente concentrandoci su chi può accedere a una risorsa non è sufficiente più. Per poter master l'equilibrio tra sicurezza e la produttività, i professionisti IT inoltre necessitano eseguire il factoring accede come una risorsa in una decisione di controllo di accesso.

- [VPN e accesso condizionale](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): client VPN può ora essere integrato con la basata sul cloud piattaforma di accesso condizionale per fornire un'opzione di conformità dei dispositivi per i client remoti. L'accesso condizionale è un motore di valutazione basato su criteri che ti consente di creare regole di accesso per qualsiasi applicazione connessa ad Azure Active Directory (Azure AD). 

---