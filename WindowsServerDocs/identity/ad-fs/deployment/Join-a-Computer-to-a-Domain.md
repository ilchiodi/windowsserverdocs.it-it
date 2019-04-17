---
ms.assetid: 10d6723e-c857-43da-9d2d-acb5641d3da8
title: Aggiungere un Computer a un dominio
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 641a1541143206d06973a6a0f11c689390abea21
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="join-a-computer-to-a-domain"></a>Aggiungere un Computer a un dominio

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Per Active Directory Federation Services \(AD FS\) alla funzione, ogni computer che funge da un server federativo deve essere aggiunto a un dominio. I proxy server federativi possono essere aggiunti a un dominio, ma questo non è un requisito.  
  
Non è necessario aggiungere un server Web a un dominio, se il server Web ospita solo applicazioni compatibili con claims\.  
  
Appartenenza al gruppo **amministratori**, o equivalente nel computer locale è il requisito minimo necessario per completare questa procedura.  Ulteriori informazioni sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-join-a-computer-to-a-domain"></a>Per aggiungere un computer a un dominio  
  
1.  Nel **Start** digitare**Pannello di controllo**, quindi premere INVIO.  
  
2.  Passare a **sistema e sicurezza**, quindi fare clic su **sistema**.  
  
3.  In **impostazioni del nome di dominio e gruppo di lavoro Computer**, fare clic su **modificare le impostazioni**.  
  
4.  Nel **nome Computer** scheda, fare clic su **modifica**.  
  
5.  In **appartenente**, fare clic su **dominio**, digitare il nome del dominio a cui aggiungere, il computer e quindi fare clic su **OK**.  
  
6.  Fare clic su **OK**e quindi riavviare il computer.  
  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
[Elenco di controllo: Configurazione di un Server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Elenco di controllo: Configurazione di un Proxy Server federativo](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

