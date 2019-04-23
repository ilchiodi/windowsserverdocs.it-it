---
title: Creare l'oggetto Criteri di gruppo per la pubblicazione di hash di BranchCache
description: Questo argomento fa parte di BranchCache distribuzione Guide per Windows Server 2016, che illustra come distribuire BranchCache in modalità cache distribuita e ospitato per ottimizzare l'utilizzo della larghezza di banda WAN nelle succursali
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: c3d33bed-83ef-4eb8-acf9-0719ecb4a931
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 15e89b961d20b6f14065392553e413374358062d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865432"
---
# <a name="create-the-branchcache-hash-publication-group-policy-object"></a>Creare l'oggetto Criteri di gruppo per la pubblicazione di hash di BranchCache

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questa procedura per creare la pubblicazione di hash di BranchCache oggetto Criteri di gruppo (GPO).  
  
L'appartenenza a **Domain Admins**, o equivalente è il requisito minimo necessario per eseguire questa procedura.  
  
> [!NOTE]  
> Prima di eseguire questa procedura, è necessario creare l'unità organizzativa dei file server BranchCache e spostare i file server nella OU. Per ulteriori informazioni, vedere [abilitare la pubblicazione di Hash per File server membri del dominio](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md).  
  
### <a name="to-create-the-branchcache-hash-publication-group-policy-object"></a>Per creare la pubblicazione di hash di BranchCache oggetto Criteri di gruppo  
  
1.  Aprire Windows PowerShell, digitare **mmc**e quindi premere INVIO. Verrà aperto Microsoft Management Console (MMC).  
  
2.  In MMC scegliere **Aggiungi/Rimuovi snap-in** dal menu **File**. Il **Aggiungi o Rimuovi Snap-in** viene visualizzata la finestra di dialogo.  
  
3.  In **Aggiungi o rimuovi snap-in** fare doppio clic su **Gestione Criteri di gruppo** in **Snap-in disponibili** e quindi fare clic su **OK**.  
  
4.  Nello snap-in di MMC Gestione Criteri di gruppo espandere il percorso della OU dei file server BranchCache creata precedentemente. Ad esempio, se la foresta è denominata example.com, il dominio è denominato example1.com e la OU è denominata File server BranchCache, espandere il percorso seguente: **Gestione criteri di gruppo**, **foresta: example.com**, **domini**, **example1.com**, **BranchCache file server**.  
  
5.  Fare doppio clic su **file server BranchCache**, quindi fare clic su **Crea un oggetto Criteri di gruppo in questo dominio e collegarlo qui**. Verrà visualizzata la finestra di dialogo **Nuovo oggetto Criteri di gruppo**. In **nome**, digitare un nome per il nuovo oggetto Criteri di gruppo. Ad esempio, se si desidera assegnare il nome dell'oggetto pubblicazione di Hash di BranchCache, digitare **pubblicazione Hash per BranchCache**. Fare clic su **OK**.  
  


