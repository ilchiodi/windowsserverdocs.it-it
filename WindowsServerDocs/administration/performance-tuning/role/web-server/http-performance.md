---
title: Ottimizzazione delle prestazioni per HTTP 1.1/2
description: Raccomandazioni per HTTP 1.1/2 l'ottimizzazione delle prestazioni
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: IvanPash; GMonte
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: bf85efa88e377966135c23a548119f19c39cceba
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866372"
---
# <a name="performance-tuning-http-112"></a>HTTP 1.1/2 l'ottimizzazione delle prestazioni

Http/2 è progettato per migliorare le prestazioni sul lato client (ad esempio, tempo di caricamento pagina in un browser). Nel server, lo può rappresentare un lieve aumento nell'utilizzo della CPU. Mentre il server non richiede più un'unica connessione TCP per ogni richiesta, alcuni di tale stato ora verranno mantenuti nel livello HTTP. Inoltre, HTTP/2 ha la compressione intestazione, che rappresenta il carico aggiuntivo della CPU.

Alcune situazioni richiedono un HTTP/1.1 fallback (il ripristino della connessione HTTP/2 e stabilire invece una nuova connessione per utilizzare HTTP/1.1). In particolare, rinegoziazione TLS e autenticazione HTTP (ad eccezione di base e Digest) richieste HTTP/1.1 fallback. Anche se si aggiunge un sovraccarico, queste operazioni già implicano un ritardo e pertanto non sono particolarmente sensibili alle prestazioni.

## <a name="see-also"></a>Vedere anche
- [Web ottimizzazione delle prestazioni Server](index.md) 
- [Ottimizzazione delle prestazioni di IIS 10.0](tuning-iis-10.md)