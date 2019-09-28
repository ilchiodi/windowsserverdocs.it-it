---
title: Abilitare la pubblicazione di hash per i file server non appartenenti al dominio
description: Questo argomento fa parte di BranchCache distribuzione Guide per Windows Server 2016, che illustra come distribuire BranchCache in modalità cache distribuita e ospitato per ottimizzare l'utilizzo della larghezza di banda WAN nelle succursali
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 11584b73-f9e2-4530-afa5-b8df970e6b24
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e870863b497c17b4b56265d99d91274e34690767
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356544"
---
# <a name="enable-hash-publication-for-non-domain-member-file-servers"></a>Abilitare la pubblicazione di hash per i file server non appartenenti al dominio

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questa procedura per configurare la pubblicazione di hash per BranchCache utilizzando criteri di gruppo del computer locali in un file server che esegue Windows Server 2016 con il **BranchCache per file di rete** servizio ruolo del ruolo server Servizi File installato.  
  
Questa procedura è destinata all'utilizzo su un file server non appartenente al dominio. Se si esegue questa procedura in un file server membro di un dominio e si configura inoltre BranchCache utilizzando Criteri di gruppo di dominio, le impostazioni di Criteri di gruppo di dominio sostituiranno le impostazioni di Criteri di gruppo locali.  
  
L'appartenenza a **amministratori**, o equivalente è il requisito minimo necessario per eseguire questa procedura.  
  
> [!NOTE]  
> Se si dispone uno o più di file server membri di dominio, è possibile aggiungerli a un'unità organizzativa (OU) in Servizi di dominio Active Directory e utilizzare quindi Criteri di gruppo per configurare contemporaneamente la pubblicazione di hash per tutti i file server, anziché che configurare singolarmente ogni file server. Per ulteriori informazioni, vedere [abilitare la pubblicazione di Hash per File server membri del dominio](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md).  
  
### <a name="to-enable-hash-publication-for-one-file-server"></a>Per abilitare la pubblicazione di hash per un file server  
  
1.  Aprire Windows PowerShell, digitare **mmc**e quindi premere INVIO. Verrà aperto Microsoft Management Console (MMC).  
  
2.  In MMC scegliere **Aggiungi/Rimuovi snap-in** dal menu **File**. Il **Aggiungi o Rimuovi Snap-in** viene visualizzata la finestra di dialogo.  
  
3.  In **Aggiungi o rimuovi Snap-in** fare doppio clic su **Editor oggetti Criteri di gruppo** in **Snap-in disponibili**. Verrà visualizzata la Procedura guidata Criteri di gruppo con l'oggetto Computer Locale selezionato. Fare clic su **Fine**e quindi su **OK**.  
  
4.  Nello snap-in MMC Editor Criteri di gruppo locali espandere il percorso seguente: **Criteri del computer locale**, **configurazione computer**, **modelli amministrativi**, **rete**, **LanMan Server**. Fare clic su **Server Lanman**.  
  
5.  Nel riquadro dei dettagli fare doppio clic su **pubblicazione Hash per BranchCache**. Verrà visualizzata la finestra di dialogo **Pubblicazione hash per BranchCache**.  
  
6.  Nella finestra di dialogo **Pubblicazione hash per BranchCache** fare clic su **Abilitata**.  
  
7.  In **Opzioni**, fare clic su **Consenti pubblicazione hash per tutte le cartelle condivise**, quindi fare clic su uno dei seguenti:  
  
    1.  Per abilitare la pubblicazione di hash per tutte le cartelle condivise sul computer, fare clic su **Consenti pubblicazione hash per tutte le cartelle condivise**.  
  
    2.  Per abilitare la pubblicazione di hash solo per le cartelle condivise per le quali è abilitata BranchCache, fare clic su **Consenti pubblicazione hash solo per cartelle condivise per cui BranchCache e` abilitata**.  
  
    3.  Per impedire la pubblicazione di hash per tutte le cartelle condivise nel computer anche se BranchCache è abilitata sulle condivisioni di file, fare clic **Non consentire pubblicazione hash per tutte le cartelle condivise**.  
  
8.  Fare clic su **OK**.  
  


