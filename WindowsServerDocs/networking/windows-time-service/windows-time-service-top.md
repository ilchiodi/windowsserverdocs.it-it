---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: Servizio ora di Windows
description: ''
author: shortpatti
ms.author: pashort
manager: brianlic
ms.date: 02/01/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b997d1f26e8da82e0d595b1ce13763e0a87d6d03
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="windows-time-service-w32time"></a>Servizio ora di Windows (W32Time)

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Il servizio ora di Windows (W32Time) consente di sincronizzare la data e ora per tutti i computer in servizi di dominio Active Directory (AD DS). Sincronizzazione dell'ora è fondamentale per il corretto funzionamento di molti servizi di Windows e applicazioni line-of-business (LOB). Il servizio ora di Windows utilizza il protocollo NTP (Network Time) per sincronizzare gli orologi dei computer nella rete. NTP garantisce che è possibile assegnare un valore di orologio accurato, o timestamp, per le richieste di accesso di convalida e risorse di rete.

Nell'argomento, il servizio ora di Windows (W32Time) è disponibile il contenuto seguente:
- **[Ora esatta Windows 2016](accurate-time.md).** Accuratezza sincronizzazione ora in Windows Server 2016 è stata migliorata sostanzialmente, mantenendo all'indietro NTP la compatibilità con le versioni precedenti di Windows.  In condizioni operative ragionevole è possibile gestire un 1 ms accuratezza rispetto a UTC o migliore per i membri del dominio Windows Server 2016 e Windows 10 Anniversary Update.
- **[Riferimento tecnico del servizio ora di Windows ](windows-time-service-tech-ref.md).** Il servizio W32Time fornisce la sincronizzazione dell'orologio di rete per i computer senza la necessità di configurazione completa. Il servizio W32Time è essenziale per il corretto funzionamento dell'autenticazione Kerberos V5 e, pertanto, per l'autenticazione basata su directory Active Directory.
    - **[Come funziona il servizio ora di Windows ](How-the-Windows-Time-Service-Works.md).** Anche se il servizio ora di Windows non è un'implementazione esatta del protocollo NTP (Network Time), Usa la suite di algoritmi complessa definiti nelle specifiche NTP per garantire che gli orologi dei computer in una rete siano più accurati possibile.
    - **[Impostazioni e strumenti servizio ora di Windows ](Windows-Time-Service-Tools-and-Settings.md).** La maggior parte dei computer membri del dominio hanno un tipo di client in fase di NT5DS, il che significa che cui sincronizzare l'ora dalla gerarchia di dominio. L'eccezione solo tipico è il controller di dominio che funziona come master operazioni dell'emulatore dominio primario (PDC) controller di dominio radice della foresta, in genere è configurato per sincronizzare l'ora con un'origine ora esterna.

## <a name="related-topics"></a>Argomenti correlati
Per ulteriori informazioni sulla gerarchia di dominio e sistema di punteggio, vedere il ["Qual è il servizio ora di Windows?"](https://blogs.msdn.microsoft.com/w32time/2007/07/07/what-is-windows-time-service/) Post di blog.

Il modello di plug-in di windows ora provider [documentata in TechNet](https://msdn.microsoft.com/en-us/library/windows/desktop/ms725475%28v=vs.85%29.aspx).

Può essere scaricato un addendum fa riferimento l'articolo ora di Windows 2016 accurata [qui](http://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf)

Per una rapida panoramica del servizio ora di Windows, da un'occhiata questo [panoramica video ](https://aka.ms/WS2016TimeVideo).

<!-- In this guide
In this guide:
Windows Accurate Time
High Accuracy
Support Boundary
Configuration for High Accuracy
Traceability for Compliance
Best Practices
Technical Reference
How the Windows Time Service Works
Windows Time Service Tools and Settings
-->

