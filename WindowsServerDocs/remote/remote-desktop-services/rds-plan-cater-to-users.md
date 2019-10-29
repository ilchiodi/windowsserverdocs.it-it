---
title: 'Servizi Desktop remoto: soddisfare le esigenze di diversi tipi di utenti'
description: Descrive i diversi tipi di utenti di Servizi Desktop remoto.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: da522a18-c33f-468e-b9d6-3ad7d3cfba26
author: spatnaik
ms.author: spatnaik
ms.date: 09/23/2016
manager: scottman
ms.openlocfilehash: c04909e9e0cfbf71b6632c154ac8b9b20b5bac10
ms.sourcegitcommit: b4b0e73ce35f8b594eb467a2bb2aa91bd6d86369
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/24/2019
ms.locfileid: "72893082"
---
# <a name="remote-desktop-services---cater-to-different-kinds-of-users"></a>Servizi Desktop remoto: soddisfare le esigenze di diversi tipi di utenti

>Si applica a: Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016

Servizi Desktop remoto supporta diversi tipi di carichi di lavoro. Ridimensiona la distribuzione a seconda delle esigenze previste per ogni tipo di utente.

## <a name="types-of-users"></a>Tipi di utenti

### <a name="knowledge-user"></a>Utente informato

Gli utenti informati usano applicazioni di produttività leggere, ad esempio Microsoft Word, Excel, Outlook e il browser Microsoft Edge.

### <a name="professional-user"></a>Utente professionale

Gli utenti professionali usano browser Internet e applicazioni di produttività, oltre a supportare carichi di lavoro più intensi, ad esempio quelli generati dallo sviluppo di software e dalla creazione di contenuto multimediale.

### <a name="power-user"></a>Utente esperto

Gli utenti esperti usano applicazioni di progettazione e grafica, ad esempio strumenti CAD e Adobe Photoshop. Le GPU rappresentano spesso una buona scelta per gli utenti che usano regolarmente programmi con uso elevato di grafica per rendering video, progettazione 3D e simulazioni.

Per altre informazioni sull'accelerazione grafica, vedi [Scegliere la tecnologia per il rendering della grafica](rds-graphics-virtualization.md).

Azure offre altre opzioni di distribuzione dell'accelerazione grafica e più dimensioni di VM disponibili per le GPU. Per informazioni, vedi [Dimensioni delle macchine virtuali ottimizzate per le GPU](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-gpu).

## <a name="test-workload"></a>Testare il carico di lavoro

Consigliamo di eseguire il test di carico delle distribuzioni con test di stress e simulazioni di utilizzo realistiche. Per eseguire il test di carico della tua distribuzione, puoi usare strumenti di simulazione come LoginVSI. Verifica che il sistema sia sufficientemente reattivo e resiliente da soddisfare le esigenze degli utenti e ricorda di variare le dimensioni del carico per evitare sorprese.
