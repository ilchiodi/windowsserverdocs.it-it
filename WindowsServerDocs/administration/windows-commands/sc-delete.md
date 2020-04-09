---
title: Eliminazione di SC
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2fe94fb3-e4d1-47b5-b999-39995ecbb644
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 05b276de04d4250cc03e4b2976bf8c1330ef82ce
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80835384"
---
# <a name="sc-delete"></a>Eliminazione di SC



Elimina una sottochiave servizio dal Registro di sistema. Se il servizio è in esecuzione o se un altro processo ha un handle aperto al servizio, il servizio è contrassegnato per l'eliminazione.

Per esempi di utilizzo di questo comando, vedere [Esempi](#examples).

## <a name="syntax"></a>Sintassi

```
sc [<ServerName>] delete [<ServiceName>]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<ServerName >|Specifica il nome del server remoto in cui si trova il servizio. Il nome deve usare il formato Universal Naming Convention (UNC), ad esempio \\\\MyServer. Per eseguire SC. exe localmente, omettere questo parametro.|
|\<ServiceName >|Specifica il nome del servizio restituito dal **getkeyname** operazione.|
|?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

Utilizzare **Aggiungi / Rimuovi programmi** su **Pannello di controllo** eliminare DHCP, DNS o altri servizi del sistema operativo predefinito. Si noti che **Aggiungi / Rimuovi programmi** non rimuoverà solo la sottochiave del Registro di sistema per il servizio, ma verrà anche disinstallare il servizio ed eliminare tutti i collegamenti a esso.

## <a name="examples"></a>Esempi

Per eliminare la sottochiave servizio **nuovosrv** dal Registro di sistema del computer locale, digitare:
```
sc delete newserv
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)