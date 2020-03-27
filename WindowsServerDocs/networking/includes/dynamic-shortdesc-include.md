---
author: eross-msft
ms.author: lizross
ms.date: 10/02/2018
ms.prod: windows-server
ms:topic: include
ms.openlocfilehash: f2065acb89af4bed4dc525453bb5a294a4e2c3ef
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316478"
---
Con i carichi dinamici, i carichi in uscita vengono distribuiti in base a un hash delle porte e degli indirizzi IP TCP. La modalità dinamica ribilancia inoltre i caricamenti in tempo reale in modo che un determinato flusso in uscita possa spostarsi tra i membri del team. I caricamenti in ingresso, invece, vengono distribuiti allo stesso modo della porta Hyper-V. In breve, la modalità dinamica utilizza gli aspetti migliori dell'hash degli indirizzi e della porta Hyper-V ed è la modalità di bilanciamento del carico con prestazioni più elevate. 

