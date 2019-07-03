---
title: Schema URI client di Desktop remoto
description: Informazioni sullo schema Uniform Resource Identifier per client Desktop remoto Microsoft
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0c3f1eb6-835c-4522-99ff-56c6ee4bb911
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 06/11/2018
ms.localizationpriority: medium
ms.openlocfilehash: f2934fed43c8f4feec2f321d684cc3593933eb5d
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/17/2019
ms.locfileid: "63743858"
---
# <a name="remote-desktop-client-universal-resource-identifier-uri-scheme-support"></a>Supporto client di Desktop remoto schema Universal Resource Identifier (URI)

>Si applica a: Windows Server versione 1803, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

L'abilitazione di uno schema URI (Uniform Resource Identifier) consente ai professionisti e agli sviluppatori IT di integrare le funzionalità dei client Desktop remoto tra piattaforme e arricchisce l'esperienza utente consentendo: 

- Alle applicazioni di terze parti di avviare Desktop remoto Microsoft e iniziare sessioni remote con le impostazioni predefinite (fornite come parte della stringa URI).
- Agli utenti finali di avviare le connessioni remote usando URL preconfigurati.

>[!NOTE]
> L'uso di un URI per la connessione al client RD NON è supportato nei sistemi operativi Windows; l'URI può essere usato con dispositivi MacOS, iOS, e Android.

Desktop remoto Microsoft Usa il rdp://query_string lo schema URI per archiviare le impostazioni di attributi preconfigurati che vengono utilizzate durante l'avvio del client. Le stringhe di query rappresentano un singolo attributo o un set di attributi RDP forniti nell'URL. 

Gli attributi RDP sono separati dal simbolo e commerciale (&). Ad esempio, quando ci si connette a un PC, la stringa è:

```
rdp://full%20address=s:mypc:3389&audiomode=i:2&disable%20themes=i:1
```

Questa tabella contiene un elenco completo degli attributi supportati che possono essere usati con i client Desktop remoto su iOS, Mac, e Android. ("x" nella colonna piattaforma indica che l'attributo è supportato. I valori indicati da virgolette acute (<>) rappresentano i valori supportati dai client Desktop remoto).

| **Attributo RDP**                                           | **Android** | **Mac** | **iOS** |
|---------------------------------------------------------|---------|-----|-----|
| allow desktop composition=i:&lt;0 o 1&gt;                    | x       | x   | x   |
| allow font smoothing=i:<0 o 1&gt;                         | x       | x   | x   |
| alternate shell=s:&lt;string&gt;                              | x       | x   | x   |
| [audiomode=i:&lt;0, 1 o 2&gt;](https://technet.microsoft.com/library/ff393707.aspx)                                | x       | x   | x   |
| [authentication level=i:&lt;0 o 1&gt;](https://technet.microsoft.com/library/ff393709.aspx)                         | x       | x   | x   |
| connect to console=i:&lt;0 o 1&gt;                           | x       | x   | x   |
| disable cursor settings=i:&lt;0 o 1&gt;                      | x       | x   | x   |
| disable full window drag=i:&lt;0 or 1&gt;                     | x       | x   | x   |
| disable menu anims=i:&lt;0 o 1&gt;                           | x       | x   | x   |
| disable themes=i:&lt;0 o 1&gt;                               | x       | x   | x   |
| disable wallpaper=i:&lt;0 o 1&gt;                            | x       | x   | x   |
| [drivestoredirect = s: *](https://technet.microsoft.com/library/ff393728(v=ws.10).aspx) (questo è l'unico valore supportato) | x       | x   |     |
| [desktopheight=i:&lt;valore in pixel&gt;](https://technet.microsoft.com/library/ff393702.aspx)                       |         | x   |     |
| [desktopwidth=i:&lt;valore in pixel&gt;](https://technet.microsoft.com/library/ff393697.aspx)                        |         | x   |     |
| [domain=s:&lt;stringa&gt;](https://technet.microsoft.com/library/ff393673.aspx)                           | x | x | x |
| [full address=s:&lt;stringa&gt;](https://technet.microsoft.com/library/ff393661.aspx)                     | x | x | x |
| gatewayhostname=s:&lt;stringa&gt;                  | x | x | x |
| [gatewayusagemethod=i:&lt;1 o 2&gt;](https://msdn.microsoft.com/aa381329.aspx)               | x | x | x |
| [prompt for credentials on client=i:&lt;0 o 1&gt;](https://technet.microsoft.com/library/ff393660(v=ws.10).aspx) |   | x |   |
| [loadbalanceinfo=s:&lt;stringa&gt;](https://technet.microsoft.com/library/ff393684.aspx)                  | x | x | x |
| [redirectprinters=i:&lt;0 o 1&gt;](https://technet.microsoft.com/library/ff393671(v=ws.10).aspx)                 |   | x |   |
| remoteapplicationcmdline=s:&lt;stringa&gt;         | x | x | x |
| remoteapplicationmode=i:&lt;0 o 1&gt;            | x | x | x |
| remoteapplicationprogram=s:&lt;stringa&gt;         | x | x | x |
| shell working directory=s:&lt;stringa&gt;          | x | x | x |
| Use redirection server name=i:&lt;0 o 1&gt;      | x | x | x |
| [username=s:&lt;stringa&gt;](https://technet.microsoft.com/library/ff393678.aspx)                         | x | x | x |
| [screen mode id=i:&lt;1 o 2&gt;](https://technet.microsoft.com/library/ff393692.aspx)                   |   | x |   |
| [session bpp=i:&lt;8, 15, 16, 24 o 32&gt;](https://technet.microsoft.com/library/ff393680.aspx)        |   | x |   |
| [use multimon=i:&lt;0 o 1&gt;](https://technet.microsoft.com/library/ff393695(v=ws.10).aspx)          |   | x |   |