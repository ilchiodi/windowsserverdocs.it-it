---
title: Configurare server di contenuti Windows Server Update Services (WSUS)
description: Questo argomento fa parte della Guida alla distribuzione di BranchCache per Windows Server 2016, che illustra come distribuire BranchCache in modalità cache distribuita e ospitata per ottimizzare l'utilizzo della larghezza di banda WAN nelle succursali.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 9724aa8d-e4ae-404c-bee6-cef1534cd3ca
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 2993c78e85609ac720fd208971dda7ed67a3610d
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80319168"
---
# <a name="configure-windows-server-update-services-wsus-content-servers"></a>Configurare server di contenuti Windows Server Update Services (WSUS)

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Dopo l'installazione della funzionalità BranchCache e l'avvio del servizio BranchCache, è necessario configurare i server WSUS per l'archiviazione dei file dell'aggiornamento sul computer locale. 

Quando si configurano i server WSUS per archiviare file dell'aggiornamento sul computer locale, sia i metadati che i file dell'aggiornamento vengono scaricati e archiviati direttamente sul server WSUS. In questo modo si garantisce che i computer client con BranchCache ricevano i file di aggiornamento dei prodotti Microsoft dal server WSUS anziché direttamente dal sito Web Microsoft Update.  
  
Per ulteriori informazioni sulla sincronizzazione di WSUS, vedere [la pagina relativa alla configurazione delle sincronizzazioni degli aggiornamenti](https://technet.microsoft.com/library/mt612311.aspx)  