---
title: Novità del client DNS in Windows Server
description: Questo argomento fornisce una panoramica delle nuove funzionalità del client DNS in Windows Server e Windows 10
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: 6edaba84-4595-4fd8-95d7-64d4d975a38a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 696ea0b499a4132d630cc0cda15a1d7efdac37a8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356054"
---
# <a name="whats-new-in-dns-client-in-windows-server-2016"></a>Novità di Client DNS in Windows Server 2016

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In questo argomento vengono descritte le funzionalità client di Domain Name System (DNS) nuove o modificate in Windows 10 e Windows Server 2016 e versioni successive di questi sistemi operativi.
  
## <a name="updates-to-dns-client"></a>Aggiornamenti per il client DNS

**Associazione al servizio client DNS**: In Windows 10, il servizio Client DNS offre supporto avanzato per i computer con più di un'interfaccia di rete. Per i computer multihomed, è con ottimizzazione per la risoluzione DNS nei modi seguenti:  
  
-   Quando un server DNS è configurato su un'interfaccia specifica viene utilizzato per risolvere una query DNS, il servizio Client DNS verrà associato a questa interfaccia prima di inviare query DNS.  
  
    Mediante l'associazione a una specifica interfaccia, il client DNS può chiaramente specificare l'interfaccia in cui si verifica la risoluzione dei nomi, abilitazione delle applicazioni per ottimizzare le comunicazioni con il client DNS attraverso questa interfaccia di rete.  
  
-   Se il server DNS utilizzato è definito da un'impostazione di criteri di gruppo dal criterio di tabella (Risoluzione dei nomi), il servizio Client DNS non viene associato a un'interfaccia specifica.  
  
> [!NOTE]  
> Le modifiche al servizio client DNS in Windows 10 sono presenti anche nei computer che eseguono Windows Server 2016 e versioni successive.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Novità del server DNS in Windows Server 2016](What-s-New-in-DNS-Server.md)  
  

