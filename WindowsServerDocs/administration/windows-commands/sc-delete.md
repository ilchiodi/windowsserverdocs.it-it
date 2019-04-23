---
title: Eliminazione di SC
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: b60127cf957a30d147c9992c74c01e37e5b8bf89
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871922"
---
# <a name="sc-delete"></a>Eliminazione di SC



Elimina una sottochiave servizio dal Registro di sistema. Se il servizio è in esecuzione o se un altro processo ha un handle aperto al servizio, il servizio è contrassegnato per l'eliminazione.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
sc [<ServerName>] delete [<ServiceName>]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<ServerName>|Specifica il nome del server remoto in cui si trova il servizio. Il nome deve rispettare il formato di Universal Naming Convention (UNC) (ad esempio, \\ \\myserver). Per eseguire SC.exe in locale, omettere questo parametro.|
|\<ServiceName>|Specifica il nome del servizio restituito dal **getkeyname** operazione.|
|?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

Utilizzare **Aggiungi / Rimuovi programmi** su **Pannello di controllo** eliminare DHCP, DNS o altri servizi del sistema operativo predefinito. Si noti che **Aggiungi / Rimuovi programmi** non rimuoverà solo la sottochiave del Registro di sistema per il servizio, ma verrà anche disinstallare il servizio ed eliminare tutti i collegamenti a esso.

## <a name="BKMK_examples"></a>Esempi

Per eliminare la sottochiave servizio **nuovosrv** dal Registro di sistema del computer locale, digitare:
```
sc delete newserv
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)