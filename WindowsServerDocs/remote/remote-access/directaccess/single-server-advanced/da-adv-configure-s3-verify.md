---
title: Passaggio 3 verificare la distribuzione di DirectAccess avanzato
description: Questo argomento fa parte della Guida di distribuire un DirectAccess Server singolo con Advanced le impostazioni per Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ae8bbff0-c981-4bc6-8df1-861621d0627f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 90edc10e01c4dabfc259256e3359463f8359a467
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820192"
---
# <a name="step-3-verify-the-advanced-directaccess-deployment"></a>Passaggio 3 verificare la distribuzione di DirectAccess avanzato

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo argomento viene descritto come verificare di aver configurato correttamente la distribuzione di DirectAccess.  
  
### <a name="to-verify-access-to-internal-resources-through-directaccess"></a>Per verificare l'accesso alle risorse interne con DirectAccess  
  
1.  Connettere un computer client DirectAccess alla rete aziendale e ottenere l'oggetto Criteri di gruppo.  
  
2.  Scegliere il **connessioni di rete** sull'icona nell'area di notifica per accedere a Gestione supporti DirectAccess.  
  
3.  Fare clic su **connessione DirectAccess**, si noterà che lo stato sia **connesso in locale**.  
  
4.  Connettere il computer client alla rete esterna e tentare di accedere alle risorse interne.  
  
    Si dovrebbe poter accedere a tutte le risorse aziendali.  
  
## <a name="BKMK_Links"></a>Passaggio precedente  
  
-   [Passaggio 2: Configurazione dei server DirectAccess](Step-2-Configuring-DirectAccess-Servers.md)  
  


