---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: Ora di Windows
author: dcuomo
ms.author: dacuo
manager: dougkim
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 5dbb0db20f7100ed7dbe99587f201f38abf632ad
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80815904"
---
# <a name="windows-time-service-w32time"></a>Servizio Ora di Windows (W32Time)

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 o versioni successive

Il servizio Ora di Windows (W32Time) sincronizza la data e l'ora di tutti i computer in esecuzione in Active Directory Domain Services. La sincronizzazione dell'ora è essenziale per il corretto funzionamento di molti servizi e applicazioni line-of-business Windows. Il servizio Ora di Windows usa il protocollo NTP per sincronizzare i clock dei computer in una rete. Il protocollo NTP garantisce che un valore di clock accurato, o timestamp, possa essere assegnato alle richieste di convalida della rete e di accesso alle risorse.

Nell'argomento Servizio Ora di Windows (W32Time) è disponibile il contenuto seguente:
- **[Ora esatta di Windows Server 2016](accurate-time.md).** Accuratezza sincronizzazione ora in Windows Server 2016 è stata migliorata sostanzialmente, garantendo contemporaneamente all'indietro NTP la compatibilità con le versioni precedenti di Windows. In condizioni operative ragionevoli, puoi gestire un'accuratezza di 1 ms o superiore rispetto all'ora UTC per i membri del dominio Windows Server 2016 e Windows 10 aggiornamento dell'anniversario.
- **[Limiti del supporto per ambienti con accuratezza elevata](support-boundary.md).** Questo articolo descrive i limiti del supporto per il servizio Ora di Windows (W32Time) in ambienti che richiedono un'ora di sistema estremamente accurata e stabile.
- **[Configurazione dei sistemi per l'accuratezza elevata](configuring-systems-for-high-accuracy.md).** La sincronizzazione dell'ora in Windows 10 e Windows Server 2016 è stata migliorata in modo significativo.  In condizioni operative ragionevoli i sistemi possono essere configurati in modo da mantenere un'accuratezza di 1 ms (millisecondo) o superiore (rispetto all'ora UTC).
- **[Ora di Windows per la tracciabilità](windows-time-for-traceability.md).** Le normative di molti settori richiedono che i sistemi siano tracciabili per l'ora UTC.  Ciò significa che l'offset di un sistema può essere attestato rispetto all'ora UTC.  Per abilitare scenari di conformità alle normative, in Windows 10 e Windows Server 2016 sono disponibili nuovi registri eventi per fornire un'immagine dal punto di vista del sistema operativo che consenta di comprendere le azioni intraprese sul clock di sistema.  Questi registri eventi vengono generati in modo continuo per il servizio Ora di Windows e possono essere esaminati o archiviati per un'analisi successiva.
- **[Documentazione tecnica del servizio Ora di Windows](windows-time-service-tech-ref.md).** Il servizio W32Time fornisce la sincronizzazione del clock di rete per i computer senza la necessità di una configurazione complessa. Il servizio W32Time è essenziale per il corretto funzionamento dell'autenticazione Kerberos V5 e, pertanto, per l'autenticazione basata su Active Directory Domain Services.
    - **[Funzionamento del servizio Ora di Windows](How-the-Windows-Time-Service-Works.md).** Sebbene il servizio ora di Windows non è un'implementazione esatta del protocollo NTP (Network Time), Usa la suite di algoritmi complessa definiti nelle specifiche NTP per garantire che gli orologi dei computer in rete siano più accurati possibile.
    - **[Strumenti e impostazioni del servizio Ora di Windows](Windows-Time-Service-Tools-and-Settings.md).** La maggior parte dei computer membri del dominio hanno un tipo di client di tempo NT5DS, ovvero sincronizzano l'ora dalla gerarchia di dominio. L'unica eccezione tipica è il controller di dominio che funziona come master operazioni emulatore del controller di dominio primario (PDC) del dominio radice della foresta, che in genere è configurato per sincronizzare l'ora con un'origine ora esterna.


## <a name="related-topics"></a>Argomenti correlati
Per altre informazioni sulla gerarchia di domini e sul sistema di punteggio, vedi il post di blog ["What is Windows Time Service?"](https://blogs.msdn.microsoft.com/w32time/2007/07/07/what-is-windows-time-service/) (Che cos'è il servizio Ora di Windows?) .

È il modello di plug-in di windows time provider [documentata in TechNet](https://msdn.microsoft.com/library/windows/desktop/ms725475%28v=vs.85%29.aspx).

Puoi scaricare un supplemento a cui viene fatto riferimento nell'articolo relativo all'ora esatta di Windows 2016 [qui](https://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf).

Per una rapida panoramica del servizio Ora di Windows, guarda questo [video di panoramica generale](https://aka.ms/WS2016TimeVideo).
