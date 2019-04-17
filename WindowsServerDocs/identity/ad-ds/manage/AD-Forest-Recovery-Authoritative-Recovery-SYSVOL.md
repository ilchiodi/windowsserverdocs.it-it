---
title: Ripristino della foresta Active Directory - sincronizzazione autorevole di SYSVOL
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 38a1c543-c76d-4b8e-a06b-53742aaa172f
ms.technology: identity-adfs
ms.openlocfilehash: 5bdc619ddf9f28fb074e90dfbf22a7be076a05d9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---performing-an-authoritative-synchronization-of-dfsr-replicated-sysvol"></a>Ripristino della foresta Active Directory - esecuzione di una sincronizzazione autorevole di SYSVOL con replica DFS  

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

 Esistono diversi modi per eseguire un ripristino autorevole di SYSVOL. È possibile modificare il **msDFSR-opzioni** attributo o eseguire un ripristino dello stato del sistema utilizzando wbadmin – authsysvol. Se hai la possibilità di ripristinare un backup dello stato del sistema (ovvero, si sta ripristinando di dominio Active Directory per la stessa istanza di hardware e sistema operativo) utilizzando wbadmin – authsysvol è più semplice. Ma se è necessario eseguire un ripristino bare metal, è necessario modificare il **msDFSR-opzioni** attributo.  
  
 Utilizzare la procedura seguente per eseguire una sincronizzazione autorevole di SYSVOL (se vengono replicati tramite replica DFS) modificando il **msDFSR-opzioni** attributo. Se è la replica di SYSVOL con replica file, vedere [articolo 290762](https://go.microsoft.com/fwlink/?LinkId=148443).  
  
## <a name="to-perform-an-authoritative-synchronization-of-dfsr-replicated-sysvol"></a>Per eseguire una sincronizzazione autorevole di SYSVOL con replica DFS  
  
1.  Aprire utenti e computer Active Directory.  
2.  Fare clic su **visualizzazione**e quindi seleziona **utenti, contatti, gruppi e computer come contenitori** e **funzionalità avanzate**. 
![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol1.png) 
3.  Nella visualizzazione albero, fare clic su **i controller di dominio**, il nome del controller di dominio è stato ripristinato, **DFSR-LocalSettings**e quindi **Volume sistema dominio**. 
![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol2.png)  
4.  Nel riquadro dei dettagli fare doppio clic su **sottoscrizione SYSVOL**, fare clic su **proprietà**e fare clic su **Editor attributi**.  
![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol3.png) 
5.  Fare clic su **msDFSR-opzioni**, fare clic su **modifica**, tipo **1**e fare clic su **OK**  
![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol4.png) 
6.  Fare clic su **OK** per chiudere l'Editor attributi.  
  
## <a name="next-steps"></a>Passaggi successivi

- [Guida di ripristino della foresta Active Directory](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md)
