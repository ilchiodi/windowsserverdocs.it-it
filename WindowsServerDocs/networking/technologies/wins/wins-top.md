---
title: Windows Internet Name Service (WINS)
description: In questo argomento vengono fornite informazioni sulla rimozione delle autorizzazioni WINS e sull'utilizzo di DNS per i servizi di risoluzione dei nomi nella rete.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 32eabe7d-1130-4001-a79a-8ddb31993e5b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 219c313dfeb26319cd5f537df417724de7044648
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405247"
---
#  <a name="windows-internet-name-service-wins"></a>Windows Internet Name Service (WINS)

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

WINS (Windows Internet Name Service) è un servizio legacy di registrazione e risoluzione dei nomi di computer che esegue il mapping dei nomi NetBIOS dei computer a indirizzi IP.

Se WINS non è ancora stato distribuito nella rete, non distribuire WINS, ma distribuire Domain Name System \(\)DNS. DNS fornisce anche servizi di registrazione e risoluzione dei nomi di computer e include molti vantaggi aggiuntivi rispetto a WINS, ad esempio l'integrazione con Active Directory Domain Services.

Per ulteriori informazioni, vedere [Domain Name System (DNS)](https://docs.microsoft.com/windows-server/networking/dns/dns-top)

Se è già stato distribuito WINS nella rete, si consiglia di distribuire DNS e quindi di rimuovere le autorizzazioni di WINS.
