---
title: Novità di Client DNS in Windows Server
description: In questo argomento viene fornita una panoramica delle nuove funzionalità di Client DNS in Windows Server e Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 6edaba84-4595-4fd8-95d7-64d4d975a38a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: defe88fe2a1e4f5393be4d5d5938d9f5bfbfb5d6
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="whats-new-in-dns-client-in-windows-server-2016"></a>Novità di Client DNS in Windows Server 2016

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Questo argomento descrive le funzionalità client sistema DNS (Domain Name) nuove o modificate in Windows 10 e Windows Server 2016 e versioni successive di questi sistemi operativi.
  
## <a name="updates-to-dns-client"></a>Aggiornamenti client DNS

**Associazione al servizio Client DNS**: In Windows 10, il servizio Client DNS offre supporto avanzato per i computer con più di un'interfaccia di rete. Per i computer multihomed, è ottimizzata la risoluzione DNS nei modi seguenti:  
  
-   Quando un server DNS è configurato su un'interfaccia specifica viene utilizzato per risolvere una query DNS, il servizio Client DNS verrà associato a questa interfaccia prima di inviare query DNS.  
  
    Eseguendo il binding a una specifica interfaccia, il client DNS può chiaramente specificare l'interfaccia in cui si verifica la risoluzione dei nomi, abilitazione delle applicazioni per ottimizzare le comunicazioni con il client DNS attraverso questa interfaccia di rete.  
  
-   Se il server DNS utilizzato è definito da un'impostazione di criteri di gruppo dal criterio di tabella (risoluzione dei nomi), il servizio Client DNS non eseguire il binding a un'interfaccia specifica.  
  
> [!NOTE]  
> Le modifiche apportate al servizio Client DNS in Windows 10 sono presenti anche nei computer che eseguono Windows Server 2016 e versioni successive.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Novità di Server DNS in Windows Server 2016](What-s-New-in-DNS-Server.md)  
  

