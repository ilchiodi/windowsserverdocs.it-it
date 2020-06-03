---
title: Schema URI di Desktop remoto
description: Informazioni sullo schema Uniform Resource Identifier per client Desktop remoto Microsoft
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: 0c3f1eb6-835c-4522-99ff-56c6ee4bb911
author: heidilohr
manager: lizross
ms.author: helohr
ms.date: 06/01/2020
ms.localizationpriority: medium
ms.openlocfilehash: aadb115c68108125abdaf980c12eac951d798bba
ms.sourcegitcommit: df94dac422d13566c32e1cdb8c6e7a4e82747947
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/29/2020
ms.locfileid: "84205612"
---
# <a name="remote-desktop-uri-scheme"></a>Schema URI di Desktop remoto

> Si applica a: Windows Server versione 1803, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Questo documento definisce il formato degli URI (Uniform Resource Identifier) per Desktop remoto. Questi schemi URI consentono di richiamare i client Desktop remoto con vari comandi.

## <a name="ms-rd-uri-scheme"></a>Schema URI ms-rd

>[!NOTE]
> Lo schema URI ms-rd è attualmente supportato solo con il client desktop di Windows.

L'URI ms-rd consente di specificare un comando per il client e un set di parametri specifici del comando usando il formato seguente:

```
ms-rd:command?parameters
```

Parameters usa il formato della stringa di query costituita da coppie chiave=valore separate da & per fornire informazioni aggiuntive sul comando specificato:

```
param1=value1&param2=value2&…
```

### <a name="commands-and-parameters"></a>Comandi e parametri

Ecco l'elenco dei comandi attualmente supportati e dei parametri corrispondenti.

L'uso di `ms-rd:` senza comandi avvia il client.

#### <a name="subscribe"></a>Subscribe

Questo comando avvia il client e il processo di sottoscrizione.

**Nome del comando:** subscribe

**Parametri del comando:**

| Parametro | Description                  | Valori |
|-----------|------------------------------|--------|
| url       | Specifica l'URL dell'area di lavoro. | Un URL valido, ad esempio <https://contoso.com>. |

**Esempio:** ms-rd:subscribe?url=https://contoso.com

## <a name="legacy-rdp-uri-scheme"></a>Schema URI rdp legacy

>[!NOTE]
> Lo schema URI seguente è supportato solo con i client per dispositivi macOS, iOS e Android. Verrà sostituito con il nuovo URI ms-rd precedente.

Desktop remoto Microsoft usa lo schema URI rdp://query_string per archiviare le impostazioni di attributi preconfigurati che vengono usate quando si avvia il client. Le stringhe di query rappresentano un singolo attributo o un set di attributi RDP forniti nell'URL.

Gli attributi RDP sono separati dal simbolo e commerciale (&). Ad esempio, quando ci si connette a un PC, la stringa è:

```
rdp://full%20address=s:mypc:3389&audiomode=i:2&disable%20themes=i:1
```

Questa tabella contiene un elenco completo degli attributi supportati che possono essere usati con i client Desktop remoto su iOS, Mac, e Android. Una "x" nella colonna relativa alla piattaforma indica che l'attributo è supportato. I valori indicati da virgolette acute (<>) rappresentano i valori supportati dai client Desktop remoto).

| Attributo RDP                                           | Android | Mac | iOS |
|---------------------------------------------------------|---------|-----|-----|
| allow desktop composition=i:&lt;0 o 1&gt;              | x       | x   | x   |
| allow font smoothing=i:<0 o 1&gt;                      | x       | x   | x   |
| alternate shell=s:&lt;string&gt;                        | x       | x   | x   |
| [audiomode=i:&lt;0, 1 o 2&gt;](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff393707(v=ws.10)) | x       | x   | x   |
| [authentication level=i:&lt;0 o 1&gt;](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff393709(v=ws.10)) | x       | x   | x   |
| connect to console=i:&lt;0 o 1&gt;                     | x       | x   | x   |
| disable cursor settings=i:&lt;0 o 1&gt;                | x       | x   | x   |
| disable full window drag=i:&lt;0 or 1&gt;               | x       | x   | x   |
| disable menu anims=i:&lt;0 o 1&gt;                     | x       | x   | x   |
| disable themes=i:&lt;0 o 1&gt;                         | x       | x   | x   |
| disable wallpaper=i:&lt;0 o 1&gt;                      | x       | x   | x   |
| [drivestoredirect = s: *](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff393728(v=ws.10)) (questo è l'unico valore supportato) | x       | x   |     |
| [desktopheight=i:&lt;valore in pixel&gt;](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff393702(v=ws.10)) |         | x   |     |
| [desktopwidth=i:&lt;valore in pixel&gt;](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff393697(v=ws.10))  |         | x   |     |
| [domain=s:&lt;stringa&gt;](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff393673(v=ws.10))                 | x | x | x |
| [full address=s:&lt;stringa&gt;](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff393661(v=ws.10))           | x | x | x |
| gatewayhostname=s:&lt;stringa&gt;                  | x | x | x |
| [gatewayusagemethod=i:&lt;1 o 2&gt;](https://docs.microsoft.com/windows/win32/termserv/imsrdpclienttransportsettings-gatewayusagemethod)                | x | x | x |
| [prompt for credentials on client=i:&lt;0 o 1&gt;](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff393660(v=ws.10)) |   | x |   |
| [loadbalanceinfo=s:&lt;stringa&gt;](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff393684(v=ws.10))                  | x | x | x |
| [redirectprinters=i:&lt;0 o 1&gt;](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff393671(v=ws.10))                 |   | x |   |
| remoteapplicationcmdline=s:&lt;stringa&gt;         | x | x | x |
| remoteapplicationmode=i:&lt;0 o 1&gt;            | x | x | x |
| remoteapplicationprogram=s:&lt;stringa&gt;         | x | x | x |
| shell working directory=s:&lt;stringa&gt;          | x | x | x |
| Use redirection server name=i:&lt;0 o 1&gt;      | x | x | x |
| [username=s:&lt;stringa&gt;](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff393678(v=ws.10))                  | x | x | x |
| [screen mode id=i:&lt;1 o 2&gt;](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff393692(v=ws.10))            |   | x |   |
| [session bpp=i:&lt;8, 15, 16, 24 o 32&gt;](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff393680(v=ws.10)) |   | x |   |
| [use multimon=i:&lt;0 o 1&gt;](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff393695(v=ws.10))              |   | x |   |
