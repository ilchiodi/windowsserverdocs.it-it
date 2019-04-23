---
title: Configurare server di contenuti Windows Server Update Services (WSUS)
description: Questo argomento fa parte di BranchCache distribuzione Guide per Windows Server 2016, che illustra come distribuire BranchCache in modalità cache distribuita e ospitato per ottimizzare l'utilizzo della larghezza di banda WAN nelle succursali.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 9724aa8d-e4ae-404c-bee6-cef1534cd3ca
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e8576282be92f02daf716da82ea75eddc755ee5c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873842"
---
# <a name="configure-windows-server-update-services-wsus-content-servers"></a>Configurare server di contenuti Windows Server Update Services (WSUS)

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Dopo l'installazione della funzionalità BranchCache e l'avvio del servizio BranchCache, è necessario configurare i server WSUS per l'archiviazione dei file dell'aggiornamento sul computer locale. 

Quando si configurano i server WSUS per archiviare file dell'aggiornamento sul computer locale, sia i metadati che i file dell'aggiornamento vengono scaricati e archiviati direttamente sul server WSUS. In questo modo si garantisce che i computer client con BranchCache ricevano i file di aggiornamento dei prodotti Microsoft dal server WSUS anziché direttamente dal sito Web Microsoft Update.  
  
Per altre informazioni sulla sincronizzazione di WSUS, vedere [impostazione delle sincronizzazioni di aggiornamento](https://technet.microsoft.com/library/mt612311.aspx)  