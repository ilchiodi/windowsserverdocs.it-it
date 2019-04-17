---
title: Preconfigurazione di un Router
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9153ac90-bb0c-4b8d-93b2-e2121ed13636
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 7dc66c8a439552c2087d0348b0115adba04027ee
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="preconfiguring-a-router"></a>Preconfigurazione di un Router

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

In genere, una nuova installazione del sistema operativo richiede un router che supporta Internet e un firewall per la connessione di rete interna del cliente a Internet. Se si fornisce un router come valore aggiuntivo con un server preconfigurato, è possibile eseguire passaggi aggiuntivi per preconfigurare il router per fornire una migliore esperienza utente.  
  
 Il router deve avere attivato DHCP. Il server deve essere assegnato un indirizzo IP statico. È possibile farlo prenotazione DHCP di un indirizzo IP oppure assegnare un indirizzo IP non compreso nell'intervallo di indirizzi DHCP.  
  
 È inoltre necessario preconfigurare le impostazioni di port forwarding sul router per inoltrare specifiche porte dall'interfaccia esterna del router all'indirizzo del server nella rete interna. La tabella seguente elenca la configurazione consigliata.  
  
|Impostazione di configurazione|Dettagli|  
|---------------------------|-------------|  
|DHCP|In|  
|Inoltro delle porte|Occorre inoltrare le seguenti porte all'indirizzo del server:<br /><br /> -80 (per la configurazione ospitata, utilizzare solo 443)<br />-   443|  
|Supporto UPnP|È necessario abilitare il supporto di stato UPnP fornire la configurazione del router più semplice per il cliente e la migliore esperienza cliente durante l'installazione.<br /><br /> **Avviso:** l'architettura UPnP può causare rischi di protezione se è abilitato a sinistra.|  
  
 Oltre alle impostazioni di preconfigurazione di base del router, è possibile completare le attività seguenti per fornire un'esperienza utente più integrata per la gestione del router:  
  
-   Estendere il Dashboard, fornendo un componente aggiuntivo nel server che consente agli utenti di gestire il router tramite un'interfaccia utente personalizzata.  
  
-   Estendere gli avvisi di integrità in modo che qualsiasi avviso emesso dal router può essere visualizzato nel Visualizzatore avvisi.  
  
-   Se il router supporta più subnet, occorre distribuire l'indirizzo IP del server come un server DNS attraverso DHCP.  
  
-   Se il router è una funzionalità di controllo di accesso integrato per servizi di dominio Active DirectoryÂ®, è possibile automatizzare l'integrazione di Active Directory durante la configurazione iniziale del server. Occorre inoltre esporre questa funzionalità tramite il router gestione aggiuntivo nel Dashboard.  
  
> [!NOTE]
>  Per ulteriori informazioni sulla configurazione delle connessioni senza fili, vedere [configurare il supporto per una rete Wireless](Configure-Support-for-a-Wireless-Network.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Guida introduttiva a Windows Server Essentials ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Ulteriori personalizzazioni](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Test di analisi utilizzo software](Testing-the-Customer-Experience.md)