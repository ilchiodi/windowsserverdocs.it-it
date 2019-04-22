---
title: dfsdiag
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c0891e67-0187-4f18-923d-5623e6127f90
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ab5c86ce7ed4760aef4941de55e8dcf8efe48c8f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819142"
---
# <a name="dfsdiag"></a>dfsdiag



Il `Dfsdiag` comando fornisce informazioni di diagnostica per spazi dei nomi DFS.

## <a name="syntax"></a>Sintassi

```
dfsdiag [ /TestDCs [/Domain:<Domain name>]| /TestSites </Machine:<server name>| /DFSPath:<namespace root or DFS folder> [/Recurse]> [/Full] | /TestDFSConfig /DFSRoot:<namespace> | /TestDFSIntegrity /DFSRoot:<DFS root path> [/Recurse] [/Full] | /TestReferral /DFSPath:<DFS path for getting referrals> [/Full] | /?] 

```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|[Dfsdiag TestDCs](dfsdiag-testdcs.md)|Controlli di configurazione del controller di dominio.|
|[Dfsdiag TestSites](dfsdiag-testsites.md)|Controlli del sito le associazioni.|
|[dfsdiag TestDFSConfig](dfsdiag-testdfsconfig.md)|Controlla la configurazione dello spazio dei nomi DFS.|
|[Dfsdiag TestDFSIntegrity](dfsdiag-testdfsintegrity.md)|Controlla l'integrità dello spazio dei nomi DFS.|
|[Dfsdiag TestReferral](dfsdiag-testreferral.md)|Controlla le risposte di riferimento.|
|/?|Visualizza la guida al prompt dei comandi.|

#### <a name="additional-references"></a>Altri riferimenti

-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)