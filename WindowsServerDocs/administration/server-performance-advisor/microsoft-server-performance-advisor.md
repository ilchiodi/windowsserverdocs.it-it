---
title: Microsoft Server Performance Advisor
description: Microsoft Server Performance Advisor
ms.assetid: 468ebcb3-9eaf-477c-ab10-e3f1b3ce63dc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: manage
ms.openlocfilehash: ab124f3efabf2ac3ae8904157a81587c0c21ebb5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856332"
---
# <a name="microsoft-server-performance-advisor"></a>Microsoft Server Performance Advisor

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Download di Microsoft Server Performance Advisor (SPA) che consentono di diagnosticare i problemi di prestazioni in un Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o distribuzione di Windows Server 2008. SPA genera i grafici e report di diagnostica completi e fornisce indicazioni che consentono rapidamente analizzano i problemi e lo sviluppo di azioni correttive.

-   [Panoramica di Server Performance Advisor](#bkmk-aboutspa)

-   [Download di Server Performance Advisor](#bkmk-downloadspa)

-   [Manuale dell'utente di Server Performance Advisor](server-performance-advisor-users-guide.md)

-   [Guida di sviluppo Server Performance Advisor Pack](server-performance-advisor-pack-development-guide.md)

## <a href="" id="bkmk-aboutspa"></a>Panoramica di Server Performance Advisor

Server Performance Advisor è costituito da due parti, il framework SPA e i pacchetti di Advisor SPA.

### <a name="the-server-performance-advisor-framework"></a>Il framework Server Performance Advisor

Il motore che è responsabile per la raccolta dei dati che sono designati per i pacchetti di advisor, scrivere i dati raccolti in un database Microsoft SQL Server, creazione di un ambiente facile per eseguire gli script per i pacchetti di Advisor SPA e che mostra il report finale. È sufficiente installare il framework SPA nella console SPA. La console SPA può essere installata sia in un computer autonomo per accedere ai server sottoposta a test in modalità remota o essere installato in un server sottoposta a test.

### <a name="server-performance-advisor-packs"></a>Pacchetti di Server Performance Advisor

Pacchetti di Advisor SPA costituiscono il fulcro di tutte le regole di ottimizzazione, costituiti da una serie di metadati e i file di script SQL. Sono disponibili i seguenti pacchetti di Advisor SPA:

-   Il pacchetto di preparazione del sistema operativo Core consente di analizzare le prestazioni delle funzioni generali del sistema operativo, indipendente dai ruoli di server specializzate.

-   Il pacchetto di advisor Internet Information Server (IIS) tiene traccia di IIS.

-   Il pacchetto di Advisor Hyper-V consente di analizzare le prestazioni generali del ruolo del server Hyper-V.

    **Nota** il pacchetto di Advisor Hyper-V non analizza i sistemi operativi guest.

     

-   Il pacchetto di advisor active directory consente di analizzare le prestazioni generali del ruolo di active directory.

SPA offre inoltre un modello estendibile per sviluppatori non Microsoft di scrivere pacchetti di advisor in base alle loro esigenze.

**Nota** SPA non è possibile comprendere tutti i contesti di uno scenario di hardware e l'utente. È consigliabile usare i consigli forniti dallo strumento per prendere decisioni e comprendere le conseguenze di tutte le modifiche potenziali apportate ai server.

 

## <a href="" id="bkmk-downloadspa"></a>Download di Server Performance Advisor


Usare i collegamenti seguenti per scaricare Server Performance Advisor per Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008:

-   [Microsoft Server Performance Advisor 3.1 (32 bit)](https://go.microsoft.com/fwlink/p/?linkid=327751)

-   [Microsoft Server Performance Advisor 3.1 (64 bit)](https://go.microsoft.com/fwlink/p/?linkid=327752)

È possibile estrarre i file nel file CAB utilizzando i comandi seguenti:

-   per x86 versione: `extrac32.exe /e /a /l  d:\SPA   d:\SPA\SPAPlus\_x86.cab`

-   per x64 versione: `extrac32.exe /e /a /l  d:\SPA   d:\SPA\SPAPlus\_amd64.cab`

**Attenzione** quando si estrae il file con estensione cab, SPA deve mantenere la struttura di directory gerarchico per funzionare correttamente. A seconda degli strumenti CAB installati nel server, estrazione può comportare una struttura di directory non operativo. Per mantenere la struttura di directory gerarchici, è possibile usare uno strumento di utilità estrazione di file CAB che estrae una struttura di directory di file.

Se lo strumento di estrazione CAB correttamente estratti i file, verranno visualizzati automaticamente nelle sottocartelle nella cartella di destinazione di estrazione.

### <a name="spa-prerequisites"></a>Prerequisiti SPA

La Console SPA è necessario che sia installato il software seguente:

-   Microsoft .NET Framework 4

-   SQL Server 2008 R2 Express SP1 o Microsoft SQL Server 2008 R2 SP1

Le versioni più recenti potrebbero essere compatibili. Eventuali incompatibilità note del prodotto con la Console SPA è indicato.
