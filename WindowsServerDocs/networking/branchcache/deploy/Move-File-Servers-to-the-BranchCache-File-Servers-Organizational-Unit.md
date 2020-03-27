---
title: Spostare i file server nell'unità organizzativa dei file server BranchCache
description: Questo argomento fa parte di BranchCache distribuzione Guide per Windows Server 2016, che illustra come distribuire BranchCache in modalità cache distribuita e ospitato per ottimizzare l'utilizzo della larghezza di banda WAN nelle succursali
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 56c915ec-edb1-43b0-8ad2-c93841bb566f
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 045764f9ed7cb7eef5a996748e293b48ec6018fb
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80319200"
---
# <a name="move-file-servers-to-the-branchcache-file-servers-organizational-unit"></a>Spostare i file server nell'unità organizzativa dei file server BranchCache

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questa procedura per aggiungere file server con BranchCache a un'unità organizzativa (OU) in Servizi di dominio Active Directory.  
  
Per eseguire questa procedura è necessaria almeno l'appartenenza al gruppo **Domain Admins** oppure a un gruppo equivalente.  
  
> [!NOTE]  
> È necessario creare una OU di file server per BranchCache nella console Utenti e computer di Active Directory prima di aggiungere account computer alla OU tramite questa procedura. Per ulteriori informazioni, vedere [creare l'unità organizzativa di file server con BranchCache](../../branchcache/deploy/Create-the-BranchCache-File-Servers-Organizational-Unit.md).  
  
### <a name="to-move-file-servers-to-the-branchcache-file-servers-organizational-unit"></a>Per spostare file server nell'unità organizzativa di file server con BranchCache  
  
1.  In un computer in cui è installato Active Directory, in Server Manager, fare clic su **strumenti**, quindi fare clic su **Active Directory Users and Computers**. Verrà aperta la console Utenti e computer di Active Directory.  
  
2.  Nella console Utenti e computer di Active Directory individuare l'account computer per un file server con BranchCache, fare clic con il pulsante sinistro del mouse per selezionare l'account, quindi trascinare l'account computer sulla OU di file server con BranchCache creata precedentemente. Se, ad esempio, in precedenza è stata creata una OU denominata **file server BranchCache**, trascinare l'account computer nell'unità organizzativa di **file server con BranchCache** .  
  
3.  Ripetere il passaggio precedente per ogni file server con BranchCache nel dominio che si desidera trasferire nella OU.  
  


