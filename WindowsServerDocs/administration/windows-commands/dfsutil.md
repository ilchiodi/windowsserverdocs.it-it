---
title: dfsutil
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ef5093a4-0d24-4b21-9d04-59933ad98e2c
robots: noindex,nofollow
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 245f8fb2e6419d22da3e2e76eebd9f801ab90664
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821482"
---
# <a name="dfsutil"></a>dfsutil

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Il comando dfsutil gestisce spazi dei nomi DFS, server e client. i comandi Dfsutil utilizzano la terminologia di File System distribuito originale, con aggiornamento della terminologia spazi dei nomi DFS fornito come descrizione per la maggior parte dei comandi.

Per esempi di come è possibile utilizzare questo comando, vedere 

## <a name="syntax"></a>Sintassi

```
command </parameter> </param2>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------|--------|
|[Dfsutil radice](dfsutil-root.md)|Visualizza, crea, Elimina, Importa, Esporta radici dello spazio dei nomi.|
|[dfsutil Link](dfsutil-link.md)|Visualizza, crea, rimuove o si sposta le cartelle \(collegamenti\).|
|[Destinazione Dfsutil](dfsutil-target.md)|Consente di visualizzare, creare, rimuovere il server di destinazione o spazio dei nomi della cartella.|
|[Proprietà Dfsutil](dfsutil-property.md)|Visualizza o modifica di un server di destinazione o spazio dei nomi di cartella.|
|[Dfsutil Client](dfsutil-client.md)|Visualizza o modifica le chiavi del Registro di sistema o informazioni client.|
|[dfsutil Server](dfsutil-server.md)|Visualizza o modifica di configurazione dello spazio dei nomi.|
|[Dfsutil Diag](dfsutil-diag.md)|Eseguire la diagnostica o visualizzare dfsdirs\/dfspath.|
|[dfsutil Domain](dfsutil-domain.md)|Visualizza tutti dominio\-basato su spazi dei nomi in un dominio.|
|[dfsutil Cache](dfsutil-cache.md)|Visualizza o scarica la cache del client.|
|[oldcli Dfsutil](dfsutil-oldcli.md)|Utilizzare il dfsutil \/oldcli comando per usare la sintassi dfsutil originale.|

## <a name="remarks-optional-section"></a>Sezione Osservazioni <optional section>
Se si specifica un oggetto \(ad esempio un server dello spazio dei nomi\) alla fine di un comando, la maggior parte dei comandi visualizzerà le informazioni sull'oggetto senza richiedere ulteriori parametri o i comandi. Ad esempio, quando si usa il comando radice dfsutil, è possibile aggiungere una radice dello spazio dei nomi per il comando per visualizzare le informazioni sulla directory principale.

## <a name="BKMK_Examples"></a>Esempi
<Here is where you put a detailed description of your example.>

```
This /is /the /example /of /calling /command /with /parameters
```

<Here is where you put a detailed description of another example.>

```
This /is /a:different /example
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)


