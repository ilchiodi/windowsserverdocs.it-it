---
title: Spostare i File server per l'unità organizzativa di BranchCache File server
description: In questo argomento fa parte di BranchCache distribuzione Guide per Windows Server 2016, che illustra come distribuire BranchCache in modalità cache distribuita e ospitato per ottimizzare l'utilizzo della larghezza di banda WAN nelle succursali
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 56c915ec-edb1-43b0-8ad2-c93841bb566f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 09afb4936545cb1f5bb14573261008ff18badd4d
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="move-file-servers-to-the-branchcache-file-servers-organizational-unit"></a>Spostare i File server per l'unità organizzativa di BranchCache File server

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questa procedura per aggiungere file server BranchCache a un'unità organizzativa (OU) in servizi di dominio Active Directory (AD DS).  
  
Appartenenza al gruppo **Domain Admins**, o equivalente è il requisito minimo necessario per eseguire questa procedura.  
  
> [!NOTE]  
> Prima di aggiungere account computer alla OU tramite questa procedura, è necessario creare un'unità Organizzativa di file server BranchCache nella console di Active Directory Users and Computers. Per ulteriori informazioni, vedere [creare l'unità organizzativa di File server BranchCache](../../branchcache/deploy/Create-the-BranchCache-File-Servers-Organizational-Unit.md).  
  
### <a name="to-move-file-servers-to-the-branchcache-file-servers-organizational-unit"></a>Per spostare file server BranchCache file l'unità organizzativa dei server  
  
1.  In un computer in cui è installato Servizi di dominio Active Directory, in Server Manager, fare clic su **strumenti**, quindi fare clic su **Active Directory Users and Computers**. Apre la console Active Directory Users and Computers.  
  
2.  Nella console di Active Directory Users and Computers, individuare l'account computer per un file server BranchCache, fare clic per selezionare l'account, quindi trascinare e rilasciare l'account computer nei file server BranchCache OU creata in precedenza. Ad esempio, se è stata creata precedentemente una OU denominata **file server BranchCache**, trascinare e rilasciare l'account computer nel **file server BranchCache** unità Organizzativa.  
  
3.  Ripetere il passaggio precedente per ogni file server BranchCache nel dominio che si desidera spostare l'unità Organizzativa.  
  


