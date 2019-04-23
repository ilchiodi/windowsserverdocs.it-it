---
title: Aggiornare Criteri di gruppo
description: Questo argomento fa parte della Guida alla distribuzione di un Server dei certificati per le distribuzioni Wireless e cablate 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: 65b36794-bb09-4c1b-a2e7-8fc780893d97
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 83dd48297535aafe30e48fe37010d81b279f4c91
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863452"
---
# <a name="refresh-group-policy"></a>Aggiornare Criteri di gruppo

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questa procedura per aggiornare manualmente Criteri di gruppo nel computer locale. Quando viene aggiornato Criteri di gruppo, se la registrazione automatica del certificato è correttamente configurata e funzionante, nel computer locale viene registrato automaticamente un certificato dall'autorità di certificazione (CA).  
  
> [!NOTE]  
> L'aggiornamento di Criteri di gruppo viene eseguito automaticamente quando si riavvia il computer membro del dominio o quando un utente accede a un computer membro del dominio. L'aggiornamento di Criteri di gruppo viene inoltre eseguito periodicamente. Per impostazione predefinita, questo aggiornamento periodico viene eseguito ogni 90 minuti con un offset casuale di un massimo di 30 minuti.  
  
L'appartenenza a **amministratori**, o equivalente è il requisito minimo necessario per completare questa procedura.  
  
### <a name="to-refresh-group-policy-on-the-local-computer"></a>Per aggiornare Criteri di gruppo nel computer locale  
  
1.  Nel computer in cui è installato NPS, aprire Windows PowerShell&reg; utilizzando l'icona della barra delle applicazioni.  
  
2.  Al prompt di Windows PowerShell, digitare **gpupdate**, quindi premere INVIO.  
  


