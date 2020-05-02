---
title: regsvr32
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3345e964-7d3e-42b8-abeb-42ed6edfe2b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: beadc9e9e614e2fe4cffad5dc263cfb1d4aecf67
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722482"
---
# <a name="regsvr32"></a>regsvr32



Registra i file DLL come componenti di un comando nel Registro di sistema.



## <a name="syntax"></a>Sintassi

```
regsvr32 [/u] [/s] [/n] [/i[:cmdline]] <DllName>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/U|Annulla la registrazione di server.|
|/s|Esecuzioni **Regsvr32** senza visualizzare i messaggi.|
|/n|Esecuzioni **Regsvr32** senza chiamare **DllRegisterServer**. (Richiede il **/i** parametro.)|
|/i:\<cmdline>|Passa una stringa della riga di comando facoltativa (*cmdline*) a **DllInstall**. Se si utilizza questo parametro in combinazione con il **/u** parametro, chiama **DllUninstall**.|
|\<> DllName|Il nome del file DLL che verrà registrato.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="examples"></a>Esempi

Per registrare la DLL dello Schema di Active Directory, digitare:
```
regsvr32 schmmgmt.dll
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)