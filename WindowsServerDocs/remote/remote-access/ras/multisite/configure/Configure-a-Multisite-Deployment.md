---
title: Configurare una distribuzione multisito
description: Questo argomento fa parte della Guida alla distribuzione di più server di accesso remoto in una distribuzione multisito di Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cb84920e-7cf5-4266-b071-d09e3d5e1f10
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b602855db271348ac48ee0a5691424a7321c7370
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849752"
---
# <a name="configure-a-multisite-deployment"></a>Configurare una distribuzione multisito

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

 Windows Server 2016 riunisce DirectAccess e servizio Accesso remoto (RAS) VPN in un unico ruolo Accesso remoto. Questa panoramica fornisce un'introduzione ai passaggi di configurazione necessari per distribuire una distribuzione multisito singola accesso remoto di Windows Server 2012 o Windows Server 2016.  
  
-   Passaggio 1: [Distribuire un Server DirectAccess singolo con impostazioni avanzate](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/single-server-advanced/deploy-a-single-directaccess-server-with-advanced-settings). Installare e configurare un singolo server di accesso remoto. La distribuzione multisita richiede di installare un singolo server prima di configurare una distribuzione multisito.  
  
-   [Passaggio 2: Configurare l'infrastruttura multisito](Step-2-Configure-the-Multisite-Infrastructure.md). Per una distribuzione multisito è necessario configurare ulteriori siti di Active Directory e controller di dominio. Oggetti Criteri di gruppo (GPO) e i gruppi di sicurezza aggiuntive sono necessari anche se non si utilizza automaticamente configurati oggetti Criteri di gruppo.  
  
-   [Passaggio 3: Configurare la distribuzione multisita](Step-3-Configure-the-Multisite-Deployment.md)-installare il ruolo Accesso remoto in altri server di accesso remoto, abilitare la distribuzione multisito e configurare i server aggiuntivi come punti di ingresso per la distribuzione.  
  
-   [Passaggio 4: Verificare la distribuzione multisita](Step-4-Verify-the-Multisite-Deployment.md) 
  


