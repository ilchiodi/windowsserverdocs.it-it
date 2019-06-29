---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: Ora di Windows
description: ''
author: shortpatti
ms.author: pashort
manager: dougkim
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 3233434403594ef9e2555c0329c4791d1fb99709
ms.sourcegitcommit: 63926404009f9e1330a4a0aa8cb9821a2dd7187e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/29/2019
ms.locfileid: "67469593"
---
# <a name="windows-time-service-w32time"></a>Windows Time Service (W32Time)

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 o versione successiva

Il servizio ora di Windows (W32Time) consente di sincronizzare la data e ora per tutti i computer in esecuzione in servizi di dominio Active Directory (AD DS). Sincronizzazione dell'ora è fondamentale per il corretto funzionamento di molti servizi di Windows e le applicazioni line-of-business (LOB). Il servizio ora di Windows Usa il protocollo NTP (Network Time) per sincronizzare gli orologi dei computer nella rete. NTP assicura che un valore di orologio accurato o timestamp, può essere assegnato alle risorse e convalida le richieste di accesso di rete.

Nell'argomento del servizio ora di Windows (W32Time) è disponibile il contenuto seguente:
- **[Ora esatta di Windows Server 2016](accurate-time.md).** Accuratezza sincronizzazione ora in Windows Server 2016 è stata migliorata sostanzialmente, garantendo contemporaneamente all'indietro NTP la compatibilità con le versioni precedenti di Windows. In condizioni operative ragionevole è possibile gestire un 1 ms accuratezza rispetto a UTC o migliore per i membri del dominio Windows Server 2016 e Windows 10 Anniversary Update.
- **[Limiti di supporto per gli ambienti ad alta precisione](support-boundary.md).** Questo articolo descrive i limiti del supporto per il servizio ora di Windows (W32Time) in ambienti che richiedono ora di sistema estremamente accurate e stabile.
- **[Configurazione dei sistemi per verificarne l'accuratezza elevata](configuring-systems-for-high-accuracy.md).** Sincronizzazione dell'ora di Windows 10 e Windows Server 2016 è stata notevolmente migliorata.  In condizioni operative ragionevole, sistemi possono essere configurati per gestire (in millisecondi) pari a 1 ms accuratezza o versione successiva (rispetto all'ora UTC).
- **[Ora di Windows per la tracciabilità](windows-time-for-traceability.md).** Le normative in molti settori richiedono i sistemi per essere riconducibile all'ora UTC.  Ciò significa che possono essere attestati offset di un sistema rispetto a UTC.  Per abilitare scenari di conformità alle normative, Windows 10 e Server 2016 fornisce nuovi registri eventi per fornire un'immagine dal punto di vista del sistema operativo in modo da formare una conoscenza delle azioni eseguite nel clock di sistema.  Questi registri eventi vengono generati in modo continuativo per il servizio ora di Windows e può essere esaminati o archiviati per un'analisi successiva.
- **[Riferimento tecnico per Windows Time service](windows-time-service-tech-ref.md).** Il servizio W32Time fornisce la sincronizzazione degli orologi dei computer senza la necessità di una configurazione complessa. Il servizio W32Time è essenziale per il corretto funzionamento dell'autenticazione Kerberos V5 e, pertanto, per l'autenticazione basata su dominio Active Directory AD.
    - **[Come funziona il servizio Windows ora](How-the-Windows-Time-Service-Works.md).** Sebbene il servizio ora di Windows non è un'implementazione esatta del protocollo NTP (Network Time), Usa la suite di algoritmi complessa definiti nelle specifiche NTP per garantire che gli orologi dei computer in rete siano più accurati possibile.
    - **[Impostazioni e strumenti di Windows ora servizio](Windows-Time-Service-Tools-and-Settings.md).** La maggior parte dei computer membri del dominio ha un tipo di client ora di NT5DS, il che significa che cui sincronizzare l'ora dalla gerarchia di dominio. L'eccezione solo tipica è il controller di dominio che funziona come il master operazioni di dominio primario (PDC) controller emulatore del dominio radice della foresta, che è in genere configurato per sincronizzare l'ora con un'origine ora esterna.


## <a name="related-topics"></a>Argomenti correlati
Per altre informazioni sulla gerarchia di dominio e sistema di punteggio, vedere il ["Che cos'è servizio ora di Windows?"](https://blogs.msdn.microsoft.com/w32time/2007/07/07/what-is-windows-time-service/) post di blog.

È il modello di plug-in di windows time provider [documentata in TechNet](https://msdn.microsoft.com/library/windows/desktop/ms725475%28v=vs.85%29.aspx).

Può essere scaricato un supplemento a cui fa riferimento l'articolo ora accurata di Windows 2016 [qui](https://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf)

Per una rapida panoramica del servizio ora di Windows, esaminiamo questa [Panoramica di alto livello video](https://aka.ms/WS2016TimeVideo).
