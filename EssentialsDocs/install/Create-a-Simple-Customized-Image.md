---
title: Creazione di un'immagine personalizzata semplice
description: Viene descritto come usare Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 29f9a09f-e4e8-476d-ada1-ab9202a670d7
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 1c3c47a9cab8e08768cef265ff823d383eda4a74
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80818354"
---
# <a name="create-a-simple-customized-image"></a>Creazione di un'immagine personalizzata semplice

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

È possibile utilizzare la seguente procedura per la creazione di un'immagine personalizzata semplice:  
  
#### <a name="to-create-the-image"></a>Creazione dell'immagine  
  
1.  Dopo l'installazione del server, sulla prima pagina della configurazione iniziale, premere MAIUS+F10 per avviare la finestra di comando.  
  
2.  Creare il file SkipIC.txt nella directory radice dell'unità di sistema.  
  
3.  Riavviare il server.  
  
4.  Avviare il server utilizzando un DVD o un'unità flash USB contenente il file unattend.xml. Per informazioni sulla creazione di un'unità flash USB di avvio, vedere [Creazione di un'unità flash USB di avvio](Create-a-Bootable-USB-Flash-Drive.md).  
  
5.  Aggiungere il marchio del logo al dashboard. Per ulteriori informazioni sull'aggiunta delle personalizzazioni, vedere [Aggiunta della personalizzazione al dashboard, al sito Accesso Web remoto e alla finestra di avvio](Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md).  
  
6.  Creare il file OOBE.xml per visualizzare le informazioni personalizzate, quali nome, logo e EULA della società. Per altre informazioni sul file OOBE.xml, vedere [Create the Oobe.xml File Including Logo and EULA](Create-the-Oobe.xml-File-Including-Logo-and-EULA.md).  
  
7.  Modificare il nome predefinito del server se non è definito nel file unattend.xml.  
  
8.  Per impostazione predefinita, il nome del server sarà una stringa casuale. Sostituire il nome del server con un'altra stringa (ad esempio, ServerContoso) e quindi comunicare al cliente il nuovo nome.  
  
9. Preparare l'immagine per la distribuzione come spiegato in [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md).  
  
## <a name="see-also"></a>Vedi anche  
 [Introduzione con Windows Server Essentials ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Personalizzazioni aggiuntive](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Test di Analisi utilizzo software](Testing-the-Customer-Experience.md)