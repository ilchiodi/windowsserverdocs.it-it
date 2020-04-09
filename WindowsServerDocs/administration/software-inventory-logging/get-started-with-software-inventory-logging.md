---
title: Introduzione alla registrazione dell'inventario software
description: Viene descritto come installare e iniziare a usare registrazione inventario software
ms.prod: windows-server
ms.technology: manage-software-inventory-logging
ms.topic: article
ms.assetid: ed51c13c-7cbf-4144-a675-011fd29379d4
author: brentfor
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e54e8f96863280263f7afee2e32bfd41ec712110
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851444"
---
# <a name="get-started-with-software-inventory-logging"></a>Introduzione alla registrazione dell'inventario software

>Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

 Registrazione inventario software raccoglie i dati di inventario software Microsoft in base ai singoli server. Prima di usare registrazione inventario software con Windows Server 2012 R2, assicurarsi che Windows Update [kb 3000850](https://support.microsoft.com/kb/3000850) e [KB 3060681](https://support.microsoft.com/kb/3060681) siano installati in ogni sistema che verrà incluso nell'inventario. Non è richiesto alcun Windows Update per Windows Server 2016. Inoltre, se si vuole usare la funzionalità di registrazione inventario software per l'invio dei dati a un server di aggregazione, assicurarsi di disporre di certificati SSL validi per la rete.

## <a name="feature-description"></a><a name="BKMK_OVER"></a>Descrizione delle funzionalità
Registrazione inventario software in Windows Server è una funzionalità costituita da una semplice serie di cmdlet PowerShell che consentono agli amministratori di server di recuperare un elenco del software Microsoft installato nei server. Offre anche la capacità di raccogliere e inoltrare periodicamente per l'aggregazione questi dati attraverso la rete a un server Web di destinazione usando il protocollo HTTPS. Con i comandi di PowerShell viene eseguita anche la gestione della funzionalità, principalmente per la raccolta oraria dei dati e per l'inoltro.

> [!NOTE]
> Un server di aggregazione che esegue un servizio Web può essere configurato separatamente, ma non viene distribuito con la funzionalità di registrazione dell'inventario software. Per altre informazioni, vedere [Software Inventory Logging Aggregator](software-inventory-logging-aggregator.md).

> [!IMPORTANT]
> Nessuno dei dati raccolti dalla registrazione dell'inventario software viene inviato a Microsoft come parte delle funzionalità della funzionalità.

## <a name="practical-applications"></a><a name="BKMK_APP"></a>Applicazioni pratiche
Lo scopo di Registrazione inventario software è ridurre i costi operativi associati al recupero di informazioni accurate sul software Microsoft distribuito localmente in un server, ma soprattutto in più server in un ambiente IT (presupponendo che sia distribuito e abilitato in tutto l'ambiente IT). La possibilità di inoltrare questi dati a un server di aggregazione (se configurato separatamente da un amministratore IT) consente di raccogliere i dati in un'unica posizione con un processo automatico uniforme. Pur essendo possibile ottenere questo risultato anche eseguendo query direttamente sulle interfacce, avvalendosi di un'architettura di inoltro (sulla rete) avviata da ogni server, la funzionalità Registrazione inventario software consente di superare le difficoltà di individuazione dei computer tipiche di molti scenari di gestione delle risorse e di inventario software. SSL viene usato per proteggere i dati trasmessi tramite HTTPS al server di aggregazione di un amministratore. Quando i dati sono disponibili in una posizione centralizzata (un server), è più semplice analizzarli, modificarli e creare report. È importante notare come nessuno di questi dati venga inviato a Microsoft con questa funzionalità. La funzionalità e i dati di registrazione dell'inventario software sono destinati esclusivamente al proprietario e agli amministratori della licenza del software server.

La registrazione dell'inventario software può fornire supporto agli amministratori di server per l'esecuzione delle attività seguenti:

-   Recupero delle informazioni di inventario software e server dai server Windows da postazione remota e su richiesta.

-   Aggregazione delle informazioni di inventario software e server per un'ampia gamma di scenari di gestione delle risorse software abilitando la funzionalità di registrazione dell'inventario software di ogni server e scegliendo un URI di destinazione del server Web e l'identificazione personale del certificato per l'autenticazione.

## <a name="see-also"></a>Vedi anche
[Aggregatore di Registrazione inventario software](https://technet.microsoft.com/library/mt572043.aspx)<br>
[Gestire Registrazione inventario software](manage-software-inventory-logging.md)<br>
[Cmdlet di registrazione inventario software in Windows PowerShell](https://technet.microsoft.com/library/dn283390.aspx)<br>
[Microsoft Assessment and Planning Toolkit](https://www.microsoft.com/download/en/details.aspx?id=7826)
[strumento di gestione dell'attivazione dei contratti multilicenza](https://blogs.technet.com/b/volume-licensing/)

