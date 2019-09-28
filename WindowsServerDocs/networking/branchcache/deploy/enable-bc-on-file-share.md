---
title: Abilitare BranchCache in una condivisione di file (facoltativo)
description: Questo argomento fa parte di BranchCache distribuzione Guide per Windows Server 2016, che illustra come distribuire BranchCache in modalità cache distribuita e ospitato per ottimizzare l'utilizzo della larghezza di banda WAN nelle succursali
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-bc
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 9c465a9e-c504-44ec-9ebc-4e06ba54db30
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 37bab11a0914a3f6854314016bb59297aa6954f2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406360"
---
# <a name="enable-branchcache-on-a-file-share-optional"></a>Abilitare BranchCache in una condivisione di file (facoltativo)

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questa procedura per abilitare BranchCache su una condivisione file.  
  
> [!IMPORTANT]  
> Non è necessaria eseguire questa procedura se si configura l'impostazione di pubblicazione di hash con il valore **Consenti pubblicazione hash per tutte le cartelle condivise**.  
  
L'appartenenza a **amministratori**, o equivalente è il requisito minimo necessario per eseguire questa procedura.  
  
### <a name="to-enable-branchcache-on-a-file-share"></a>Per abilitare BranchCache su una condivisione file  
  
1.  Aprire Windows PowerShell, digitare **mmc**e quindi premere INVIO. Verrà aperto Microsoft Management Console (MMC).  
  
2.  In MMC scegliere **Aggiungi/Rimuovi snap-in** dal menu **File**. Il **Aggiungi o Rimuovi Snap-in** viene visualizzata la finestra di dialogo.  
  
3.  In **Aggiungi o Rimuovi Snap-in**, in **snap-in disponibili**, fare doppio clic su **cartelle condivise**. Verrà visualizzata la procedura guidata cartelle condivise con l'oggetto Computer locale selezionato. Configurare la visualizzazione che si preferisce, fare clic su **Fine**, quindi fare clic su **OK**.  
  
4.  Fare doppio clic su **cartelle condivise (locale)** , quindi fare clic su **condivisioni**.  
  
5.  Nel riquadro dei dettagli, fare doppio clic su una condivisione e quindi fare clic su **proprietà**. La condivisione **proprietà** verrà visualizzata la finestra di dialogo.  
  
6.  Nel **proprietà** della finestra di dialogo di **Generale** scheda, fare clic su **impostazioni non in linea**. Il **Impostazioni Offline** verrà visualizzata la finestra di dialogo.  
  
7.  Assicurarsi che **solo i file e programmi specificati dall'utente sono disponibili offline** sia selezionata e quindi fare clic su **Abilita BranchCache**.  
  
8.  Fare clic su **OK** due volte.  
  

