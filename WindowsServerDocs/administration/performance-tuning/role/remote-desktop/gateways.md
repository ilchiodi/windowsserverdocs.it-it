---
title: Gateway Desktop remoto l'ottimizzazione delle prestazioni
description: Raccomandazioni per i gateway Desktop remoto per l'ottimizzazione delle prestazioni
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: HammadBu; VladmiS
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: f3ac020b3137621f6b2535c973ab7759443e1535
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811430"
---
# <a name="performance-tuning-remote-desktop-gateways"></a>Gateway Desktop remoto l'ottimizzazione delle prestazioni

> [!NOTE]
> In Windows 8 e versioni successive e Windows Server 2012 R2 +, Gateway Desktop remoto (Gateway Desktop remoto) supporta TCP, UDP e trasporti RPC legacy. La maggior parte dei dati seguenti sia per quanto riguarda il trasporto RPC legacy. Se non viene utilizzato il trasporto RPC legacy, in questa sezione non è applicabile.

Questo argomento descrive i parametri relativi alle prestazioni che consentono di migliorare le prestazioni di una distribuzione dei clienti e se che si basano su modelli di utilizzo di rete del cliente.

In sostanza, Gateway Desktop remoto consente di eseguire pacchetti molte operazioni tra le istanze di connessione Desktop remoto e le istanze del server Host sessione Desktop remoto all'interno di rete del cliente di inoltro.

> [!NOTE]
> I parametri seguenti si applicano a solo il trasporto RPC.

Internet Information Services (IIS) e Gateway Desktop remoto Esporta i parametri del Registro di sistema seguenti per contribuire a migliorare le prestazioni del sistema in Gateway Desktop remoto.

**Se di thread**

-   **MaxIoThreads**

    ``` syntax
    HKLM\Software\Microsoft\Terminal Server Gateway\Maxiothreads (REG_DWORD)
    ```

    Questo pool di thread specifico dell'app specifica il numero di thread che Gateway Desktop remoto consente di creare e per gestire le richieste in ingresso. Se questa impostazione del Registro di sistema è presente, ha effetto. Il numero di thread è uguale al numero di processi logici. Se il numero di processori logici è minore di 5, il valore predefinito è 5 thread.

-   **MaxPoolThreads**

    ``` syntax
    HKLM\System\CurrentControlSet\Services\InetInfo\Parameters\MaxPoolThreads (REG_DWORD)
    ```

    Questo parametro specifica il numero di thread di pool IIS da creare per ogni processore logico. I thread di pool IIS guardare la rete per le richieste ed elaborano tutte le richieste in ingresso. Il **MaxPoolThreads** numero non include thread che utilizza Gateway Desktop remoto. Il valore predefinito è 4.

**Se chiamata di procedura remota per Gateway Desktop remoto**

I parametri seguenti consentono di ottimizzare le chiamate di procedura remota (RPC) che vengono ricevute dai computer di connessione Desktop remoto e Gateway Desktop remoto. La modifica di windows consente di limitare la quantità di dati viene trasmesso attraverso ogni connessione e può migliorare le prestazioni per RPC su scenari HTTP v2.

-   **ServerReceiveWindow**

    ``` syntax
    HKLM\Software\Microsoft\Rpc\ServerReceiveWindow (REG_DWORD)
    ```

    Il valore predefinito è 64 KB. Questo valore specifica la finestra usata per i dati ricevuti dal proxy RPC dal server. Il valore minimo è impostato su 8 KB, e il valore massimo è impostato su 1 GB. Se un valore non è presente, viene utilizzato il valore predefinito. Quando vengono apportate modifiche a questo valore, è necessario riavviare IIS rendere effettiva la modifica.

-   **ServerReceiveWindow**

    ``` syntax
    HKLM\Software\Microsoft\Rpc\ServerReceiveWindow (REG_DWORD)
    ```

    Il valore predefinito è 64 KB. Questo valore specifica la finestra utilizzata dal client per i dati ricevuti dal proxy RPC. Il valore minimo è 8 KB, e il valore massimo è 1 GB. Se un valore non è presente, viene utilizzato il valore predefinito.

## <a name="monitoring-and-data-collection"></a>Monitoraggio e la raccolta dati

Il seguente elenco di contatori delle prestazioni è considerato un set di contatori di base quando si monitora l'utilizzo delle risorse Gateway Desktop remoto:

-   \\Gateway di Servizi terminal\\\*

-   \\RPC/HTTP Proxy\\\*

-   \\Proxy RPC/HTTP per ogni Server\\\*

-   \\Servizio Web\\\*

-   \\W3SVC\_W3WP\\\*

-   \\IPv4\\\*

-   \\Memoria\\\*

-   \\Interfaccia di rete (\*)\\\*

-   \\Process(\*)\\\*

-   \\Informazioni sul processore (\*)\\\*

-   \\Synchronization(\*)\\\*

-   \\System\\\*

-   \\TCPv4\\\*

Contatori delle prestazioni seguenti sono applicabili solo per il trasporto RPC legacy:

-   \\RPC/HTTP Proxy\\\* RPC

-   \\Proxy RPC/HTTP per ogni Server\\ \* RPC

-   \\Web Service\\\* RPC

-   \\W3SVC\_W3WP\\\* RPC

> [!NOTE]
> Se applicabile, aggiungere il \\IPv6\\ \* e \\TCPv6\\ \* oggetti. ReplaceThisText

