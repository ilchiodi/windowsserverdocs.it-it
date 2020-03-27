---
title: Personalizzare l'accesso per l'attività Servizio di backup online Microsoft
description: Viene descritto come usare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a7eafbb3-7728-487e-b287-90bbd6fee7f0
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f6e0a499fd149706dc3d7cce21935a7f8b5c5a48
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80311901"
---
# <a name="customize-sign-up-for-microsoft-online-backup-service-task"></a>Personalizzare l'accesso per l'attività Servizio di backup online Microsoft

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Per impostazione predefinita, l'attività **Registrazione per Microsoft Online Backup Service** sulla scheda **DISPOSITIVI** del dashboard determina l'apertura dell sito Web Microsoft Online Backup Service. Il sito Web fornisce informazioni sul servizio e consente di registrasi al servizio e scaricare il software necessario.  
  
 È possibile personalizzare l'attività **Registrazione per Microsoft Online Backup Service** in due diversi modi:  
  
-   È possibile sostituire l'URL per il sito Web predefinito con un URL personalizzato. Per sostituire l'URL predefinito, aprire l'Editor del Registro di sistema, creare la chiave: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\OnlineBackup\LinkUrl**, quindi assegnare l'URL personalizzato come valore della chiave.  
  
-   È possibile nascondere l'attività. Per nasconderla, aprire Editor del Registro di sistema e creare la chiave **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\OnlineBackup\OnlineBackupInstalled**.  
  
## <a name="see-also"></a>Vedi anche  
 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Personalizzazioni aggiuntive](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Test di Analisi utilizzo software](Testing-the-Customer-Experience.md)