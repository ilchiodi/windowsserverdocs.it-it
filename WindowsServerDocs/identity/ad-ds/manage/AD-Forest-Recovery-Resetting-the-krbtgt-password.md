---
title: Ripristino della foresta di Active Directory-reimpostazione della password KRBTGT
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 3bd6c1d0-d316-4b03-b7b4-557d4537635c
ms.technology: identity-adds
ms.openlocfilehash: e2d19621c80b40fa70706bf75434dce74693990d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80823634"
---
# <a name="ad-forest-recovery---resetting-the-krbtgt-password"></a>Ripristino della foresta di Active Directory-reimpostazione della password KRBTGT

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Utilizzare la seguente procedura per reimpostare la password di KRBTGT per il dominio. Nella procedura seguente vengono applicati i controller di dominio scrivibili, ma non i controller di dominio di sola lettura (RODC).
  
> [!IMPORTANT]
> Se si prevede di ripristinare RODC online durante il ripristino della foresta, non eliminare gli account krbtgt per RODC. L'account krbtgt per un RODC è elencato nel formato krbtgt_*numero*.
>
> Se si usa un filtro personalizzato per la password, ad esempio file Passfilt. dll, in un controller di dominio, è possibile che venga visualizzato un errore quando si tenta di reimpostare la password di krbtgt. Per ulteriori informazioni, inclusa una soluzione alternativa, vedere l' [articolo 2549833](https://support.microsoft.com/kb/2549833) della Microsoft Knowledge Base (https://support.microsoft.com/kb/2549833).
  
## <a name="to-reset-the-krbtgt-password"></a>Per reimpostare la password di KRBTGT  
  
1. Fare clic sul pulsante **Start**, scegliere **Pannello di controllo**, **strumenti di amministrazione**, quindi fare clic su **Active Directory utenti e computer**.
2. Fare clic su **Visualizza**, quindi su **Funzionalità avanzate**.
3. Nell'albero della console fare doppio clic sul contenitore di dominio, quindi fare clic su **utenti**.
4. Nel riquadro dei dettagli fare clic con il pulsante destro del mouse sull'account utente **krbtgt** , quindi scegliere **Reimposta password**.
   ![reimpostare la password](media/AD-Forest-Recovery-Resetting-the-krbtgt-password/resetpass1.png)
5. In **nuova password**Digitare una nuova password, digitare nuovamente la password in **Conferma password**e quindi fare clic su **OK**. La password specificata non è significativa perché il sistema genererà automaticamente una password complessa indipendentemente dalla password specificata.
  
> [!NOTE]
> Questa operazione deve essere eseguita due volte. La cronologia delle password dell'account KRBTGT è due, ovvero include le due password più recenti. Reimpostando la password due volte è possibile cancellare efficacemente le password obsolete dalla cronologia, quindi non esiste alcun modo in cui un altro controller di dominio verrà replicato con il controller di dominio utilizzando una vecchia password.

## <a name="next-steps"></a>Passaggi successivi

- [Guida al ripristino della foresta di Active Directory](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta di Active Directory - Procedure](AD-Forest-Recovery-Procedures.md) 
