---
title: Controllo degli accessi in base al ruolo
description: Questo argomento fa parte della Guida alla gestione di gestione indirizzi IP in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ipam
ms.topic: article
ms.assetid: ecdfc589-fa14-4bb3-ab7e-456ebc719385
ms.author: lizross
author: eross-msft
ms.openlocfilehash: ae5e36e9c0931ca5735df6111f73e87ef012ee5d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860594"
---
# <a name="role-based-access-control"></a>Controllo degli accessi in base al ruolo

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Questo argomento fornisce informazioni sull'uso del controllo degli accessi in base al ruolo in gestione indirizzi IP.  
  
> [!NOTE]  
> Oltre a questo argomento, in questa sezione è disponibile la seguente documentazione relativa al controllo di accesso di gestione indirizzi IP.  
>   
> -   [Gestire il controllo degli accessi in base al ruolo con Server Manager](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Server-Manager.md)  
> -   [Gestire il controllo degli accessi in base al ruolo con Windows PowerShell](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md)  
  
Il controllo degli accessi in base al ruolo consente di specificare i privilegi di accesso a diversi livelli, tra cui il server DNS, la zona DNS e i livelli dei record di risorse DNS.  
Usando il controllo degli accessi in base al ruolo, è possibile specificare chi ha un controllo granulare delle operazioni per creare, modificare ed eliminare diversi tipi di record di risorse DNS.  
  
È possibile configurare il controllo degli accessi in modo che gli utenti siano limitati alle autorizzazioni seguenti.  
  
-   Gli utenti possono modificare solo i record di risorse DNS specifici  
  
-   Gli utenti possono modificare i record di risorse DNS di un tipo specifico, ad esempio PTR o MX  
  
-   Gli utenti possono modificare i record di risorse DNS per zone specifiche  
  
## <a name="see-also"></a>Vedi anche  
[Gestire il controllo degli accessi in base al ruolo con Server Manager](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Server-Manager.md)  
[Gestire il controllo degli accessi in base al ruolo con Windows PowerShell](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md)  
[Gestire Gestione indirizzi IP](Manage-IPAM.md)  
  


