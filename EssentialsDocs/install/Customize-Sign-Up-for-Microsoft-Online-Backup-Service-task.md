---
title: Personalizzare l'accesso per l'attività Servizio di backup online Microsoft
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879932"
---
# <a name="customize-sign-up-for-microsoft-online-backup-service-task"></a>Personalizzare l'accesso per l'attività Servizio di backup online Microsoft

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Per impostazione predefinita, l'attività **Registrazione per Microsoft Online Backup Service** sulla scheda **DISPOSITIVI** del dashboard determina l'apertura dell sito Web Microsoft Online Backup Service. Il sito Web fornisce informazioni sul servizio e consente di registrasi al servizio e scaricare il software necessario.  
  
 È possibile personalizzare l'attività **Registrazione per Microsoft Online Backup Service** in due diversi modi:  
  
-   È possibile sostituire l'URL per il sito Web predefinito con un URL personalizzato. Per sostituire l'URL predefinito, aprire l'editor del Registro di sistema, creare la chiave del Registro di sistema **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\OnlineBackup\LinkUrl** e quindi assegnare l'URL personalizzato come valore della chiave.  
  
-   È possibile nascondere l'attività. Per nascondere l'attività, aprire l'editor del Registro di sistema e creare la chiave del Registro di sistema **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\OnlineBackup\OnlineBackupInstalled**.  
  
## <a name="see-also"></a>Vedere anche  
 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Personalizzazioni aggiuntive](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Testare l'esperienza dei clienti](Testing-the-Customer-Experience.md)