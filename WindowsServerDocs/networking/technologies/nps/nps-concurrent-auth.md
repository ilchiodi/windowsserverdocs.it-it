---
title: Aumentare il numero di autenticazioni simultanee elaborate dal Server dei criteri di rete
description: Questo argomento fornisce istruzioni sulla configurazione di autenticazioni simultanee del server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 2d9cdada-0625-41c8-8248-a32259b03e47
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 5397a1de41717dfc65ee0b0582c1289c6a53b33a
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316292"
---
# <a name="increase-concurrent-authentications-processed-by-nps"></a>Aumentare il numero di autenticazioni simultanee elaborate dal Server dei criteri di rete

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per istruzioni sulla configurazione di autenticazioni simultanee del server dei criteri di rete.

Se è stato installato Server dei criteri di rete \(NPS\) in un computer diverso da un controller di dominio e il server dei criteri di rete riceve un numero elevato di richieste di autenticazione al secondo, è possibile migliorare le prestazioni di NPS aumentando il numero di autenticazioni simultanee consentite tra il server dei criteri di rete e il controller di dominio

A tale scopo, è necessario modificare la chiave del registro di sistema seguente: 

`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Netlogon\Parameters`

Aggiungere un nuovo valore denominato **MaxConcurrentApi** e assegnargli un valore compreso tra 2 e 5. 

>[!CAUTION]
>Se si assegna un valore a **MaxConcurrentApi** troppo elevato, il server dei criteri di dominio potrebbe inserire un carico eccessivo sul controller di dominio.

Per ulteriori informazioni sulla gestione dei server dei criteri di rete, vedere [Manage Network Policy Server](nps-manage-top.md).

Per ulteriori informazioni su NPS, vedere [Server dei criteri di rete (NPS)](nps-top.md).