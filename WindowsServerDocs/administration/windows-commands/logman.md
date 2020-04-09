---
title: logman
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 574a5203-5b3b-4759-a678-f26d00dde447
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bb6654cce0e23ac08a2fa6334d6144b08c8b65f3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80840404"
---
# <a name="logman"></a>logman

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

**logman** crea e gestisce i registri di sessione di traccia eventi e di prestazioni e supporta molte funzioni di performance monitor dalla riga di comando.
## <a name="syntax"></a>Sintassi
```
logman [create | query | start | stop | delete| update | import | export | /?] [options]
```
## <a name="actions"></a>Azioni
|Azione|Descrizione|
|-----|--------|
|[logman create](logman-create.md)|creare un contatore, una traccia, un agente di raccolta dati di configurazione o un'API.|
|[logman query](logman-query.md)|eseguire query sulle proprietà dell'agente di raccolta dati.|
|[logman start &#124; stop](logman-start-stop.md)|avviare o arrestare la raccolta dei dati.|
|[logman delete](logman-delete.md)|Elimina un agente di raccolta dati esistente.|
|[logman update](logman-update.md)|Aggiornare le proprietà di un agente di raccolta dati esistente.|
|[logman import &#124; export](logman-import-export.md)|importare un insieme agenti di raccolta dati da un file XML o esportare un insieme agenti di raccolta dati in un file XML.|
