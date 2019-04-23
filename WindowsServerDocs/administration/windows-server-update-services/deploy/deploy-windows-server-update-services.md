---
title: Distribuire Windows Server Update Services
description: Argomento di Windows Server Update Service (WSUS) - Panoramica del processo di distribuzione con collegamenti per i quattro passaggi per ottenere questo risultato
ms.prod: windows-server-threshold
ms.reviewer: na
ms.technology: manage-wsus
ms.topic: get-started-article
ms.assetid: 2708f6b2-4252-4b8f-9b7e-84c9b4222075
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 51972ad352f6530c8ee2aa84aec57b62784da728
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873182"
---
# <a name="deploy-windows-server-update-services"></a>Distribuire Windows Server Update Services

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Windows Server Update Services (WSUS) consente agli amministratori IT di distribuire gli aggiornamenti dei prodotti Microsoft più recenti. WSUS è un ruolo del server di Windows Server che può essere installato per gestire e distribuire gli aggiornamenti. Un server WSUS può fungere da origine degli aggiornamenti per altri server WSUS all'interno dell'organizzazione. Quando il server WSUS svolge questa funzione, viene chiamato server upstream.  

In un'implementazione di Windows Server Update Services è necessario che almeno un server WSUS in rete si connetta a Microsoft Update per ottenere le informazioni sugli aggiornamenti disponibili. È possibile determinare, in base alla configurazione e sicurezza di rete, quanti altri server di connettersi direttamente a Microsoft Update.  

Questa guida fornisce informazioni di carattere generale per la pianificazione e distribuzione di Windows Server Update Services.  

-   [Pianificare la distribuzione di WSUS](../plan/plan-your-wsus-deployment.md)  

-   [Passaggio 1: Installare il ruolo Server WSUS](1-install-the-wsus-server-role.md)  

-   [Passaggio 2: Configurare WSUS](2-configure-wsus.md)  

-   [Passaggio 3: Approvare e distribuire gli aggiornamenti in WSUS](3-approve-and-deploy-updates-in-wsus.md)  

-   [Passaggio 4: Configurare le impostazioni di criteri di gruppo per gli aggiornamenti automatici](4-configure-group-policy-settings-for-automatic-updates.md)  
