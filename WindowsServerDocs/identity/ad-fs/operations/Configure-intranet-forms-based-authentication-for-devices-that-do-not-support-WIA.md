---
ms.assetid: d562ef46-f240-48be-bbd4-fd88fc6bbbdc
title: Configurazione autenticazione intranet basata su form per i dispositivi che non supportano WIA
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c78dc702cbff8eab487c6c077dfe57b0f0663342
ms.sourcegitcommit: 5012c078b410f59261edf0cc917393467243a5c7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/23/2018
---
# <a name="configuring-intranet-forms-based-authentication-for-devices-that-do-not-support-wia"></a>Configurazione autenticazione intranet basata su form per i dispositivi che non supportano WIA

>Si applica a: Windows Server 2016, Windows Server 2012 R2

Per impostazione predefinita, l'autenticazione integrata Windows (WIA) è abilitata in Active Directory Federation Services (ADFS) in Windows Server 2012 R2 per le richieste di autenticazione che si verificano nella rete interna dell'organizzazione (intranet) per qualsiasi applicazione che utilizza un browser per l'autenticazione. Può trattarsi, ad esempio, applicazioni basate su browser che utilizzano WS-Federation o SAML protocolli e rich applicazioni che utilizzano il protocollo OAuth. WIA fornisce agli utenti finali con accesso trasparente alle applicazioni senza dover immettere manualmente le proprie credenziali. Tuttavia, alcuni dispositivi e browser non sono in grado di supportare WIA e di conseguenza non le richieste di autenticazione da tali dispositivi. Inoltre, l'esperienza in alcuni browser che negoziano NTLM non è auspicabile. L'approccio consigliato è di fallback per l'autenticazione basata su form per tali dispositivi e browser.

ADFS in Windows Server 2012 R2 e Windows Server 2016 fornisce agli amministratori la possibilità di configurare l'elenco degli agenti utente che supportano il fallback all'autenticazione basata su form. Il fallback è reso possibile da due configurazioni:


- Il **WIASupportedUserAgentStrings** proprietà il `Set-ADFSProperties`cmdlet
- Il **WindowsIntegratedFallbackEnabled** proprietà il `Set-AdfsGlobalAuthenticationPolicy`commmandlet

Il **WIASupportedUserAgentStrings** definisce gli agenti utente che supportano WIA. ADFS consente di analizzare la stringa agente utente durante l'esecuzione di account di accesso in un browser o un controllo browser. Se il componente della stringa agente utente non corrisponde ad alcuno dei componenti delle stringhe agente utente configurate nel **WIASupportedUserAgentStrings** proprietà ADFS eseguirà il fallback a fornire l'autenticazione basata su form, a condizione che il **WindowsIntegratedFallbackEnabled** flag è impostato su True.

Per impostazione predefinita, una nuova installazione di AD FS include un set di corrispondenze di stringa agente utente creato. Tuttavia, le possono essere aggiornate in base alle modifiche apportate al browser e dispositivi. In particolare, i dispositivi Windows hanno stringhe agente utente simili con lievi variazioni nei token. Nell'esempio seguente di Windows PowerShell fornisce le migliori indicazioni per il set corrente di dispositivi sul mercato che supportano WIA trasparente:

    Set-AdfsProperties -WIASupportedUserAgents @("MSIE 6.0", "MSIE 7.0; Windows NT", "MSIE 8.0", "MSIE 9.0", "MSIE 10.0; Windows NT 6", "Windows NT 6.3; Trident/7.0", "Windows NT 6.3; Win64; x64; Trident/7.0", "Windows NT 6.3; WOW64; Trident/7.0", "Windows NT 6.2; Trident/7.0", "Windows NT 6.2; Win64; x64; Trident/7.0", "Windows NT 6.2; WOW64; Trident/7.0", "Windows NT 6.1; Trident/7.0", "Windows NT 6.1; Win64; x64; Trident/7.0", "Windows NT 6.1; WOW64; Trident/7.0", "MSIPC", "Windows Rights Management Client")

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
Windows NT 6.1; Trident/7.0</br></br>Windows NT 6.1; Win64; x64; Trident/7.0</br></br>Windows NT 6.1; WOW64; Trident/7.0|Sistema operativo desktop Windows 7, piattaforme diverse|
MSIPC| Protezione delle informazioni di Microsoft e Client di controllo|
Client Windows Rights Management|Client Windows Rights Management|

Per permettere il fallback all'autenticazione form di base per gli agenti utente diverse da quelle indicate nella stringa di WIASupportedUserAgents, impostare il flag WindowsIntegratedFallbackEnabled su true

    Set-AdfsGlobalAuthenticationPolicy -WindowsIntegratedFallbackEnabled $true

Assicurarsi inoltre che l'autenticazione basata su form è abilitata per la rete intranet.

## <a name="configuring-wia-for-chrome"></a>Configurazione WIA per Chrome
È possibile aggiungere altri agenti utente o Chrome per la configurazione di ADFS che supporta WIA. In questo modo l'accesso trasparente alle applicazioni senza dover immettere manualmente le credenziali quando si accede a risorse protette da AD FS. Attenersi alla procedura seguente per abilitare WIA in Chrome:

Aggiungere una stringa agente utente per Chrome nella configurazione di ADFS

    Set-AdfsProperties -WIASupportedUserAgents ((Get-ADFSProperties | Select -ExpandProperty WIASupportedUserAgents) + “Chrome”)
    
Verificare che la stringa agente utente per Chrome è ora impostata nelle proprietà di ADFS

    Get-AdfsProperties | Select -ExpandProperty WIASupportedUserAgents

![Configurare l'autenticazione](media/Configure-intranet-forms-based-authentication-for-devices-that-do-not-support-WIA/chrome1.png) 

>[!NOTE]   
> Non vengono rilasciati nuovi browser e dispositivi, è consigliabile che le funzionalità di tali agenti utente di riconciliare e aggiornare di conseguenza la configurazione di ADFS per ottimizzare l'esperienza di autenticazione dell'utente quando si utilizzando detto browser e dispositivi. Più specificamente, è consigliabile valutare nuovamente la **WIASupportedUserAgents** impostazione in ADFS, quando si aggiunge un nuovo tipo di dispositivo o browser alla matrice di supporto per WIA.


