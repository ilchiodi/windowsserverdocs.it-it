---
title: Informazioni importanti per l'utilizzo di Windows Server Essentials ADK
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 26cb2992-1250-4672-98ee-8b870baa45d5
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 4dec1fdf01538ca119b991675f932d2d8ec1e097
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="important-information-for-using-the-windows-server-essentials-adk"></a>Informazioni importanti per l'utilizzo di Windows Server Essentials ADK

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Per creare e personalizzare un'immagine di Windows Server Essentials, è possibile usare molti degli strumenti nel [Windows 8 ADK](https://go.microsoft.com/fwlink/?LinkId=248647), ma esistono alcune importanti differenze tra Windows 8 ADK e Windows Server Essentials ADK.  
  
 È opportuno tenere delle importanti differenze riportate di seguito:  
  
-   Alcune impostazioni sono state modificate **%windir%\setup\script\SetupComplete.cmd**. Se si desidera utilizzare questo comando, è possibile aggiungere ulteriori righe di comandi, ma non rimuovere le righe esistenti.  
  
## <a name="working-with-passwords"></a>Utilizzo delle password  
  
-   La password dell'amministratore è impostata su Admin@123 e l'accesso automatico è abilitata nel Install.wim\unattend.Xml. Pertanto, non è necessario ridigitare la password più volte durante la configurazione iniziale del server. Se si dispone di un file unattend.xml personalizzato nella radice del supporto rimovibile, queste impostazioni verranno sovrascritte e sarà necessario impostare la password e credenziali di accesso all'avvio.  
  
-   Durante la configurazione iniziale, l'utente finale viene richiesto di creare un nuovo account e una password. Questo nuovo account diventerà l'account di amministratore di rete per il sistema operativo. L'amministratore account e l'accesso automatico verrà quindi disabilitato. È possibile automatizzare questo processo utilizzando il file cfg.ini per la qualità.  
  
-   Fare riferimento al [Windows 8 ADK](https://go.microsoft.com/fwlink/?LinkId=248694) documentazione per informazioni dettagliate sulla creazione di un file unattend.xml.  
  
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

