---
title: DirectAccess
description: È possibile utilizzare questo argomento per una breve panoramica di DirectAccess in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 6b71d18e-1939-4fc0-bb42-29e0e5ffc8da
ms.author: lizross
author: eross-msft
ms.openlocfilehash: facd4fc6821d8f1dc927e1247aa3e2537e027f2f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857464"
---
# <a name="directaccess"></a>DirectAccess

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per una breve panoramica di DirectAccess, inclusi i sistemi operativi server e client che supportano DirectAccess e per i collegamenti alla documentazione aggiuntiva di DirectAccess per Windows Server 2016.  
  
> [!NOTE]  
> Oltre a questo argomento, è disponibile la seguente documentazione di DirectAccess.  
>   
> -   [Percorsi di distribuzione di DirectAccess in Windows Server](DirectAccess-Deployment-Paths-in-Windows-Server.md)  
> -   [Prerequisiti per la distribuzione di DirectAccess](Prerequisites-for-Deploying-DirectAccess.md)  
> -   [Configurazioni non supportate da DirectAccess](DirectAccess-Unsupported-Configurations.md)  
> -   [Guide al lab di test di DirectAccess](DirectAccess-Test-Lab-Guides.md)  
> -   [Problemi noti relativi a DirectAccess](DirectAccess-Known-Issues.md)  
> -   [Pianificazione della capacità di DirectAccess](DirectAccess-Capacity-Planning.md) 
> -   [Aggiunta a un dominio offline di DirectAccess](DirectAccess-Offline-Domain-Join.md)  
> -   [Risoluzione dei problemi relativi a DirectAccess](Troubleshooting-DirectAccess.md)  
> -   [Distribuire un server DirectAccess singolo tramite la procedura guidata di Introduzione](single-server-wizard/Deploy-a-Single-DirectAccess-Server-Using-the-Getting-Started-Wizard.md)  
> -   [Distribuire un singolo server di DirectAccess con impostazioni avanzate](single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)  
> -   [Aggiungere DirectAccess a una distribuzione di Accesso remoto (VPN) esistente](add-to-existing-vpn/Add-DirectAccess-to-an-Existing-Remote-Access-VPN-Deployment.md)  
  
DirectAccess consente la connettività agli utenti remoti per le risorse di rete dell'organizzazione senza la necessità di connessioni VPN (Virtual Private Network) tradizionali. Con le connessioni DirectAccess, i computer client remoti sono sempre connessi all'organizzazione. non è necessario che gli utenti remoti avviino e interrompano le connessioni, come richiesto con le connessioni VPN. Inoltre, gli amministratori IT possono gestire i computer client DirectAccess ogni volta che eseguono e Internet connessi.

>[!IMPORTANT]
>Non tentare di distribuire accesso remoto in una macchina virtuale \(\) VM in Microsoft Azure. L'uso di accesso remoto in Microsoft Azure non è supportato. Non è possibile usare accesso remoto in una macchina virtuale di Azure per distribuire VPN, DirectAccess o qualsiasi altra funzionalità di accesso remoto in Windows Server 2016 o versioni precedenti di Windows Server. Per ulteriori informazioni, vedere [supporto del software server Microsoft per Microsoft Azure macchine virtuali](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).
  
DirectAccess fornisce supporto solo per client aggiunti a un dominio che includono il supporto del sistema operativo per DirectAccess.  
  
I sistemi operativi server seguenti supportano DirectAccess.  
  
-   È possibile distribuire tutte le versioni di Windows Server 2016 come un client DirectAccess o un server DirectAccess.  
  
-   È possibile distribuire tutte le versioni di Windows Server 2012 R2 come un client DirectAccess o un server DirectAccess.  
  
-   È possibile distribuire tutte le versioni di Windows Server 2012 come un client DirectAccess o un server DirectAccess.  
  
-   È possibile distribuire tutte le versioni di Windows Server 2008 R2 come un client DirectAccess o un server DirectAccess.  
  
I sistemi operativi client seguenti supportano DirectAccess.  
  
-   Windows 10 Enterprise  
  
-   Windows 10 Enterprise 2015 Long Term Servicing Branch (LTSB)  
  
-   Windows 8 e 8,1 Enterprise  
  
-   Windows 7 Ultimate  
  
-   Windows 7 Enterprise
