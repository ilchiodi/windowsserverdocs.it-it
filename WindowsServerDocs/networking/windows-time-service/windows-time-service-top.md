---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: Ora di Windows
description: ''
author: shortpatti
ms.author: pashort
manager: dougkim
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: e3dbaa188426ac81073e706db3adc6ab0a655c01
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405189"
---
# <a name="windows-time-service-w32time"></a>Servizio ora di Windows (W32Time)

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 o versione successiva

Il servizio ora di Windows (W32Time) sincronizza la data e l'ora per tutti i computer in esecuzione in Active Directory Domain Services (AD DS). La sincronizzazione dell'ora è essenziale per il corretto funzionamento di molti servizi Windows e applicazioni line-of-business (LOB). Il servizio ora di Windows usa il protocollo NTP (Network Time Protocol) per sincronizzare gli orologi dei computer in rete. NTP garantisce che un valore di clock accurato, o timestamp, possa essere assegnato alle richieste di convalida della rete e di accesso alle risorse.

Nell'argomento servizio ora di Windows (W32Time) è disponibile il contenuto seguente:
- **[Ora esatta di Windows Server 2016](accurate-time.md).** Accuratezza sincronizzazione ora in Windows Server 2016 è stata migliorata sostanzialmente, garantendo contemporaneamente all'indietro NTP la compatibilità con le versioni precedenti di Windows. In condizioni operative ragionevoli è possibile mantenere un'accuratezza di 1 ms rispetto all'ora UTC o superiore per i membri del dominio Windows Server 2016 e aggiornamento dell'anniversario di Windows 10.
- **[Limite di supporto per ambienti ad alta precisione](support-boundary.md).** Questo articolo descrive i limiti di supporto per il servizio ora di Windows (W32Time) in ambienti che richiedono un'ora di sistema altamente accurata e stabile.
- **[Configurazione dei sistemi per un'accuratezza elevata](configuring-systems-for-high-accuracy.md).** La sincronizzazione dell'ora in Windows 10 e Windows Server 2016 è stata notevolmente migliorata.  In condizioni operative ragionevoli, i sistemi possono essere configurati in modo da mantenere l'accuratezza 1 ms (millisecondi) o superiore (rispetto all'ora UTC).
- **[Tempo di Windows per la tracciabilità](windows-time-for-traceability.md).** Le normative in molti settori richiedono che i sistemi siano tracciabili in formato UTC.  Ciò significa che l'offset di un sistema può essere attestato rispetto all'ora UTC.  Per abilitare gli scenari di conformità alle normative, Windows 10 e il server 2016 forniscono nuovi registri eventi per fornire un'immagine dal punto di vista del sistema operativo per comprendere le azioni intraprese sul clock di sistema.  Questi registri eventi vengono generati in modo continuo per il servizio ora di Windows e possono essere esaminati o archiviati per un'analisi successiva.
- **[Riferimento tecnico per il servizio ora di Windows](windows-time-service-tech-ref.md).** Il servizio W32Time fornisce la sincronizzazione dell'orologio di rete per i computer senza la necessità di una configurazione completa. Il servizio W32Time è essenziale per il corretto funzionamento dell'autenticazione Kerberos V5 e, pertanto, per l'autenticazione basata su servizi di dominio Active Directory.
    - **Funzionamento [del servizio ora di Windows](How-the-Windows-Time-Service-Works.md).** Sebbene il servizio ora di Windows non è un'implementazione esatta del protocollo NTP (Network Time), Usa la suite di algoritmi complessa definiti nelle specifiche NTP per garantire che gli orologi dei computer in rete siano più accurati possibile.
    - **[Strumenti e impostazioni del servizio ora di Windows](Windows-Time-Service-Tools-and-Settings.md).** La maggior parte dei computer membri del dominio hanno un tipo di client Time NT5DS, il che significa che sincronizzano l'ora dalla gerarchia di dominio. L'unica eccezione tipica è il controller di dominio che funziona come master operazioni dell'emulatore del controller di dominio primario (PDC) del dominio radice della foresta, che in genere è configurato per sincronizzare l'ora con un'origine ora esterna.


## <a name="related-topics"></a>Argomenti correlati
Per ulteriori informazioni sulla gerarchia del dominio e sul sistema di assegnazione dei punteggi, vedere "informazioni sul [servizio ora di Windows".](https://blogs.msdn.microsoft.com/w32time/2007/07/07/what-is-windows-time-service/) Post di Blog.

È il modello di plug-in di windows time provider [documentata in TechNet](https://msdn.microsoft.com/library/windows/desktop/ms725475%28v=vs.85%29.aspx).

Un supplemento a cui viene fatto riferimento nell'articolo relativo all'ora esatta di Windows 2016 può essere scaricato [qui](https://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf)

Per una rapida panoramica del servizio ora di Windows, vedere questo [video introduttivo di alto livello](https://aka.ms/WS2016TimeVideo).
