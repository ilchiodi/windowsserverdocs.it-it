---
title: Configurazione dell'archiviazione server
description: Viene descritto come usare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ef7ddcdd-3a74-40ca-9487-c3a6fc5155a5
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 3d7c2b49afc9d740e6a4b3fa7ed659e8358c8dc6
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80312139"
---
# <a name="configure-server-storage"></a>Configurazione dell'archiviazione server

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

## <a name="sample-hard-disk-configurations"></a>Esempi di configurazione del disco rigido  
 La seguente tabella suggerisce degli esempi di configurazione del disco rigido. Le stime si basano sulla funzionalità e sull'utilizzo tipico, ma non si riferiscono a problemi che influiscono sulle prestazioni ottimali. Per queste configurazioni, è possibile utilizzare qualsiasi tipo di disco rigido supportato (come SATA o SCSI), in base alle preferenze e alle esigenze del cliente.  
  
> [!IMPORTANT]
>   Windows Server Essentials deve essere installato come volume C: e la dimensione del volume deve essere almeno di 60 GB. È consigliabile creare due partizioni sul disco del sistema operativo ed è preferibile non utilizzare la partizione di sistema C: per archiviare i dati aziendali.  
  
|Livello del server|Configurazione del disco|  
|------------------|------------------------|  
|Voce|-Due dischi fisici<br /><br /> -Configurato come un set con mirroring RAID 1 che contiene quanto segue:<br /><br /> -C: volume? 60 GB<br /><br /> -D: volume? 1000 GB|  
|Media|-Tre dischi fisici<br /><br /> -Configurato come un set RAID 5 che contiene quanto segue:<br /><br /> -C: volume? 60 GB<br /><br /> -D: volume? 1500 GB|  
|Alta|-Cinque o più dischi fisici totali<br /><br /> -Due dischi in un set con mirroring RAID 1 che contiene il volume C:? 100 GB<br /><br /> -Tutti i dischi rimanenti in un set RAID 5 che contiene quanto segue:<br /><br /> -D: volume? 1500 GB<br /><br /> -E: volume? 1500 GB|  
  
 Queste indicazioni prendono in considerazione le dimensioni del sistema operativo installato, le dimensioni medie dell'archivio dei dati utilizzate dal server e l'espansione prevista dell'archivio dei dati nella durata del server. I volumi possono essere partizioni su un singolo disco fisico o possono essere posizionati su dischi fisici separati. Poiché il server archivia dati importanti per il cliente, è consigliabile utilizzare più dischi fisici e proteggere i dati del cliente utilizzando RAID hardware o spazi di archiviazione.  
  
## <a name="configuring-your-server-backup"></a>Configurazione del backup del server  
 Oltre ai dischi rigidi interni del server, è opportuno che i clienti prendano in considerazione l'uso di dischi rigidi USB esterni per i backup. Idealmente, il cliente dovrebbe disporre di almeno due dischi rigidi esterni con una capacità sufficiente a eseguire il backup di tutti i dati presenti sul server. Se si utilizzano dischi rigidi esterni, ogni sera il cliente può portare un disco fuori sede per proteggere ulteriormente i dati.  
  
## <a name="partition-configuration"></a>Configurazione delle partizioni  
 Durante la configurazione iniziale per il server, nella partizione dati più grande del Disco 0 viene creato un gruppo di cartelle server predefinite che includono cartelle condivise e una cartella di backup del computer client.  
  
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

