---
title: Ripristino della foresta di Active Directory-rimuovere il catalogo globale
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 60087a62-11e6-4750-a70e-510f35315688
ms.technology: identity-adds
ms.openlocfilehash: 3ba1336828ad6031ce7fb47a659d084494466e4a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71409092"
---
# <a name="ad-forest-recovery---removing-the-global-catalog"></a>Ripristino della foresta di Active Directory-rimozione del catalogo globale  

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

 Utilizzare la procedura seguente per rimuovere il catalogo globale da un controller di dominio. 
  
 Il ripristino di un server di catalogo globale da un backup può comportare il catalogo globale contenente dati più recenti per una delle relative repliche parziali rispetto al dominio corrispondente autorevole per la replica parziale. In tal caso, i dati più recenti non verranno rimossi dal catalogo globale e potrebbero persino essere replicati in altri server di catalogo globale. Di conseguenza, anche se è stato eseguito il ripristino di un controller di dominio che era un server di catalogo globale, inavvertitamente o perché era il backup solitario considerato attendibile, è necessario rimuovere il catalogo globale subito dopo il completamento dell'operazione di ripristino. Quando il catalogo globale viene rimosso, il computer rimuove tutte le relative repliche parziali. 
  
## <a name="to-remove-the-global-catalog-using-active-directory-sites-and-services"></a>Per rimuovere il catalogo globale utilizzando siti e servizi di Active Directory  
 
1. Aprire Server Manager, fare clic su **strumenti** e quindi su **siti e servizi di Active Directory**. 
2. Nell'albero della console espandere il contenitore **siti** , quindi selezionare il sito appropriato che contiene il server di destinazione. 
3. Espandere il contenitore **Server** , quindi espandere l'oggetto *Server* per il controller di dominio da cui si desidera rimuovere il catalogo globale. 
4. Fare clic con il pulsante destro del mouse su **Impostazioni NTDS**, quindi scegliere **Proprietà**. 
5. Deselezionare la casella di controllo **catalogo globale** . 
   @no__t 0Remove GC @ no__t-1
6. Fare clic su **Applica**.
  
## <a name="to-remove-the-global-catalog-using-repadmin"></a>Per rimuovere il catalogo globale tramite repadmin  
  
Aprire un prompt dei comandi con privilegi elevati, digitare il comando seguente e premere INVIO:  

   ```
   repadmin.exe /options DC_NAME –IS_GC  
   ```  

## <a name="next-steps"></a>Passaggi successivi

- [Guida al ripristino della foresta di Active Directory](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta di Active Directory - Procedure](AD-Forest-Recovery-Procedures.md)
