---
title: Configurare l'oggetto Criteri di gruppo per la pubblicazione di hash di BranchCache
description: Questo argomento fa parte di BranchCache distribuzione Guide per Windows Server 2016, che illustra come distribuire BranchCache in modalità cache distribuita e ospitato per ottimizzare l'utilizzo della larghezza di banda WAN nelle succursali
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: da74fea7-52b2-4d6d-9d21-19184eedbe3c
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 660a4979aa5e29a6dd22cb2e0e6659966d077a8e
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80319333"
---
# <a name="configure-the-branchcache-hash-publication-group-policy-object"></a>Configurare l'oggetto Criteri di gruppo per la pubblicazione di hash di BranchCache

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Questa procedura consente di configurare la pubblicazione di hash di BranchCache oggetto Criteri di gruppo (GPO) in modo che tutti i file server che sono aggiunti all'unità Organizzativa hanno la stessa impostazione criteri pubblicazione hash applicata.  
  
Per eseguire questa procedura è necessaria almeno l'appartenenza al gruppo **Domain Admins** oppure a un gruppo equivalente.  
  
> [!NOTE]  
> Prima di eseguire questa procedura, è necessario creare l'unità file server BranchCache aziendale, spostare i file server nell'unità Organizzativa e creare la pubblicazione di hash di BranchCache oggetto Criteri di gruppo. Per ulteriori informazioni, vedere [abilitare la pubblicazione di Hash per File server membri del dominio](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md).  
  
### <a name="to-configure-the-branchcache-hash-publication-group-policy-object"></a>Per configurare la pubblicazione di hash di BranchCache oggetto Criteri di gruppo  
  
1.  Eseguire Windows PowerShell come amministratore, di tipo **mmc**, quindi premere INVIO. Verrà aperto Microsoft Management Console (MMC).  
  
2.  Nella console MMC scegliere **Aggiungi/Rimuovi snap-in** dal menu **File**. Verrà visualizzata la finestra di dialogo **Aggiungi o rimuovi snap-in**.  
  
3.  In **Aggiungi o rimuovi snap-in** fare doppio clic su **Gestione Criteri di gruppo** in **Snap-in disponibili** e quindi fare clic su **OK**.  
  
4.  Nello snap-in di MMC Gestione Criteri di gruppo espandere il percorso dell'oggetto Criteri di gruppo Pubblicazione di hash per BranchCache creato precedentemente. Ad esempio, se la foresta è denominata example.com, il dominio è denominato example1. com e l'oggetto Criteri di gruppo è denominato **pubblicazione Hash per BranchCache**, espandere il percorso seguente: **Gestione criteri di gruppo**, **foresta: example.com**, **domini**, **esempio1**, **oggetti Criteri di gruppo**, **pubblicazione Hash per BranchCache**.  
  
5.  Fare clic con il pulsante destro del mouse sull'oggetto Criteri di gruppo **Pubblicazione di hash per BranchCache** e scegliere **Modifica**. Si apre la console Editor Gestione Criteri di gruppo.  
  
6.  Nella console di Editor Gestione criteri di gruppo, espandere il percorso seguente: **Configurazione Computer**, **criteri**, **modelli amministrativi**, **rete**, **Lanman Server**.  
  
7.  Nella console Editor Gestione Criteri di gruppo fare clic su **Lanman Server**. Nel riquadro dei dettagli fare doppio clic su **Pubblicazione hash per BranchCache**. Verrà visualizzata la finestra di dialogo **Pubblicazione hash per BranchCache**.  
  
8.  Nella finestra di dialogo **Pubblicazione hash per BranchCache** fare clic su **Abilitata**.  
  
9. In **Opzioni**, fare clic su **Consenti pubblicazione hash per tutte le cartelle condivise**, quindi fare clic su uno dei seguenti:  
  
    1.  Per abilitare la pubblicazione di hash per tutte le cartelle condivise per tutti i server aggiunti all'unità Organizzativa di file, fare clic su **Consenti pubblicazione hash per tutte le cartelle condivise**.  
  
    2.  Per abilitare la pubblicazione di hash solo per le cartelle condivise per le quali è abilitata BranchCache, fare clic su **Consenti pubblicazione hash solo per cartelle condivise per cui BranchCache e` abilitata**.  
  
    3.  Per impedire la pubblicazione di hash per tutte le cartelle condivise nel computer anche se BranchCache è abilitata sulle condivisioni di file, fare clic **Non consentire pubblicazione hash per tutte le cartelle condivise**.  
  
10. Fare clic su **OK**.  
  
> [!NOTE]  
> Nella maggior parte dei casi è necessario salvare la console MMC e aggiornare la visualizzazione per vedere le modifiche di configurazione apportate.  
  


