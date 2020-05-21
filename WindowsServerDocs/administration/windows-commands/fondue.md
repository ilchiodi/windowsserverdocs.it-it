---
title: fondue
description: Argomento di riferimento per il comando fondue, che Abilita le funzionalità facoltative di Windows scaricando i file necessari da Windows Update o da un'altra origine specificata da Criteri di gruppo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fc4467f6-ddbb-4d6d-b51e-5a50a957b8c0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7a9e751a5ad46d557aa2317ebe4c144fa6f004fa
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2020
ms.locfileid: "83437206"
---
# <a name="fondue"></a>fondue

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Abilita la funzionalità facoltative di Windows per download dei file richiesti da Windows Update o un'altra origine, specificata dai criteri di gruppo. Il file manifesto per la funzionalità deve essere già installato nell'immagine Windows.

## <a name="syntax"></a>Sintassi

```
fondue.exe /enable-feature:<feature_name> [/caller-name:<program_name>] [/hide-ux:{all | rebootrequest}]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| /Enable-Feature`<feature_name>` | Specifica il nome della funzione facoltativa di Windows che si desidera abilitare. È possibile abilitare solo una funzionalità per ogni riga di comando. Per abilitare più funzionalità, usare fondue. exe per ciascuna funzionalità. |
| /caller-name:`<program_name>` | Specifica il nome del programma o del processo quando si chiama fondue. exe da uno script o da un file batch. È possibile utilizzare questa opzione per aggiungere il nome del programma per il report SQM se si verifica un errore. |
| /hide-ux:`{all | rebootrequest}` | Utilizzare **tutti** per nascondere tutti i messaggi all'utente incluse le richieste di stato di avanzamento e l'autorizzazione per accedere a Windows Update. Se è necessaria l'autorizzazione, l'operazione avrà esito negativo.<p>Usare **rebootrequest** per nascondere solo i messaggi utente che richiedono l'autorizzazione per riavviare il computer. Utilizzare questa opzione se si dispone di uno script che controlli richieste di riavvio. |

### <a name="examples"></a>Esempi

Per abilitare Microsoft .NET Framework 4,8, digitare:

```
fondue.exe /enable-feature:NETFX4
```

Per abilitare Microsoft .NET Framework 4,8, aggiungere il nome del programma al report SQM e non visualizzare messaggi all'utente, digitare:

```
fondue.exe /enable-feature:NETFX4 /caller-name:Admin.bat /hide-ux:all
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Download di Microsoft .NET Framework 4,8](https://dotnet.microsoft.com/download/dotnet-framework/net48)
