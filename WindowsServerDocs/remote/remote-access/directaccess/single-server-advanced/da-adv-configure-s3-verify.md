---
title: Passaggio 3 verificare la distribuzione avanzata di DirectAccess
description: Questo argomento fa parte della Guida distribuire un server DirectAccess singolo con impostazioni avanzate per Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: ae8bbff0-c981-4bc6-8df1-861621d0627f
ms.author: lizross
author: eross-msft
ms.openlocfilehash: aca9cc008a096b16e01e5adb2608279ac7ded58b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861474"
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
  


