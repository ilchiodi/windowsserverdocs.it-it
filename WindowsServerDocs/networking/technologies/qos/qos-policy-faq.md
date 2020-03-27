---
title: Domande frequenti su QoS
description: Questo argomento fornisce le risposte alle domande sui criteri di qualità del servizio (QoS) in Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 74c97a14-b957-4568-b48e-8963a674fdb3
manager: brianlic
ms.author: lizross
author: eross-msft
ms.openlocfilehash: a2b0990bd1416032cd519c60878c389f391e053d
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315532"
---
# <a name="qos-policy-frequently-asked-questions"></a>Domande frequenti sui criteri QoS

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Di seguito sono riportate le domande frequenti e le risposte a tali domande, per i criteri QoS.
  
1.  **Quale sistema operativo deve essere in esecuzione il controller di dominio per usare i criteri QoS?**
  
     Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008

2.  **Quali sistemi operativi supportano l'applicazione dei criteri QoS all'utente o al computer?**

     È possibile applicare criteri QoS a utenti o computer che eseguono Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 e Windows Vista.

3.  **I criteri QoS si applicano al mittente o al destinatario del traffico?**

     Per influire sul traffico in uscita, è necessario applicare i criteri QoS nel computer di origine. Per influenzare il traffico bidirezionale di due computer, è necessario applicare i criteri QoS a entrambi i computer.

4.  **Cosa accade se i criteri QoS in conflitto vengono distribuiti nello stesso computer?**  
  
     Se vengono applicati più criteri, i criteri QoS più specifici hanno la precedenza. Ad esempio, viene applicato un criterio che dichiara un indirizzo host (192.168.4.12) invece di un indirizzo di rete meno specifico (192.168.0.0/16). Se un criterio a livello di computer e di utente ha la stessa specificità, viene applicato il criterio QoS a livello di utente anziché i criteri QoS a livello di computer. 

5.  **I criteri QoS sono abilitati per impostazione predefinita?**

     No, i criteri QoS non sono abilitati per impostazione predefinita. Per abilitare QoS, è necessario creare manualmente i criteri QoS.  Per ulteriori informazioni, vedere [Manage QoS Policy](qos-policy-manage.md).

Per il primo argomento di questa guida, vedere [criteri di qualità del servizio (QoS)](qos-policy-top.md).
