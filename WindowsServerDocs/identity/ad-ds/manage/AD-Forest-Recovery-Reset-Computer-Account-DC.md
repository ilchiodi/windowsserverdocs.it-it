---
title: Ripristino della foresta di Active Directory-reimpostazione dell'account computer nel controller di dominio
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 4e1a6070-df0a-4dfe-8773-899a010bfabd
ms.technology: identity-adds
ms.openlocfilehash: 320d02f789b8dda771b648b0aa9a58f545f3358b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71368874"
---
# <a name="ad-forest-recovery---resetting-the-computer-account-on-the-dc"></a>Ripristino della foresta di Active Directory-reimpostazione dell'account computer nel controller di dominio

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

 Utilizzare la seguente procedura per reimpostare la password dell'account computer del controller di dominio. 
  
## <a name="to-reset-the-computer-account-password-of-the-domain-controller"></a>Per reimpostare la password dell'account computer del controller di dominio  

1. Al prompt dei comandi digitare il comando seguente e quindi premere INVIO:  

   ```
   netdom help resetpwd  
   ```
  
2. Utilizzare la sintassi fornita da questo comando per l'utilizzo dello strumento da riga di comando Netdom per reimpostare la password dell'account computer, ad esempio:  

   ```
   netdom resetpwd /server:domain controller name /userD:administrator /passwordd:*  
   ```  
  
    Dove il *nome del controller di dominio* è il controller di dominio locale da ripristinare. 
  
   > [!NOTE]
   > Eseguire questo comando due volte.
  
## <a name="next-steps"></a>Passaggi successivi

- [Guida al ripristino della foresta di Active Directory](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta di Active Directory - Procedure](AD-Forest-Recovery-Procedures.md)
