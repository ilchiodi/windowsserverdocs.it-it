---
title: Express gli aggiornamenti per Windows Server 2016 riattivato per novembre 2018 aggiornare
description: Fornisce informazioni sugli aggiornamenti Express in Windows Server 2016
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
author: lizap
ms.author: elizapo
ms.localizationpriority: medium
ms.openlocfilehash: c48979440ab7c5cfa86aa1287b354a1e43692f48
ms.sourcegitcommit: 23e0a68e21985d709e029e7771d3c52d6815bcb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/13/2018
ms.locfileid: "6507852"
---
# Express gli aggiornamenti per Windows Server 2016 riattivato per novembre 2018 aggiornare

>Da Joel Frauenheim

>Si applica a: WindowsServer 2016

Avvio con 13 novembre 2018 martedì Update, Windows verrà nuovamente pubblicare aggiornamenti Express per Windows Server 2016. Rapida degli aggiornamenti per Windows Server 2016 smesso di medie 2017 dopo che è stato rilevato un problema significativo che mantenuti gli aggiornamenti da installare correttamente. Anche se il problema è stato risolto in novembre 2017, il team di aggiornamento ha avuto un approccio tradizionale per pubblicare i pacchetti per garantire che la maggior parte dei clienti sarebbero il 14 novembre 2017 installato l'aggiornamento ([4048953 KB](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) in ambienti di server e non essere Express interessati dal problema.

Gli amministratori di sistema per WSUS e System Center Configuration Manager (SCCM) devono tenere presente che in novembre 2018 ancora una volta vedrà due pacchetti per l'aggiornamento di Windows Server 2016: un aggiornamento completo e un aggiornamento Express. Gli amministratori di sistema che desiderano usare Express per i propri ambienti server devono verificare che il dispositivo ha eseguito un aggiornamento completo dopo 14 novembre 2017 ([4048953 KB](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) per garantire che l'aggiornamento Express viene installato correttamente. Errori che richiedono la larghezza di banda e le risorse di CPU in un ciclo infinito se l'aggiornamento Express viene tentato un ripetuto in qualsiasi dispositivo che non è stato aggiornato dopo l'aggiornamento 14 novembre 2017 ([4048953 KB](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) verrà visualizzata.  Correzione per tale stato sarebbe per l'amministratore di sistema impedire che spinge l'aggiornamento Express e inviare un aggiornamento recente completo per interrompere il ciclo di errore.

Con il 13 novembre 2018 Express update clienti vedranno un'immediata riduzione delle dimensioni del pacchetto tra loro sistema di gestione e i punti finali di Windows Server 2016.  