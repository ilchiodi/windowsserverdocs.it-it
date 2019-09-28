---
title: Case Study di Windows Admin Center SDK-Fujitsu
description: Case Study di Windows Admin Center SDK-Fujitsu
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 05/23/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 9acfa873e4ce7d3e91a23abff726836f0e11ce59
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357224"
---
# <a name="fujitsu-serverview-health-and-raid-extensions"></a>Stato di ServerView Fujitsu e estensioni RAID

## <a name="bringing-end-to-end-visibility-from-operating-system-to-hardware-into-windows-admin-center"></a>Introduzione alla visibilità end-to-end, dal sistema operativo all'hardware, all'interfaccia di amministrazione di Windows

Fujitsu è una società leader nel settore delle informazioni e della comunicazione giapponese e un produttore di prodotti server [PRIMERGY](http://www.fujitsu.com/fts/products/computing/servers/primergy/) e [PRIMEQUEST](http://www.fujitsu.com/fts/products/computing/servers/mission-critical/) . [Fujitsu Serverview Management Suite](http://www.fujitsu.com/fts/products/computing/servers/primergy/management/) fornisce un set di strumenti completo per la gestione del ciclo di vita del server, incluso un agente lato server che fornisce un'interfaccia CIM e PowerShell per la gestione dell'hardware.

Fujitsu ha visto un'opportunità di integrazione con facilità con l'interfaccia di amministrazione di Windows, in quanto forniva le interfacce CIM e PowerShell che potevano comunicare con gli agenti lato server. Il team di sviluppo di Fujitsu è stato in grado di implementare facilmente le chiamate CIM a cui aveva familiarità con l'agente e visualizzare le informazioni all'interno dell'interfaccia di amministrazione di Windows usando i componenti dell'interfaccia utente disponibili.

![Estensione Fujitsu-visualizzazione albero integrità](../../media/extend-case-study-fujitsu/health-tree.png)

Una volta che il team ha acquisito familiarità con l'SDK di amministrazione di Windows, l'aggiunta dell'interfaccia utente per esporre informazioni aggiuntive sull'hardware è stata spesso semplicemente costituita da un numero ridotto di righe di codice HTML ed è stato rapidamente in grado di espandersi da un singolo strumento alla visualizzazione di una visualizzazione riepilogativa integrità, visualizzazioni dettagliate per i registri eventi di sistema, monitoraggio driver, visualizzazioni separate per processore, memoria, ventilatori, alimentatori, temperature e tensioni e anche uno strumento aggiuntivo per la gestione RAID. L'uso dei controlli dell'interfaccia utente disponibili nell'SDK, ad esempio i controlli struttura ad albero, griglia e riquadro dettagli, ha permesso al team di creare rapidamente l'interfaccia utente e di ottenere una progettazione visiva e di interazione molto simile al resto dell'interfaccia di amministrazione di Windows.

![Estensione Fujitsu-visualizzazione albero RAID](../../media/extend-case-study-fujitsu/raid-tree.png)

![Estensione Fujitsu-visualizzazione volumi RAID](../../media/extend-case-study-fujitsu/raid-volumes.png)

La partnership tra Fujitsu e il team di amministrazione di Windows illustra chiaramente il valore dell'integrazione all'interno dell'interfaccia di amministrazione di Windows, consentendo ai clienti di ottenere informazioni dettagliate end-to-end sui ruoli e i servizi del server, sul sistema operativo e sulla gestione dell'hardware .