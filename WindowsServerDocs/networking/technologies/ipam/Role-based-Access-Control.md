---
title: Controllo degli accessi in base al ruolo
description: Questo argomento fa parte della Guida di gestione di gestione indirizzi IP (IPAM) in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ecdfc589-fa14-4bb3-ab7e-456ebc719385
ms.author: pashort
author: shortpatti
ms.openlocfilehash: eec6d2a89b24d4847cb993bab31d86881f2cae0f
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="role-based-access-control"></a>Controllo degli accessi in base al ruolo

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

In questo argomento fornisce informazioni sull'utilizzo di controllo degli accessi in base al ruolo in Gestione indirizzi IP.  
  
> [!NOTE]  
> Oltre a questo argomento, la seguente documentazione di controllo di accesso gestione indirizzi IP è disponibile in questa sezione.  
>   
> -   [Gestire in base al ruolo con Server Manager di controllo di accesso](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Server-Manager.md)  
> -   [Gestire in base al ruolo controllo di accesso con Windows PowerShell](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md)  
  
Il controllo degli accessi in base al ruolo consente di specificare i privilegi di accesso a vari livelli, tra cui il server DNS, zona DNS e livelli di record risorse DNS.  
Utilizzando il controllo degli accessi in base al ruolo, è possibile specificare quali utenti hanno il controllo granulare operazioni per creare, modificare ed eliminare diversi tipi di record di risorse DNS.  
  
È possibile configurare il controllo degli accessi in modo che gli utenti sono limitati alle seguenti autorizzazioni.  
  
-   Gli utenti possono modificare solo record di risorse DNS specifici  
  
-   Gli utenti possono modificare i record DNS di un tipo specifico, ad esempio PTR o MX  
  
-   Gli utenti possono modificare i record di risorse DNS per le zone specifico  
  
## <a name="see-also"></a>Vedere anche  
[Gestire in base al ruolo con Server Manager di controllo di accesso](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Server-Manager.md)  
[Gestire in base al ruolo controllo di accesso con Windows PowerShell](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md)  
[Gestire gestione indirizzi IP](Manage-IPAM.md)  
  


