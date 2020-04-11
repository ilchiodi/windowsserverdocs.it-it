---
title: Servizi Desktop remoto - Disponibilità elevata
description: Informazioni di pianificazione sulla configurazione di una distribuzione di Servizi Desktop remoto a disponibilità elevata.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: ec630ea0-ab80-4dfe-a25f-f4f601651f72
author: lizap
ms.author: elizapo
ms.date: 09/07/2016
manager: dongill
ms.openlocfilehash: 86c5f59fcd403e838a316174840b93608d7f06ec
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858144"
---
# <a name="remote-desktop-services---high-availability"></a>Servizi Desktop remoto - Disponibilità elevata

>Si applica a: Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016

Gli errori e la limitazione delle richieste sono inevitabili in sistemi su larga scala. È semplice configurare ruoli di infrastruttura Desktop remoto per supportare la disponibilità elevata e consentire agli utenti finali di connettersi ogni volta senza problemi.

In Servizi Desktop remoto gli elementi seguenti rappresentano i ruoli di infrastruttura Desktop remoto, con le rispettive indicazioni per stabilire la disponibilità elevata:
- [Gestore connessione Desktop remoto](Deploy-a-Remote-Desktop-Connection-Broker-cluster.md)
- [Gateway Desktop remoto](Deploy-a-RD-Web-Access-and-Gateway-farm.md)
- Servizio licenze Desktop remoto
- [Accesso Web Desktop remoto](Deploy-a-RD-Web-Access-and-Gateway-farm.md)

La disponibilità elevata viene stabilita attraverso la duplicazione di ognuno dei servizi di ruoli in un secondo computer. In Azure puoi ricevere un tempo di attività garantito inserendo il set delle due macchine virtuali (che ospitano lo stesso ruolo) in un set di disponibilità.

Insieme ai set di disponibilità, ora puoi sfruttare la potenza del database SQL di Azure e il relativo contratto di servizio supportato da Azure per disporre sempre delle informazioni di connessione e per poter reindirizzare gli utenti ai loro desktop e applicazioni.

Per le procedure consigliate su come creare l'ambiente di Servizi Desktop remoto, vedi l'[architettura dell'hosting desktop](desktop-hosting-reference-architecture.md).