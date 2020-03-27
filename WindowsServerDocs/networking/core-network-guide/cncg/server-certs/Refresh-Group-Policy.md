---
title: Aggiornare Criteri di gruppo
description: Questo argomento fa parte della Guida alla distribuzione di un Server dei certificati per le distribuzioni Wireless e cablate 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: 65b36794-bb09-4c1b-a2e7-8fc780893d97
ms.prod: windows-server
ms.technology: networking
ms.author: lizross
author: eross-msft
ms.openlocfilehash: b9522909960470d9f5f3e183afbd97ab1b919019
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318253"
---
# <a name="refresh-group-policy"></a>Aggiornare Criteri di gruppo

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questa procedura per aggiornare manualmente Criteri di gruppo nel computer locale. Quando viene aggiornato Criteri di gruppo, se la registrazione automatica del certificato è correttamente configurata e funzionante, nel computer locale viene registrato automaticamente un certificato dall'autorità di certificazione (CA).  
  
> [!NOTE]  
> L'aggiornamento di Criteri di gruppo viene eseguito automaticamente quando si riavvia il computer membro del dominio o quando un utente accede a un computer membro del dominio. L'aggiornamento di Criteri di gruppo viene inoltre eseguito periodicamente. Per impostazione predefinita, questo aggiornamento periodico viene eseguito ogni 90 minuti con un offset casuale di un massimo di 30 minuti.  
  
Per poter completare questa procedura, è richiesta almeno l'appartenenza al gruppo **Administrators** o a un gruppo equivalente.  
  
### <a name="to-refresh-group-policy-on-the-local-computer"></a>Per aggiornare Criteri di gruppo nel computer locale  
  
1.  Nel computer in cui è installato NPS, aprire Windows PowerShell&reg; utilizzando l'icona della barra delle applicazioni.  
  
2.  Al prompt di Windows PowerShell, digitare **gpupdate**, quindi premere INVIO.  
  


