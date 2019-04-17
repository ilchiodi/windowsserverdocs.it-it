---
title: Configurare il supporto per una rete Wireless
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4d7020d4-fd46-4858-a406-de5c0f21ea06
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c5c98727b81bf37fdb3f90c612270462a51908c8
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="configure-support-for-a-wireless-network"></a>Configurare il supporto per una rete Wireless

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

È possibile configurare il sistema operativo per supportare una rete wireless. Per abilitare il supporto wireless nel server, è necessario soddisfare i requisiti seguenti:  
  
-   Il server deve disporre di una scheda di rete cablata.  
  
-   Il driver corretto per la scheda di rete wireless deve essere preinstallato qualora la scheda di rete non è supportata dal sistema operativo.  
  
-   La possibilità di abilitare e disabilitare la scheda di rete wireless deve essere rese disponibile. Metodi per eseguire questa operazione potrebbero includere un pulsante fisico sul server o un'interfaccia utente personalizzata nel Dashboard. Il manuale del prodotto include le istruzioni per abilitare e disabilitare la scheda di rete wireless.  
  
-   La possibilità di selezionare una rete senza fili e connettersi ad essa deve essere rese disponibile. Questa operazione deve essere eseguita mediante l'aggiunta di un'interfaccia utente personalizzata al Dashboard. Il manuale del prodotto include le istruzioni per la selezione e la connessione a una rete wireless.  
  
-   Se è necessaria la possibilità di supportare una rete ad hoc senza fili, è necessario fornire un'interfaccia utente estesa nel Dashboard. L'interfaccia utente può essere un pulsante o un collegamento che consente di avviare la configurazione guidata rete Ad hoc senza fili nel Pannello di controllo in Windows Server 2008 R2.  
  
## <a name="additional-considerations"></a>Considerazioni aggiuntive  
 Quando si configura il supporto per una rete wireless, considerare anche le seguenti informazioni:  
  
-   Il server deve essere collegato alla rete tramite un cavo per eseguire l'installazione.  
  
-   Un computer di rete in cui viene eseguito un ripristino bare metal deve essere connesso alla rete tramite un cavo.  
  
-   Il server deve essere collegato alla rete con un cavo per eseguire un ripristino bare metal del server.  
  
-   Se nel server viene creata una rete ad hoc, la scheda di rete wireless è dedicata per la rete ad hoc in modo che l'utente dovrà sempre collegare il cavo di rete al server per ottenere una connessione Internet.  
  
> [!NOTE]
>  Per ulteriori informazioni sulla configurazione delle connessioni di rete, vedere [preconfigurazione di un Router](Preconfiguring-a-Router.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Ulteriori personalizzazioni](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Test di analisi utilizzo software](Testing-the-Customer-Experience.md)