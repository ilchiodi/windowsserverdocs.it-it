---
title: Pianificare una distribuzione multisito
description: Questo argomento fa parte della Guida distribuire più server di accesso remoto in una distribuzione multisito di Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 8387eabe-7363-4367-b5b1-03c67baa2933
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 507dae03ca13f4d485d6d1db0676f9d3c7b057bc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858334"
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
  


