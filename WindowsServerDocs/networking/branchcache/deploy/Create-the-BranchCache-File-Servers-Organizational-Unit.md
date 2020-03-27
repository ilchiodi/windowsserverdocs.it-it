---
title: Creare l'unità organizzativa dei file server BranchCache
description: Questo argomento fa parte di BranchCache distribuzione Guide per Windows Server 2016, che illustra come distribuire BranchCache in modalità cache distribuita e ospitato per ottimizzare l'utilizzo della larghezza di banda WAN nelle succursali
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 2cda192f-6b45-4e6c-88d9-70ca179ddb94
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 40e871c25e31bbb15964d856da0f83cdaf50c48b
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80319309"
---
# <a name="create-the-branchcache-file-servers-organizational-unit"></a>Creare l'unità organizzativa dei file server BranchCache

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questa procedura per creare un'unità organizzativa (OU) in Servizi di dominio Active Directory per file server con BranchCache.  
  
Per eseguire questa procedura è necessaria almeno l'appartenenza al gruppo **Domain Admins** oppure a un gruppo equivalente.  
  
### <a name="to-create-the-branchcache-file-servers-organizational-unit"></a>Per creare l'unità organizzativa di file server con BranchCache  
  
1.  In un computer in cui è installato Active Directory, in Server Manager, fare clic su **strumenti**, quindi fare clic su **Active Directory Users and Computers**. Verrà aperta la console Utenti e computer di Active Directory.  
  
2.  Nella console Utenti e computer di Active Directory fare clic con il pulsante destro del mouse sul dominio al quale si desidera aggiungere una OU. Se ad esempio il nome del dominio è example.com, fare clic con il pulsante destro del mouse su **example.com**. Scegliere **Nuovo**, quindi **Unità organizzativa**. Il **nuovo oggetto - unità organizzativa** verrà visualizzata la finestra di dialogo.  
  
3.  Nel **nuovo oggetto - unità organizzativa** della finestra di dialogo **nome**, digitare un nome per la nuova unità Organizzativa. Ad esempio, se si desidera assegnare alla OU il nome File server BranchCache, digitare **File server BranchCache**, quindi fa clic **su OK**.  
  


