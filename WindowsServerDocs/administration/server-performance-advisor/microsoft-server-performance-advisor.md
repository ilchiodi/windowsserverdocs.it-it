---
title: Microsoft Server Performance Advisor
description: Microsoft Server Performance Advisor
ms.assetid: 468ebcb3-9eaf-477c-ab10-e3f1b3ce63dc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.topic: article
ms.prod: windows-server
ms.technology: manage
ms.openlocfilehash: 49f6132cfe99d9d4b719aeeecf149ecb1d7b76f2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382990"
---
# <a name="microsoft-server-performance-advisor"></a>Microsoft Server Performance Advisor

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Scaricare Microsoft Server Performance Advisor (SPA) per facilitare la diagnosi dei problemi di prestazioni in una distribuzione di Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008. SPA genera report e grafici di diagnostica completi e fornisce indicazioni utili per analizzare rapidamente i problemi e sviluppare azioni correttive.

-   [Panoramica di Server Performance Advisor](#bkmk-aboutspa)

-   [Scarica Server Performance Advisor](#bkmk-downloadspa)

-   [Manuale dell'utente di Server Performance Advisor](server-performance-advisor-users-guide.md)

-   [Guida di sviluppo Server Performance Advisor Pack](server-performance-advisor-pack-development-guide.md)

## <a href="" id="bkmk-aboutspa"></a>Panoramica di Server Performance Advisor

Server Performance Advisor è costituito da due parti, il Framework di SPA e i pacchetti di Advisor SPA.

### <a name="the-server-performance-advisor-framework"></a>Framework Server Performance Advisor

Il motore responsabile della raccolta dei dati designati dai pacchetti di Advisor, della scrittura dei dati raccolti in un database di Microsoft SQL Server, della creazione di un ambiente intuitivo per l'esecuzione di script per pacchetti di Advisor SPA e la visualizzazione dei report finali. È sufficiente installare il Framework SPA nella console SPA. La console SPA può essere installata in un computer autonomo per accedere in remoto ai server sottoposti a test o essere installata in un server sottoposto a test.

### <a name="server-performance-advisor-packs"></a>Pacchetti di Server Performance Advisor

I pacchetti di Advisor SPA sono al centro di tutte le regole di ottimizzazione, costituite da una serie di metadati e file script SQL. SPA viene fornito con i seguenti Pack di Advisor:

-   Il pacchetto del sistema operativo di base analizza le prestazioni delle funzioni generali del sistema operativo, indipendentemente dai ruoli server specializzati.

-   Il pacchetto di Advisor di Internet Information Server (IIS) tiene traccia delle prestazioni di IIS.

-   Hyper-V Advisor Pack analizza le prestazioni generali del ruolo del server Hyper-V.

    **Nota** Hyper-V Advisor Pack non analizza i sistemi operativi guest.

     

-   Active Directory Advisor Pack analizza le prestazioni generali del ruolo Active Directory.

SPA offre inoltre un modello estendibile per gli sviluppatori non Microsoft per la scrittura di pacchetti di Advisor in base alle esigenze.

**Nota** SPA non è in grado di comprendere tutti i contesti di scenari hardware e utente. È consigliabile utilizzare le raccomandazioni fornite dallo strumento che consentono di prendere decisioni e comprendere le conseguenze di eventuali modifiche apportate ai server.

 

## <a href="" id="bkmk-downloadspa"></a>Scarica Server Performance Advisor


Usare i collegamenti seguenti per scaricare Server Performance Advisor per Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008:

-   [Microsoft Server Performance Advisor 3,1 (32 bit)](https://go.microsoft.com/fwlink/p/?linkid=327751)

-   [Microsoft Server Performance Advisor 3,1 (64 bit)](https://go.microsoft.com/fwlink/p/?linkid=327752)

È possibile estrarre i file nel file CAB usando i comandi seguenti:

-   per la versione x86: `extrac32.exe /e /a /l  d:\SPA   d:\SPA\SPAPlus\_x86.cab`

-   per la versione x64: `extrac32.exe /e /a /l  d:\SPA   d:\SPA\SPAPlus\_amd64.cab`

**Attenzione** Quando si estrae il file con estensione cab, SPA deve mantenere la struttura di directory gerarchica per funzionare correttamente. A seconda degli strumenti CAB installati nel server, l'estrazione può produrre una struttura di directory non operativa. Per mantenere la struttura di directory gerarchica, è possibile utilizzare uno strumento di utilità di estrazione CAB che estrae una struttura di directory di file.

Se lo strumento di estrazione CAB ha estratto correttamente i file, le sottocartelle verranno visualizzate automaticamente nella cartella destinazione estrazione.

### <a name="spa-prerequisites"></a>Prerequisiti SPA

Per la console SPA è necessario che sia installato il seguente software:

-   Microsoft .NET Framework 4

-   SQL Server 2008 R2 Express SP1 o Microsoft SQL Server 2008 R2 SP1

Le versioni più recenti possono essere compatibili. Eventuali incompatibilità dei prodotti noti con la console SPA saranno indicate.
