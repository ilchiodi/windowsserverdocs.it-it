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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838642"
---
# <a name="important-information-for-using-the-windows-server-essentials-adk"></a>Informazioni importanti per l'utilizzo di Windows Server Essentials ADK

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Per creare e personalizzare un'immagine di Windows Server Essentials, è possibile utilizzare molti degli strumenti nel [Windows 8 ADK](https://go.microsoft.com/fwlink/?LinkId=248647), ma esistono alcune importanti differenze tra Windows 8 ADK e Windows Server Essentials ADK.  
  
 È necessario tenere conto delle importanti differenze riportate di seguito:  
  
-   Alcune impostazioni sono state modificate in **%windir%\setup\script\SetupComplete.cmd**. Se si desidera utilizzare questo comando, è possibile aggiungere ulteriori righe di comandi, ma non rimuovere le righe esistenti.  
  
## <a name="working-with-passwords"></a>Utilizzo delle password  
  
-   La password dell'amministratore è impostata su Admin@123 e accesso automatico è abilitato in Install.wim\unattend.Xml. Di conseguenza, non è necessario ridigitare più volte la password durante la procedura di configurazione iniziale del server. Se è stato personalizzato il file unattend.xml nella directory radice del supporto rimovibile, queste impostazioni verranno sovrascritte e sarà di nuovo necessario fornire password e credenziali di accesso all'avvio.  
  
-   Durante la configurazione iniziale, all'utente finale viene richiesto di creare un nuovo account e una nuova password. Questo nuovo account diventerà l'account amministratore di rete del sistema operativo. L'account amministratore e l'accesso automatico vengono quindi disabilitati. È possibile automatizzare questo processo utilizzando il file cfg.ini per verificare la qualità.  
  
-   Per altre informazioni sulla creazione di un file unattend.xml, vedere la documentazione di [Windows 8 ADK](https://go.microsoft.com/fwlink/?LinkId=248694) .  
  
## <a name="see-also"></a>Vedere anche  

 [Guida introduttiva a Windows Server Essentials ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Personalizzazioni aggiuntive](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Testare l'esperienza dei clienti](Testing-the-Customer-Experience.md)

 [Guida introduttiva a Windows Server Essentials ADK](../install/Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Creazione e personalizzazione dell'immagine](../install/Creating-and-Customizing-the-Image.md)   
 [Personalizzazioni aggiuntive](../install/Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](../install/Preparing-the-Image-for-Deployment.md)   
 [Testare l'esperienza dei clienti](../install/Testing-the-Customer-Experience.md)

