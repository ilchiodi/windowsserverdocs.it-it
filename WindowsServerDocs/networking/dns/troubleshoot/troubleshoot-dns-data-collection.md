---
title: Risoluzione dei problemi di Domain Name System (DNS)
description: Questo articolo illustra come raccogliere dati quando si verificano problemi DNS.
manager: dcscontentpm
ms.technology: networking-dns
ms.topic: article
ms.author: delhan
ms.date: 8/8/2019
author: Deland-Han
ms.openlocfilehash: 04bfbfbd2957aec21da966a48feca8d7f160a29c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860044"
---
# <a name="troubleshooting-domain-name-system-dns-issues"></a>Risoluzione dei problemi di Domain Name System (DNS)
 
I problemi di risoluzione dei nomi di dominio possono essere suddivisi in problemi sul lato client e sul lato server. In generale, è consigliabile iniziare con la risoluzione dei problemi lato client, a meno che non si determini durante la fase di definizione dell'ambito che il problema si sta verificando in modo sicuro sul lato server.

- [Risoluzione dei problemi dei client DNS](troubleshoot-dns-client.md)

- [Risoluzione dei problemi relativi ai server DNS](troubleshoot-dns-server.md)
 
## <a name="data-collection"></a>Raccolta dati
 
Quando si verifica il problema, è consigliabile raccogliere contemporaneamente i dati sia sul lato client che sul lato server. Tuttavia, a seconda del problema effettivo, è possibile avviare la raccolta in un singolo set di dati nel client DNS o nel server DNS.
 
Per raccogliere una diagnostica di rete Windows da un client interessato e il server DNS configurato, attenersi alla seguente procedura:

1. Avviare le acquisizioni di rete nel client e nel server:

   ```cmd
   netsh trace start capture=yes tracefile=c:\%computername%_nettrace.etl
   ```

2. Cancellare la cache DNS nel client DNS eseguendo il comando seguente:

   ```cmd
   ipconfig /flushdns
   ```

3. Riprodurre il problema.

4. Arrestare e salvare le tracce:

   ```cmd
   netsh trace stop
   ```

5. Salvare i file NetTrace. cab da ogni computer. Queste informazioni saranno utili quando si contatta supporto tecnico Microsoft.