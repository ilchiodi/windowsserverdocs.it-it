---
title: Domande frequenti su QoS
description: In questo argomento fornisce le risposte alle domande sui criteri di qualità del servizio (QoS) in Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 74c97a14-b957-4568-b48e-8963a674fdb3
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f9fb54cc3bda9a259188ae02fb2ef2836dd8718d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833752"
---
# <a name="qos-policy-frequently-asked-questions"></a>Domande frequenti sui criteri QoS

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Seguenti sono frequenti, domande e risposte a queste domande – in caso di criteri QoS.
  
1.  **Quale sistema operativo è necessario che il controller di dominio sia in esecuzione per utilizzare i criteri di QoS?**
  
     Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008

2.  **Quali sistemi operativi supportano l'applicazione dei criteri QoS per l'utente o computer?**

     È possibile applicare i criteri QoS a utenti o computer che eseguono Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 e Windows Vista.

3.  **I criteri QoS si applicano a mittente o il destinatario del traffico?**

     Sul computer mittente per determinare il traffico in uscita, è necessario applicare i criteri QoS. Per effetto il traffico bidirezionale di due computer, i criteri QoS devono essere applicato a entrambi i computer.

4.  **Cosa accade se i criteri di QoS in conflitto vengono distribuiti nello stesso computer?**  
  
     Se più criteri vengono applicati, i criteri QoS più specifico ha la precedenza. Ad esempio, viene applicato un criterio che specifichi un indirizzo dell'host (192.168.4.12) anziché un indirizzo di rete (192.168.0.0/16) meno specifico. Se un criterio a livello di computer e a livello di utente dispone di specificità stesso, anziché i criteri QoS a livello di computer viene applicato il criterio di QoS a livello di utente. 

5.  **I criteri QoS è abilitato per impostazione predefinita?**

     No, i criteri QoS non è abilitato per impostazione predefinita. È necessario creare criteri QoS manualmente per abilitare QoS.  Per altre informazioni, vedere [gestire i criteri QoS](qos-policy-manage.md).

Per il primo argomento in questa Guida, vedere [dei criteri di qualità del servizio (QoS)](qos-policy-top.md).
