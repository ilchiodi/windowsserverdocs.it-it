---
title: fondue
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fc4467f6-ddbb-4d6d-b51e-5a50a957b8c0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e2e3b64ff05ab9a539b7734f37b7515c6f9ef123
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725593"
---
# <a name="fondue"></a>fondue

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Abilita la funzionalità facoltative di Windows per download dei file richiesti da Windows Update o un'altra origine, specificata dai criteri di gruppo. Il file manifesto per la funzionalità deve essere già installato nell'immagine Windows. 
## <a name="syntax"></a>Sintassi
```
fondue.exe /enable-feature:<feature_name> [/caller-name:<program_name>] [/hide-ux:{all | rebootRequest}]
```
#### <a name="parameters"></a>Parametri

|              Parametro              |                                                                                                                                                                     Descrizione                                                                                                                                                                     |
|-------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  /Enable-Feature: *feature_name* <>   |                                                                               Specifica il nome della funzione facoltativa di Windows che si desidera abilitare. È possibile abilitare solo una funzionalità per ogni riga di comando. Per abilitare più funzionalità, usare fondue. exe per ciascuna funzionalità.                                                                                |
|    /Caller-Name: *program_name* <>    |                                                                                 Specifica il nome del programma o del processo quando si chiama fondue. exe da uno script o da un file batch. È possibile utilizzare questa opzione per aggiungere il nome del programma per il report SQM se si verifica un errore.                                                                                 |
| /Hide-UX: {tutti & #124; rebootRequest} | Utilizzare **tutti** per nascondere tutti i messaggi all'utente incluse le richieste di stato di avanzamento e l'autorizzazione per accedere a Windows Update. Se è necessaria l'autorizzazione, l'operazione avrà esito negativo.<p>Utilizzare **rebootRequest** per nascondere solo i messaggi dell'utente che richiede l'autorizzazione per riavviare il computer. Utilizzare questa opzione se si dispone di uno script che controlli richieste di riavvio. |

## <a name="examples"></a>Esempi
Per abilitare Microsoft .NET Framework 3.5, digitare:
```
fondue.exe /enable-feature:NETFX3
```
Per abilitare Microsoft .NET Framework 3.5, aggiungere il nome del programma per il report SQM e visualizza messaggi per l'utente, tipo:
```
fondue.exe /enable-feature:NETFX3 /caller-name:Admin.bat /hide-ux:all
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
- - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
  ## <a name="see-also"></a>Vedi anche
  [Considerazioni sulla distribuzione di Microsoft .NET Framework 3.5](https://go.microsoft.com/fwlink/?LinkId=248869)
