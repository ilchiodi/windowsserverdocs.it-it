---
title: Ripristino della foresta Active Directory - rimuovere il catalogo globale
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 60087a62-11e6-4750-a70e-510f35315688
ms.technology: identity-adds
ms.openlocfilehash: d730ce65fc179aee6a98f7cfc1a5b693bfcd6c93
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817072"
---
# <a name="ad-forest-recovery---removing-the-global-catalog"></a>Ripristino della foresta Active Directory - rimuovere il catalogo globale  

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

 Utilizzare la procedura seguente per rimuovere il catalogo globale da un controller di dominio. 
  
 Il ripristino di un server di catalogo globale da un backup può comportare il catalogo globale che contiene i dati più recenti per una delle relative repliche parziale rispetto al dominio corrispondente che è autorevole per la replica parziale. In tal caso, i dati più recenti non verranno rimosso dal catalogo globale e potrebbero anche eseguire la replica in altri server di catalogo globale. Di conseguenza, anche se invece è stato ripristinato un controller di dominio che è stato di un server di catalogo globale, inavvertitamente o perché questo è il backup solitari si considera attendibile, è necessario rimuovere il catalogo globale subito dopo l'operazione di ripristino è stata completata. Quando viene rimosso il catalogo globale, il computer rimuove tutte le relative repliche parziale. 
  
## <a name="to-remove-the-global-catalog-using-active-directory-sites-and-services"></a>Per rimuovere il catalogo globale con servizi e siti di Active Directory  
 
1. Aprire Server Manager, fare clic su **degli strumenti** e fare clic su **Active Directory Sites and Services**. 
2. Nell'albero della console, espandere la **siti** contenitore e quindi selezionare il sito appropriato che contiene il server di destinazione. 
3. Espandere la **i server** contenitore, quindi espandere il *server* oggetto per il controller di dominio da cui si desidera rimuovere il catalogo globale. 
4. Fare doppio clic su **impostazioni NTDS**, quindi fare clic su **proprietà**. 
5. Cancella il **catalogo globale** casella di controllo. 
   ![Rimuovere GC](media/AD-Forest-Recovery-Remove-GC/removegc1.png)
6. Fare clic su **Applica**.
  
## <a name="to-remove-the-global-catalog-using-repadmin"></a>Per rimuovere il catalogo globale con Repadmin  
  
Aprire un prompt dei comandi con privilegi elevati, digitare il comando seguente e premere INVIO:  

   ```
   repadmin.exe /options DC_NAME –IS_GC  
   ```  

## <a name="next-steps"></a>Passaggi successivi

- [Guida al ripristino della foresta AD](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md)
