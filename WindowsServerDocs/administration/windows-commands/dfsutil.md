---
title: Dfsutil
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 1a06806b109bbd324213f935892bbbab415362df
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377984"
---
# <a name="dfsutil"></a>Dfsutil

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Il comando dfsutil gestisce spazi dei nomi DFS, server e client. i comandi dfsutil utilizzano la terminologia file system distribuito originale, con la terminologia degli spazi dei nomi DFS aggiornata fornita come spiegazione per la maggior parte dei comandi.

per esempi di come è possibile usare questo comando, vedere 

## <a name="syntax"></a>Sintassi

```
command </parameter> </param2>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------|--------|
|[Radice Dfsutil](dfsutil-root.md)|Visualizza, crea, Elimina, Importa, Esporta radici dello spazio dei nomi.|
|[Collegamento Dfsutil](dfsutil-link.md)|Visualizza, crea, rimuove o si sposta le cartelle \(collegamenti\).|
|[Destinazione Dfsutil](dfsutil-target.md)|Consente di visualizzare, creare, rimuovere il server di destinazione o spazio dei nomi della cartella.|
|[Proprietà Dfsutil](dfsutil-property.md)|Visualizza o modifica di un server di destinazione o spazio dei nomi di cartella.|
|[Client Dfsutil](dfsutil-client.md)|Visualizza o modifica le chiavi del Registro di sistema o informazioni client.|
|[Server Dfsutil](dfsutil-server.md)|Visualizza o modifica di configurazione dello spazio dei nomi.|
|[Dfsutil diag](dfsutil-diag.md)|Eseguire la diagnostica o visualizzare dfsdirs\/dfspath.|
|[Dominio Dfsutil](dfsutil-domain.md)|Visualizza tutti dominio\-basato su spazi dei nomi in un dominio.|
|[Cache Dfsutil](dfsutil-cache.md)|Visualizza o scarica la cache del client.|
|[oldcli Dfsutil](dfsutil-oldcli.md)|Usare il comando Dfsutil \/oldcli per usare la sintassi Dfsutil originale.|

## <a name="remarks-optional-section"></a><optional section> osservazioni
Se si specifica un oggetto \(come un server dello spazio dei nomi\) alla fine di un comando, la maggior parte dei comandi visualizzerà le informazioni sull'oggetto senza richiedere ulteriori parametri o comandi. Ad esempio, quando si usa il comando radice Dfsutil, è possibile aggiungere una radice dello spazio dei nomi al comando per visualizzare le informazioni sulla radice.

## <a name="BKMK_Examples"></a>Esempi
&lt;qui è possibile inserire una descrizione dettagliata dell'esempio.&gt;

```
This /is /the /example /of /calling /command /with /parameters
```

&lt;qui è possibile inserire una descrizione dettagliata di un altro esempio.&gt;

```
This /is /a:different /example
```

## <a name="additional-references"></a>riferimenti aggiuntivi

-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)


