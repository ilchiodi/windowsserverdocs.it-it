---
title: rpcinfo
description: Informazioni su come elencare i programmi in un computer remoto.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7c342232-a8f0-42ff-8f11-d18c4981f5ca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 4aba1e57d5a61103310fbe7abcac391e543be5aa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826372"
---
# <a name="rpcinfo"></a>rpcinfo

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vengono elencati i programmi nei computer remoti. Il **rpcinfo** utilità della riga di comando esegue una procedura remota (RPC call) a un server RPC e segnala trovato. 

## <a name="syntax"></a>Sintassi
```
rpcinfo [/p [<Node>]] [/b <Program version>] [/t <Node Program> [<version>]] [/u <Node Program> [<version>]]
```

### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|/p [\<nodo >]|Elenca tutti i programmi registrati con il mapping di porta nell'host specificato. Se non si specifica un nome di nodo (computer), il programma esegue una query il mapping di porta nell'host locale.|
|/ b \<versione del programma >|Richiede una risposta da tutti i nodi di rete con il programma specificato e la versione registrata con il mapping di porta. È necessario specificare sia il nome del programma o numero sia un numero di versione.|
|/t \<nodo programma > [\<versione >]|Usa il protocollo di trasporto TCP per chiamare il programma specificato. È necessario specificare un nome nodo (computer) e il nome del programma. Se non si specifica una versione, il programma chiama tutte le versioni.|
|/u \<nodo programma > [\<versione >]|Usa il protocollo di trasporto UDP per chiamare il programma specificato. È necessario specificare un nome nodo (computer) e il nome del programma. Se non si specifica una versione, il programma chiama tutte le versioni.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="BKMK_Examples"></a>Esempi
Per elencare tutti i programmi registrati con il mapping di porta, digitare:
```
rpcinfo /p [<Node>]
```
Per richiedere una risposta dai nodi di rete che dispongono di un programma specifico, digitare:
```
rpcinfo /b <Program version>
```
Per utilizzare controllo TCP (Transmission Protocol) per chiamare un programma, digitare:
```
rpcinfo /t <Node Program> [<version>]
```
Per chiamare un programma, utilizzare protocollo UDP (User Datagram):
```
rpcinfo /u <Node Program> [<version>]
```

## <a name="additional-references"></a>Altri riferimenti
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)
