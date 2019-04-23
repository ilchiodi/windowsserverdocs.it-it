---
title: Ripristino della foresta Active Directory - reimpostare l'account computer nel controller di dominio
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 4e1a6070-df0a-4dfe-8773-899a010bfabd
ms.technology: identity-adds
ms.openlocfilehash: 388b460196888d4ca0cd12218972197afb6d49c5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852762"
---
# <a name="ad-forest-recovery---resetting-the-computer-account-on-the-dc"></a>Ripristino della foresta Active Directory - reimpostare l'account computer nel controller di dominio

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

 Utilizzare la procedura seguente per reimpostare la password dell'account computer del controller di dominio. 
  
## <a name="to-reset-the-computer-account-password-of-the-domain-controller"></a>Per reimpostare la password dell'account computer del controller di dominio  

1. Al prompt dei comandi digitare il comando seguente e quindi premere INVIO:  

   ```
   netdom help resetpwd  
   ```
  
2. Usare la sintassi che questo comando fornisce per utilizzando lo strumento da riga di comando Netdom per reimpostare la password dell'account computer, ad esempio:  

   ```
   netdom resetpwd /server:domain controller name /userD:administrator /passwordd:*  
   ```  
  
    In cui *nome controller di dominio* è il controller di dominio locale di ripristino. 
  
   > [!NOTE]
   > È consigliabile eseguire questo comando due volte.
  
## <a name="next-steps"></a>Passaggi successivi

- [Guida al ripristino della foresta AD](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md)
