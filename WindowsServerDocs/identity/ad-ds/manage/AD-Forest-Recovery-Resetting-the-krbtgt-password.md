---
title: Ripristino della foresta Active Directory - la reimpostazione della password krbtgt
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 3bd6c1d0-d316-4b03-b7b4-557d4537635c
ms.technology: identity-adds
ms.openlocfilehash: 1ac0dcb9da1d10a417c128cb8498a5d8362d9a9f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883742"
---
# <a name="ad-forest-recovery---resetting-the-krbtgt-password"></a>Ripristino della foresta Active Directory - la reimpostazione della password krbtgt

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Utilizzare la procedura seguente per reimpostare la password krbtgt per il dominio. La procedura seguente si applica i controller di dominio scrivibile, ma i controller di dominio non di sola lettura (RODC).
  
> [!IMPORTANT]
> Se si prevede di ripristinare i RODC online durante il ripristino della foresta, non eliminare gli account krbtgt per il controller. L'account krbtgt per un RODC è racchiuso tra il formato krbtgt_*numero*.
>
> Se si usa un filtro password personalizzato (ad esempio file Passfilt. dll) in un controller di dominio, si potrebbe ricevere un errore quando si prova a reimpostare la password krbtgt. Per altre informazioni, tra cui risolvere il problema, vedere il della Microsoft Knowledge Base [articolo 2549833](https://support.microsoft.com/kb/2549833) (https://support.microsoft.com/kb/2549833).
  
## <a name="to-reset-the-krbtgt-password"></a>Per reimpostare la password krbtgt  
  
1. Fare clic su **avviare**, scegliere **Pannello di controllo**, scegliere **strumenti di amministrazione**, quindi fare clic su **Active Directory Users and Computers**.
2. Fare clic su **Visualizza**, quindi su **Funzionalità avanzate**.
3. Nell'albero della console, fare doppio clic sul contenitore di dominio e quindi fare clic su **utenti**.
4. Nel riquadro dei dettagli, fare doppio clic il **krbtgt** account utente e quindi fare clic su **Reimposta Password**.
   ![La reimpostazione della Password](media/AD-Forest-Recovery-Resetting-the-krbtgt-password/resetpass1.png)
5. Nella **nuova password**, digitare una nuova password, confermare la password nelle **Conferma password**, quindi fare clic su **OK**. La password specificata non è significativa perché il sistema genererà una password complessa automaticamente indipendente della password specificato.
  
> [!NOTE]
> È consigliabile eseguire questa operazione due volte. La cronologia delle password dell'account krbtgt è due, ovvero che include le due password più recente. Per la reimpostazione della password due volte si cancellare in modo efficace le password precedente dalla cronologia, in modo che non è possibile un altro controller di dominio eseguirà la replica con il controller di dominio con una password vecchia.

## <a name="next-steps"></a>Passaggi successivi

- [Guida al ripristino della foresta AD](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md) 
