---
title: Configurazione del supporto per una rete senza fili
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833112"
---
# <a name="configure-support-for-a-wireless-network"></a>Configurazione del supporto per una rete senza fili

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

È possibile configurare il sistema operativo per il supporto di una rete senza fili. A tale scopo, è necessario che vengano soddisfatti i seguenti requisiti:  
  
-   Sul server deve essere installata una scheda di rete cablata.  
  
-   Il driver corretto per la scheda di rete wireless deve essere preinstallato qualora la scheda di rete non sia supportata dal sistema operativo.  
  
-   È necessario che sia possibile abilitare e disabilitare la scheda di rete wireless. Per questa operazione, potrebbe esistere un pulsante fisico sul server oppure un'interfaccia utente personalizzata nel dashboard. Il manuale del prodotto include le istruzioni per abilitare e disabilitare la scheda di rete wireless.  
  
-   Deve essere possibile selezionare una rete senza fili e connettersi ad essa. Per questa operazione, è possibile aggiungere un'interfaccia utente personalizzata al dashboard. Il manuale del prodotto include le istruzioni per la selezione e la connessione a una rete senza fili.  
  
-   Se è necessario poter supportare una rete ad hoc senza fili, occorre disporre di un'interfaccia utente estesa nel dashboard. L'interfaccia utente può essere rappresentata da un pulsante oppure da un collegamento che permette di avviare la procedura guidata per la configurazione di una rete ad hoc senza fili nel pannello di controllo in Windows Server 2008 R2.  
  
## <a name="additional-considerations"></a>Considerazioni aggiuntive  
 Anche le informazioni riportate di seguito devono essere prese in considerazione durante la configurazione del supporto per una rete senza fili:  
  
-   Il server deve essere collegato alla rete tramite un cavo per poter eseguire la configurazione.  
  
-   Un computer di rete sul quale eseguire un ripristino bare metal deve essere collegato alla rete tramite un cavo.  
  
-   Il server deve essere collegato alla rete con un cavo per eseguire un ripristino bare metal del server.  
  
-   Se sul server viene creata una rete ad hoc, la scheda di rete wireless sarà dedicata a questa rete; pertanto l'utente dovrà sempre collegare il cavo di rete al server per poter usufruire della connessione Internet.  
  
> [!NOTE]
>  Per altre informazioni sulla configurazione di connessioni di rete, vedere [Preconfiguring a Router](Preconfiguring-a-Router.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Personalizzazioni aggiuntive](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Testare l'esperienza dei clienti](Testing-the-Customer-Experience.md)