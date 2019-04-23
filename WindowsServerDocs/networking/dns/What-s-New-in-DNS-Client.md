---
title: Che cosa sono le novità di Client DNS in Windows Server
description: In questo argomento viene fornita una panoramica delle nuove funzionalità di Client DNS in Windows Server e Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 6edaba84-4595-4fd8-95d7-64d4d975a38a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 34b1a64465e217fbd7e6b3ae55e89832a7a4e48c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860562"
---
# <a name="whats-new-in-dns-client-in-windows-server-2016"></a>Novità di Client DNS in Windows Server 2016

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Questo argomento descrive le funzionalità del client Domain Name System (DNS) nuove o modificate in Windows 10 e Windows Server 2016 e versioni successive di questi sistemi operativi.
  
## <a name="updates-to-dns-client"></a>Aggiornamenti client DNS

**Associazione al servizio Client DNS**: In Windows 10, il servizio Client DNS offre supporto avanzato per i computer con più di un'interfaccia di rete. Per i computer multihomed, è con ottimizzazione per la risoluzione DNS nei modi seguenti:  
  
-   Quando un server DNS è configurato su un'interfaccia specifica viene utilizzato per risolvere una query DNS, il servizio Client DNS verrà associato a questa interfaccia prima di inviare query DNS.  
  
    Mediante l'associazione a una specifica interfaccia, il client DNS può chiaramente specificare l'interfaccia in cui si verifica la risoluzione dei nomi, abilitazione delle applicazioni per ottimizzare le comunicazioni con il client DNS attraverso questa interfaccia di rete.  
  
-   Se il server DNS utilizzato è definito da un'impostazione di criteri di gruppo dal criterio di tabella (Risoluzione dei nomi), il servizio Client DNS non viene associato a un'interfaccia specifica.  
  
> [!NOTE]  
> Le modifiche al servizio Client DNS in Windows 10 sono anche presenti nel computer che eseguono Windows Server 2016 e versioni successive.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Che cosa sono le novità di Server DNS in Windows Server 2016](What-s-New-in-DNS-Server.md)  
  

