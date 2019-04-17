---
title: Aggiornare criteri di gruppo
description: Questo argomento fa parte della Guida di certificati del Server di distribuzione per le distribuzioni Wireless e cablate 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: 65b36794-bb09-4c1b-a2e7-8fc780893d97
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4d9f5d38199f8cf3c0ffe46df4cd975cd9c56ff6
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="refresh-group-policy"></a>Aggiornare criteri di gruppo

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questa procedura per aggiornare manualmente criteri di gruppo nel computer locale. Quando viene aggiornato criteri di gruppo, se la registrazione automatica del certificato è configurata e funzioni correttamente, il computer locale viene registrato automaticamente un certificato dall'autorità di certificazione (CA).  
  
> [!NOTE]  
> Criteri di gruppo vengono aggiornati automaticamente quando si riavvia il computer membro del dominio o quando un utente accede a un computer membro del dominio. Inoltre, criteri di gruppo viene aggiornato periodicamente. Per impostazione predefinita, questo aggiornamento periodico viene eseguito ogni 90 minuti con un offset casuale fino a 30 minuti.  
  
Appartenenza al gruppo **amministratori**, o equivalente è il requisito minimo necessario per completare questa procedura.  
  
### <a name="to-refresh-group-policy-on-the-local-computer"></a>Per aggiornare criteri di gruppo nel computer locale  
  
1.  Nel computer in cui è installato NPS, aprire Windows PowerShell&reg; utilizzando l'icona della barra delle applicazioni.  
  
2.  Al prompt di Windows PowerShell, digitare **gpupdate**, quindi premere INVIO.  
  


