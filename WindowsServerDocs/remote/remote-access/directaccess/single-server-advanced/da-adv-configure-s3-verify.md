---
title: Passaggio 3 verificare la distribuzione avanzata di DirectAccess
description: Questo argomento fa parte della Guida distribuire un server DirectAccess singolo con impostazioni avanzate per Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ae8bbff0-c981-4bc6-8df1-861621d0627f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 51ce3fa1a72420f7272141bb5361b20360b7000c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404902"
---
# <a name="step-3-verify-the-advanced-directaccess-deployment"></a>Passaggio 3 verificare la distribuzione avanzata di DirectAccess

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In questo argomento viene descritto come verificare di avere configurato correttamente la distribuzione di DirectAccess.  
  
### <a name="to-verify-access-to-internal-resources-through-directaccess"></a>Per verificare l'accesso alle risorse interne con DirectAccess  
  
1.  Connettere un computer client DirectAccess alla rete aziendale e ottenere l'oggetto Criteri di gruppo.  
  
2.  Fare clic sull'icona **connessioni di rete** nell'area di notifica per accedere a gestione supporti DirectAccess.  
  
3.  Fare clic su **connessione DirectAccess**. si noterà che lo stato è **connesso localmente**.  
  
4.  Connettere il computer client alla rete esterna e tentare di accedere alle risorse interne.  
  
    Si dovrebbe poter accedere a tutte le risorse aziendali.  
  
## <a name="BKMK_Links"></a>Passaggio precedente  
  
-   [Passaggio 2: Configurazione di server DirectAccess @ no__t-0  
  


