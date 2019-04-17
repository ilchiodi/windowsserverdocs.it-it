---
title: Configurare l'oggetto Criteri di gruppo pubblicazione Hash di BranchCache
description: In questo argomento fa parte di BranchCache distribuzione Guide per Windows Server 2016, che illustra come distribuire BranchCache in modalità cache distribuita e ospitato per ottimizzare l'utilizzo della larghezza di banda WAN nelle succursali
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: da74fea7-52b2-4d6d-9d21-19184eedbe3c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6f528c9f0a8a39b286ad7ac4fa728d93c311f779
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="configure-the-branchcache-hash-publication-group-policy-object"></a>Configurare l'oggetto Criteri di gruppo pubblicazione Hash di BranchCache

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questa procedura per configurare la pubblicazione di hash di BranchCache oggetto Criteri di gruppo (GPO) in modo che tutti i file server che è stato aggiunto all'unità Organizzativa hanno la stessa impostazione criteri pubblicazione hash applicata.  
  
Appartenenza al gruppo **Domain Admins**, o equivalente è il requisito minimo necessario per eseguire questa procedura.  
  
> [!NOTE]  
> Prima di eseguire questa procedura, è necessario creare l'unità file server BranchCache aziendale, spostare i file server nell'unità Organizzativa e creare la pubblicazione di hash di BranchCache oggetto Criteri di gruppo. Per ulteriori informazioni, vedere [abilitare la pubblicazione di Hash per File server membri del dominio](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md).  
  
### <a name="to-configure-the-branchcache-hash-publication-group-policy-object"></a>Per configurare la pubblicazione di hash di BranchCache oggetto Criteri di gruppo  
  
1.  Eseguire Windows PowerShell come amministratore, digitare **mmc**, quindi premere INVIO. Verrà visualizzata la finestra di Microsoft Management Console (MMC).  
  
2.  In MMC nel **File** menu, fare clic su **Aggiungi/Rimuovi Snap-in**. Il **Aggiungi o Rimuovi Snap-in** apre la finestra di dialogo.  
  
3.  In **Aggiungi o Rimuovi Snap-in**, in **snap-in disponibili**, fare doppio clic su **Gestione criteri di gruppo**, quindi fare clic su **OK**.  
  
4.  In MMC Gestione criteri di gruppo, espandere il percorso per la pubblicazione di hash di BranchCache oggetto Criteri di gruppo creato in precedenza. Ad esempio, se la foresta è denominata example.com, il dominio è denominato example1.com e l'oggetto Criteri di gruppo è denominato **pubblicazione di Hash per BranchCache**, espandere il percorso seguente: **Gestione criteri di gruppo**, **foresta: example.com**, **domini**, **example1.com**, **oggetti Criteri di gruppo**, **pubblicazione di Hash per BranchCache**.  
  
5.  Fare doppio clic su di **pubblicazione di Hash per BranchCache** oggetto Criteri di gruppo e fare clic su **modifica**. Apre la console di Editor Gestione criteri di gruppo.  
  
6.  Nella console di Editor Gestione criteri di gruppo, espandere il percorso seguente: **configurazione Computer**, **criteri**, **modelli amministrativi**, **rete**, **Lanman Server**.  
  
7.  Nella console di Editor Gestione criteri di gruppo, fare clic su **Lanman Server**. Nel riquadro dei dettagli fare doppio clic su **pubblicazione Hash per BranchCache**. Il **pubblicazione Hash per BranchCache** apre la finestra di dialogo.  
  
8.  Nel **pubblicazione Hash per BranchCache** la finestra di dialogo, fare clic su **abilitato**.  
  
9. In **opzioni**, fare clic su **Consenti pubblicazione hash per tutte le cartelle condivise**, quindi fare clic su uno dei seguenti:  
  
    1.  Per abilitare la pubblicazione di hash per tutte le cartelle condivise per tutti i server aggiunti all'unità Organizzativa di file, fare clic su **Consenti pubblicazione hash per tutte le cartelle condivise**.  
  
    2.  Per abilitare la pubblicazione di hash solo per le cartelle condivise per cui è abilitata BranchCache, fare clic su **Consenti pubblicazione hash solo per le cartelle condivise in cui è attivato BranchCache**.  
  
    3.  Per disattivare la pubblicazione di hash per tutte le cartelle condivise nel computer anche se BranchCache è abilitata sulle condivisioni di file, fare clic su **non consentire pubblicazione di hash su tutte le cartelle condivise**.  
  
10. Fare clic su **OK**.  
  
> [!NOTE]  
> Nella maggior parte dei casi, è necessario salvare la console MMC e aggiornare la visualizzazione per visualizzare le modifiche di configurazione apportate.  
  


