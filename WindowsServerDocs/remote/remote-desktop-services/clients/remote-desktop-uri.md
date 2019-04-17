---
title: Schema URI client Desktop remoto
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
ms.openlocfilehash: f2934fed43c8f4feec2f321d684cc3593933eb5d
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297470"
---
# Supporto di client Desktop remoto schema Universal Resource Identifier (URI)

>Si applica a: Windows Server, versione 1803, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Abilitando una combinazione di Uniform Resource Identifier (URI) offre ai professionisti IT e agli sviluppatori un modo per integrare le funzionalità dei client Desktop remoto tra piattaforme e ciò consente di migliorare l'esperienza utente, consentendo: 

- Applicazioni di terze parti per avviare Desktop remoto Microsoft e avviare le sessioni remote con le impostazioni predefinite (fornite come parte della stringa URI).
- Utenti finali per avviare le connessioni remote con gli URL preconfigurati.

>[!NOTE]
> Con un URI per la connessione al client desktop remoto non è supportato per i sistemi operativi Windows: usare l'URI con dispositivi Android, iOS e MacOS.

Desktop remoto Microsoft Usa la rdp://query_string di schema URI per archiviare le impostazioni di attributo preconfigurato che vengono utilizzate durante l'avvio del client. Le stringhe di query rappresentano una singola o un set di attributi RDP fornito nell'URL. 

Gli attributi RDP sono separati dal simbolo di e commerciale (&). Ad esempio, quando ti Connetti a un PC, la stringa è:

```
rdp://full%20address=s:mypc:3389&audiomode=i:2&disable%20themes=i:1
```

Questa tabella fornisce un elenco completo degli attributi supportati che può essere usato con iOS, Mac e Android client Desktop remoto. (Una "x" nella colonna piattaforma indica che l'attributo è supportato. I valori caratterizzati da frecce (<>) rappresentano i valori supportati per i client Desktop remoto).

| **Attributo RDP**                                           | **Android** | **Mac** | **iOS** |
|---------------------------------------------------------|---------|-----|-----|
| Consenti composizione desktop = i:&lt;0 o 1&gt;                    | x       | x   | x   |
| Allow smussati = i:<0 o 1&gt;                         | x       | x   | x   |
| alternative shell = s:&lt;stringa&gt;                              | x       | x   | x   |
| [audiomode = i:&lt;0, 1 o 2&gt;](https://technet.microsoft.com/library/ff393707.aspx)                                | x       | x   | x   |
| [livello di autenticazione = i:&lt;0 o 1&gt;](https://technet.microsoft.com/library/ff393709.aspx)                         | x       | x   | x   |
| connettersi alla console = i:&lt;0 o 1&gt;                           | x       | x   | x   |
| disattivare le impostazioni del cursore = i:&lt;0 o 1&gt;                      | x       | x   | x   |
| disabilitare trascinamento dell'intera finestra = i:&lt;0 o 1&gt;                     | x       | x   | x   |
| disabilitare menu anims = i:&lt;0 o 1&gt;                           | x       | x   | x   |
| disabilitare i temi = i:&lt;0 o 1&gt;                               | x       | x   | x   |
| disattivare lo sfondo del = i:&lt;0 o 1&gt;                            | x       | x   | x   |
| [drivestoredirect = s: *](https://technet.microsoft.com/library/ff393728(v=ws.10).aspx) (questo è l'unico valore supportato) | x       | x   |     |
| [desktopheight = i:&lt;valore in pixel&gt;](https://technet.microsoft.com/library/ff393702.aspx)                       |         | x   |     |
| [desktopwidth = i:&lt;valore in pixel&gt;](https://technet.microsoft.com/library/ff393697.aspx)                        |         | x   |     |
| [dominio = s:&lt;stringa&gt;](https://technet.microsoft.com/library/ff393673.aspx)                           | x | x | x |
| [indirizzo completo = s:&lt;stringa&gt;](https://technet.microsoft.com/library/ff393661.aspx)                     | x | x | x |
| gatewayhostname = s:&lt;stringa&gt;                  | x | x | x |
| [gatewayusagemethod = i:&lt;1 o 2&gt;](https://msdn.microsoft.com/aa381329.aspx)               | x | x | x |
| [Richiedi conferma le credenziali nel client = i:&lt;0 o 1&gt;](https://technet.microsoft.com/library/ff393660(v=ws.10).aspx) |   | x |   |
| [loadbalanceinfo = s:&lt;stringa&gt;](https://technet.microsoft.com/library/ff393684.aspx)                  | x | x | x |
| [redirectprinters = i:&lt;0 o 1&gt;](https://technet.microsoft.com/library/ff393671(v=ws.10).aspx)                 |   | x |   |
| remoteapplicationcmdline = s:&lt;stringa&gt;         | x | x | x |
| remoteapplicationmode = i:&lt;0 o 1&gt;            | x | x | x |
| remoteapplicationprogram = s:&lt;stringa&gt;         | x | x | x |
| directory di lavoro shell = s:&lt;stringa&gt;          | x | x | x |
| Nome del server di utilizzare il reindirizzamento = i:&lt;0 o 1&gt;      | x | x | x |
| [nome utente = s:&lt;stringa&gt;](https://technet.microsoft.com/library/ff393678.aspx)                         | x | x | x |
| [id modalità schermo = i:&lt;1 o 2&gt;](https://technet.microsoft.com/library/ff393692.aspx)                   |   | x |   |
| [sessione bpp = i:&lt;8, 15, 16, 24 o 32&gt;](https://technet.microsoft.com/library/ff393680.aspx)        |   | x |   |
| [usare multimon = i:&lt;0 o 1&gt;](https://technet.microsoft.com/library/ff393695(v=ws.10).aspx)          |   | x |   |