---
title: Abilitare la pubblicazione di Hash per File server membri Non appartenenti al dominio
description: In questo argomento fa parte di BranchCache distribuzione Guide per Windows Server 2016, che illustra come distribuire BranchCache in modalità cache distribuita e ospitato per ottimizzare l'utilizzo della larghezza di banda WAN nelle succursali
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 11584b73-f9e2-4530-afa5-b8df970e6b24
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4cb3d217115cff3a9b30ee11acb7ba0de6672b24
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="enable-hash-publication-for-non-domain-member-file-servers"></a>Abilitare la pubblicazione di Hash per File server membri Non appartenenti al dominio

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

You can use this procedure to configure hash publication for BranchCache using local computer Group Policy on a file server that is running Windows Server 2016 with the **BranchCache for Network Files** role service of the File Services server role installed.  
  
This procedure is intended for use on a non-domain member file server. If you perform this procedure on a domain member file server and you also configure BranchCache using domain Group Policy, domain Group Policy settings override local Group Policy settings.  
  
Appartenenza al gruppo **amministratori**, o equivalente è il requisito minimo necessario per eseguire questa procedura.  
  
> [!NOTE]  
> If you have one or more domain member file servers, you can add them to an organizational unit (OU) in Active Directory Domain Services and then use Group Policy to configure hash publication for all of the file servers at one time, rather than individually configuring each file server. Per ulteriori informazioni, vedere [abilitare la pubblicazione di Hash per File server membri del dominio](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md).  
  
### <a name="to-enable-hash-publication-for-one-file-server"></a>To enable hash publication for one file server  
  
1.  Aprire Windows PowerShell, digitare **mmc**, quindi premere INVIO. Verrà visualizzata la finestra di Microsoft Management Console (MMC).  
  
2.  In MMC nel **File** menu, fare clic su **Aggiungi/Rimuovi Snap-in**. Il **Aggiungi o Rimuovi Snap-in** apre la finestra di dialogo.  
  
3.  In **Add or Remove Snap-ins**, in **Available snap-ins**, double-click **Group Policy Object Editor**. The Group Policy Wizard opens with the Local Computer object selected. Fare clic su **fine**, quindi fare clic su **OK**.  
  
4.  In the Local Group Policy Editor MMC, expand the following path: **Local Computer Policy**, **Computer Configuration**, **Administrative Templates**, **Network**, **Lanman Server**. Click **Lanman Server**.  
  
5.  Nel riquadro dei dettagli fare doppio clic su **pubblicazione Hash per BranchCache**. Il **pubblicazione Hash per BranchCache** apre la finestra di dialogo.  
  
6.  Nel **pubblicazione Hash per BranchCache** la finestra di dialogo, fare clic su **abilitato**.  
  
7.  In **opzioni**, fare clic su **Consenti pubblicazione hash per tutte le cartelle condivise**, quindi fare clic su uno dei seguenti:  
  
    1.  To enable hash publication for all shared folders on this computer, click **Allow hash publication for all shared folders**.  
  
    2.  Per abilitare la pubblicazione di hash solo per le cartelle condivise per cui è abilitata BranchCache, fare clic su **Consenti pubblicazione hash solo per le cartelle condivise in cui è attivato BranchCache**.  
  
    3.  Per disattivare la pubblicazione di hash per tutte le cartelle condivise nel computer anche se BranchCache è abilitata sulle condivisioni di file, fare clic su **non consentire pubblicazione di hash su tutte le cartelle condivise**.  
  
8.  Fare clic su **OK**.  
  


