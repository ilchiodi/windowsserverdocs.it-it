---
title: Ripristino della foresta Active Directory - reimpostare l'account computer nel controller di dominio
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 4e1a6070-df0a-4dfe-8773-899a010bfabd
ms.technology: identity-adfs
ms.openlocfilehash: 588510a27f56abb4497b2f80fa0281a858a7e8eb
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/07/2017
---
# <a name="ad-forest-recovery---resetting-the-computer-account-on-the-dc"></a>Ripristino della foresta Active Directory - reimpostare l'account computer nel controller di dominio 

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

 Utilizzare la procedura seguente per reimpostare la password dell'account computer del controller di dominio.  
  
## <a name="to-reset-the-computer-account-password-of-the-domain-controller"></a>Per reimpostare la password dell'account computer del controller di dominio  
  
1.  Al prompt dei comandi, digitare il comando seguente e quindi premere INVIO:  
  
    ```  
    netdom help resetpwd  
    ```  
  
2.  Utilizzare la sintassi che questo comando vengono per utilizzare lo strumento da riga di comando Netdom per reimpostare la password dell'account computer, ad esempio:  
  
    ```  
    netdom resetpwd /server:domain controller name /userD:administrator /passwordd:*  
    ```  
  
     Dove *nome controller di dominio* è il controller di dominio locale che si desidera ripristinare.  
  
    > [!NOTE]
    >  È necessario eseguire questo comando due volte.  
  
## <a name="next-steps"></a>Passaggi successivi

- [Guida di ripristino della foresta Active Directory](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md)
