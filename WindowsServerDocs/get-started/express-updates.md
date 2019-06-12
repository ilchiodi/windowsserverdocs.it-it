---
title: Degli aggiornamenti rapidi per Windows Server 2016 riabilitato per mese di novembre 2018 update
description: Vengono fornite informazioni sugli aggiornamenti Express in Windows Server 2016
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
author: lizap
ms.author: elizapo
ms.localizationpriority: medium
ms.openlocfilehash: 1644a61c87953e465895e23c3c8454bae7f3a056
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66443379"
---
# <a name="express-updates-for-windows-server-2016-re-enabled-for-november-2018-update"></a>Degli aggiornamenti rapidi per Windows Server 2016 riabilitato per mese di novembre 2018 update

> Da Joel Frauenheim
> 
> Si applica a: Windows Server 2016

A partire con 13 novembre 2018 martedì dedicato all'aggiornamento, Windows verrà nuovamente pubblicare gli aggiornamenti di Express per Windows Server 2016. Gli aggiornamenti rapidi per Windows Server 2016 è arrestato in metà del 2017 dopo che è stato rilevato un problema significativo che conservati gli aggiornamenti da installare in modo corretto. Anche se il problema è stato risolto in novembre 2017, il team di aggiornamento ha impiegato un approccio conservativo per pubblicare i pacchetti Express per garantire maggior parte dei clienti sarebbe necessario aggiorna il 14 novembre 2017 ([KB 4048953](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) installato nel proprio server gli ambienti e non interessate dal problema.

Gli amministratori di sistema per Windows Server Update Services e System Center Configuration Manager (SCCM) devono essere consapevoli che nel novembre 2018 verranno ancora una volta vedono due pacchetti per l'aggiornamento di Windows Server 2016: un aggiornamento completo e un aggiornamento rapido. Gli amministratori di sistema che desiderano utilizzare Express per ambienti server devono verificare che il dispositivo ha eseguito un aggiornamento completo partire dal 14 novembre 2017 ([KB 4048953](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) per garantire l'aggiornamento di Express viene installato correttamente. Qualsiasi dispositivo che non è stato aggiornato dopo il 14 novembre 2017 aggiornare ([KB 4048953](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) vengono visualizzati errori ripetuti che utilizzano la larghezza di banda e le risorse della CPU in un ciclo infinito se si tenta l'aggiornamento rapido.  Monitoraggio e aggiornamento per tale stato potrebbe essere all'amministratore di sistema interrompere il push dell'aggiornamento rapido e push un recente aggiornamento completo per interrompere il ciclo di errore.

Con 13 novembre 2018 i clienti di aggiornamento rapido visualizzeranno un'immediata riduzione delle dimensioni di pacchetto tra il sistema di gestione e i punti finali di Windows Server 2016.  