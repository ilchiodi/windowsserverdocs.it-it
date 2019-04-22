---
title: Case Study di Windows Admin Center SDK - Fujitsu
description: Case Study di Windows Admin Center SDK - Fujitsu
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 05/23/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 6d916920b187dd3c637644a0f40ae9f9cca72b66
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814992"
---
# <a name="fujitsu-serverview-health-and-raid-extensions"></a>Estensioni di Fujitsu ServerView integrità e RAID

## <a name="bringing-end-to-end-visibility-from-operating-system-to-hardware-into-windows-admin-center"></a>Offrendo visibilità end-to-end, dal sistema operativo a hardware, in Windows Admin Center

Fujitsu è un'azienda produttrice di e un'azienda di tecnologia e delle comunicazioni giapponese leader [PRIMERGY](http://www.fujitsu.com/fts/products/computing/servers/primergy/) e [PRIMEQUEST](http://www.fujitsu.com/fts/products/computing/servers/mission-critical/) prodotti server. Il [suite di gestione di Fujitsu ServerView](http://www.fujitsu.com/fts/products/computing/servers/primergy/management/) offre un set completo di strumenti per server di gestione del ciclo di vita tra cui un agente sul lato server che fornisce un'interfaccia CIM e PowerShell per la gestione dell'hardware.

Fujitsu individuato un'opportunità per integrare facilmente con Windows Admin Center modo in cui fornite interfacce CIM e PowerShell che è stato possibile comunicare con gli agenti del lato server. Il team di sviluppo in Fujitsu è stato in grado di implementare le chiamate CIM che fossero familiarità con l'agente con facilità e visualizzare le informazioni all'interno di Windows Admin Center utilizzando i componenti dell'interfaccia utente disponibili.

![Estensione di Fujitsu - visualizzazione struttura ad albero dell'integrità](../../media/extend-case-study-fujitsu/health-tree.png)

Una volta che il team è acquisita familiarità con il SDK di Windows Admin Center, l'aggiunta di un'interfaccia utente per esporre le informazioni hardware aggiuntivo, spesso era semplicemente alcune altre righe di codice HTML e ha avuto rapidamente in grado di espandere da un unico strumento per la visualizzazione di una visualizzazione di riepilogo del componente hardware integrità, le visualizzazioni dettagliate per i registri eventi di sistema, il monitoraggio di driver, separare le visualizzazioni per processore, memoria, le ventole, alimentatori, le temperature e tensioni e anche uno strumento aggiuntivo per la gestione di RAID. Uso di controlli dell'interfaccia utente disponibili nel SDK, ad esempio la struttura ad albero, i controlli riquadro griglia e i dettagli abilitato al team di creare rapidamente l'interfaccia utente e anche realizzare un progetto di oggetto visivo e interazione molto simile al resto del Windows Admin Center.

![Estensione di Fujitsu - visualizzazione struttura ad albero RAID](../../media/extend-case-study-fujitsu/raid-tree.png)

![Estensione di Fujitsu - visualizzare i volumi RAID](../../media/extend-case-study-fujitsu/raid-volumes.png)

La collaborazione tra team di Windows Admin Center e Fujitsu mostra chiaramente il valore di integrazione all'interno di Windows Admin Center, consentendo ai clienti avere informazioni dettagliate end-to-end in servizi, il sistema operativo di gestione dell'hardware e i ruoli del server .