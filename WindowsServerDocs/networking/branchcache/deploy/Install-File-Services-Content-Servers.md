---
title: Installare server di contenuti di Servizi file
description: Questo argomento fa parte di BranchCache distribuzione Guide per Windows Server 2016, che illustra come distribuire BranchCache in modalità cache distribuita e ospitato per ottimizzare l'utilizzo della larghezza di banda WAN nelle succursali
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 74b0a5ed-dc20-4974-9d4b-2426987a01a1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c55f8df57ed98d13d6d0d6d2a281edfb55883bea
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356558"
---
# <a name="install-file-services-content-servers"></a>Installare server di contenuti di Servizi file

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Per distribuire server di contenuti che eseguono il ruolo server Servizi file, è necessario installare il servizio ruolo BranchCache per file di rete del ruolo server Servizi file. Inoltre, è necessario abilitare BranchCache sulle condivisioni file in base ai requisiti.  
  
Durante la configurazione del server di contenuti è possibile consentire a BranchCache di pubblicare contenuto per tutte le condivisioni file o selezionare un sottoinsieme di condivisioni file da pubblicare.  
  
> [!NOTE]  
> Quando si distribuisce un file abilitato server BranchCache server Web o come server di contenuti, informazioni sul contenuto viene ora calcolate non in linea, anche prima che un client con BranchCache richiede un file. A causa di questo miglioramento, non è necessario configurare la pubblicazione di hash per i server di contenuti, come indicato nella versione precedente di BranchCache.  
>   
> Questa generazione hash automatica fornisce prestazioni più veloci e ulteriori risparmi di larghezza di banda, perché le informazioni sul contenuto è pronti per il primo client che richiede il contenuto e i calcoli sono già stati eseguiti.  
  
Vedere gli argomenti seguenti per distribuire server di contenuti.  
  
-   [Configurare il ruolo server Servizi file](../../branchcache/deploy/Configure-the-File-Services-server-role.md)  
  
-   [Abilitare la pubblicazione di hash per i file server](../../branchcache/deploy/Enable-Hash-Publication-for-File-Servers.md)  
  
-   [Abilitare BranchCache su una condivisione &#40;file facoltativa&#41;](../../branchcache/deploy/enable-bc-on-file-share.md)  
  


