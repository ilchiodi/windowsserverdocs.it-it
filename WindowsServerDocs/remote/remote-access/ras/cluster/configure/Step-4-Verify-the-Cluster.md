---
title: Passaggio 4 verificare il cluster
description: Questo argomento fa parte della Guida deploy Remote Access in a cluster in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f22dcf10-b453-4664-a9ef-e40e95c72f63
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 16a31e5435bcb3f0fa3a332bba0e767beb913c08
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80308203"
---
# <a name="step-4-verify-the-cluster"></a>Passaggio 4 verificare il cluster

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In questo argomento viene descritto come verificare di avere configurato correttamente la distribuzione del cluster DirectAccess.  
  
### <a name="to-verify-access-to-internal-resources-through-the-cluster"></a>Per verificare l'accesso alle risorse interne tramite il cluster  
  
1.  Connettere un computer client DirectAccess alla rete aziendale e ottenere i criteri di gruppo.  
  
2.  Connettere il computer client alla rete esterna e tentare di accedere alle risorse interne.  
  
    Si dovrebbe poter accedere a tutte le risorse aziendali.  
  
3.  Testare la connettività attraverso ogni server del cluster disattivando o disconnettendosi dalla rete esterna, tutti tranne uno dei server del cluster. Nel computer client tentare di accedere alle risorse aziendali. Ripetere il test in un server cluster diverso.  
  
    Dovrebbe essere possibile accedere a tutte le risorse aziendali tramite ogni server del cluster.  
  


