---
ms.assetid: ddfbbda3-30ca-4537-af12-667efc6f63ff
title: Configure Additional Authentication Methods for AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 10/04/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 73de8908677b3f74651b10c29ef2abe62e484694
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868412"
---
# <a name="configure-additional-authentication-methods-for-ad-fs"></a>Configure Additional Authentication Methods for AD FS

>Si applica a: Windows Server 2016, Windows Server 2012 R2

Per abilitare l'autenticazione a più fattori (MFA), è necessario selezionare almeno un metodo di autenticazione aggiuntivo. Per impostazione predefinita, in Active Directory Federation Services (ADFS) in Windows Server 2012 R2, è possibile selezionare l'autenticazione del certificato (in altre parole, autenticazione basata su smart card) come metodo di autenticazione aggiuntivo.

> [!NOTE]
> Se si seleziona l'autenticazione del certificato, assicurarsi che il provisioning dei certificati smart card sia stato eseguito in modo sicuro e che per tali certificati sia richiesto il PIN.

Non tutti sanno che Microsoft Azure offre funzionalità simili nel cloud. Altre informazioni sulle [soluzioni di gestione delle identità di Microsoft Azure](http://aka.ms/m2w274).<br /><br />Creare una soluzione di gestione delle identità ibrida in Microsoft Azure:<br /> - [Informazioni su Azure multi-Factor Authentication.](http://aka.ms/ey6o9r)<br /> - [Gestire le identità per ambienti ibridi a foresta singola mediante l'autenticazione cloud.](http://aka.ms/g1jat8)<br /> - [Gestire i rischi con l'autenticazione a più fattori aggiuntiva per le applicazioni sensibili.](http://aka.ms/kt1bbm)

## <a name="microsoft-and-third-party-additional-authentication-methods"></a>Metodi di autenticazione aggiuntivi Microsoft e di terze parti
È anche possibile configurare e abilitare metodi di autenticazione di terze parti in ADFS in Windows Server 2012 R2 e Microsoft. Una volta installato e registrato con AD FS, è possibile adottare MFA come parte dei criteri di autenticazione globali o per ogni relying party.

L'elenco seguente riporta in ordine alfabetico i provider Microsoft e di terze parti che offrono soluzioni MFA disponibili per ADFS in Windows Server 2012 R2.

|Provider|Offerta|Collegamento per maggiori informazioni|
|-|-|-| 
|aPersona|aPersona Adaptive multi-Factor Authentication per Microsoft ADFS SSO|[aPersona ASM ADFS Adapter](https://www.apersona.com/adfs)|
|Sicurezza Duo|Duo scheda MFA per ADFS|[Duo autenticazione per AD FS](https://duo.com/docs/adfs)|
|Gemalto|Gemalto Identity & Security Services|[http://www.gemalto.com/identity](http://www.gemalto.com/identity)|
|inWebo Technologies|inWebo Enterprise Authentication service|[inWebo autenticazione Enterprise](http://www.inwebo.com)|
|Login People|Login People MFA API connector for AD FS 2012 R2 (versione beta pubblica)|[https://www.loginpeople.com](https://www.loginpeople.com)|
|Microsoft Corp.|Microsoft Azure MFA|[Guida allo scenario: Gestire i rischi con l'autenticazione a più fattori aggiuntiva per le applicazioni con esigenze particolari](https://technet.microsoft.com/library/dn280946.aspx) (vedere il passaggio 3)|
Mideye | Provider di autenticazione mideye per ad FS | [Autenticazione a due fattori mideye con Microsoft Active Directory Federation Services](https://www.mideye.com/support/administrators/documentation/integration/microsoft-adfs/)|
|Okta | MFA Okta per Active Directory Federation Services | [MFA Okta per Active Directory Federation Services (ADFS)](https://help.okta.com/en/prod/Content/Topics/integrations/adfs-okta-int.htm)|
|Una sola identità| Starling 2FA AD FS|[Starling 2FA scheda ADFS](https://www.oneidentity.com/products/starling-two-factor-authentication/)|
|Una sola identità| Defender AD FS|[Adapter di Defender AD FS](https://www.oneidentity.com/products/defender/)|
|Ping Identity|Adapter PingID MFA per ADFS|[Adapter PingID MFA per ADFS](https://documentation.pingidentity.com/pingid/pingidAdminGuide/index.shtml#pid_c_PingIDforADFSSSO.html)|
|RSA, The Security Division of EMC|RSA SecurID Authentication Agent for Microsoft Active Directory Federation Services|[Agente di autenticazione RSA SecurID per Microsoft Active Directory Federation Services](http://www.emc.com/security/rsa-securid/rsa-authentication-agents/microsoft-ad-fs.htm)|
|SafeNet, Inc.|SafeNet Authentication Service (SAS) Agent for AD FS|[SafeNet Authentication Service: AD FS Agent Configuration Guide](http://www.safenet-inc.com/resources/integration-guide/data-protection/Safenet_Authentication_Service/SafeNet_Authentication_Service__AD_FS_Agent_Configuration_Guide/?langtype=1033)|
|SecureMFA|Provider SecureMFA OTP| [Provider di ad FS Multi-Factor Authentication](https://www.securemfa.com/)|
|Swisscom|Mobile ID Authentication Service and Signature Services|[Servizio di autenticazione per dispositivi mobili ID](http://swisscom.ch/mid)|
|Symantec|Symantec Validation and ID Protection Service (VIP)|[Convalida di Symantec e ID servizio di protezione (VIP)](http://www.symantec.com/vip-authentication-service)|
|Trusona|Essential (senza password MFA) e dirigente (essenziali + identità correzione)| [Trusona multi-factor Authentication](https://www.trusona.com/solution-overview/)|


## <a name="custom-authentication-method-for-ad-fs-in-windows-server-2012-r2"></a>Metodo di autenticazione personalizzato per ADFS in Windows Server 2012 R2
Sono disponibili le istruzioni per creare un metodo di autenticazione personalizzato per ADFS in Windows Server 2012 R2. Per altre informazioni, vedere l'articolo sulla [creazione di un metodo di autenticazione personalizzato per ADFS in Windows Server 2012 R2](https://go.microsoft.com/fwlink/?LinkID=511980).

## <a name="see-also"></a>Vedere anche
[Gestire i rischi con l'autenticazione a più fattori aggiuntiva per le applicazioni sensibili](Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)


