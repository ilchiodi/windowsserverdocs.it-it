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
ms.author: lizross
author: eross-msft
ms.openlocfilehash: d2b98e7ccd3604de60bfabf865404506569f8c82
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80319135"
---
# <a name="enable-branchcache-on-a-file-share-optional"></a>Abilitare BranchCache in una condivisione di file (facoltativo)

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questa procedura per abilitare BranchCache su una condivisione file.  
  
> [!IMPORTANT]  
> Non è necessaria eseguire questa procedura se si configura l'impostazione di pubblicazione di hash con il valore **Consenti pubblicazione hash per tutte le cartelle condivise**.  
  
Per eseguire questa procedura è necessaria almeno l'appartenenza al gruppo **Administrators** o a un gruppo equivalente.  
  
### <a name="to-enable-branchcache-on-a-file-share"></a>Per abilitare BranchCache su una condivisione file  
  
1.  Aprire Windows PowerShell, digitare **mmc**, quindi premere INVIO. Verrà aperto Microsoft Management Console (MMC).  
  
2.  Nella console MMC scegliere **Aggiungi/Rimuovi snap-in** dal menu **File**. Verrà visualizzata la finestra di dialogo **Aggiungi o rimuovi snap-in**.  
  
3.  In **Aggiungi o Rimuovi Snap-in**, in **snap-in disponibili**, fare doppio clic su **cartelle condivise**. Verrà visualizzata la procedura guidata cartelle condivise con l'oggetto Computer locale selezionato. Configurare la visualizzazione che si preferisce, fare clic su **Fine**, quindi fare clic su **OK**.  
  
4.  Fare doppio clic su **cartelle condivise (locale)** , quindi fare clic su **condivisioni**.  
  
5.  Nel riquadro dei dettagli, fare doppio clic su una condivisione e quindi fare clic su **proprietà**. La condivisione **proprietà** verrà visualizzata la finestra di dialogo.  
  
6.  Nel **proprietà** della finestra di dialogo di **Generale** scheda, fare clic su **impostazioni non in linea**. Il **Impostazioni Offline** verrà visualizzata la finestra di dialogo.  
  
7.  Assicurarsi che **solo i file e programmi specificati dall'utente sono disponibili offline** sia selezionata e quindi fare clic su **Abilita BranchCache**.  
  
8.  Fare clic su **OK** due volte.  
  

