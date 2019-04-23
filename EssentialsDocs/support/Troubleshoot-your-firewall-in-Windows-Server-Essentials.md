---
title: Risoluzione dei problemi relativi al firewall in Windows Server Essentials
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 51d94b67-8b9b-4159-80dd-f652d73a43cb
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 3c48d2abb7fd8431f40f76f8eece5c4142be4c75
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846552"
---
# <a name="troubleshoot-your-firewall-in-windows-server-essentials"></a>Risoluzione dei problemi relativi al firewall in Windows Server Essentials
 
>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
 Se si verificano problemi con l'accesso remoto, eseguire la procedura guidata Ripristina Accesso remoto via Internet.  
  
### <a name="to-run-the-repair-anywhere-access-wizard"></a>Per eseguire la procedura guidata Ripristina Accesso remoto via Internet  
  
1.  Aprire il dashboard.  
  
2.  Fare clic su **Impostazioni**, selezionare la scheda **Accesso remoto via Internet** e quindi fare clic su **Ripristina**.  
  
3.  Seguire le istruzioni visualizzate nella procedura guidata Ripristina Accesso remoto via Internet.  
  
 Se si usa una configurazione di rete avanzata o un firewall non Microsoft, potrebbe essere necessario aprire porte aggiuntive nel firewall. Le porte nella tabella seguente sono registrate con IANA (Internet Assigned Numbers Authority).  
  
|Numero di porta|Descrizione|  
|-----------------|-----------------|  
|65500|Servizio Web per i certificati|  
|65510 e 65515|Sito Web di distribuzione computer client|  
|65520|Servizio Web per computer client Mac|  
|65532|Framework provider per le comunicazioni loopback del server|  
|6602|Framework provider per la comunicazione tra computer client e server|  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Usare accesso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Gestire accesso Web remoto](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Gestire l'accesso remoto via Internet](../manage/Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [Gestire Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)  
  

-   [Supporto per Windows Server Essentials](Support-Windows-Server-Essentials.md)

-   [Supporto per Windows Server Essentials](../support/Support-Windows-Server-Essentials.md)

