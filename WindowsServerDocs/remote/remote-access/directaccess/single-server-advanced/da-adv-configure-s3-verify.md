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
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 6d8f9b5ecbc322de12cb00179a4ebb044fbd2baf
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309089"
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
  
## <a name="previous-step"></a><a name="BKMK_Links"></a>Passaggio precedente  
  
-   [Passaggio 2: configurazione di server DirectAccess](Step-2-Configuring-DirectAccess-Servers.md)  
  


