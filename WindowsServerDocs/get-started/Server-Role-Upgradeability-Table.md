---
title: Matrice di aggiornamento e migrazione dei ruoli server per Windows Server 2016
description: Vengono indicati i ruoli di cui è possibile eseguire l'aggiornamento o la migrazione a Windows Server 2016.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 10/05/2016
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7e031a64-b1e6-4cf6-994a-e7c575835f6a
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: 9f8310baf659810d5d51587bafcc868c59ace61a
ms.sourcegitcommit: 5b05bae2f47ef5d9f6940e13a2e8097f311206de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/19/2018
ms.locfileid: "2304427"
---
# <a name="server-role-upgrade-and-migration-matrix-for-windows-server-2016"></a>Matrice di aggiornamento e migrazione dei ruoli server per Windows Server 2016

>Si applica a: Windows Server 2016

La griglia riportata in questa pagina illustra l'aggiornamento del ruolo server e le opzioni di migrazione in modo specifico per il passaggio a Windows Server 2016. Per indicazioni sulla migrazione dei singoli ruoli, consultare [Migrazione dei ruoli e delle funzionalità in Windows Server](https://docs.microsoft.com/windows-server/get-started/migrate-roles-and-features). Per altre informazioni sulle opzioni di installazione e aggiornamento del server, vedere [Windows Server Installation, Upgrade, and Migration](https://docs.microsoft.com/windows-server/get-started/installation-and-upgrade) (Installazione, aggiornamento e migrazione di Windows Server).

|Ruolo server|Aggiornamento da Windows Server 2012 R2|Aggiornamento da Windows Server 2012|Supporto della migrazione|Completamento della migrazione senza tempi di inattività|  
|-------------------|----------|--------------|--------------|----------|  
|Servizi certificati Active Directory| Sì|    Sì|    Sì|    No|
|Active Directory Domain Services|  Sì|    Sì|    Sì|    Sì|
|Active Directory Federation Services|  No| No| Sì|    No (è necessario aggiungere nuovi nodi alla farm)|
|Active Directory Lightweight Directory Services|   Sì|    Sì|    Sì|    Sì|
|Active Directory Rights Management Services|   Sì|    Sì|    Sì|    No|
|Server DHCP|   Sì|    Sì|    Sì|    Sì|
|Server DNS|    Sì|    Sì|    Sì|    No|
|Cluster di failover|Sì con il processo di [aggiornamento in sequenza del sistema operativo del cluster](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade) che include le operazioni di sospensione-svuotamento, rimozione e aggiornamento dei nodi a Windows Server 2016 e la ricostituzione del cluster originale. Sì, quando il server viene rimosso dal cluster per l'aggiornamento e quindi aggiunto a un cluster diverso.|Non mentre il server fa parte di un cluster. Sì, quando il server viene rimosso dal cluster per l'aggiornamento e quindi aggiunto a un cluster diverso.  |Sì|No per i cluster di failover di Windows Server 2012. Sì per i cluster di failover di Windows Server 2012 R2 con macchine virtuali Hyper-V o per i cluster di failover di Windows Server 2012 R2 che eseguono il ruolo File server di scalabilità orizzontale. Vedere [Cluster OS Rolling Upgrade](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade) (Aggiornamento in sequenza del sistema operativo del cluster).|
|Servizi file e archiviazione| Sì|    Sì|    Varia in base alla funzionalità secondaria|  No|
|Hyper-V| Sì. Quando l'host è parte di un cluster con processo di aggiornamento in sequenza del sistema operativo del cluster che include le operazioni di sospensione-svuotamento, rimozione e aggiornamento dei nodi a Windows Server 2016 e la ricostituzione del cluster originale.|  No|   Sì|  No per i cluster di failover di Windows Server 2012. Sì per i cluster di failover di Windows Server 2012 R2 con macchine virtuali Hyper-V o per i cluster di failover di Windows Server 2012 R2 che eseguono il ruolo File server di scalabilità orizzontale. Vedere [Cluster OS Rolling Upgrade](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade) (Aggiornamento in sequenza del sistema operativo del cluster).| 
|Servizi di stampa e fax|    No| No| Sì (Printbrm.exe)| No|
|Servizi Desktop remoto|   Sì, per tutti i ruoli secondari, ma la farm in modalità mista non è supportata.|   Sì, per tutti i ruoli secondari, ma la farm in modalità mista non è supportata.|   Sì|    No|
|Server Web (IIS)|  Sì|    Sì|    Sì|    No|
|Esperienza Windows Server Essentials|  Sì|    N/D, nuove funzionalità|  Sì|    No|
|Windows Server Update Services|    Sì|    Sì|    Sì|    No|
|Cartelle di lavoro|  Sì|    Sì|    Sì|    Sì da WS 2012 R2 durante l'uso di [Cluster OS Rolling Upgrade](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade) (Aggiornamento in sequenza del sistema operativo del cluster).|

