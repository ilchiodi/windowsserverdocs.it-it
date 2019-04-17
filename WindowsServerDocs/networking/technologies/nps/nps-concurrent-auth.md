---
title: Aumentare autenticazioni simultanee elaborate dei criteri di rete
description: In questo argomento vengono fornite istruzioni sulla configurazione di autenticazioni simultanee Server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2d9cdada-0625-41c8-8248-a32259b03e47
ms.author: pashort
author: shortpatti
ms.openlocfilehash: aa70c1a26e2c22d26545e1b46a6151d71a2b4095
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="increase-concurrent-authentications-processed-by-nps"></a>Aumentare autenticazioni simultanee elaborate dei criteri di rete

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Per istruzioni sulla configurazione di autenticazioni simultanee Server dei criteri di rete, è possibile utilizzare questo argomento.

Se è installato Server dei criteri di rete \(NPS\) in un computer diverso da un controller di dominio e il server dei criteri di rete riceve un numero elevato di richieste di autenticazione al secondo, è possibile migliorare le prestazioni dei criteri di rete, aumentando il numero di autenticazioni simultanee consentite tra il server dei criteri di rete e il controller di dominio.

A tale scopo, è necessario modificare la chiave del Registro di sistema seguente: 

`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Netlogon\Parameters`

Aggiungere un nuovo valore denominato **MaxConcurrentApi** e assegnare a esso un valore da 2 a 5. 

>[!CAUTION]
>Se si assegna un valore per **MaxConcurrentApi** che è troppo elevata, il server dei criteri di rete può posizionare un carico eccessivo sul controller di dominio.

Per ulteriori informazioni sulla gestione dei criteri di rete, vedere [gestire Server dei criteri di rete](nps-manage-top.md).

Per ulteriori informazioni sui criteri di rete, vedere [Server dei criteri di rete (NPS)](nps-top.md).