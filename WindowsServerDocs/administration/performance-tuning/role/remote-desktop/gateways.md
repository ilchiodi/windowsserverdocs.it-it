---
title: Ottimizzazione delle prestazioni Desktop remoto Gateway
description: Suggerimenti per l'ottimizzazione delle prestazioni per Gateway Desktop remoto
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: HammadBu; VladmiS
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: fcd7afd840df12ec19e162f751df9e5c0c9c84d4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385008"
---
# <a name="performance-tuning-remote-desktop-gateways"></a>Ottimizzazione delle prestazioni Desktop remoto Gateway

> [!NOTE]
> In Windows 8 + e Windows Server 2012 R2 +, Desktop remoto Gateway (Gateway Desktop remoto) supporta i trasporti TCP, UDP e RPC legacy. La maggior parte dei seguenti dati riguarda il trasporto RPC legacy. Se il trasporto RPC legacy non è in uso, questa sezione non è applicabile.

In questo argomento vengono descritti i parametri correlati alle prestazioni che consentono di migliorare le prestazioni di una distribuzione dei clienti e le ottimizzazioni basate sui modelli di utilizzo della rete del cliente.

Al suo interno, Gateway Desktop remoto esegue numerose operazioni di invio di pacchetti tra Connessione Desktop remoto istanze e le istanze del server Host sessione Desktop remoto all'interno della rete del cliente.

> [!NOTE]
> I parametri seguenti si applicano solo al trasporto RPC.

Internet Information Services (IIS) e Gateway Desktop remoto esportano i parametri del registro di sistema seguenti per contribuire a migliorare le prestazioni del sistema nel Gateway Desktop remoto.

**Ottimizzazioni di thread**

-   **MaxIOThreads**

    ``` syntax
    HKLM\Software\Microsoft\Terminal Server Gateway\Maxiothreads (REG_DWORD)
    ```

    Questo pool di thread specifico dell'app specifica il numero di thread creati da Gateway Desktop remoto per gestire le richieste in ingresso. Se questa impostazione del registro di sistema è presente, avrà effetto. Il numero di thread è uguale al numero di processi logici. Se il numero di processori logici è minore di 5, il valore predefinito è 5 thread.

-   **MaxPoolThreads**

    ``` syntax
    HKLM\System\CurrentControlSet\Services\InetInfo\Parameters\MaxPoolThreads (REG_DWORD)
    ```

    Questo parametro specifica il numero di thread del pool IIS da creare per ogni processore logico. I thread del pool di IIS controllano la rete per le richieste ed elaborano tutte le richieste in ingresso. Il numero di **MaxPoolThreads** non include i thread utilizzati da Gateway Desktop remoto. Il valore predefinito è 4.

**Ottimizzazione delle chiamate a procedure remote per Gateway Desktop remoto**

I parametri seguenti consentono di ottimizzare le chiamate a procedure remote (RPC) ricevute da Connessione Desktop remoto e computer Gateway Desktop remoto. La modifica delle finestre consente di limitare la quantità di dati che passano attraverso ogni connessione e può migliorare le prestazioni per gli scenari RPC su HTTP V2.

-   **ServerReceiveWindow**

    ``` syntax
    HKLM\Software\Microsoft\Rpc\ServerReceiveWindow (REG_DWORD)
    ```

    Il valore predefinito è 64 KB. Questo valore specifica la finestra utilizzata dal server per i dati ricevuti dal proxy RPC. Il valore minimo è impostato su 8 KB e il valore massimo è impostato su 1 GB. Se non è presente alcun valore, viene utilizzato il valore predefinito. Quando vengono apportate modifiche a questo valore, è necessario riavviare IIS per rendere effettive le modifiche.

-   **ServerReceiveWindow**

    ``` syntax
    HKLM\Software\Microsoft\Rpc\ServerReceiveWindow (REG_DWORD)
    ```

    Il valore predefinito è 64 KB. Questo valore specifica la finestra utilizzata dal client per i dati ricevuti dal proxy RPC. Il valore minimo è 8 KB e il valore massimo è 1 GB. Se non è presente alcun valore, viene utilizzato il valore predefinito.

## <a name="monitoring-and-data-collection"></a>Monitoraggio e raccolta dati

Il seguente elenco di contatori delle prestazioni è considerato un set di contatori di base quando si monitora l'utilizzo delle risorse nel Gateway Desktop remoto:

-   \\Gateway di Servizi terminal\\\*

-   \\Proxy RPC/HTTP\\\*

-   \\Proxy RPC/HTTP per server\\\*

-   \\Servizio Web\\\*

-   \\W3SVC\_W3WP\\\*

-   \\IPv4\\\*

-   \\Memoria\\\*

-   \\Interfaccia di rete\*()\\\*

-   \\Elabora (\*)\\\*

-   \\Informazioni sul processore\*()\\\*

-   \\Sincronizzazione (\*)\\\*

-   \\Sistema\\\*

-   \\TCPv4\\\*

I contatori delle prestazioni seguenti sono applicabili solo per il trasporto RPC legacy:

-   \\RPC proxy\\ \* RPC/http

-   \\RPC/proxy HTTP per server\\RPC \*

-   \\RPC servizio\\ \* Web

-   \\\\ RPC\_W3WPW3WP\*

> [!NOTE]
> Se applicabile, aggiungere gli \\oggetti\\ IPv6 \\\* e\\ TCPv6.\* ReplaceThisText

