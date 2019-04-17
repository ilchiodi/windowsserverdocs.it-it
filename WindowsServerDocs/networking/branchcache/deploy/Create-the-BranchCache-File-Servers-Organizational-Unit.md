---
title: Creare l'unità organizzativa di BranchCache File server
description: In questo argomento fa parte di BranchCache distribuzione Guide per Windows Server 2016, che illustra come distribuire BranchCache in modalità cache distribuita e ospitato per ottimizzare l'utilizzo della larghezza di banda WAN nelle succursali
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 2cda192f-6b45-4e6c-88d9-70ca179ddb94
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a92cb3110e4aecb1ef09a45ed14173305722c655
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="create-the-branchcache-file-servers-organizational-unit"></a>Creare l'unità organizzativa di BranchCache File server

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questa procedura per creare un'unità organizzativa (OU) in servizi Active Directory dominio (AD DS) per BranchCache file server.  
  
Appartenenza al gruppo **Domain Admins**, o equivalente è il requisito minimo necessario per eseguire questa procedura.  
  
### <a name="to-create-the-branchcache-file-servers-organizational-unit"></a>Per creare l'unità organizzativa di file server BranchCache  
  
1.  In un computer in cui è installato Servizi di dominio Active Directory, in Server Manager, fare clic su **strumenti**, quindi fare clic su **Active Directory Users and Computers**. Apre la console Active Directory Users and Computers.  
  
2.  Nella console di Active Directory Users and Computers, fare clic sul dominio a cui si desidera aggiungere un'unità Organizzativa. Ad esempio, se il dominio è denominato example.com, fare clic destro **example.com**. Scegliere **New**, quindi fare clic su **unità organizzativa**. Il **nuovo oggetto - unità organizzativa** apre la finestra di dialogo.  
  
3.  Nel **nuovo oggetto - unità organizzativa** della finestra di dialogo **nome**, digitare un nome per la nuova unità Organizzativa. Ad esempio, se si desidera assegnare un nome di file server BranchCache unità Organizzativa, digitare **file server BranchCache**, quindi fare clic su **OK**.  
  


