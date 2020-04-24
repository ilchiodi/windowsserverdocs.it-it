---
title: Distribuire Windows Server Update Services
description: 'Argomento di Windows Server Update Services (WSUS): Panoramica del processo di distribuzione con collegamenti ai quattro passaggi da eseguire'
ms.prod: windows-server
ms.technology: manage-wsus
ms.topic: get-started-article
ms.assetid: 2708f6b2-4252-4b8f-9b7e-84c9b4222075
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: aa724fc028e245ab404375ba94b074f10078d755
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "80828764"
---
# <a name="deploy-windows-server-update-services"></a>Distribuire Windows Server Update Services

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Windows Server Update Services (WSUS) consente agli amministratori IT di distribuire gli aggiornamenti dei prodotti Microsoft più recenti. WSUS è un ruolo del server di Windows Server che può essere installato per gestire e distribuire gli aggiornamenti. Un server WSUS può fungere da origine degli aggiornamenti per altri server WSUS all'interno dell'organizzazione. Quando il server WSUS svolge questa funzione, viene chiamato server upstream.  

In un'implementazione di Windows Server Update Services è necessario che almeno un server WSUS in rete si connetta a Microsoft Update per ottenere le informazioni sugli aggiornamenti disponibili. È possibile determinare, in base alla configurazione e sicurezza di rete, quanti altri server di connettersi direttamente a Microsoft Update.  

Questa guida fornisce informazioni di carattere generale per la pianificazione e distribuzione di Windows Server Update Services.  

-   [Pianificare la distribuzione di WSUS](../plan/plan-your-wsus-deployment.md)  

-   [Passaggio 1: Installare il ruolo del server WSUS](1-install-the-wsus-server-role.md)  

-   [Passaggio 2: Configurare WSUS](2-configure-wsus.md)  

-   [Passaggio 3: Approvare e distribuire gli aggiornamenti in WSUS](3-approve-and-deploy-updates-in-wsus.md)  

-   [Passaggio 4: Configurare le impostazioni di criteri di gruppo per gli aggiornamenti automatici](4-configure-group-policy-settings-for-automatic-updates.md)  
