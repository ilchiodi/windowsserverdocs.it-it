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
ms.openlocfilehash: 3aeb7cb06f82b6f2220e42866682ce918389bf1d
ms.sourcegitcommit: b17ccf7f81e58e8f4dd844be8acf784debbb20ae
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/14/2019
ms.locfileid: "69023901"
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

## <a name="using-the-registry-to-control-the-caching-time"></a>Uso del registro di sistema per controllare il tempo di memorizzazione nella cache

> [!IMPORTANT]  
> Segui con attenzione la procedura descritta in questa sezione. Se le modifiche al Registro di sistema vengono apportate in modo non corretto, possono verificarsi problemi gravi. Prima di modificarlo, [esegui il backup del Registro di sistema per il ripristino](https://support.microsoft.com/help/322756) nel caso in cui si verifichino problemi.

L'intervallo di tempo durante il quale una risposta positiva o negativa viene memorizzata nella cache dipende dai valori delle voci nella chiave del registro di sistema seguente:

**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNSCache\Parameters**

Il valore TTL per le risposte positive è minore dei valori seguenti: 

- Numero di secondi specificati nella risposta alla query ricevuta dal resolver

- Valore dell'impostazione del registro di sistema **MaxCacheTtl** .

>[!Note]
>- Il valore TTL predefinito per le risposte positive è 86.400 secondi (1 giorno).
>- Il valore TTL per le risposte negative è il numero di secondi specificato nell'impostazione del registro di sistema MaxNegativeCacheTtl.
>- Il valore TTL predefinito per le risposte negative è 900 secondi (15 minuti).
Se non si desidera che le risposte negative vengano memorizzate nella cache, impostare l'impostazione del registro di sistema MaxNegativeCacheTtl su 0.

Per impostare il tempo di memorizzazione nella cache in un computer client:

1. Avviare l'editor del registro di sistema (Regedit. exe).

2. Individuare e quindi fare clic sulla chiave seguente nel registro di sistema:

   **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Dnscache\Parameters**

3. Scegliere Nuovo dal menu modifica, fare clic su valore DWORD, quindi aggiungere i valori del registro di sistema seguenti:

   - Nome valore: MaxCacheTtl

     Tipo di dati: REG_DWORD

     Dati valore: Valore predefinito di 86400 secondi. 
     
     Se si riduce il valore TTL massimo nella cache DNS del client a 1 secondo, si ottiene l'aspetto che la cache DNS sul lato client è stata disabilitata.    

   - Nome valore: MaxNegativeCacheTtl

     Tipo di dati: REG_DWORD

     Dati valore: Valore predefinito di 900 secondi. 
     
     Impostare il valore su 0 se non si desidera che le risposte negative vengano memorizzate nella cache.

4. Digitare il valore che si desidera utilizzare, quindi fare clic su OK.

5. Chiudere l'Editor del Registro di sistema.