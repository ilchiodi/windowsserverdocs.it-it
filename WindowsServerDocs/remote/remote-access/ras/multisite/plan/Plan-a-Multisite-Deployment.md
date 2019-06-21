---
title: Pianificare una distribuzione multisito
description: Questo argomento fa parte della Guida alla distribuzione di più server di accesso remoto in una distribuzione multisito di Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8387eabe-7363-4367-b5b1-03c67baa2933
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ba813fc5da53e9635e9ef1363f1a559a29419312
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282595"
---
# <a name="plan-a-multisite-deployment"></a>Pianificare una distribuzione multisito

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

 Windows Server 2016, Windows Server 2012 combina DirectAccess e Routing e accesso remoto (RRAS) VPN in un unico ruolo Accesso remoto. Questa panoramica fornisce un'introduzione ai passaggi di pianificazione necessari per distribuire accesso remoto di Windows Server 2012 o Windows Server 2016 in una configurazione multisito.  
  
1.  [Distribuire un Server DirectAccess singolo con impostazioni avanzate](https://technet.microsoft.com/library/hh831436(v=ws.11).aspx). Questo passaggio consente di pianificare l'infrastruttura necessaria per distribuire un server singolo. Include la pianificazione per la rete e le impostazioni del server, i requisiti dei certificati, le impostazioni DNS, distribuzione server del percorso di rete, server di Gestione DirectAccess, le impostazioni di Active Directory e oggetti Criteri di gruppo (GPO).  
  
2.  [Passaggio 2 piano infrastruttura multisito](Step-2-Plan-the-Multisite-Infrastructure.md). Questo passaggio include Active Directory e l'oggetto Criteri di gruppo di pianificazione e la configurazione del DNS.  
  
3.  [Passaggio 3 pianificare la distribuzione Multisita](Step-3-Plan-the-Multisite-Deployment.md). Questo passaggio consente di pianificare le impostazioni del certificato, configurazione di rete percorso server, impostazioni del punto di ingresso client, le impostazioni di prefisso IPv6 e le impostazioni di bilanciamento del carico globale facoltativamente.  
  
> [!NOTE]  
> Registrare le decisioni di pianificazione per la distribuzione avanzata di accesso remoto. Questo record può essere usato per supportare il lavoro di tutte le persone coinvolte nel completamento dei passaggi di distribuzione.  
  
Dopo aver completato questi passaggi di pianificazione, vedere [configurazione di una distribuzione multisita](../configure/Configure-a-Multisite-Deployment.md).  
  


