---
title: Abilitazione degli aggiornamenti rapidi per Windows Server 2016 per l'aggiornamento di novembre 2018
description: Fornisce informazioni sugli aggiornamenti rapidi in Windows Server 2016
ms.prod: windows-server
ms.technology: server-general
ms.topic: article
author: lizap
ms.author: elizapo
ms.localizationpriority: medium
ms.openlocfilehash: 57a6d13836a4f8c633afa6d37e7fa42e6d817deb
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "76822054"
---
# <a name="express-updates-for-windows-server-2016-re-enabled-for-november-2018-update"></a>Abilitazione degli aggiornamenti rapidi per Windows Server 2016 per l'aggiornamento di novembre 2018

> A cura di Joel Frauenheim
> 
> Si applica a: Windows Server 2016

A partire dall'aggiornamento di martedì 13 novembre 2018, Windows pubblica di nuovo gli aggiornamenti rapidi per Windows Server 2016. Gli aggiornamenti rapidi per Windows Server 2016 sono cessati a metà del 2017 dopo un problema significativo riscontrato che ne impediva la corretta installazione. Anche se il problema è stato risolto a novembre 2017, il team di aggiornamento ha adottato un approccio conservativo alla pubblicazione dei pacchetti rapidi, in modo che per la maggior parte dei clienti venisse installato l'aggiornamento del 14 novembre 2017 ([KB 4048953](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) negli ambienti server senza impatto a causa del problema.

A novembre 2018 gli amministratori di sistema per WSUS e Configuration Manager hanno notato di nuovo due pacchetti per l'aggiornamento di Windows Server 2016: un aggiornamento completo e un aggiornamento rapido. Gli amministratori che vogliono usare l'aggiornamento rapido per i propri ambienti server devono verificare che il dispositivo abbia ricevuto un aggiornamento completo dal 14 novembre 2017 [(KB 4048953)](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953), per garantire la corretta installazione dell'aggiornamento rapido. Qualsiasi dispositivo che non sia stato aggiornato dal 14 novembre 2017 ([KB 4048953](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) riscontrerà funzionalità ripetute che consumano a ciclo infinito la larghezza di banda e le risorse della CPU se viene tentato l'aggiornamento rapido.  La soluzione per questo caso consiste nello smettere di provare l'aggiornamento rapido e di passare a un aggiornamento completo recente per arrestare il ciclo di errori.

Con l'aggiornamento rapido del 13 novembre 2018 i clienti noteranno un'immediata riduzione della dimensione del pacchetto tra il sistema di gestione e gli endpoint di Windows Server 2016.  