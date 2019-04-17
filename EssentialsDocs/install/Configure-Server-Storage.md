---
title: Configurazione dell'archiviazione Server
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ef7ddcdd-3a74-40ca-9487-c3a6fc5155a5
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6de485f6fd46464ba707bc0871f60ac2fec5a1db
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="configure-server-storage"></a>Configurazione dell'archiviazione Server

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

## <a name="sample-hard-disk-configurations"></a>Esempi di configurazione del disco rigido  
 La seguente tabella suggerisce degli esempi di configurazione del disco rigido. Le stime si basano sulla funzionalità e sull'utilizzo tipico, ma non si riferiscono a problemi che influiscono sulle prestazioni ottimali. È possibile utilizzare qualsiasi tipo di disco rigido supportato per queste configurazioni (come SATA o SCSI), in base alle specifiche preferenze ed esigenze del cliente.  
  
> [!IMPORTANT]
>   Windows Server Essentials deve essere installato come volume c:, e le dimensioni del volume devono essere di almeno 60 GB. È consigliabile creare due partizioni sul disco del sistema operativo e non utilizzare l'unità c: (partizione di sistema) per archiviare i dati aziendali.  
  
|Livello di server|Configurazione del disco|  
|------------------|------------------------|  
|Voce|-Due dischi fisici<br /><br /> -Configurato come un set con mirroring RAID 1 che contiene quanto segue:<br /><br /> -Volume c:? 60 GB<br /><br /> -Volume d:? 1000 GB|  
|Media|-Tre dischi fisici<br /><br /> -Configurato come un set RAID 5 che contiene quanto segue:<br /><br /> -Volume c:? 60 GB<br /><br /> -Volume d:? 1500 GB|  
|Elevata|-Cinque o più dischi fisici totali<br /><br /> -Due dischi in un set con mirroring RAID 1 contenente il volume c:? 100 GB<br /><br /> -I dischi restanti tutto in un set RAID 5 che contiene quanto segue:<br /><br /> -Volume d:? 1500 GB<br /><br /> -Volume e:? 1500 GB|  
  
 Queste indicazioni prendono in considerazione le dimensioni del sistema operativo installato, la dimensione media dell'archiviazione dei dati utilizzati dal server e la crescita di archiviazione dati previsti per tutta la durata del server. I volumi possono essere partizioni su un singolo disco fisico o possono essere posizionati su dischi fisici separati. Poiché il server archivia dati importanti per il cliente, è consigliabile utilizzare più dischi fisici e proteggere i dati di s cliente utilizzando RAID hardware o spazi di archiviazione.  
  
## <a name="configuring-your-server-backup"></a>Configurazione del backup del server  
 Oltre ai dischi rigidi interni nel server, i clienti considerare l'uso di dischi rigidi USB esterni per i backup. In teoria, il cliente dovrebbe disporre di almeno due dischi rigidi esterni con una capacità sufficiente per eseguire il backup di tutti i dati nel server. Se si utilizzano dischi rigidi esterni, il cliente può portare un disco fuori sede ogni sera per proteggere ulteriormente i dati.  
  
## <a name="partition-configuration"></a>Configurazione della partizione  
 Durante la configurazione iniziale per il server, un set predefinito di cartelle del server che includono cartelle condivise e della cartella di backup computer client vengono creati nella partizione dati più grande sul disco 0.  
  
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

