---
ms.assetid: 10d6723e-c857-43da-9d2d-acb5641d3da8
title: Aggiungere un computer a un dominio
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 174f585f3e156fc8e068b9300fc90a20a67869cf
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855354"
---
# <a name="join-a-computer-to-a-domain"></a>Aggiungere un computer a un dominio

Per il funzionamento di Active Directory Federation Services \(AD FS\), ogni computer che funge da server federativo deve essere aggiunto a un dominio. i proxy server federativi possono essere aggiunti a un dominio, ma questo non è un requisito.  
  
Non è necessario aggiungere un server Web a un dominio se nel server Web sono ospitate le attestazioni\-solo le applicazioni in grado di riconoscere.  
  
L'appartenenza al gruppo **Administrators**, o a un gruppo equivalente, nel computer locale è il requisito minimo per eseguire questa procedura.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-join-a-computer-to-a-domain"></a>Per aggiungere un computer a un dominio  
  
1.  Nella schermata **Start** digitare Pannello di **controllo**e quindi premere INVIO.  
  
2.  Passare a **sistema e sicurezza**e quindi fare clic su **sistema**.  
  
3.  In **Impostazioni relative a nome computer, dominio e gruppo di lavoro** fare clic su **Cambia impostazioni**.  
  
4.  Nella scheda **Nome computer** fare clic su **Cambia**.  
  
5.  In **membro di**fare clic su **dominio**, digitare il nome del dominio a cui si desidera aggiungere il computer e quindi fare clic su **OK**.  
  
6.  Fare clic su **OK**, quindi riavviare il computer.  
  
## <a name="additional-references"></a>Altri riferimenti  
[Elenco di controllo: configurazione di un server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Elenco di controllo: configurazione di un proxy server federativo](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

