---
title: Configurare i server del contenuto di Windows Server Update Services (WSUS)
description: In questo argomento fa parte di BranchCache distribuzione Guide per Windows Server 2016, che illustra come distribuire BranchCache in modalità cache distribuita e ospitato per ottimizzare l'utilizzo della larghezza di banda WAN nelle succursali.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 9724aa8d-e4ae-404c-bee6-cef1534cd3ca
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8200a0905f7bc5c403288a22faece5f84eac8af9
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="configure-windows-server-update-services-wsus-content-servers"></a>Configurare i server del contenuto di Windows Server Update Services (WSUS)

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Dopo aver installato la funzionalità BranchCache e avviare il servizio BranchCache, è necessario configurare i server WSUS per archiviare i file di aggiornamento nel computer locale. 

Quando si configura il server WSUS per archiviare i file di aggiornamento nel computer locale, sia i metadati degli aggiornamenti e i file di aggiornamento vengono scaricati e archiviati direttamente il server WSUS. Ciò garantisce che i computer client con BranchCache ricevano i file di aggiornamento del prodotto Microsoft dal server WSUS anziché direttamente da Microsoft Update Website.  
  
Per ulteriori informazioni sulla sincronizzazione di WSUS, vedere [impostazione delle sincronizzazioni di aggiornamento](https://technet.microsoft.com/en-us/library/mt612311.aspx)  