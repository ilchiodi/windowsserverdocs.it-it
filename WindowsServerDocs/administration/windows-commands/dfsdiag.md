---
title: Dfsdiag
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 61a6ab9a90e4d0220cfe27d2d21120be19b9ff1f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378315"
---
# <a name="dfsdiag"></a>Dfsdiag



Il `Dfsdiag` comando fornisce informazioni di diagnostica per spazi dei nomi DFS.

## <a name="syntax"></a>Sintassi

```
dfsdiag [ /TestDCs [/Domain:<Domain name>]| /TestSites </Machine:<server name>| /DFSPath:<namespace root or DFS folder> [/Recurse]> [/Full] | /TestDFSConfig /DFSRoot:<namespace> | /TestDFSIntegrity /DFSRoot:<DFS root path> [/Recurse] [/Full] | /TestReferral /DFSPath:<DFS path for getting referrals> [/Full] | /?] 

```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|[TestDCs Dfsdiag](dfsdiag-testdcs.md)|Controlli di configurazione del controller di dominio.|
|[TestSites Dfsdiag](dfsdiag-testsites.md)|Controlli del sito le associazioni.|
|[TestDFSConfig Dfsdiag](dfsdiag-testdfsconfig.md)|Controlla la configurazione dello spazio dei nomi DFS.|
|[TestDFSIntegrity Dfsdiag](dfsdiag-testdfsintegrity.md)|Controlla l'integrità dello spazio dei nomi DFS.|
|[TestReferral Dfsdiag](dfsdiag-testreferral.md)|Controlla le risposte di riferimento.|
|/?|Visualizza la guida al prompt dei comandi.|

#### <a name="additional-references"></a>Altri riferimenti

-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)