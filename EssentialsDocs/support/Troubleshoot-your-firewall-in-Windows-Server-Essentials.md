---
title: Risolvere i problemi relativi al firewall in Windows Server Essentials
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
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="troubleshoot-your-firewall-in-windows-server-essentials"></a>Risolvere i problemi relativi al firewall in Windows Server Essentials
 
>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
 Se si verificano problemi di accesso remoto, eseguire l'ovunque procedura guidata Ripristina accesso.  
  
### <a name="to-run-the-repair-anywhere-access-wizard"></a>Per l'esecuzione di qualsiasi procedura guidata Ripristina accesso  
  
1.  Aprire il Dashboard.  
  
2.  Fare clic su **impostazioni**, fare clic su di **accesso remoto via Internet** scheda e quindi fare clic su **riparazione**.  
  
3.  Seguire le istruzioni di qualsiasi procedura guidata Ripristina accesso.  
  
 Se si utilizza una configurazione di rete avanzata o utilizza un firewall non Microsoft, potrebbe essere necessario aprire porte aggiuntive nel firewall. Le porte nella tabella seguente sono registrate con IANA Internet Assigned Numbers autorità ().  
  
|Numero di porta|Descrizione|  
|-----------------|-----------------|  
|65500|Servizio web di certificati|  
|65510 e 65515|Sito Web di distribuzione computer client|  
|65520|Servizio Web per i computer client Mac|  
|65532|Framework provider per le comunicazioni loopback del server|  
|6602|Framework provider per la comunicazione tra i computer client e server|  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Usare accesso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Gestire accesso Web remoto](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Gestire l'accesso remoto via Internet](../manage/Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [Gestire Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)  
  

-   [Supporto per Windows Server Essentials](Support-Windows-Server-Essentials.md)

-   [Supporto per Windows Server Essentials](../support/Support-Windows-Server-Essentials.md)

