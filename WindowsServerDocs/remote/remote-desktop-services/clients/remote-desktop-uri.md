---
title: Schema URI client di Desktop remoto
description: Scopri la combinazione di Uniform Resource Identifier per i client Desktop remoto
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
ms.openlocfilehash: 86de6468e2fa45c976711aef43a1a274e04498d3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870502"
---
# <a name="remote-desktop-client-universal-resource-identifier-uri-scheme-support"></a>Supporto client di Desktop remoto schema Universal Resource Identifier (URI)

>Si applica a: Windows Server, versione 1803, Windows Server 2016, Windows Server 2012 R2

L'abilitazione di uno schema URI (Uniform Resource Identifier) consente ai professionisti e agli sviluppatori IT di integrare le funzionalità dei client Desktop remoto tra piattaforme e arricchisce l'esperienza utente consentendo: 

- Alle applicazioni di terze parti di avviare Desktop remoto Microsoft e iniziare sessioni remote con le impostazioni predefinite (fornite come parte della stringa URI).
- Agli utenti finali di avviare le connessioni remote usando URL preconfigurati.

>[!NOTE]
> Tramite un URI per la connessione al client desktop remoto non è supportata per i sistemi operativi Windows, usare l'URI con dispositivi Android, iOS e MacOS.

Desktop remoto Microsoft Usa il rdp://query_string lo schema URI per archiviare le impostazioni di attributi preconfigurati che vengono utilizzate durante l'avvio del client. Le stringhe di query rappresentano un singolo attributo o un set di attributi RDP forniti nell'URL. 

Gli attributi RDP sono separati dal simbolo e commerciale (&). Ad esempio, quando ci si connette a un PC, la stringa è:

```
rdp://full%20address=s:mypc:3389&audiomode=i:2&disable%20themes=i:1
```

Questa tabella contiene un elenco completo degli attributi supportati che possono essere usati con i client Desktop remoto su iOS, Mac, e Android. ("x" nella colonna piattaforma indica che l'attributo è supportato. I valori indicati da virgolette acute (<>) rappresentano i valori supportati dai client Desktop remoto).

| **Attributo RDP**                                           | **Android** | **Mac** | **iOS** |
|---------------------------------------------------------|---------|-----|-----|
| Consenti composizione del desktop = i:<0:&lt;0 o 1&gt;                    | x       | x   | x   |
| Allow font smoothing=i:<0 = i:<0: < 0 o 1&gt;                         | x       | x   | x   |
| alternare shell = s:&lt;stringa&gt;                              | x       | x   | x   |
| [audiomode=i:<0 = i:<0:&lt;0, 1 o 2&gt;](https://technet.microsoft.com/library/ff393707.aspx)                                | x       | x   | x   |
| [livello di autenticazione = i:<0:&lt;0 o 1&gt;](https://technet.microsoft.com/library/ff393709.aspx)                         | x       | x   | x   |
| connettersi alla console = i:<0:&lt;0 o 1&gt;                           | x       | x   | x   |
| disabilitare le impostazioni di cursore = i:<0:&lt;0 o 1&gt;                      | x       | x   | x   |
| Disable full window Drag=i:<0 = i:<0:&lt;0 o 1&gt;                     | x       | x   | x   |
| Disable menu anims=i:<0 = i:<0:&lt;0 o 1&gt;                           | x       | x   | x   |
| disabilitare i temi = i:<0:&lt;0 o 1&gt;                               | x       | x   | x   |
| disable wallpaper=i:<0 = i:<0:&lt;0 o 1&gt;                            | x       | x   | x   |
| [drivestoredirect=s:*](https://technet.microsoft.com/library/ff393728(v=ws.10).aspx) (unico valore supportato) | x       | x   |     |
| [desktopheight = i:<0:&lt;i:<valore in pixel&gt;](https://technet.microsoft.com/library/ff393702.aspx)                       |         | x   |     |
| [desktopwidth=i:&lt;value in pixels&gt;](https://technet.microsoft.com/library/ff393697.aspx)                        |         | x   |     |
| [domain=s:&lt;string&gt;](https://technet.microsoft.com/library/ff393673.aspx)                           | x | x | x |
| [indirizzo completo = s:&lt;stringa&gt;](https://technet.microsoft.com/library/ff393661.aspx)                     | x | x | x |
| gatewayhostname=s:&lt;string&gt;                  | x | x | x |
| [gatewayusagemethod=i:<1 = i:<0:&lt;1 o 2&gt;](https://msdn.microsoft.com/aa381329.aspx)               | x | x | x |
| [Richiedi credenziali sul client = i:<0:&lt;0 o 1&gt;](https://technet.microsoft.com/library/ff393660(v=ws.10).aspx) |   | x |   |
| [loadbalanceinfo=s:&lt;string&gt;](https://technet.microsoft.com/library/ff393684.aspx)                  | x | x | x |
| [redirectprinters=i:&lt;0 or 1&gt;](https://technet.microsoft.com/library/ff393671(v=ws.10).aspx)                 |   | x |   |
| remoteapplicationcmdline=s:&lt;string&gt;         | x | x | x |
| remoteapplicationmode=i:&lt;0 or 1&gt;            | x | x | x |
| remoteapplicationprogram=s:&lt;string&gt;         | x | x | x |
| directory di lavoro shell = s:&lt;stringa&gt;          | x | x | x |
| Nome del server usare il reindirizzamento = i:<0:&lt;0 o 1&gt;      | x | x | x |
| [username=s:&lt;string&gt;](https://technet.microsoft.com/library/ff393678.aspx)                         | x | x | x |
| [id modalità schermo = i:<0:&lt;1 o 2&gt;](https://technet.microsoft.com/library/ff393692.aspx)                   |   | x |   |
| [sessione bpp = i:<0:&lt;8, 15, 16, 24 o 32&gt;](https://technet.microsoft.com/library/ff393680.aspx)        |   | x |   |
| [Utilizzare multimon = i:<0:&lt;0 o 1&gt;](https://technet.microsoft.com/library/ff393695(v=ws.10).aspx)          |   | x |   |