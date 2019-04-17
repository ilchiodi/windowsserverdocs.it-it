---
title: Ripristino della foresta Active Directory - pulizia dei metadati di controller di dominio rimossi
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: e7543381-4081-407f-adad-a9de792c6616
ms.technology: identity-adfs
ms.openlocfilehash: 3027c59b58801b44d20127e6bcf62dd7319708bd
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---cleaning-metadata-of-removed-writable-domain-controllers"></a>Ripristino della foresta Active Directory - pulizia dei metadati di controller di dominio scrivibili di 

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2
 
 Pulizia dei metadati rimuove i dati di Active Directory che identifica un controller di dominio per il sistema di replica.  
  
 Utilizzare la seguente procedura per eliminare gli oggetti controller di dominio per i controller di dominio che si prevede di aggiungere nuovamente alla rete tramite la reinstallazione di dominio Active Directory.  
  
 Se si utilizza la versione di Active Directory Users and Computers o siti di Active Directory e servizi che è incluso strumenti di amministrazione remota Server (RSAT), la pulizia dei metadati viene eseguita automaticamente quando si elimina un oggetto controller di dominio.  
  

## <a name="deleting-a-domain-controller-using-active-directory-users-and-computers"></a>L'eliminazione di un controller di dominio con Active Directory Users and Computers  
 Quando si utilizza la versione di Active Directory Users e computer o centro di amministrazione in remoto Server strumenti di amministrazione di Active Directory, la pulizia dei metadati viene eseguita automaticamente quando si elimina l'oggetto controller di dominio. L'oggetto server e l'oggetto computer vengono eliminati anche automaticamente.  
  
 In alternativa, è possibile anche utilizzare Active Directory Sites and Services in amministrazione remota del server per eliminare un oggetto controller di dominio. Se si utilizza Active Directory Sites and Services, è necessario eliminare l'oggetto server associato e un oggetto impostazioni NTDS prima di poter eliminare l'oggetto controller di dominio.  
  
 Per scaricare l'amministrazione remota del server:  

-   [Strumenti di amministrazione Server remota per Windows 10](https://www.microsoft.com/download/details.aspx?id=45520)
  
-   [Strumenti di amministrazione Server remota per Windows 8](https://www.microsoft.com/download/details.aspx?id=28972)  

-   [Strumenti di amministrazione Server remota per Windows 7 con Service Pack 1 (SP1)](https://www.microsoft.com/download/details.aspx?id=7887)  
  
-   [Strumenti di amministrazione Server remoto Microsoft per Windows Vista](https://www.microsoft.com/download/details.aspx?id=21090)  
  
 La procedura seguente è lo stesso per i controller di dominio che eseguono uno dei due Windows Server 2016, 2012, 2008 R2 o 2008. Il controller di dominio di destinazione dell'operazione di pulizia dei metadati possa eseguire qualsiasi versione di Windows Server.  
  
### <a name="to-delete-a-domain-controller-object-using-active-directory-users-and-computers-in-rsat"></a>Per eliminare un oggetto controller di dominio con Active Directory Users and Computers in amministrazione remota del server  
  
1.  Fare clic su **Start**, fare clic su **strumenti di amministrazione**, quindi fare clic su **Active Directory Users and Computers**.  
2.  Nell'albero della console, fare doppio clic il contenitore di dominio e quindi fare doppio clic il **i controller di dominio** unità organizzativa (OU).  
3.  Nel riquadro dei dettagli fare doppio clic su controller di dominio che si desidera eliminare e quindi fare clic su **eliminare**. 
![Eliminare](media/AD-Forest-Recovery-Cleaning-Metadata/delete1.png) 
4.  Fare clic su **Sì** per confermare l'eliminazione. Selezionare il **questo Controller di dominio è definitivamente offline e può non essere abbassato di livello usando il Active Directory Domain Services installazione guidata (DCPROMO)** casella di controllo e fare clic su **eliminare**.  
5.  Se il controller di dominio è un server di catalogo globale, fare clic su **Sì** verificare che l'eliminazione.  
  
## <a name="next-steps"></a>Passaggi successivi

- [Guida di ripristino della foresta Active Directory](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md)
  
