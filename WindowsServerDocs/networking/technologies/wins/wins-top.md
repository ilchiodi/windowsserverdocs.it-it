---
title: Windows Internet Name Service (WINS)
description: In questo argomento vengono fornite informazioni sulla rimozione delle autorizzazioni WINS e sull'utilizzo di DNS per i servizi di risoluzione dei nomi nella rete.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 32eabe7d-1130-4001-a79a-8ddb31993e5b
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 54e3f69500ccb3dbf6b2dfe47f6dd035e0a20511
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315161"
---
#  <a name="windows-internet-name-service-wins"></a>Windows Internet Name Service (WINS)

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

WINS (Windows Internet Name Service) è un servizio legacy di registrazione e risoluzione dei nomi di computer che esegue il mapping dei nomi NetBIOS dei computer a indirizzi IP.

Se WINS non è ancora stato distribuito nella rete, non distribuire WINS, ma distribuire Domain Name System \(\)DNS. DNS fornisce anche servizi di registrazione e risoluzione dei nomi di computer e include molti vantaggi aggiuntivi rispetto a WINS, ad esempio l'integrazione con Active Directory Domain Services.

Per ulteriori informazioni, vedere [Domain Name System (DNS)](https://docs.microsoft.com/windows-server/networking/dns/dns-top)

Se è già stato distribuito WINS nella rete, si consiglia di distribuire DNS e quindi di rimuovere le autorizzazioni di WINS.
