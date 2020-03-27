---
title: Configurare una distribuzione multisito
description: Questo argomento fa parte della Guida distribuire più server di accesso remoto in una distribuzione multisito di Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cb84920e-7cf5-4266-b071-d09e3d5e1f10
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 4a0229d5605271876f89e8e0ae75f8612e3f5762
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80314011"
---
# <a name="configure-a-multisite-deployment"></a>Configurare una distribuzione multisito

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

 Windows Server 2016 combina DirectAccess e VPN RAS (Remote Access Service) in un singolo ruolo accesso remoto. Questa panoramica offre un'introduzione ai passaggi di configurazione necessari per distribuire una singola distribuzione multisito di accesso remoto di Windows Server 2016 o Windows Server 2012.  
  
-   Passaggio 1: [distribuire un server DirectAccess singolo con impostazioni avanzate](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/single-server-advanced/deploy-a-single-directaccess-server-with-advanced-settings). Installare e configurare un singolo server di accesso remoto. Per la distribuzione multisito è necessario installare un server singolo prima di configurare una distribuzione multisito.  
  
-   [Passaggio 2: configurare l'infrastruttura multisito](Step-2-Configure-the-Multisite-Infrastructure.md). Per una distribuzione multisito è necessario configurare altri siti di Active Directory e controller di dominio. Se non si utilizzano oggetti Criteri di gruppo configurati automaticamente, è necessario che siano necessari anche altri gruppi di sicurezza e oggetti di Criteri di gruppo (GPO).  
  
-   [Passaggio 3: configurare la distribuzione multisito:](Step-3-Configure-the-Multisite-Deployment.md)installare il ruolo accesso remoto in server di accesso remoto aggiuntivi, abilitare la distribuzione multisito e configurare i server aggiuntivi come punti di ingresso per la distribuzione.  
  
-   [Passaggio 4: verificare la distribuzione multisito](Step-4-Verify-the-Multisite-Deployment.md) 
  


