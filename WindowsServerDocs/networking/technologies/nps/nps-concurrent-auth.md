---
title: Aumentare il numero di autenticazioni simultanee elaborate dal Server dei criteri di rete
description: Questo argomento fornisce istruzioni sulla configurazione delle autenticazioni simultanee di Server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2d9cdada-0625-41c8-8248-a32259b03e47
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fd930e34a4adf6c55812385b691df3e3575a4280
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818262"
---
# <a name="increase-concurrent-authentications-processed-by-nps"></a>Aumentare il numero di autenticazioni simultanee elaborate dal Server dei criteri di rete

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per istruzioni sulla configurazione di autenticazioni simultanee di Server dei criteri di rete.

Se è stato installato Server dei criteri di rete \(NPS\) in un computer diverso da un dominio controller e i criteri di rete riceve un numero elevato di richieste di autenticazione al secondo, è possibile migliorare le prestazioni dei criteri di rete, aumentando il numero di autenticazioni simultanee consentite tra i criteri di rete e il controller di dominio.

A tale scopo, è necessario modificare la chiave del Registro di sistema seguente: 

`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Netlogon\Parameters`

Aggiungere un nuovo valore denominato **MaxConcurrentApi** alla quale assegna un valore da 2 a 5. 

>[!CAUTION]
>Se si assegna un valore per **MaxConcurrentApi** che è troppo elevato, i criteri di rete potrebbe inserire un carico eccessivo sul controller di dominio.

Per altre informazioni sulla gestione dei criteri di rete, vedere [gestire i Server dei criteri di rete](nps-manage-top.md).

Per altre informazioni sui criteri di rete, vedere [Strumentazione gestione Windows (NPS, Network Policy Server)](nps-top.md).