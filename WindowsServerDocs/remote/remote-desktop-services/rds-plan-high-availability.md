---
title: Servizi Desktop remoto - Disponibilità elevata
description: Informazioni di pianificazione sulla configurazione di una distribuzione di Servizi Desktop remoto a disponibilità elevata.
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
ms.openlocfilehash: ab9a6118641b49dfa971e66dc9ba75ef88bbd84a
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870885"
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