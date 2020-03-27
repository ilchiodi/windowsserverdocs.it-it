---
title: Risoluzione dei problemi relativi al firewall in Windows Server Essentials
description: Viene descritto come usare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 51d94b67-8b9b-4159-80dd-f652d73a43cb
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 15a2361284d041898d9ad7240643fdb55aa5b866
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318580"
---
# <a name="troubleshoot-your-firewall-in-windows-server-essentials"></a>Risoluzione dei problemi relativi al firewall in Windows Server Essentials
 
>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
 Se si verificano problemi con l'accesso remoto, eseguire la procedura guidata Ripristina Accesso remoto via Internet.  
  
### <a name="to-run-the-repair-anywhere-access-wizard"></a>Per eseguire la procedura guidata Ripristina Accesso remoto via Internet  
  
1. Aprire il Dashboard.  
  
2. Fare clic su **Impostazioni**, selezionare la scheda **Accesso remoto via Internet** e quindi fare clic su **Ripristina**.  
  
3. Seguire le istruzioni visualizzate nella procedura guidata Ripristina Accesso remoto via Internet.  
  
   Se si usa una configurazione di rete avanzata o un firewall non Microsoft, potrebbe essere necessario aprire porte aggiuntive nel firewall. Le porte nella tabella seguente sono registrate con IANA (Internet Assigned Numbers Authority).  
  
|Numero porta|Descrizione|  
|-----------------|-----------------|  
|65500|Servizio Web per i certificati|  
|65510 e 65515|Sito Web di distribuzione computer client|  
|65520|Servizio Web per computer client Mac|  
|65532|Framework provider per le comunicazioni loopback del server|  
|6602|Framework provider per la comunicazione tra computer client e server|  
  
## <a name="see-also"></a>Vedere anche  
  
-   [USA Accesso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Gestisci Accesso Web Remote](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Gestire l'accesso remoto via Internet](../manage/Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [Gestire Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)  
  

-   [Supportare Windows Server Essentials](Support-Windows-Server-Essentials.md)

-   [Supportare Windows Server Essentials](../support/Support-Windows-Server-Essentials.md)

