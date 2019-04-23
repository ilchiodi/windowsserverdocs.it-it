---
title: Creare l'unità organizzativa dei file server BranchCache
description: Questo argomento fa parte di BranchCache distribuzione Guide per Windows Server 2016, che illustra come distribuire BranchCache in modalità cache distribuita e ospitato per ottimizzare l'utilizzo della larghezza di banda WAN nelle succursali
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 2cda192f-6b45-4e6c-88d9-70ca179ddb94
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b7b26ec5808f5b11141e81dc5e738c83c94ef6b8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874082"
---
# <a name="create-the-branchcache-file-servers-organizational-unit"></a>Creare l'unità organizzativa dei file server BranchCache

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questa procedura per creare un'unità organizzativa (OU) in Servizi di dominio Active Directory per file server con BranchCache.  
  
L'appartenenza a **Domain Admins**, o equivalente è il requisito minimo necessario per eseguire questa procedura.  
  
### <a name="to-create-the-branchcache-file-servers-organizational-unit"></a>Per creare l'unità organizzativa di file server con BranchCache  
  
1.  In un computer in cui è installato Active Directory, in Server Manager, fare clic su **strumenti**, quindi fare clic su **Active Directory Users and Computers**. Verrà aperta la console Utenti e computer di Active Directory.  
  
2.  Nella console Utenti e computer di Active Directory fare clic con il pulsante destro del mouse sul dominio al quale si desidera aggiungere una OU. Ad esempio, se il dominio è example.com, fare clic destro **example.com**. Scegliere **Nuovo**, quindi **Unità organizzativa**. Il **nuovo oggetto - unità organizzativa** verrà visualizzata la finestra di dialogo.  
  
3.  Nel **nuovo oggetto - unità organizzativa** della finestra di dialogo **nome**, digitare un nome per la nuova unità Organizzativa. Ad esempio, se si desidera assegnare alla OU il nome File server BranchCache, digitare **File server BranchCache**, quindi fa clic **su OK**.  
  


