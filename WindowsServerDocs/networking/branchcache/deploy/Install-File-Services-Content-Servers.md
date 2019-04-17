---
title: Installare server di contenuti di servizi File
description: In questo argomento fa parte di BranchCache distribuzione Guide per Windows Server 2016, che illustra come distribuire BranchCache in modalità cache distribuita e ospitato per ottimizzare l'utilizzo della larghezza di banda WAN nelle succursali
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 74b0a5ed-dc20-4974-9d4b-2426987a01a1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7036323e7cc31bd14be8025b6064a806fb45ef19
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="install-file-services-content-servers"></a>Installare server di contenuti di servizi File

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Per distribuire server di contenuti che eseguono il ruolo server Servizi File, è necessario installare il BranchCache per il servizio ruolo file di rete del ruolo server Servizi File. Inoltre, è necessario abilitare BranchCache sulle condivisioni file in base alle esigenze.  
  
Durante la configurazione del server di contenuti, è possibile consentire a BranchCache di pubblicare contenuto per tutte le condivisioni file oppure è possibile selezionare un sottoinsieme di condivisioni file da pubblicare.  
  
> [!NOTE]  
> Quando si distribuisce un file abilitato server BranchCache server Web o come server di contenuti, informazioni sul contenuto viene ora calcolate non in linea, anche prima che un client con BranchCache richiede un file. A causa di questo miglioramento, non è necessario configurare la pubblicazione di hash per i server di contenuti, come hai fatto nella versione precedente di BranchCache.  
>   
> Questa generazione hash automatica fornisce prestazioni più veloci e ulteriori risparmi di larghezza di banda, perché le informazioni sul contenuto è pronte per il primo client che richiede il contenuto e i calcoli sono già stati eseguiti.  
  
Vedere gli argomenti seguenti per distribuire server di contenuti.  
  
-   [Configurare il ruolo server Servizi File](../../branchcache/deploy/Configure-the-File-Services-server-role.md)  
  
-   [Abilitare la pubblicazione di Hash per File server](../../branchcache/deploy/Enable-Hash-Publication-for-File-Servers.md)  
  
-   [Abilitare BranchCache su una condivisione File & #40; facoltativo & #41;](../../branchcache/deploy/enable-bc-on-file-share.md)  
  


