---
title: Servizi Desktop remoto - disponibilità elevata
description: Informazioni sulla configurazione di una distribuzione di servizi desktop remoto a disponibilità elevata di pianificazione.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ec630ea0-ab80-4dfe-a25f-f4f601651f72
author: lizap
ms.author: elizapo
ms.date: 09/07/2016
manager: dongill
ms.openlocfilehash: b5a2bd38c8831063d6fd2ba525b71a10403b8fc2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839262"
---
# <a name="remote-desktop-services---high-availability"></a>Servizi Desktop remoto - disponibilità elevata

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Gli errori e la limitazione delle richieste sono inevitabili nei sistemi su larga scala. È semplice da configurare ruoli di infrastruttura di Desktop remoto per supportare la disponibilità elevata e consentire agli utenti finali per connettersi senza problemi, ogni volta.

In Servizi Desktop remoto, gli elementi seguenti rappresentano i ruoli di infrastruttura di Desktop remoto, con le rispettive indicazioni per stabilire la disponibilità elevata:
- [Gestore connessione Desktop remoto](Deploy-a-Remote-Desktop-Connection-Broker-cluster.md)
- [Gateway Desktop remoto](Deploy-a-RD-Web-Access-and-Gateway-farm.md)
- Servizio licenze Desktop remoto
- [Accesso Web Desktop remoto](Deploy-a-RD-Web-Access-and-Gateway-farm.md)

Disponibilità elevata viene stabilita attraverso la duplicazione di ognuno dei servizi di ruoli in un secondo computer. In Azure, è possibile ricevere un tempo di attività garantito, inserendo il set di due macchine virtuali (che ospita il ruolo stesso) in un disponibilità imposta.

Insieme ai set di disponibilità, è ora possibile sfruttare la potenza del Database SQL di Azure e il contratto di servizio supportato da Azure per verificare che sempre le informazioni di connessione e può reindirizzare gli utenti per i desktop e applicazioni.

Per le procedure consigliate su come creare l'ambiente di servizi desktop remoto, vedere la [architettura dell'hosting del desktop](desktop-hosting-reference-architecture.md).