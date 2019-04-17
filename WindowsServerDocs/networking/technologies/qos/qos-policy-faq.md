---
title: Domande frequenti relative a QoS
description: In questo argomento vengono fornite le risposte alle domande sui criteri di qualità del servizio (QoS) in Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 74c97a14-b957-4568-b48e-8963a674fdb3
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e63cd00a2016411e9f2c532c0e4c301dabdfd816
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="qos-policy-frequently-asked-questions"></a>Domande frequenti sui criteri QoS

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Seguenti sono domande frequenti: domande e risposte a queste domande: per i criteri QoS.
  
1.  **Quale sistema operativo usare il controller di dominio sia in esecuzione per l'utilizzo di criteri QoS**
  
     Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008

2.  **Quali sistemi operativi supportano l'applicazione dei criteri QoS per utente o al computer?**

     È possibile applicare i criteri QoS a utenti o computer che eseguono Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 e Windows Vista.

3.  **I criteri QoS si applicano al mittente o ricevitore del traffico?**

     Nel computer di invio influenzano il traffico in uscita, è necessario applicare i criteri QoS. Per poter influire il traffico bidirezionale di due computer, criteri QoS devono essere applicate a entrambi i computer.

4.  **Cosa accade se i criteri QoS in conflitto vengono distribuiti nello stesso computer?**  
  
     Se più criteri da applicare il criterio QoS più specifico ha la precedenza. Ad esempio, un criterio che gli stati di un host in indirizzi (192.168.4.12) viene applicata anziché un meno specifica rete indirizzo (192.168.0.0/16). Se la specificità stesso dispone di un criterio a livello di computer e a livello di utente, viene applicato il criterio di QoS a livello di utente anziché i criteri QoS a livello di computer. 

5.  **Criterio QoS viene abilitata per impostazione predefinita?**

     No, i criteri QoS non è abilitato per impostazione predefinita. È necessario creare criteri QoS manualmente per abilitare QoS.  Per ulteriori informazioni, vedere [gestire criteri di QoS](qos-policy-manage.md).

Per il primo argomento in questa Guida, vedere [criteri Quality of Service (QoS)](qos-policy-top.md).
