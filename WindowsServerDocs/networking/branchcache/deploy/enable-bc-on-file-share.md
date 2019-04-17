---
title: Abilitare BranchCache su una condivisione di File (facoltativa)
description: In questo argomento fa parte di BranchCache distribuzione Guide per Windows Server 2016, che illustra come distribuire BranchCache in modalità cache distribuita e ospitato per ottimizzare l'utilizzo della larghezza di banda WAN nelle succursali
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-bc
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 9c465a9e-c504-44ec-9ebc-4e06ba54db30
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 33ed40ef91d9389bb7940dcf928cba43f0c9dbd2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="enable-branchcache-on-a-file-share-optional"></a>Abilitare BranchCache su una condivisione di File (facoltativa)

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

You can use this procedure to enable BranchCache on a file share.  
  
> [!IMPORTANT]  
> You do not need to perform this procedure if you configure the hash publication setting with the value **Allow hash publication for all shared folders**.  
  
Appartenenza al gruppo **amministratori**, o equivalente è il requisito minimo necessario per eseguire questa procedura.  
  
### <a name="to-enable-branchcache-on-a-file-share"></a>To enable BranchCache on a file share  
  
1.  Aprire Windows PowerShell, digitare **mmc**, quindi premere INVIO. Verrà visualizzata la finestra di Microsoft Management Console (MMC).  
  
2.  In MMC nel **File** menu, fare clic su **Aggiungi/Rimuovi Snap-in**. Il **Aggiungi o Rimuovi Snap-in** apre la finestra di dialogo.  
  
3.  In **Add or Remove Snap-ins**, in **Available snap-ins**, double-click **Shared Folders**. The Shared Folders Wizard opens with the Local Computer object selected. Configure the View that you prefer, click **Finish**, and then click **OK**.  
  
4.  Double-click **Shared Folders (Local)**, and then click **Shares**.  
  
5.  In the details pane, right-click a share, and then click **Properties**. The share's **Properties** dialog box opens.  
  
6.  In the **Properties** dialog box, on the **General** tab, click **Offline Settings**. The **Offline Settings** dialog box opens.  
  
7.  Ensure that **Only the files and programs that users specify are available offline** is selected, and then click **Enable BranchCache**.  
  
8.  Fare clic su **OK** due volte.  
  

