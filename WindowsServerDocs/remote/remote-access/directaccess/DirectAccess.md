---
title: DirectAccess
description: È possibile utilizzare questo argomento per una breve panoramica di DirectAccess in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6b71d18e-1939-4fc0-bb42-29e0e5ffc8da
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 11c5aa093ddd5aa4777e88c536195bb70bd846db
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281928"
---
# <a name="directaccess"></a>DirectAccess

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per una breve panoramica di DirectAccess, inclusi i server e sistemi operativi client che supporta DirectAccess, e per i collegamenti ad altra documentazione di DirectAccess per Windows Server 2016.  
  
> [!NOTE]  
> Oltre a questo argomento, è disponibile la documentazione seguente di DirectAccess.  
>   
> -   [Percorsi di distribuzione di DirectAccess in Windows Server](DirectAccess-Deployment-Paths-in-Windows-Server.md)  
> -   [Prerequisiti per la distribuzione di DirectAccess](Prerequisites-for-Deploying-DirectAccess.md)  
> -   [Configurazioni non supportate da DirectAccess](DirectAccess-Unsupported-Configurations.md)  
> -   [Guide al lab di test di DirectAccess](DirectAccess-Test-Lab-Guides.md)  
> -   [Problemi noti relativi a DirectAccess](DirectAccess-Known-Issues.md)  
> -   [Pianificazione della capacità di DirectAccess](DirectAccess-Capacity-Planning.md) 
> -   [Aggiunta a dominio Offline di DirectAccess](DirectAccess-Offline-Domain-Join.md)  
> -   [Risoluzione dei problemi relativi a DirectAccess](Troubleshooting-DirectAccess.md)  
> -   [Distribuire un Server DirectAccess singolo con attività iniziali guidate](single-server-wizard/Deploy-a-Single-DirectAccess-Server-Using-the-Getting-Started-Wizard.md)  
> -   [Distribuire un singolo server di DirectAccess con impostazioni avanzate](single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)  
> -   [Aggiungere DirectAccess a una distribuzione di Accesso remoto (VPN) esistente](add-to-existing-vpn/Add-DirectAccess-to-an-Existing-Remote-Access-VPN-Deployment.md)  
  
DirectAccess consente la connettività per gli utenti remoti alle risorse di rete dell'organizzazione senza la necessità di connessioni di rete privata virtuale (VPN, Virtual Private Network) tradizionali. Con le connessioni DirectAccess, i computer client remoti siano sempre collegati all'organizzazione: non è necessario per gli utenti remoti avviare e arrestare le connessioni, come richiesto con le connessioni VPN. Inoltre, gli amministratori IT possono gestire i computer client DirectAccess ogni volta che sono in esecuzione e connesso a Internet.

>[!IMPORTANT]
>Non tentare di distribuire accesso remoto in una macchina virtuale \(VM\) in Microsoft Azure. Uso di accesso remoto in Microsoft Azure non è supportato. È possibile utilizzare l'accesso remoto in una VM di Azure per distribuire VPN, DirectAccess o qualsiasi altra funzionalità di accesso remoto in Windows Server 2016 o versioni precedenti di Windows Server. Per altre informazioni, vedere [supporto di software server Microsoft per le macchine virtuali di Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).
  
DirectAccess fornisce supporto solo per i client appartenenti a un dominio che includono il supporto del sistema operativo per DirectAccess.  
  
I seguenti sistemi operativi server supportano DirectAccess.  
  
-   È possibile distribuire tutte le versioni di Windows Server 2016 come un client DirectAccess o un server DirectAccess.  
  
-   È possibile distribuire tutte le versioni di Windows Server 2012 R2 come un client DirectAccess o un server DirectAccess.  
  
-   È possibile distribuire tutte le versioni di Windows Server 2012 come un client DirectAccess o un server DirectAccess.  
  
-   È possibile distribuire tutte le versioni di Windows Server 2008 R2 come un client DirectAccess o un server DirectAccess.  
  
I seguenti sistemi operativi client supportano DirectAccess.  
  
-   Windows 10 Enterprise  
  
-   Windows 10 Enterprise 2015 Long Term Servicing Branch (LTSB)  
  
-   Windows 8 e 8.1 Enterprise  
  
-   Windows 7 Ultimate  
  
-   Windows 7 Enterprise
