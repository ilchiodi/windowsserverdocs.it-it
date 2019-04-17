---
title: "Creare un'unità Flash USB di avvio"
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2fe8e35c-69f9-40b3-a270-22e2402510d8
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 9d587329e1141040b2511e1574649f1844dcec90
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-bootable-usb-flash-drive"></a>Creare un'unità Flash USB di avvio

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

È possibile creare un'unità flash USB avviabile da utilizzare per distribuire Windows Server Essentials. Il primo passaggio consiste nella preparazione dell'unità flash USB utilizzando DiskPart, un'utilità della riga di comando. Per informazioni su DiskPart, vedere [opzioni della riga di comando DiskPart ](https://go.microsoft.com/fwlink/?LinkId=207073).  
  
 Per conoscere altri scenari in cui si desidera creare o utilizzare una porta USB di avvio unità memoria flash, vedere gli argomenti seguenti:  
  
-   [Ripristinare un intero sistema dal backup di un computer client esistente](https://technet.microsoft.com/library/jj713539.aspx#BKMK_CreateBootable)  
  
-   [Ripristinare o riparare il server che esegue Windows Server Essentials](https://technet.microsoft.com/library/jj593197.aspx#BKMK_Restore_2)  
  
### <a name="to-create-a-bootable-usb-flash-drive"></a>Per creare un'unità flash USB avviabile  
  
1.  Inserire un'unità flash USB in un computer in esecuzione.  
  
2.  Aprire una finestra del prompt dei comandi come amministratore.  
  
3.  Tipo `diskpart`.  
  
4.  Nella nuova finestra della riga di comando che viene aperta, per determinare il flash USB numero di unità o la lettera di unità, al prompt dei comandi, digitare `list disk`e quindi premere INVIO. Il `list disk`comando Visualizza tutti i dischi nel computer. Si noti il numero di unità o la lettera dell'unità flash USB.  
  
5.  Al prompt dei comandi, digitare `select disk <X>`, dove X è il numero di unità o un'unità flash, lettera di unità di USB e quindi premere INVIO.  
  
6.  Tipo `clean`e quindi premere INVIO. Questo comando Elimina tutti i dati dall'unità flash USB.  
  
7.  Per creare una nuova partizione primaria nell'unità flash USB, digitare `create part pri`e quindi premere INVIO.  
  
8.  Per selezionare la partizione appena creata, digitare `select part 1`e quindi premere INVIO.  
  
9. Per formattare la partizione, digitare `format fs=ntfs quick`e quindi premere INVIO.  
  
    > [!IMPORTANT]
    >  Se la piattaforma del server supporta la Unified Extensible Firmware Interface (UEFI), è necessario formattare l'unità flash USB come FAT32 piuttosto che come NTFS. Per formattare la partizione come FAT32, digitare `format fs=fat32 quick`e quindi premere INVIO.  
  
10. Tipo `active`e quindi premere INVIO.  
  
11. Tipo `exit`e quindi premere INVIO.  
  
12. Al termine della preparazione dell'immagine personalizzata, salvare la radice dell'unità flash USB.  
  
## <a name="see-also"></a>Vedere anche  

 [Guida introduttiva a Windows Server Essentials ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Ulteriori personalizzazioni](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Test di analisi utilizzo software](Testing-the-Customer-Experience.md)   

 [Guida introduttiva a Windows Server Essentials ADK](../install/Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Creazione e personalizzazione dell'immagine](../install/Creating-and-Customizing-the-Image.md)   
 [Ulteriori personalizzazioni](../install/Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](../install/Preparing-the-Image-for-Deployment.md)   
 [Test di analisi utilizzo software](../install/Testing-the-Customer-Experience.md)   

 [Come possiamo aiutarti?](https://windows.microsoft.com/windows/support)
