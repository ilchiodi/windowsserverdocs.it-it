---
title: Ottimizzazione delle prestazioni per HTTP 1.1/2
description: Suggerimenti per l'ottimizzazione delle prestazioni per HTTP 1.1/2
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: ivanpash; gmonte
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 5e62f7428f015193896aba5c7d9c146bd11e7225
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851684"
---
# <a name="performance-tuning-http-112"></a>Ottimizzazione delle prestazioni HTTP 1.1/2

HTTP/2 ha lo scopo di migliorare le prestazioni sul lato client, ad esempio il tempo di caricamento della pagina in un browser. Sul server può rappresentare un lieve aumento dei costi della CPU. Mentre il server non richiede più una singola connessione TCP per ogni richiesta, parte di tale stato verrà ora mantenuta nel livello HTTP. Inoltre, HTTP/2 ha una compressione di intestazione, che rappresenta un carico aggiuntivo della CPU.

Per alcune situazioni è necessario un fallback HTTP/1.1 (reimpostazione della connessione HTTP/2 e stabilire invece una nuova connessione per l'utilizzo di HTTP/1.1). In particolare, la rinegoziazione TLS e l'autenticazione HTTP (ad eccezione di Basic e digest) richiedono il fallback HTTP/1.1. Anche se questa operazione aggiunge un sovraccarico, queste operazioni implicano già un certo ritardo, quindi non sono particolarmente sensibili alle prestazioni.

## <a name="see-also"></a>Vedere anche
- [Ottimizzazione delle prestazioni del server Web](index.md) 
- [Ottimizzazione delle prestazioni di IIS 10.0](tuning-iis-10.md)