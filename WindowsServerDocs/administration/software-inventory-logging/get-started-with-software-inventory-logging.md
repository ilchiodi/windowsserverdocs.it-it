---
title: Introduzione a Software di registrazione inventario
description: Viene descritto come installare e iniziare a usare registrazione inventario Software
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: manage-software-inventory-logging
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ed51c13c-7cbf-4144-a675-011fd29379d4
author: brentfor
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5a3e51effa6321c3575e385f1c56bba57318eca5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822832"
---
# <a name="get-started-with-software-inventory-logging"></a>Introduzione a Software di registrazione inventario

>Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

 Registrazione inventario software raccoglie i dati di inventario software Microsoft in ogni singolo server. Prima di usare registrazione inventario Software con Windows Server 2012 R2, assicurarsi che gli aggiornamenti di Windows [KB 3000850](https://support.microsoft.com/kb/3000850) e [KB 3060681](https://support.microsoft.com/kb/3060681) vengono installati in ogni sistema che verrà inventariato. Non sono disponibili aggiornamenti di Windows è necessario per Windows Server 2016. Inoltre, se si desidera utilizzare la funzionalità di registrazione inventario software per inoltrare i dati a un server di aggregazione, assicurarsi che si hanno i certificati SSL validi per la rete.

## <a name="BKMK_OVER"></a>Descrizione funzionalità
Registrazione inventario software in Windows Server è una funzionalità costituita da una semplice serie di cmdlet PowerShell che consentono agli amministratori di server di recuperare un elenco del software Microsoft installato nei server. Offre anche la capacità di raccogliere e inoltrare periodicamente per l'aggregazione questi dati attraverso la rete a un server Web di destinazione usando il protocollo HTTPS. Con i comandi di PowerShell viene eseguita anche la gestione della funzionalità, principalmente per la raccolta oraria dei dati e per l'inoltro.

> [!NOTE]
> Un server di aggregazione che esegue un servizio Web può essere configurato separatamente, ma non viene distribuito con la funzionalità di registrazione dell'inventario software. Per altre informazioni, vedere [Software Inventory Logging Aggregator](software-inventory-logging-aggregator.md).

> [!IMPORTANT]
> Nessuno dei dati raccolti dalla registrazione dell'inventario software viene inviato a Microsoft.

## <a name="BKMK_APP"></a>Applicazioni pratiche
Lo scopo di Registrazione inventario software è ridurre i costi operativi associati al recupero di informazioni accurate sul software Microsoft distribuito localmente in un server, ma soprattutto in più server in un ambiente IT (presupponendo che sia distribuito e abilitato in tutto l'ambiente IT). La possibilità di inoltrare questi dati a un server di aggregazione (se configurato separatamente da un amministratore IT) consente di raccogliere i dati in un'unica posizione con un processo automatico uniforme. Pur essendo possibile ottenere questo risultato anche eseguendo query direttamente sulle interfacce, avvalendosi di un'architettura di inoltro (sulla rete) avviata da ogni server, la funzionalità Registrazione inventario software consente di superare le difficoltà di individuazione dei computer tipiche di molti scenari di gestione delle risorse e di inventario software. Per proteggere i dati inoltrati su HTTPS al server di aggregazione di un amministratore, viene usato SSL. Quando i dati sono disponibili in una posizione centralizzata (un server), è più semplice analizzarli, modificarli e creare report. È importante notare come nessuno di questi dati venga inviato a Microsoft con questa funzionalità. I dati e la funzionalità della registrazione dell'inventario software sono destinati unicamente agli amministratori e al proprietario della licenza del software del server.

La registrazione dell'inventario software può fornire supporto agli amministratori di server per l'esecuzione delle attività seguenti:

-   Recupero delle informazioni di inventario software e server dai server Windows da postazione remota e su richiesta.

-   Aggregazione delle informazioni di inventario software e server per una vasta gamma di scenari di gestione delle risorse software abilitando la funzionalità Registrazione inventario software di ogni server e scegliendo l'URI di destinazione di un server Web e l'identificazione personale del certificato per l'autenticazione.

## <a name="see-also"></a>Vedere anche
[Software Inventory Logging Aggregator](https://technet.microsoft.com/library/mt572043.aspx)<br>
[Gestire il Software di inventario registrazione](manage-software-inventory-logging.md)<br>
[Cmdlet di registrazione inventario software in Windows PowerShell](https://technet.microsoft.com/library/dn283390.aspx)<br>
[Microsoft Assessment and Planning Toolkit](https://www.microsoft.com/download/en/details.aspx?id=7826)
[Volume Activation Management Tool](http://blogs.technet.com/b/volume-licensing/)

