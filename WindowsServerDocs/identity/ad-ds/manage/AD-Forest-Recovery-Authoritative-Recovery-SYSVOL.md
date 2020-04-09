---
title: Ripristino della foresta di Active Directory-sincronizzazione autorevole di SYSVOL
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 38a1c543-c76d-4b8e-a06b-53742aaa172f
ms.technology: identity-adds
ms.openlocfilehash: 4c797c98fc8d41621954077ebf470f1f52604cf7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80824304"
---
# <a name="ad-forest-recovery---performing-an-authoritative-synchronization-of-dfsr-replicated-sysvol"></a>Ripristino della foresta di Active Directory: esecuzione di una sincronizzazione autorevole di SYSVOL con replica DFSR  

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Esistono diversi modi per eseguire un ripristino autorevole di SYSVOL. È possibile modificare l'attributo **msDFSR-Options** o eseguire un ripristino dello stato del sistema utilizzando WBADMIN – authsysvol. Se è possibile ripristinare un backup dello stato del sistema, ovvero se si esegue il ripristino di servizi di dominio Active Directory sulla stessa istanza hardware e del sistema operativo, l'utilizzo di Wbadmin – authsysvol è più semplice. Tuttavia, se è necessario eseguire un ripristino bare metal, è necessario modificare l'attributo **msDFSR-Options** .  

Usare la procedura seguente per eseguire una sincronizzazione autorevole di SYSVOL (se viene replicata usando DFSR) modificando l'attributo **msDFSR-Options** . Se SYSVOL viene replicato con FRS, vedere l' [articolo 290762](https://go.microsoft.com/fwlink/?LinkId=148443).  

## <a name="to-perform-an-authoritative-synchronization-of-dfsr-replicated-sysvol"></a>Per eseguire una sincronizzazione autorevole di SYSVOL con replica DFSR  

1. Aprire Utenti e computer di Active Directory.  
2. Fare clic su **Visualizza**, quindi selezionare **utenti, contatti, gruppi e computer come contenitori** e **funzionalità avanzate**. 

   ![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol1.png) 

3. Nella visualizzazione albero fare clic su **controller di dominio**, il nome del controller di dominio ripristinato, **DFSR-LocalSettings**e quindi **volume del sistema di dominio**. 

   ![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol2.png)  

4. Nel riquadro dei dettagli fare clic con il pulsante destro del mouse su **sottoscrizione SYSVOL**, scegliere **Proprietà**, quindi fare clic su **Editor attributi**.  

   ![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol3.png) 

5. Fare clic su **msDFSR-Options**, fare clic su **modifica**, digitare **1**e quindi fare clic su **OK** .  

   ![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol4.png) 

6. Fare clic su **OK** per chiudere l'Editor attributi.  
  
## <a name="next-steps"></a>Passaggi successivi

- [Guida al ripristino della foresta di Active Directory](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta di Active Directory - Procedure](AD-Forest-Recovery-Procedures.md)
