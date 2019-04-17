---
title: Creare l'oggetto Criteri di gruppo pubblicazione Hash di BranchCache
description: In questo argomento fa parte di BranchCache distribuzione Guide per Windows Server 2016, che illustra come distribuire BranchCache in modalità cache distribuita e ospitato per ottimizzare l'utilizzo della larghezza di banda WAN nelle succursali
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: c3d33bed-83ef-4eb8-acf9-0719ecb4a931
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4510d13c806adc2db46dfccce02a5a1b2814a2c4
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="create-the-branchcache-hash-publication-group-policy-object"></a>Creare l'oggetto Criteri di gruppo pubblicazione Hash di BranchCache

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questa procedura per creare la pubblicazione di hash di BranchCache oggetto Criteri di gruppo (GPO).  
  
Appartenenza al gruppo **Domain Admins**, o equivalente è il requisito minimo necessario per eseguire questa procedura.  
  
> [!NOTE]  
> Prima di eseguire questa procedura, è necessario creare l'unità organizzativa di file server BranchCache e spostare i file server nell'unità Organizzativa. Per ulteriori informazioni, vedere [abilitare la pubblicazione di Hash per File server membri del dominio](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md).  
  
### <a name="to-create-the-branchcache-hash-publication-group-policy-object"></a>Per creare la pubblicazione di hash di BranchCache oggetto Criteri di gruppo  
  
1.  Aprire Windows PowerShell, digitare **mmc**, quindi premere INVIO. Verrà visualizzata la finestra di Microsoft Management Console (MMC).  
  
2.  In MMC nel **File** menu, fare clic su **Aggiungi/Rimuovi Snap-in**. Il **Aggiungi o Rimuovi Snap-in** apre la finestra di dialogo.  
  
3.  In **Aggiungi o Rimuovi Snap-in**, in **snap-in disponibili**, fare doppio clic su **Gestione criteri di gruppo**, quindi fare clic su **OK**.  
  
4.  In MMC Gestione criteri di gruppo, espandere il percorso ai file server BranchCache OU creata in precedenza. Ad esempio, se la foresta è denominata example.com, il dominio è denominato example1.com e la OU è denominata file server BranchCache, espandere il percorso seguente: **Gestione criteri di gruppo**, **foresta: example.com**, **domini**, **example1.com**, **file server BranchCache**.  
  
5.  Fare doppio clic su **file server BranchCache**, quindi fare clic su **crea un oggetto Criteri di gruppo in questo dominio e collegarlo qui**. Il **nuovo oggetto Criteri di gruppo** apre la finestra di dialogo. In **nome**, digitare un nome per il nuovo oggetto Criteri di gruppo. Ad esempio, se si desidera assegnare all'oggetto pubblicazione di Hash per BranchCache, digitare **pubblicazione di Hash per BranchCache**. Fare clic su **OK**.  
  


