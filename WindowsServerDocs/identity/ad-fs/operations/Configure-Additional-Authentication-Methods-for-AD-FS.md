---
ms.assetid: ddfbbda3-30ca-4537-af12-667efc6f63ff
title: Configurare metodi di autenticazione aggiuntivi per AD FS
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 65baa6f94e1efa1fa337eab477b18f947cf0bb2d
ms.sourcegitcommit: 36d7b1dfd7da8e9f303d007a628e76149de000f2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/21/2018
---
# <a name="configure-additional-authentication-methods-for-ad-fs"></a>Configurare metodi di autenticazione aggiuntivi per AD FS

>Si applica a: Windows Server 2016, Windows Server 2012 R2

Per abilitare l'autenticazione a più fattori (MFA), è necessario selezionare almeno un metodo di autenticazione aggiuntivo. Per impostazione predefinita, in Active Directory Federation Services (ADFS) in Windows Server 2012 R2, è possibile selezionare l'autenticazione del certificato (in altre parole, autenticazione basata su smart card) come un metodo di autenticazione aggiuntivo.

> [!NOTE]
> Se si seleziona l'autenticazione del certificato, assicurarsi che i certificati smart card sono stato eseguito il provisioning in modo sicuro e sono i requisiti del pin.

Non tutti sanno che Microsoft Azure offre funzionalità simili nel cloud. Per ulteriori informazioni su [soluzioni con identità Microsoft Azure ](http://aka.ms/m2w274).<br /><br />Creare una soluzione con identità ibrida in Microsoft Azure:<br /> - [Informazioni su Azure multi-Factor Authentication.](http://aka.ms/ey6o9r)<br /> - [Gestire le identità per ambienti ibridi a foresta singola mediante l'autenticazione cloud.](http://aka.ms/g1jat8)<br /> - [Gestire i rischi con l'autenticazione a più fattori aggiuntiva per le applicazioni sensibili.](http://aka.ms/kt1bbm)|

## <a name="microsoft-and-third-party-additional-authentication-methods"></a>Metodi di autenticazione aggiuntivi Microsoft e di terze parti
È inoltre possibile configurare e abilitare metodi di autenticazione di terze parti in ADFS in Windows Server 2012 R2 e Microsoft. Una volta installato e registrato con AD FS, è possibile applicare AMF come parte del criterio di autenticazione globali o per ricorrere a terze parti.

Di seguito è attualmente disponibile un elenco alfabetico di Microsoft e i provider di terze parti offrono soluzioni MFA per ADFS in Windows Server 2012 R2.

||||
|-|-|-|
|**Provider**|**Offerta**|**Collegamento per maggiori informazioni**|
|Gemalto|Gemalto identità e servizi di sicurezza|[http://www.gemalto.com/identity](http://www.gemalto.com/identity)|
|inWebo tecnologie|Servizio di autenticazione Enterprise inWebo|[inWebo autenticazione Enterprise](http://www.inwebo.com)|
|Utenti di accesso|Connettore di login People MFA API per AD FS 2012 R2 (versione beta pubblica)|[https://www.loginpeople.com](https://www.loginpeople.com)|
|Microsoft Corporation.|Microsoft Azure MFA|[Guida allo scenario: Gestire i rischi con l'autenticazione a più fattori aggiuntiva per le applicazioni sensibili](https://technet.microsoft.com/library/dn280946.aspx) (vedere il passaggio 3)|
|RSA, la divisione di sicurezza di EMC|Agente di autenticazione RSA SecurID per Microsoft Active Directory Federation Services|[Agente di autenticazione RSA SecurID per Microsoft Active Directory Federation Services](http://www.emc.com/security/rsa-securid/rsa-authentication-agents/microsoft-ad-fs.htm)|
|SafeNet, Inc.|SafeNet Authentication Service (SAS) agente per AD FS|[SafeNet Authentication Service: Guida alla configurazione di AD FS agente](http://www.safenet-inc.com/resources/integration-guide/data-protection/Safenet_Authentication_Service/SafeNet_Authentication_Service__AD_FS_Agent_Configuration_Guide/?langtype=1033)|
|Swisscom|Il servizio di autenticazione di ID per dispositivi mobili e servizi di firma digitale|[Servizio di autenticazione per dispositivi mobili ID](http://swisscom.ch/mid)|
|Symantec|Symantec convalida e il servizio di protezione ID (VIP)|[Symantec convalida e il servizio di protezione ID (VIP)](http://www.symantec.com/vip-authentication-service)|

## <a name="custom-authentication-method-for-ad-fs-in-windows-server-2012-r2"></a>Metodo di autenticazione personalizzato per ADFS in Windows Server 2012 R2
Sono disponibili istruzioni per la creazione di un metodo di autenticazione personalizzato per ADFS in Windows Server 2012 R2. Per ulteriori informazioni, vedere [creare un metodo di autenticazione personalizzato per ADFS in Windows Server 2012 R2](https://go.microsoft.com/fwlink/?LinkID=511980).

## <a name="see-also"></a>Vedere anche
[Gestire i rischi con l'autenticazione a più fattori aggiuntiva per le applicazioni sensibili](Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)


