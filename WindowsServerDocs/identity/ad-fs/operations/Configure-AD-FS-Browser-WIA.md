---
title: Configurare i browser per l'uso dell'autenticazione integrata di Windows (WIA) con AD FS
description: Questo documento descrive come configurare i browser per l'uso di WIA con AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/20/2020
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b0c64d90fcbeaf2aa03312b9707bcfa43379271f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859934"
---
# <a name="configure-browsers-to-use-windows-integrated-authentication-wia-with-ad-fs"></a>Configurare i browser per l'uso dell'autenticazione integrata di Windows (WIA) con AD FS

Per impostazione predefinita, l'autenticazione integrata di Windows (WIA) è abilitata in Active Directory Federation Services (AD FS) in Windows Server 2012 R2 per le richieste di autenticazione che si verificano all'interno della rete interna dell'organizzazione (Intranet) per qualsiasi applicazione che utilizza un browser per l'autenticazione.

AD FS 2016 dispone ora di un'impostazione predefinita migliorata che consente al browser Microsoft Edge di eseguire WIA anche in modo non Windows Phone corretto:

    =~Windows\s*NT.*Edge

Il precedente significa che non è più necessario configurare singole stringhe agente utente per supportare scenari Edge comuni, anche se vengono aggiornate abbastanza spesso.

Per gli altri browser, configurare la proprietà AD FS **WiaSupportedUserAgents** per aggiungere i valori richiesti in base ai browser in uso.  È possibile utilizzare le procedure riportate di seguito.



### <a name="view-wiasupporteduseragent-settings"></a>Visualizza impostazioni WIASupportedUserAgent
**WIASupportedUserAgents** definisce gli agenti utente che supportano WIA. ADFS consente di analizzare la stringa agente utente durante l'esecuzione di account di accesso in un browser o un controllo browser.

È possibile visualizzare le impostazioni correnti usando l'esempio di PowerShell seguente:

```powershell
    Get-AdfsProperties | select -ExpandProperty WiaSupportedUserAgents
```

![Supporto WIA](../operations/media/Configure-AD-FS-Browser-WIA/wiasupport.png)

### <a name="change-wiasupporteduseragent-settings"></a>Modificare le impostazioni di WIASupportedUserAgent
Per impostazione predefinita, una nuova installazione di ADFS è un insieme di corrispondenze di stringa agente utente creato. Tuttavia, le possono essere aggiornate in base alle modifiche apportate al browser e dispositivi. In particolare, i dispositivi Windows hanno stringhe agente utente simili con lievi variazioni nei token. Nell'esempio seguente di Windows PowerShell fornisce le migliori indicazioni per il set corrente di dispositivi presenti sul mercato che supportano WIA trasparente:

Se si dispone di AD FS in Windows Server 2012 R2 o versioni precedenti:

```powershell
   Set-AdfsProperties -WIASupportedUserAgents @("MSIE 6.0", "MSIE 7.0; Windows NT", "MSIE 8.0", "MSIE 9.0", "MSIE 10.0; Windows NT 6", "Windows NT 6.3; Trident/7.0", "Windows NT 6.3; Win64; x64; Trident/7.0", "Windows NT 6.3; WOW64; Trident/7.0", "Windows NT 6.2; Trident/7.0", "Windows NT 6.2; Win64; x64; Trident/7.0", "Windows NT 6.2; WOW64; Trident/7.0", "Windows NT 6.1; Trident/7.0", "Windows NT 6.1; Win64; x64; Trident/7.0", "Windows NT 6.1; WOW64; Trident/7.0", "MSIPC", "Windows Rights Management Client", "Edg/79.0.309.43")
```

Se si dispone di AD FS in Windows Server 2016 o versione successiva:

```powershell
   Set-AdfsProperties -WIASupportedUserAgents @("MSIE 6.0", "MSIE 7.0; Windows NT", "MSIE 8.0", "MSIE 9.0", "MSIE 10.0; Windows NT 6", "Windows NT 6.3; Trident/7.0", "Windows NT 6.3; Win64; x64; Trident/7.0", "Windows NT 6.3; WOW64; Trident/7.0", "Windows NT 6.2; Trident/7.0", "Windows NT 6.2; Win64; x64; Trident/7.0", "Windows NT 6.2; WOW64; Trident/7.0", "Windows NT 6.1; Trident/7.0", "Windows NT 6.1; Win64; x64; Trident/7.0", "Windows NT 6.1; WOW64; Trident/7.0", "MSIPC", "Windows Rights Management Client", "Edg/*")
```

Il comando precedente garantisce che ADFS riguarda solo i seguenti casi di utilizzo per WIA:



|Agenti utente|Casi di utilizzo|
|-----|-----|
|MSIE 6.0|INTERNET EXPLORER 6.0|
|MSIE 7.0; Windows NT|Internet Explorer 7, Internet Explorer nell'area intranet. Il frammento "Windows NT" viene inviato dal sistema operativo desktop.|
|MSIE 8.0|Internet Explorer 8.0 (i dispositivi non inviare questa, pertanto è necessario rendere più specifica)|
|MSIE 9.0|Internet Explorer 9.0 (i dispositivi non inviano, non è necessario rendere più specifico)|
|MSIE 10.0; Windows NT 6|Internet Explorer 10.0 per Windows XP e versioni più recenti del sistema operativo desktop</br></br>I dispositivi Windows Phone 8.0 (con preferenza impostata al cellulare) sono esclusi perché inviano</br></br>Agente utente: Mozilla/5.0 (compatibile; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Tocco. NOKIA; Lumia 920)|
|Windows NT 6.3; Trident/7.0</br></br>Windows NT 6.3; Win64; x64; Trident/7.0</br></br>Windows NT 6.3; WOW64; Trident/7.0| Sistema operativo Windows 8.1 desktop, piattaforme diverse|
|Windows NT 6.2; Trident/7.0</br></br>Windows NT 6.2; Win64; x64; Trident/7.0</br></br>Windows NT 6.2; WOW64; Trident/7.0|Sistema operativo desktop Windows 8, piattaforme diverse|
|Windows NT 6.1; Trident/7.0</br></br>Windows NT 6.1; Win64; x64; Trident/7.0</br></br>Windows NT 6.1; WOW64; Trident/7.0|Sistema operativo desktop Windows 7, piattaforme diverse|
|EDG/79.0.309.43 | Microsoft Edge (cromo) per Windows Server 2012 R2 o versioni precedenti |
|EDG/*| Microsoft Edge (cromo) per Windows Server 2016 o versione successiva|  
|MSIPC| Protezione delle informazioni di Microsoft e Client di controllo|
|Client Windows Rights Management|Client Windows Rights Management|


### <a name="additional-links"></a>Altri collegamenti

[Documentazione di Microsoft Edge](https://docs.microsoft.com/microsoft-edge/web-platform/user-agent-string)
