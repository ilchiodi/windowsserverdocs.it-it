---
title: Ripristino della foresta Active Directory - sincronizzazione autorevole di SYSVOL
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 38a1c543-c76d-4b8e-a06b-53742aaa172f
ms.technology: identity-adds
ms.openlocfilehash: 246a2ea589ee05110362cff99d50e93a0a18c2b9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827002"
---
# <a name="ad-forest-recovery---performing-an-authoritative-synchronization-of-dfsr-replicated-sysvol"></a>Ripristino della foresta Active Directory - esecuzione di una sincronizzazione autorevole di DFSR-replicated SYSVOL  

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Esistono diversi modi per eseguire un ripristino autorevole di SYSVOL. È possibile modificare il **msDFSR-Options** attributo o eseguire un ripristino dello stato del sistema utilizzando wbadmin – authsysvol. Se è disponibile l'opzione per ripristinare un backup dello stato del sistema (ovvero, si sta ripristinando Active Directory Domain Services nella stessa istanza di hardware e sistema operativo), quindi usando wbadmin – authsysvol è più semplice. Se è necessario eseguire un ripristino bare metal, allora è necessario modificare il **msDFSR-Options** attributo.  

Utilizzare la procedura seguente per eseguire una sincronizzazione autorevole di SYSVOL (se si viene replicato tramite replica DFS) modificando il **msDFSR-Options** attributo. Se è la replica di SYSVOL con replica file, vedere [articolo 290762](https://go.microsoft.com/fwlink/?LinkId=148443).  

## <a name="to-perform-an-authoritative-synchronization-of-dfsr-replicated-sysvol"></a>Per eseguire una sincronizzazione autorevole di DFSR-replicated SYSVOL  

1. Aprire Utenti e computer di Active Directory.  
2. Fare clic su **View**e quindi selezionare **agli utenti, contatti, gruppi e computer come contenitori** e **Advanced Features**. 

   ![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol1.png) 

3. Nella visualizzazione struttura ad albero, fare clic su **controller di dominio**, il nome del controller di dominio è stato ripristinato, **DFSR-LocalSettings**e quindi **Volume sistema dominio**. 

   ![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol2.png)  

4. Nel riquadro dei dettagli, fare doppio clic su **sottoscrizione SYSVOL**, fare clic su **delle proprietà**, fare clic su **Editor attributi**.  

   ![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol3.png) 

5. Fare clic su **msDFSR-Options**, fare clic su **Edit**, digitare **1**, fare clic su **OK**  

   ![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol4.png) 

6. Fare clic su **OK** per chiudere l'Editor di attributi.  
  
## <a name="next-steps"></a>Passaggi successivi

- [Guida al ripristino della foresta AD](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md)
