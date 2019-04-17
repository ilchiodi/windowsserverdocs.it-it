---
title: "Personalizzazione dell'accesso per attività di Microsoft Online Backup Service"
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a7eafbb3-7728-487e-b287-90bbd6fee7f0
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: cd148e0e58cd80dbff7f7884ead95dc1e46b6257
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="customize-sign-up-for-microsoft-online-backup-service-task"></a>Personalizzazione dell'accesso per attività di Microsoft Online Backup Service

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Per impostazione predefinita, il **iscriversi a Microsoft Online Backup Service** attività sul **dispositivi** il sito Web Microsoft Online Backup Service viene visualizzata la scheda del Dashboard. Il sito Web fornisce informazioni sul servizio e consente di eseguire la sottoscrizione al servizio e scaricare il software necessario.  
  
 È possibile personalizzare il **iscriversi a Microsoft Online Backup Service** attività in due modi:  
  
-   È possibile sostituire l'URL per il sito Web predefinito con un URL che rappresenta un'esperienza utente personalizzati. Per sostituire l'URL predefinito, aprire l'Editor del Registro di sistema, creare la chiave del Registro di sistema: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\OnlineBackup\LinkUrl**, quindi assegnare l'URL personalizzato come valore della chiave.  
  
-   È possibile nascondere l'attività. Per nascondere l'attività, aprire l'Editor del Registro di sistema e creare la chiave del Registro di sistema: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\OnlineBackup\OnlineBackupInstalled **.  
  
## <a name="see-also"></a>Vedere anche  
 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Ulteriori personalizzazioni](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Test di analisi utilizzo software](Testing-the-Customer-Experience.md)