---
title: Eliminazione di SC
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2fe94fb3-e4d1-47b5-b999-39995ecbb644
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ad64d0f7c772b8d29a191b5f3e690d74c8765717
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371284"
---
# <a name="sc-delete"></a>Eliminazione di SC



Elimina una sottochiave servizio dal Registro di sistema. Se il servizio è in esecuzione o se un altro processo ha un handle aperto al servizio, il servizio è contrassegnato per l'eliminazione.

Per esempi di utilizzo di questo comando, vedere [Esempi](#examples).

## <a name="syntax"></a>Sintassi

```
sc [<ServerName>] delete [<ServiceName>]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<ServerName >|Specifica il nome del server remoto in cui si trova il servizio. Il nome deve usare il formato Universal Naming Convention (UNC), ad esempio \\\\MyServer. Per eseguire SC. exe localmente, omettere questo parametro.|
|\<ServiceName >|Specifica il nome del servizio restituito dal **getkeyname** operazione.|
|?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni

Utilizzare **Aggiungi / Rimuovi programmi** su **Pannello di controllo** eliminare DHCP, DNS o altri servizi del sistema operativo predefinito. Si noti che **Aggiungi / Rimuovi programmi** non rimuoverà solo la sottochiave del Registro di sistema per il servizio, ma verrà anche disinstallare il servizio ed eliminare tutti i collegamenti a esso.

## <a name="examples"></a>Esempi

Per eliminare la sottochiave servizio **nuovosrv** dal Registro di sistema del computer locale, digitare:
```
sc delete newserv
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)