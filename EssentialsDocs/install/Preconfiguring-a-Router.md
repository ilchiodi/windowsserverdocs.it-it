---
title: Preconfigurazione di un router
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
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433507"
---
# <a name="preconfiguring-a-router"></a>Preconfigurazione di un router

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Di solito, una nuova installazione del sistema operativo richiede un firewall e un router che supporta Internet per connettere la rete interna del cliente a Internet. Se si fornisce un router come valore aggiuntivo con un server preconfigurato, è possibile eseguire passaggi aggiuntivi per preconfigurare il router in modo da ottimizzare l'esperienza utente.  
  
 È necessario che sul router sia attivato DHCP. Al server deve essere assegnato un indirizzo IP statico. A tale scopo, effettuare la prenotazione DHCP di un indirizzo IP oppure assegnare un indirizzo IP non compreso nell'intervallo di indirizzi DHCP.  
  
 È inoltre necessario preconfigurare le impostazioni di port forwarding sul router per inoltrare specifiche porte dall'interfaccia esterna del router all'indirizzo del server nella rete interna. La tabella seguente elenca la configurazione consigliata.  
  
|Impostazione della configurazione|Dettagli|  
|---------------------------|-------------|  
|DHCP|Attivato|  
|Port forwarding|Occorre inoltrare le seguenti porte all'indirizzo del server:<br /><br /> -80 (per la configurazione ospitata, utilizzare solo 443)<br />-   443|  
|Supporto UPnP|È consigliabile abilitare il supporto UPnP e fornire la configurazione del router per il cliente e la miglior esperienza utente durante l'installazione.<br /><br /> **Avviso:** L'architettura UPnP può causare rischi di protezione se rimane attivata.|  
  
 Oltre alle impostazioni di preconfigurazione di base del router, è possibile completare le seguenti attività per garantire un'esperienza utente più integrata in termini di gestione del router:  
  
-   Estendere le funzionalità del dashboard attraverso un componente aggiuntivo sul server che consente agli utenti di gestire il router grazie ad un'interfaccia utente personalizzata.  
  
-   Estendere le funzionalità degli avvisi di stato in modo tale che qualsiasi avviso emesso dal router possa essere consultato nell'apposito visualizzatore.  
  
-   Se il router supporta più subnet, occorre distribuire l'indirizzo IP del server come un server DNS attraverso DHCP.  
  
-   Se il router dispone di una funzionalità di controllo di accesso integrato per servizi di dominio Active DirectoryÂ®, è possibile automatizzare l'integrazione di Active Directory durante la configurazione iniziale del server. Occorre inoltre esporre questa funzione attraverso il componente aggiuntivo di gestione del router nel dashboard.  
  
> [!NOTE]
>  Per altre informazioni sulla configurazione di connessioni wireless, vedere [Configure Support for a Wireless Network](Configure-Support-for-a-Wireless-Network.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Guida introduttiva a Windows Server Essentials ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Personalizzazioni aggiuntive](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Test di Analisi utilizzo software](Testing-the-Customer-Experience.md)