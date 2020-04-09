---
title: Dfsdiag
description: Windows Commands argomento per Dfsdiag, che fornisce informazioni di diagnostica per gli spazi dei nomi DFS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c0891e67-0187-4f18-923d-5623e6127f90
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2c895dabbbafbe8ea253920d3bc6de17f42918e6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846194"
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
|[TestDCs Dfsdiag](dfsdiag-testdcs.md)|Controlli di configurazione del controller di dominio.|
|[TestSites Dfsdiag](dfsdiag-testsites.md)|Controlli del sito le associazioni.|
|[TestDFSConfig Dfsdiag](dfsdiag-testdfsconfig.md)|Controlla la configurazione dello spazio dei nomi DFS.|
|[TestDFSIntegrity Dfsdiag](dfsdiag-testdfsintegrity.md)|Controlla l'integrità dello spazio dei nomi DFS.|
|[TestReferral Dfsdiag](dfsdiag-testreferral.md)|Controlla le risposte di riferimento.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="additional-references"></a>Altre informazioni di riferimento

-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)