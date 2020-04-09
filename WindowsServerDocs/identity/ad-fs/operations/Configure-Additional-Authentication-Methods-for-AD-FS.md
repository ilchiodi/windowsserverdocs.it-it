---
ms.assetid: ddfbbda3-30ca-4537-af12-667efc6f63ff
title: Configure Additional Authentication Methods for AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/26/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b9bef61382812372f966cd6771c39d6a8ebbdd7a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859874"
---
# <a name="configure-additional-authentication-methods-for-ad-fs"></a>Configure Additional Authentication Methods for AD FS

Per abilitare l'autenticazione a più fattori (MFA), è necessario selezionare almeno un metodo di autenticazione aggiuntivo. Per impostazione predefinita, in Active Directory Federation Services (AD FS) in Windows Server 2012 R2, è possibile selezionare l'autenticazione del certificato (ovvero l'autenticazione basata su smart card) come metodo di autenticazione aggiuntivo.

> [!NOTE]
> Se si seleziona l'autenticazione del certificato, assicurarsi che il provisioning dei certificati smart card sia stato eseguito in modo sicuro e che per tali certificati sia richiesto il PIN.

Non tutti sanno che Microsoft Azure offre funzionalità simili nel cloud. Altre informazioni sulle [soluzioni di gestione delle identità di Microsoft Azure](https://aka.ms/m2w274).<p>Creare una soluzione di gestione delle identità ibrida in Microsoft Azure:<br /> - informazioni [sulle multi-factor authentication di Azure.](https://aka.ms/ey6o9r)<br /> - [gestire le identità per gli ambienti ibridi a foresta singola con l'autenticazione cloud.](https://aka.ms/g1jat8)<br /> - [gestire i rischi con multi-factor authentication aggiuntivi per le applicazioni riservate.](https://aka.ms/kt1bbm)

## <a name="microsoft-and-third-party-additional-authentication-methods"></a>Metodi di autenticazione aggiuntivi Microsoft e di terze parti
È inoltre possibile configurare e abilitare metodi di autenticazione Microsoft e di terze parti in AD FS in Windows Server 2012 R2. Una volta installato e registrato con AD FS, è possibile applicare l'autenticazione a più fattori nell'ambito dei criteri di autenticazione globali o per relying party.

L'elenco seguente riporta in ordine alfabetico i provider Microsoft e di terze parti che offrono soluzioni MFA disponibili per ADFS in Windows Server 2012 R2.

|Provider|Offerta|Collegamento per maggiori informazioni|
|-|-|-| 
|aPersona|aPersona Adaptive Multi-Factor Authentication per Microsoft ADFS SSO|[Adapter ADFS aPersona ASM](https://www.apersona.com/adfs)|
|Cyphercor Inc.|Multi-Factor Authentication LoginTC per AD FS|[Connettore AD FS LoginTC](https://www.logintc.com/docs/connectors/adfs.html)|
|Sicurezza Duo|Scheda Duo multi-factor authentication per AD FS|[Autenticazione Duo per AD FS](https://duo.com/docs/adfs)|
|Futura|Suite di autenticazione futura per AD FS|[Autenticazione avanzata futura](https://futurae.com)|
|Gemalto|Gemalto Identity & Security Services|[http://www.gemalto.com/identity](http://www.gemalto.com/identity)|
|inWebo Technologies|inWebo Enterprise Authentication service|[inWebo Enterprise Authentication](http://www.inwebo.com)|
|Login People|Login People MFA API connector for AD FS 2012 R2 (versione beta pubblica)|[https://www.loginpeople.com](https://www.loginpeople.com)|
|Microsoft Corp.|Microsoft Azure MFA|[Guida allo scenario: Gestire i rischi con l'autenticazione a più fattori aggiuntiva per le applicazioni con esigenze particolari](https://technet.microsoft.com/library/dn280946.aspx) (vedere il passaggio 3)|
Mideye | Provider di autenticazione mideye per ADFS | [Mideye l'autenticazione a due fattori con Microsoft Active Directory Servizio federativo](https://www.mideye.com/support/administrators/documentation/integration/microsoft-adfs/)|
|Okta | Okta multi-factor authentication per Active Directory Federation Services | [Okta multi-factor authentication per Active Directory Federation Services (ADFS)](https://help.okta.com/en/prod/Content/Topics/integrations/adfs-okta-int.htm)|
|Una sola identità| Starling 2FA AD FS|[Starling 2FA AD FS adapter](https://www.oneidentity.com/products/starling-two-factor-authentication/)|
|Una sola identità| AD FS Defender|[Adattatore AD FS Defender](https://www.oneidentity.com/products/defender/)|
|Identità ping|Scheda PingID multi-factor authentication per AD FS|[Scheda PingID multi-factor authentication per AD FS](https://documentation.pingidentity.com/pingid/pingidAdminGuide/index.shtml#pid_c_PingIDforADFSSSO.html)|
|RSA, The Security Division of EMC|RSA SecurID Authentication Agent for Microsoft Active Directory Federation Services|[RSA SecurID Authentication Agent per Microsoft Active Directory Federation Services](http://www.emc.com/security/rsa-securid/rsa-authentication-agents/microsoft-ad-fs.htm)|
|SafeNet, Inc.|SafeNet Authentication Service (SAS) Agent for AD FS|[SafeNet Authentication Service: Guida alla configurazione dell'agente AD FS](http://www.safenet-inc.com/resources/integration-guide/data-protection/Safenet_Authentication_Service/SafeNet_Authentication_Service__AD_FS_Agent_Configuration_Guide/?langtype=1033)|
|SecureMFA|Provider OTP SecureMFA| [Provider di autenticazione a più fattori di ADFS](https://www.securemfa.com/)|
|Swisscom|Mobile ID Authentication Service and Signature Services|[Servizio di autenticazione ID mobile](http://swisscom.ch/mid)|
|Symantec|Symantec Validation and ID Protection Service (VIP)|[Servizio di convalida e ID di protezione Symantec (VIP)](http://www.symantec.com/vip-authentication-service)|
|Trusona|Essential (autenticazione a più fattori) e Executive (Essential + Identity Proofing)| [Autenticazione a più fattori Trusona](https://www.trusona.com/solution-overview/)|


## <a name="custom-authentication-method-for-ad-fs-in-windows-server-2012-r2"></a>Metodo di autenticazione personalizzato per ADFS in Windows Server 2012 R2
Sono disponibili le istruzioni per creare un metodo di autenticazione personalizzato per ADFS in Windows Server 2012 R2. Per altre informazioni, vedere l'articolo sulla [creazione di un metodo di autenticazione personalizzato per ADFS in Windows Server 2012 R2](https://go.microsoft.com/fwlink/?LinkID=511980).

## <a name="see-also"></a>Vedi anche
[Gestire i rischi con l'autenticazione a più fattori aggiuntiva per le applicazioni con esigenze particolari](Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)


