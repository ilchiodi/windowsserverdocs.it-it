---
title: Windows Internet Name Service (WINS)
description: In questo argomento fornisce informazioni sulla rimozione delle autorizzazioni WINS e l'utilizzo di DNS per i servizi di risoluzione dei nomi nella rete.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 32eabe7d-1130-4001-a79a-8ddb31993e5b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5a3d132ada7b1ede83b046499058399a9da12190
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
#  <a name="windows-internet-name-service-wins"></a>Windows Internet Name Service (WINS)

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Windows Internet Name Service (WINS) è un computer legacy nome risoluzione e registrazione del servizio che esegue il mapping di nomi computer NetBIOS in indirizzi IP.

Se non si dispone già di WINS distribuito nella rete, non distribuire WINS, invece, distribuire \(DNS\) Domain Name System. DNS anche fornisce servizi di registrazione e risoluzione dei nomi di computer e include molti altri vantaggi tramite WINS, ad esempio l'integrazione con servizi di dominio Active Directory.

Per ulteriori informazioni, vedere [sistema DNS (Domain Name)](https://docs.microsoft.com/windows-server/networking/dns/dns-top)

Se è già stato distribuito WINS nella rete, è consigliabile distribuire DNS e quindi rimuovere WINS.
