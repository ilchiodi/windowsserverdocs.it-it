---
title: Pianificare una distribuzione multisito
description: Questo argomento fa parte della Guida distribuire più server di accesso remoto in una distribuzione multisito di Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8387eabe-7363-4367-b5b1-03c67baa2933
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 95c575b255e7495f85e731bdb84ae441e35b1463
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404479"
---
# <a name="plan-a-multisite-deployment"></a>Pianificare una distribuzione multisito

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

 Windows Server 2016, Windows Server 2012 combinare DirectAccess e VPN servizio Routing e accesso remoto (RRAS) in un singolo ruolo accesso remoto. Questa panoramica offre un'introduzione ai passaggi di pianificazione necessari per la distribuzione di accesso remoto a Windows Server 2016 o Windows Server 2012 in una configurazione multisito.  
  
1.  [Distribuire un server DirectAccess singolo con impostazioni avanzate](https://technet.microsoft.com/library/hh831436(v=ws.11).aspx). Questo passaggio include la pianificazione dell'infrastruttura necessaria per distribuire un singolo server. Include la pianificazione delle impostazioni di rete e del server, i requisiti dei certificati, le impostazioni DNS, la distribuzione del server dei percorsi di rete, i server di Gestione DirectAccess, le impostazioni di Active Directory e gli oggetti di Criteri di gruppo (GPO).  
  
2.  [Passaggio 2 pianificare l'infrastruttura multisito](Step-2-Plan-the-Multisite-Infrastructure.md). Questo passaggio include la pianificazione di Active Directory e GPO e la configurazione DNS.  
  
3.  [Passaggio 3 pianificare la distribuzione multisito](Step-3-Plan-the-Multisite-Deployment.md). Questo passaggio include la pianificazione delle impostazioni del certificato, la configurazione del server del percorso di rete, le impostazioni del punto di ingresso del client, le impostazioni del prefisso IPv6 e facoltativamente le impostazioni di bilanciamento del carico globale  
  
> [!NOTE]  
> Registrare le decisioni di pianificazione per la distribuzione avanzata di accesso remoto. Questo record può essere usato per supportare il lavoro di tutte le persone coinvolte nel completamento dei passaggi di distribuzione.  
  
Dopo aver completato questi passaggi di pianificazione, vedere [configurazione di una distribuzione multisito](../configure/Configure-a-Multisite-Deployment.md).  
  


