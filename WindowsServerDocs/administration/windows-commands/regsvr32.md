---
title: regsvr32
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3345e964-7d3e-42b8-abeb-42ed6edfe2b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d3b775b0c49e4191a9fee6dc9e2e91f968142085
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836214"
---
# <a name="regsvr32"></a>regsvr32



Registra i file DLL come componenti di un comando nel Registro di sistema.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
regsvr32 [/u] [/s] [/n] [/i[:cmdline]] <DllName>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/u|Annulla la registrazione di server.|
|/s|Esecuzioni **Regsvr32** senza visualizzare i messaggi.|
|/n|Esecuzioni **Regsvr32** senza chiamare **DllRegisterServer**. (Richiede il **/i** parametro.)|
|/i:\<cmdline >|Passa una stringa della riga di comando facoltativa (*cmdline*) a **DllInstall**. Se si utilizza questo parametro in combinazione con il **/u** parametro, chiama **DllUninstall**.|
|\<DllName >|Il nome del file DLL che verrà registrato.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per registrare la DLL dello Schema di Active Directory, digitare:
```
regsvr32 schmmgmt.dll
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)