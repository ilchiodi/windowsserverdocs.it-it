---
title: Configurazione dell'archiviazione server
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859982"
---
# <a name="configure-server-storage"></a>Configurazione dell'archiviazione server

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

## <a name="sample-hard-disk-configurations"></a>Esempi di configurazione del disco rigido  
 La seguente tabella suggerisce degli esempi di configurazione del disco rigido. Le stime si basano sulla funzionalità e sull'utilizzo tipico, ma non si riferiscono a problemi che influiscono sulle prestazioni ottimali. Per queste configurazioni, è possibile utilizzare qualsiasi tipo di disco rigido supportato (come SATA o SCSI), in base alle preferenze e alle esigenze del cliente.  
  
> [!IMPORTANT]
>   Windows Server Essentials deve essere installato come volume c:, e le dimensioni del volume devono essere almeno 60 GB. È consigliabile creare due partizioni sul disco del sistema operativo ed è preferibile non utilizzare la partizione di sistema C: per archiviare i dati aziendali.  
  
|Livello del server|Configurazione del disco|  
|------------------|------------------------|  
|Voce|-Due dischi fisici<br /><br /> -Configurata come un set con mirroring RAID 1 che contiene gli elementi seguenti:<br /><br /> - C: volume  ? 60 GB<br /><br /> - D: volume  ? 1000 GB|  
|Medio|-Tre dischi fisici<br /><br /> -Configurata come un set RAID 5 che contiene gli elementi seguenti:<br /><br /> - C: volume  ? 60 GB<br /><br /> - D: volume  ? 1500 GB|  
|Alto|-Cinque o più dischi fisici totali<br /><br /> -Due dischi in un set con mirroring RAID 1 che contiene il volume c:? 100 GB<br /><br /> -Tutti gli altri dischi in un set RAID 5 che contiene gli elementi seguenti:<br /><br /> - D: volume  ? 1500 GB<br /><br /> - E: volume  ? 1500 GB|  
  
 Queste indicazioni prendono in considerazione le dimensioni del sistema operativo installato, le dimensioni medie dell'archivio dei dati utilizzate dal server e l'espansione prevista dell'archivio dei dati nella durata del server. I volumi possono essere partizioni su un singolo disco fisico o possono essere posizionati su dischi fisici separati. Dal momento che il server archivia i dati importanti per il cliente, è consigliabile usare più dischi fisici e proteggere i dati di s cliente utilizzando RAID hardware o spazi di archiviazione.  
  
## <a name="configuring-your-server-backup"></a>Configurazione del backup del server  
 Oltre ai dischi rigidi interni del server, è opportuno che i clienti prendano in considerazione l'uso di dischi rigidi USB esterni per i backup. Idealmente, il cliente dovrebbe disporre di almeno due dischi rigidi esterni con una capacità sufficiente a eseguire il backup di tutti i dati presenti sul server. Se si utilizzano dischi rigidi esterni, ogni sera il cliente può portare un disco fuori sede per proteggere ulteriormente i dati.  
  
## <a name="partition-configuration"></a>Configurazione delle partizioni  
 Durante la configurazione iniziale per il server, nella partizione dati più grande del Disco 0 viene creato un gruppo di cartelle server predefinite che includono cartelle condivise e una cartella di backup del computer client.  
  
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

