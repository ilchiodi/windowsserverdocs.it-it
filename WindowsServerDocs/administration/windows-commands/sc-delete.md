---
title: Eliminazione di SC. exe
description: Informazioni su come annullare la registrazione dei servizi tramite l'utilità SC. exe
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2fe94fb3-e4d1-47b5-b999-39995ecbb644
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 284012cf6799df52832e62c3eea1b2f0fcd84805
ms.sourcegitcommit: 95b60384b0b070263465eaffb27b8e3bb052a4de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/06/2020
ms.locfileid: "82850112"
---
# <a name="scexe-delete"></a>Eliminazione di SC. exe

Elimina una sottochiave servizio dal Registro di sistema. Se il servizio è in esecuzione o se un altro processo ha un handle aperto al servizio, il servizio è contrassegnato per l'eliminazione.

Per esempi di utilizzo di questo comando, vedere [Esempi](#examples).

## <a name="syntax"></a>Sintassi

```
sc.exe [<ServerName>] delete [<ServiceName>]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<Nomeserver>|Specifica il nome del server remoto in cui si trova il servizio. Il nome deve usare il formato UNC (Universal Naming Convention), \\ \\ad esempio myserver. Per eseguire SC. exe localmente, omettere questo parametro.|
|\<ServiceName>|Specifica il nome del servizio restituito dal **getkeyname** operazione.|
|?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni

Non è consigliabile usare SC. exe per eliminare i servizi del sistema operativo incorporati, ad esempio DHCP, DNS o Internet Information Services. Per installare, rimuovere o riconfigurare i ruoli, i servizi e i componenti del sistema operativo, vedere [installare o disinstallare ruoli, servizi ruolo o funzionalità](/WindowsServerDocs/administration/server-manager/install-or-uninstall-roles-role-services-or-features.md)

## <a name="examples"></a>Esempi

Per eliminare la sottochiave servizio **nuovosrv** dal Registro di sistema del computer locale, digitare:
```
sc.exe delete newserv
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
