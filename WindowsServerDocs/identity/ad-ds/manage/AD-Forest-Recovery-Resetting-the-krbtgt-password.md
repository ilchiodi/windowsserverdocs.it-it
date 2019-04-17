---
title: Ripristino della foresta Active Directory - la reimpostazione della password krbtgt
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 3bd6c1d0-d316-4b03-b7b4-557d4537635c
ms.technology: identity-adfs
ms.openlocfilehash: 445fa18503e26d04e20a61cfe652424f78631abe
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---resetting-the-krbtgt-password"></a>Ripristino della foresta Active Directory - la reimpostazione della password krbtgt 

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

 Utilizzare la procedura seguente per reimpostare la password krbtgt per il dominio. La procedura seguente si applica a controller di dominio scrivibile, ma il controller di dominio non di sola lettura (RODC).  
  
> [!IMPORTANT]
>  Se si intende ripristinare lettura online durante il ripristino della foresta, non eliminare gli account krbtgt per la lettura. L'account krbtgt per un controller di dominio è elencato nella krbtgt_ formato*numero *.  
>   
>  Se si utilizza un filtro password personalizzato (ad esempio passfilt.dll) in un controller di dominio, potresti ricevere un errore quando si tenta di reimpostare la password krbtgt. Per ulteriori informazioni, relativa soluzione, vedere l'articolo della Microsoft Knowledge Base [articolo 2549833](https://support.microsoft.com/kb/2549833) (https://support.microsoft.com/kb/2549833).  
  
## <a name="to-reset-the-krbtgt-password"></a>Per reimpostare la password krbtgt  
  
1.  Fare clic su **Start**, scegliere **Pannello di controllo**, scegliere **strumenti di amministrazione**, quindi fare clic su **Active Directory Users and Computers **.  
2.  2.  Fare clic su **visualizzazione**, quindi fare clic su **funzionalità avanzate **.  
3.  Nell'albero della console, fare doppio clic il contenitore di dominio e quindi fare clic su **utenti **.  
4.  Nel riquadro dei dettagli fare doppio clic su di **krbtgt** account utente, quindi fare clic su **Reimposta Password **.  
![Reimpostazione Password](media/AD-Forest-Recovery-Resetting-the-krbtgt-password/resetpass1.png)
5.  In **nuova password**, digitare una nuova password, la password in **Conferma password**, quindi fare clic su **OK **. La password specificati non è significativo perché il sistema genera una password complessa indipendente automaticamente la password specificati.  
  
    > [!NOTE]
    >  È consigliabile eseguire questa operazione due volte. Cronologia delle password dell'account krbtgt è due, vale a dire che include le due password più recente. Per reimpostare la password due volte cancellare in modo efficace qualsiasi password precedenti dalla cronologia, in modo che non è un altro controller di dominio eseguirà la replica con il controller di dominio utilizzando una vecchia password.  
 
## <a name="next-steps"></a>Passaggi successivi

- [Guida di ripristino della foresta Active Directory](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md) 
  
