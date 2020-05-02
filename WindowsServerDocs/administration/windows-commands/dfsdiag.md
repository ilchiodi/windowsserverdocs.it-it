---
title: Dfsdiag
description: Argomento di riferimento per Dfsdiag, che fornisce informazioni di diagnostica per gli spazi dei nomi DFS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c0891e67-0187-4f18-923d-5623e6127f90
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2d5a9b147994628ccad6a723311decbccbe82ec6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719547"
---
# <a name="dfsdiag"></a>Dfsdiag

Fornisce informazioni di diagnostica per gli spazi dei nomi DFS.

## <a name="syntax"></a>Sintassi

```
dfsdiag [ /TestDCs [/Domain:<Domain name>]| /TestSites </Machine:<server name>| /DFSPath:<namespace root or DFS folder> [/Recurse]> [/Full] | /TestDFSConfig /DFSRoot:<namespace> | /TestDFSIntegrity /DFSRoot:<DFS root path> [/Recurse] [/Full] | /TestReferral /DFSPath:<DFS path for getting referrals> [/Full] | /?] 

```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|[Dfsdiag TestDCs](dfsdiag-testdcs.md)|Controlli di configurazione del controller di dominio.|
|[Dfsdiag TestSites](dfsdiag-testsites.md)|Controlli del sito le associazioni.|
|[Dfsdiag TestDFSConfig](dfsdiag-testdfsconfig.md)|Controlla la configurazione dello spazio dei nomi DFS.|
|[Dfsdiag TestDFSIntegrity](dfsdiag-testdfsintegrity.md)|Controlla l'integrità dello spazio dei nomi DFS.|
|[Dfsdiag TestReferral](dfsdiag-testreferral.md)|Controlla le risposte di riferimento.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="additional-references"></a>Riferimenti aggiuntivi

-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)