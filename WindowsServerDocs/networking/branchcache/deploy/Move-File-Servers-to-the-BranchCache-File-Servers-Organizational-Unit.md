---
title: Spostare i file server nell'unità organizzativa dei file server BranchCache
description: Questo argomento fa parte di BranchCache distribuzione Guide per Windows Server 2016, che illustra come distribuire BranchCache in modalità cache distribuita e ospitato per ottimizzare l'utilizzo della larghezza di banda WAN nelle succursali
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 56c915ec-edb1-43b0-8ad2-c93841bb566f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 037b354bb6725ac7f91fc323b81bbdf15d03ac15
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885902"
---
# <a name="move-file-servers-to-the-branchcache-file-servers-organizational-unit"></a>Spostare i file server nell'unità organizzativa dei file server BranchCache

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questa procedura per aggiungere file server con BranchCache a un'unità organizzativa (OU) in Servizi di dominio Active Directory.  
  
L'appartenenza a **Domain Admins**, o equivalente è il requisito minimo necessario per eseguire questa procedura.  
  
> [!NOTE]  
> È necessario creare una OU di file server per BranchCache nella console Utenti e computer di Active Directory prima di aggiungere account computer alla OU tramite questa procedura. Per altre informazioni, vedere [creare l'unità organizzativa di File server BranchCache](../../branchcache/deploy/Create-the-BranchCache-File-Servers-Organizational-Unit.md).  
  
### <a name="to-move-file-servers-to-the-branchcache-file-servers-organizational-unit"></a>Per spostare file server nell'unità organizzativa di file server con BranchCache  
  
1.  In un computer in cui è installato Active Directory, in Server Manager, fare clic su **strumenti**, quindi fare clic su **Active Directory Users and Computers**. Verrà aperta la console Utenti e computer di Active Directory.  
  
2.  Nella console Utenti e computer di Active Directory individuare l'account computer per un file server con BranchCache, fare clic con il pulsante sinistro del mouse per selezionare l'account, quindi trascinare l'account computer sulla OU di file server con BranchCache creata precedentemente. Ad esempio, se è stata creata precedentemente una OU denominata **i file server BranchCache**e trascinare l'account computer nel **BranchCache file server** unità Organizzativa.  
  
3.  Ripetere il passaggio precedente per ogni file server con BranchCache nel dominio che si desidera trasferire nella OU.  
  


