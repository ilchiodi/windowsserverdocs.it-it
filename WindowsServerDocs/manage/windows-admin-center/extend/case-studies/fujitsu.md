---
title: Interfaccia di amministrazione di Windows SDK Case Study - Fujitsu
description: Interfaccia di amministrazione di Windows SDK Case Study - Fujitsu
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 05/23/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 6d916920b187dd3c637644a0f40ae9f9cca72b66
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/22/2018
ms.locfileid: "2052375"
---
# <a name="fujitsu-serverview-health-and-raid-extensions"></a>Estensioni Fujitsu ServerView integrità e RAID

## <a name="bringing-end-to-end-visibility-from-operating-system-to-hardware-into-windows-admin-center"></a>Importazione di visibilità end-to-end, dal sistema operativo a hardware, in interfaccia di amministrazione di Windows

Fujitsu è un iniziale informazioni in lingua giapponese e società tecnologia di comunicazione e un produttore di prodotti server [PRIMERGY](http://www.fujitsu.com/fts/products/computing/servers/primergy/) e [PRIMEQUEST](http://www.fujitsu.com/fts/products/computing/servers/mission-critical/) . [Suite di gestione Fujitsu ServerView](http://www.fujitsu.com/fts/products/computing/servers/primergy/management/) offre un set di strumenti completo per il server Gestione del ciclo di vita tra cui un agente sul lato server che fornisce un'interfaccia CIM e PowerShell per la gestione dell'hardware.

Fujitsu avanzati possibilità di integrare facilmente con l'interfaccia di amministrazione di Windows come fornito interfacce CIM e PowerShell in grado di comunicare con gli agenti sul lato server. Il team di sviluppo Fujitsu è stato in grado di implementare le chiamate CIM che fossero familiarità con all'agente facilmente e visualizzare le informazioni nell'interfaccia di amministrazione di Windows con i componenti dell'interfaccia utente disponibili.

![Estensione Fujitsu - visualizzazione albero dello stato](../../media/extend-case-study-fujitsu/health-tree.png)

Dopo che il team acquisita familiarità con Windows Admin Center SDK, aggiunta dell'interfaccia utente per esporre le informazioni di hardware aggiuntivo trattava semplicemente alcune più righe di codice HTML e sono stati rapidamente in grado di espandersi da un singolo strumento per la visualizzazione di una visualizzazione sintetica delle componente hardware integrità, visualizzazioni dettagliate dei registri eventi di sistema, monitor driver, separare le visualizzazioni di processori, memoria, ventagli, alimentatori, temperature e voltaggi e anche uno strumento aggiuntivo per la gestione di RAID. Utilizzo dei controlli dell'interfaccia utente disponibili in SDK, ad esempio la struttura ad albero, controlli del riquadro griglia e dettagli abilitato al team di creare rapidamente applicazioni dell'interfaccia utente e anche ottenere una progettazione di visualizzazione e interazione molto simile per il resto dell'interfaccia di amministrazione di Windows.

![Estensione Fujitsu - visualizzazione albero RAID](../../media/extend-case-study-fujitsu/raid-tree.png)

![Visualizzare i volumi RAID estensione Fujitsu-](../../media/extend-case-study-fujitsu/raid-volumes.png)

La relazione tra il team di interfaccia di amministrazione di Windows e Fujitsu chiaramente viene visualizzato il valore dell'integrazione nell'interfaccia di amministrazione di Windows, consentendo ai clienti di disporre delle conoscenze end-to-end a ruoli del server e servizi, il sistema operativo e la gestione dell'hardware .