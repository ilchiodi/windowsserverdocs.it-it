---
title: regsvr32
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3345e964-7d3e-42b8-abeb-42ed6edfe2b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 444af0ccf7c9bbe21c013f32b396997b7cb2e00f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371627"
---
# <a name="regsvr32"></a>regsvr32



Registra i file DLL come componenti di un comando nel Registro di sistema.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
regsvr32 [/u] [/s] [/n] [/i[:cmdline]] <DllName>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/u|Annulla la registrazione di server.|
|/s|Esecuzioni **Regsvr32** senza visualizzare i messaggi.|
|/n|Esecuzioni **Regsvr32** senza chiamare **DllRegisterServer**. (Richiede il **/i** parametro.)|
|/i: \<cmdline >|Passa una stringa della riga di comando facoltativa (*cmdline*) a **DllInstall**. Se si utilizza questo parametro in combinazione con il **/u** parametro, chiama **DllUninstall**.|
|\<DllName >|Il nome del file DLL che verrà registrato.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="BKMK_examples"></a>Esempi

Per registrare la DLL dello Schema di Active Directory, digitare:
```
regsvr32 schmmgmt.dll
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)