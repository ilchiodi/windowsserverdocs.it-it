---
title: Configurare i browser per utilizzare l'autenticazione integrata Windows (WIA) con AD FS
description: Questo documento viene descritto come configurare i browser per l'utilizzo di WIA con AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f7d43931d1fe4958a539ff1b728e4cc154d06248
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="configure-browsers-to-use-windows-integrated-authentication-wia-with-ad-fs"></a>Configurare i browser per utilizzare l'autenticazione integrata Windows (WIA) con AD FS

Per impostazione predefinita, l'autenticazione integrata Windows (WIA) è abilitata in Active Directory Federation Services (ADFS) in Windows Server 2012 R2 per le richieste di autenticazione che si verificano nella rete interna dell'organizzazione (intranet) per qualsiasi applicazione che utilizza un browser per l'autenticazione.

AD FS 2016 ora è un'impostazione predefinite migliorate che consente il browser Edge eseguire WIA mentre Windows Phone non anche (in modo errato) intercettazione anche:

    =~Windows\s*NT.*Edge

Il metodo precedente che non è più necessario configurare le stringhe agente utente per supportare scenari comuni di Edge, anche se tali file vengono aggiornati spesso.

Per altri browser, configurare la proprietà di ADFS **WiaSupportedUserAgents** per aggiungere i valori necessari in base i browser in uso.  È possibile utilizzare le procedure riportate di seguito.



### <a name="view-wiasupporteduseragent-settings"></a>Visualizzare le impostazioni WIASupportedUserAgent
Il **WIASupportedUserAgents** definisce gli agenti utente che supportano WIA. ADFS consente di analizzare la stringa agente utente durante l'esecuzione di account di accesso in un browser o un controllo browser.

È possibile visualizzare le impostazioni correnti usando l'esempio di PowerShell seguente:

```powershell
    $strings = Get-AdfsProperties | select -ExpandProperty WiaSupportedUserAgents
```

![Supporto per WIA](../operations/media/Configure-AD-FS-Browser-WIA/wiasupport.png)

### <a name="change-wiasupporteduseragent-settings"></a>Modificare le impostazioni WIASupportedUserAgent
Per impostazione predefinita, una nuova installazione di AD FS include un set di corrispondenze di stringa agente utente creato. Tuttavia, le possono essere aggiornate in base alle modifiche apportate al browser e dispositivi. In particolare, i dispositivi Windows hanno stringhe agente utente simili con lievi variazioni nei token. Nell'esempio seguente di Windows PowerShell fornisce le migliori indicazioni per il set corrente di dispositivi sul mercato che supportano WIA trasparente:

```powershell
    Set-AdfsProperties -WIASupportedUserAgents @("MSIE 6.0", "MSIE 7.0; Windows NT", "MSIE 8.0", "MSIE 9.0", "MSIE 10.0; Windows NT 6", "Windows NT 6.3; Trident/7.0", "Windows NT 6.3; Win64; x64; Trident/7.0", "Windows NT 6.3; WOW64; Trident/7.0", "Windows NT 6.2; Trident/7.0", "Windows NT 6.2; Win64; x64; Trident/7.0", "Windows NT 6.2; WOW64; Trident/7.0", "Windows NT 6.1; Trident/7.0", "Windows NT 6.1; Win64; x64; Trident/7.0", "Windows NT 6.1; WOW64; Trident/7.0", "MSIPC", "Windows Rights Management Client")
```

Il comando precedente garantisce che ADFS riguarda solo i seguenti casi di utilizzo per WIA:

Agenti utente|Casi di utilizzo|
-----|-----|
MSIE 6.0|INTERNET EXPLORER 6.0|
MSIE 7.0; Windows NT|Internet Explorer 7, Internet Explorer nell'area intranet. Il frammento "Windows NT" viene inviato dal sistema operativo desktop.|
MSIE 8.0|Internet Explorer 8.0 (i dispositivi non inviare questa, pertanto è necessario rendere più specifica)|
MSIE 9.0|Internet Explorer 9.0 (i dispositivi non inviano, non è necessario rendere più specifico)|
MSIE 10.0; Windows NT 6|Internet Explorer 10.0 per Windows XP e versioni successive del sistema operativo desktop</br></br>I dispositivi Windows Phone 8.0 (con preferenza impostata al cellulare) sono esclusi perché inviano</br></br>Agente utente: Mozilla/5.0 (compatibile; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Tocco. NOKIA; Lumia 920)|
Windows NT 6.3; Trident/7.0</br></br>Windows NT 6.3; Win64; x64; Trident/7.0</br></br>Windows NT 6.3; WOW64; Trident/7.0| Sistema operativo desktop Windows 8.1, piattaforme diverse|
Windows NT 6.2; Trident/7.0</br></br>Windows NT 6.2; Win64; x64; Trident/7.0</br></br>Windows NT 6.2; WOW64; Trident/7.0|Sistema operativo desktop Windows 8, piattaforme diverse|
Windows NT 6.1; Trident/7.0</br></br>Windows NT 6.1; Win64; x64; Trident/7.0</br></br>Windows NT 6.1; WOW64; Trident/7.0|Sistema operativo desktop Windows 7, platoforms diversi|
MSIPC| Protezione delle informazioni di Microsoft e Client di controllo|
Client Windows Rights Management|Client Windows Rights Management|
