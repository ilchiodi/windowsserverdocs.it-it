---
title: Ripristino della foresta Active Directory - Remove il catalogo globale
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 60087a62-11e6-4750-a70e-510f35315688
ms.technology: identity-adfs
ms.openlocfilehash: b7da7c03cbae4691e8f902f1be0cb33c0c912248
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/07/2017
---
# <a name="ad-forest-recovery---removing-the-global-catalog"></a>Ripristino della foresta Active Directory - rimozione del catalogo globale  

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

 Utilizzare la procedura seguente per rimuovere il catalogo globale da un controller di dominio.  
  
 Ripristino di un server di catalogo globale dal backup potrebbe causare il catalogo globale che contiene i dati più recenti per una delle relative repliche parziale rispetto al dominio corrispondente che è autorevole per la replica parziale. In tal caso, i dati più recenti non verranno rimossi dal catalogo globale e potrebbero anche replicare in altri server di catalogo globale. Di conseguenza, anche se invece è stato ripristinato un controller di dominio che è stato di un server di catalogo globale, inavvertitamente o perché il backup unico metodo che non è attendibile, è consigliabile rimuovere il catalogo globale subito dopo l'operazione di ripristino è stata completata. Quando il catalogo globale viene rimosso, il computer rimuove tutte le relative repliche parziale.  
  
## <a name="to-remove-the-global-catalog-using-active-directory-sites-and-services"></a>Per rimuovere il catalogo globale tramite Active Directory Sites and Services  
 
1.  Aprire Server Manager fare clic su **strumenti** e fare clic su **Active Directory Sites and Services **.  
2.  Nell'albero della console, espandere il **siti** contenitore e quindi selezionare il sito appropriato che contiene il server di destinazione.  
3.  Espandere il **server** contenitore, quindi espandere il *server* oggetto per il controller di dominio da cui si desidera rimuovere il catalogo globale.  
4.  Fare doppio clic su **impostazioni NTDS**, quindi fare clic su **proprietà **.  
5.  Cancella il **catalogo globale** casella di controllo.  
![Rimuovere i cataloghi globali](media/AD-Forest-Recovery-Remove-GC/removegc1.png)
6.  Fare clic su **applicare **.
  
## <a name="to-remove-the-global-catalog-using-repadmin"></a>Per rimuovere il catalogo globale con Repadmin  
  
1.  Aprire un prompt dei comandi con privilegi elevati, digitare il comando seguente e premere INVIO:  
  
    ```  
    repadmin.exe /options DC_NAME –IS_GC  
    ```  
  
 ## <a name="next-steps"></a>Passaggi successivi

- [Guida di ripristino della foresta Active Directory](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md)
