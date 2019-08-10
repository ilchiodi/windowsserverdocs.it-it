---
title: Disabilitare la memorizzazione nella cache sul lato client DNS nei client DNS
description: Questo articolo illustra come disabilitare la memorizzazione nella cache sul lato client DNS nei client DNS.
manager: willchen
ms.prod: ''
ms.technology: networking-dns
ms.topic: article
ms.author: delhan
ms.date: 8/8/2019
author: Deland-Han
ms.openlocfilehash: 0b20400029b462798587c2291431b5a7c3d61775
ms.sourcegitcommit: 0e3c2473a54f915d35687d30d1b4b1ac2bae4068
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/09/2019
ms.locfileid: "68917808"
---
# <a name="disable-dns-client-side-caching-on-dns-clients"></a>Disabilitare la memorizzazione nella cache sul lato client DNS nei client DNS

Windows contiene una cache DNS sul lato client. La funzionalità di memorizzazione nella cache DNS sul lato client può generare una falsa impressione che il bilanciamento del carico "round robin" DNS non sia in corso dal server DNS al computer client Windows. Quando si usa il comando ping per cercare lo stesso nome di dominio A record, il client può usare lo stesso indirizzo IP.  

## <a name="how-to-disable-client-side-caching"></a>Come disabilitare la memorizzazione nella cache sul lato client

Per arrestare la memorizzazione nella cache DNS, eseguire uno dei comandi seguenti:

```cmd
net stop dnscache
```

```cmd
sc servername stop dnscache
```


Per disabilitare la cache DNS in modo permanente in Windows, usare lo strumento controller di servizio o lo strumento Servizi per impostare il tipo di avviodel servizio client DNS su disabilitato. Si noti che il nome del servizio client DNS di Windows può essere visualizzato anche come "dnscache". 

> [!NOTE]
> Se la cache del resolver DNS viene disattivata, le prestazioni complessive del computer client diminuiscono e il traffico di rete per le query DNS aumenta. 

Il servizio client DNS ottimizza le prestazioni della risoluzione dei nomi DNS archiviando in memoria i nomi risolti in precedenza. Se il servizio client DNS è disattivato, il computer può comunque risolvere i nomi DNS usando i server DNS della rete. 

Quando il resolver di Windows riceve una risposta, positiva o negativa, a una query, aggiunge tale risposta alla cache e quindi crea un record di risorse DNS. Il resolver controlla sempre la cache prima di eseguire una query su qualsiasi server DNS. Se un record di risorse DNS si trova nella cache, il resolver usa il record dalla cache anziché eseguire una query su un server. Questo comportamento accelera le query e riduce il traffico di rete per le query DNS. 

È possibile utilizzare lo strumento Ipconfig per visualizzare e scaricare la cache del resolver DNS. Per visualizzare la cache del resolver DNS, eseguire il comando seguente al prompt dei comandi:

```cmd
ipconfig /displaydns 
```

Questo comando Visualizza il contenuto della cache del resolver DNS, inclusi i record di risorse DNS precaricati dal file host e tutti i nomi di cui è stata eseguita una query di recente che sono stati risolti dal sistema. Dopo un certo periodo di tempo, il resolver Elimina il record dalla cache. Il periodo di tempo è specificato dal valore **TTL (time to Live)** associato al record di risorse DNS. È possibile svuotare la cache anche manualmente. Dopo lo scaricamento della cache, il computer deve eseguire di nuovo la query sui server DNS per tutti i record di risorse DNS risolti in precedenza dal computer. Per eliminare le voci nella cache del resolver DNS, eseguire `ipconfig /flushdns` al prompt dei comandi.

## <a name="next-step"></a>Passaggio successivo

Per ulteriori informazioni [, vedere come disabilitare la memorizzazione nella cache DNS sul lato client in Windows](https://support.microsoft.com/kb/318803) .
