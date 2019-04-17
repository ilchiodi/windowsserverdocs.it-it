---
title: Creare un'immagine personalizzata semplice
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 29f9a09f-e4e8-476d-ada1-ab9202a670d7
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e18ff5ded94127449072d28d00b98e17dbe63c3a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="create-a-simple-customized-image"></a>Creare un'immagine personalizzata semplice

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

È possibile utilizzare la seguente procedura per la creazione di un'immagine personalizzata semplice:  
  
#### <a name="to-create-the-image"></a>Per creare l'immagine  
  
1.  Dopo l'installazione di server, nella prima pagina della configurazione iniziale, premere MAIUSC + F10 per avviare la finestra di comando.  
  
2.  Creare un file SkipIC.txt sotto la radice dell'unità di sistema.  
  
3.  Riavviare il server.  
  
4.  Avviare il server con un'unità flash USB o un DVD, che include il file unattend.xml. Per informazioni sulla creazione di un'unità flash USB avviabile, vedere [creare un'unità Flash USB avviabile](Create-a-Bootable-USB-Flash-Drive.md).  
  
5.  Aggiungere marchio del logo al Dashboard. Per ulteriori informazioni sull'aggiunta delle personalizzazioni, vedere [aggiunta della personalizzazione al Dashboard, accesso Web remoto e finestra di avvio](Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md).  
  
6.  Creare il file OOBE.xml per visualizzare le informazioni personalizzate, come nome della società, logo ed EULA. Per ulteriori informazioni sul file OOBE.xml, vedere [creare i Oobe.xml File Including Logo ed EULA](Create-the-Oobe.xml-File-Including-Logo-and-EULA.md).  
  
7.  Se non definirla in unattend.xml, modificare il nome predefinito del server.  
  
8.  Per impostazione predefinita, il nome del server sarà una stringa casuale. Modificare il nome del server con un'altra stringa (ad esempio, Servercontoso) e quindi comunicare al cliente il nuovo nome del server.  
  
9. Preparare l'immagine per la distribuzione, come descritto in [preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Guida introduttiva a Windows Server Essentials ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Ulteriori personalizzazioni](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Test di analisi utilizzo software](Testing-the-Customer-Experience.md)