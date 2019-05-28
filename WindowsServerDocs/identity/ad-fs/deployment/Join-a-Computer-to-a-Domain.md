---
ms.assetid: 10d6723e-c857-43da-9d2d-acb5641d3da8
title: Aggiungere un Computer a un dominio
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 02df9659ee3a1121c0cee3f7c5fa21b91c36b87c
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192048"
---
# <a name="join-a-computer-to-a-domain"></a>Aggiungere un Computer a un dominio

Per Active Directory Federation Services \(ADFS\) a funzionare, ogni computer che funziona come un server federativo deve essere aggiunto a un dominio. server federativi possono essere aggiunti a un dominio, ma questo non è un requisito.  
  
Non è necessario aggiungere un server Web a un dominio, se il server Web ospita attestazioni\-solo applicazioni con riconoscimento.  
  
Per completare questa procedura, è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente nel computer locale.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-join-a-computer-to-a-domain"></a>Per aggiungere un computer a un dominio  
  
1.  Nel **avviare** digitare **Pannello di controllo**, quindi premere INVIO.  
  
2.  Passare a **sistema e sicurezza**, quindi fare clic su **sistema**.  
  
3.  In **Impostazioni relative a nome computer, dominio e gruppo di lavoro**fare clic su **Cambia impostazioni**.  
  
4.  Nella scheda **Nome computer** , fare clic su **Modifica**.  
  
5.  Sotto **appartenente**, fare clic su **Domain**, digitare il nome del dominio da questo computer per aggiungere e quindi fare clic su **OK**.  
  
6.  Fare clic su **OK**, quindi riavviare il computer.  
  
## <a name="additional-references"></a>Altri riferimenti  
[Elenco di controllo: Configurazione di un server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Elenco di controllo: Configurazione di un proxy server federativo](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

