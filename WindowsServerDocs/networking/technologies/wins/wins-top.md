---
title: Windows Internet Name Service (WINS)
description: In questo argomento vengono fornite informazioni sulla rimozione delle autorizzazioni WINS e l'uso di DNS per i servizi di risoluzione dei nomi nella rete.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 32eabe7d-1130-4001-a79a-8ddb31993e5b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bbc1871d29021aa3c99f14368a4711dac63f4cee
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843632"
---
#  <a name="windows-internet-name-service-wins"></a>Windows Internet Name Service (WINS)

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

WINS (Windows Internet Name Service) è un servizio legacy di registrazione e risoluzione dei nomi di computer che esegue il mapping dei nomi NetBIOS dei computer a indirizzi IP.

Se non hai già WINS distribuito nella rete, non distribuiscono WINS, in alternativa, distribuire Domain Name System \(DNS\). DNS inoltre fornisce servizi di registrazione e risoluzione dei nomi di computer e include molti altri vantaggi su WINS, come l'integrazione con Active Directory Domain Services.

Per altre informazioni, vedere [sistema DNS (Domain Name)](https://docs.microsoft.com/windows-server/networking/dns/dns-top)

Se è già stata distribuita WINS sulla rete, si consiglia di DNS di distribuzione e quindi rimuovere le autorizzazioni WINS.
