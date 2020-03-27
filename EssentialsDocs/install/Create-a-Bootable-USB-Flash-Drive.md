---
title: Creazione di un'unità flash USB di avvio
description: Viene descritto come usare Windows Server Essentials
ms.custom: na
ms.date: 05/04/2018
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2fe8e35c-69f9-40b3-a270-22e2402510d8
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 88ae4bc7b560b0571ca339138cb99843c2d078c8
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80312123"
---
# <a name="create-a-bootable-usb-flash-drive"></a>Creazione di un'unità flash USB di avvio

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

È possibile creare un'unità flash USB di avvio da usare per distribuire Windows Server Essentials. Il primo passaggio consiste nella preparazione dell'unità flash USB utilizzando DiskPart, un'utilità da riga di comando. Per informazioni su DiskPart, vedere [Opzioni della riga di comando di DiskPart](https://go.microsoft.com/fwlink/?LinkId=207073).  


> [!TIP]
> Per creare un'unità flash USB di avvio da usare per il ripristino o la reinstallazione di Windows in un PC anziché in un server, vedere [creare un'unità di ripristino](https://support.microsoft.com/help/4026852/windows-create-a-recovery-drive).
  
 Per altri scenari in cui è possibile scegliere di creare o usare un'unità flash USB di avvio, vedere gli argomenti seguenti:  
  
-   [Ripristinare un intero sistema dal backup di un computer client esistente](../manage/restore-a-full-system-from-an-existing-client-computer-backup.md)  
  
-   [Ripristinare un server che esegue Windows Server Essentials](../manage/restore-or-repair-your-server-running-windows-server-essentials.md)  

  
### <a name="to-create-a-bootable-usb-flash-drive"></a>Per creare un'unità flash USB di avvio  
  
1.  Inserire un'unità flash USB in un computer in esecuzione.  
  
2.  Aprire una finestra del prompt dei comandi come amministratore.  
  
3.  Digitare `diskpart`.  
  
4.  Nella nuova finestra della riga di comando che viene aperta, per stabilire il numero o la lettera dell'unità flash USB, nel prompt dei comandi, digitare `list disk` e quindi premere INVIO. Il comando `list disk` consente di visualizzare tutti i dischi presenti sul computer. Annotare il numero o la lettera dell'unità flash USB.  
  
5.  Nel prompt dei comandi digitare `select disk <X>`, dove X sta per numero o lettera dell'unità flash USB, quindi premere INVIO.  
  
6.  Digitare `clean`e premere INVIO. Questo comando consente di eliminare tutti i dati dall'unità flash USB.  
  
7.  Per creare una nuova partizione primaria sull'unità flash USB, digitare `create partition primary` e premere INVIO.  
  
8.  Per selezionare la partizione appena creata, digitare `select partition 1`e premere INVIO.  
  
9. Per formattare la partizione, digitare `format fs=ntfs quick`e premere INVIO.  
  
    > [!IMPORTANT]
    >  Se la piattaforma del server supporta la Unified Extensible Firmware Interface (UEFI), è opportuno formattare l'unità flash USB come FAT32 piuttosto che come NTFS. Per formattare la partizione come FAT32, digitare `format fs=fat32 quick`e premere INVIO.  
  
10. Digitare `active`e premere INVIO.  
  
11. Digitare `exit`e premere INVIO.  
  
12. Al termine della preparazione dell'immagine personalizzata, salvare l'immagine nella radice del unità flash USB.  
  
## <a name="see-also"></a>Vedi anche  

 [Introduzione con Windows Server Essentials ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Personalizzazioni aggiuntive](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Test di Analisi utilizzo software](Testing-the-Customer-Experience.md)   

 [Introduzione con Windows Server Essentials ADK](../install/Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Creazione e personalizzazione dell'immagine](../install/Creating-and-Customizing-the-Image.md)   
 [Personalizzazioni aggiuntive](../install/Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](../install/Preparing-the-Image-for-Deployment.md)   
 [Test di Analisi utilizzo software](../install/Testing-the-Customer-Experience.md)   

 [Come possiamo aiutarti?](https://windows.microsoft.com/windows/support)
