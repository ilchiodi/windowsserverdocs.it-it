---
title: Passaggio 4 verificare che il Cluster
description: Questo argomento fa parte della Guida alla distribuzione di accesso remoto in un Cluster in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f22dcf10-b453-4664-a9ef-e40e95c72f63
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 424c4f881c168ea691dd51cd2d86a4a234c41075
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282984"
---
# <a name="step-4-verify-the-cluster"></a>Passaggio 4 verificare che il Cluster

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo argomento viene descritto come verificare di aver configurato correttamente la distribuzione cluster con DirectAccess.  
  
### <a name="to-verify-access-to-internal-resources-through-the-cluster"></a>Per verificare l'accesso alle risorse interne tramite il cluster  
  
1.  Connettere un computer client DirectAccess alla rete aziendale e ottenere i criteri di gruppo.  
  
2.  Connettere il computer client alla rete esterna e tentare di accedere alle risorse interne.  
  
    Si dovrebbe poter accedere a tutte le risorse aziendali.  
  
3.  Testare la connettività tramite ogni server del cluster per la disattivazione o la disconnessione dalla rete esterna, tutte tranne uno dei server cluster. Nel computer client, tentare di accedere alle risorse aziendali. Ripetere il test in un server di cluster diverso.  
  
    È necessario essere in grado di accedere a tutte le risorse aziendali tramite ogni server del cluster.  
  


