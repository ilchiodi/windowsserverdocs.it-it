---
title: Configurare i browser per usare l'autenticazione integrata Windows (WIA) con AD FS
description: Questo documento descrive come configurare i browser per usare WIA con AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f71680bb721635bd37197dca9d3ae4726099525f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845482"
---
# <a name="configure-browsers-to-use-windows-integrated-authentication-wia-with-ad-fs"></a>Configurare i browser per usare l'autenticazione integrata Windows (WIA) con AD FS

Per impostazione predefinita, l'autenticazione integrata Windows (WIA) sia attivato in Active Directory Federation Services (ADFS) in Windows Server 2012 R2 per le richieste di autenticazione che si verificano nella rete interna dell'organizzazione (intranet) per qualsiasi applicazione che utilizza un browser per l'autenticazione.

AD FS 2016 ora dispone di un'impostazione predefinite migliorate che consente il browser Microsoft Edge eseguire WIA mentre Windows Phone non anche (in modo errato) rilevamento anche:

    =~Windows\s*NT.*Edge

Quanto descritto sopra indica che non è più necessario configurare le stringhe di agente utente singoli per supportare scenari comuni di Edge, anche se vengono aggiornati molto spesso.

Per altri browser, configurare le proprietà di AD FS **WiaSupportedUserAgents** per aggiungere i valori necessari in base i browser in uso.  È possibile usare le procedure seguenti.



### <a name="view-wiasupporteduseragent-settings"></a>Visualizzare le impostazioni WIASupportedUserAgent
Il **WIASupportedUserAgents** definisce gli agenti utente che supportano WIA. ADFS consente di analizzare la stringa agente utente durante l'esecuzione di account di accesso in un browser o un controllo browser.

È possibile visualizzare le impostazioni correnti usando l'esempio di PowerShell seguente:

```powershell
    $strings = Get-AdfsProperties | select -ExpandProperty WiaSupportedUserAgents
```

![Supporto per WIA](../operations/media/Configure-AD-FS-Browser-WIA/wiasupport.png)

### <a name="change-wiasupporteduseragent-settings"></a>Modificare le impostazioni di WIASupportedUserAgent
Per impostazione predefinita, una nuova installazione di ADFS è un insieme di corrispondenze di stringa agente utente creato. Tuttavia, le possono essere aggiornate in base alle modifiche apportate al browser e dispositivi. In particolare, i dispositivi Windows hanno stringhe agente utente simili con lievi variazioni nei token. Nell'esempio seguente di Windows PowerShell fornisce le migliori indicazioni per il set corrente di dispositivi presenti sul mercato che supportano WIA trasparente:

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
MSIE 10.0; Windows NT 6|Internet Explorer 10.0 per Windows XP e versioni più recenti del sistema operativo desktop</br></br>I dispositivi Windows Phone 8.0 (con preferenza impostata al cellulare) sono esclusi perché inviano</br></br>Agente utente: Mozilla/5.0 (compatibile; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Tocco. NOKIA; Lumia 920)|
Windows NT 6.3; Trident/7.0</br></br>Windows NT 6.3; Win64; x64; Trident/7.0</br></br>Windows NT 6.3; WOW64; Trident/7.0| Sistema operativo desktop di Windows 8.1, piattaforme diverse|
Windows NT 6.2; Trident/7.0</br></br>Windows NT 6.2; Win64; x64; Trident/7.0</br></br>Windows NT 6.2; WOW64; Trident/7.0|Sistema operativo desktop Windows 8, piattaforme diverse|
Windows NT 6.1; Trident/7.0</br></br>Windows NT 6.1; Win64; x64; Trident/7.0</br></br>Windows NT 6.1; WOW64; Trident/7.0|Sistema operativo desktop Windows 7, piattaforme diverse|
MSIPC| Protezione delle informazioni di Microsoft e Client di controllo|
Client Windows Rights Management|Client Windows Rights Management|
