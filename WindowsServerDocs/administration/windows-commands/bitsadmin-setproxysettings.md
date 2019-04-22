---
title: bitsadmin setproxysettings
description: Argomento i comandi di Windows per **bitsadmin setproxysettings** -imposta le impostazioni proxy per il processo specificato.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: be8edb1b-614e-4d0b-a8f8-64b4bde3e64b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a3503aab55f5650cb9283ce8a9f1a17359bfd48b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825592"
---
# <a name="bitsadmin-setproxysettings"></a>bitsadmin setproxysettings



Imposta le impostazioni proxy per il processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /SetProxySettings <Job> <Usage> [List] [Bypass]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|
|Uso|Uno dei valori seguenti:</br>-PRECONFIGURAZIONE, usare i valori predefiniti di Internet Explorer del proprietario.</br>-NO_PROXY: non usare un server proxy.</br>-SOSTITUIRE, usare un elenco di proxy esplicito e l'elenco di esclusione. È necessario seguire un proxy e l'elenco proxy da ignorare.</br>-Rilevamento automatico, ovvero rileva automaticamente impostazioni proxy.|
|List|Utilizzato quando la *utilizzo* parametro è impostato su Sostituisci, ovvero contiene un elenco delimitato da virgole dei server proxy da utilizzare.|
|Bypass|Utilizzato quando la *utilizzo* parametro è impostato su Sostituisci, ovvero contiene un elenco delimitato da spazi di nomi host o gli indirizzi IP o entrambi, per cui trasferimenti non sono devono essere instradati attraverso un proxy. Può trattarsi  **\<locale >** per fare riferimento a tutti i server alla stessa rete LAN. I valori null o "" può essere utilizzato per un elenco di esclusione proxy vuoto.|

## <a name="BKMK_examples"></a>Esempi

L'esempio seguente imposta le impostazioni proxy per il processo denominato *myDownloadJob*.

```
C:\>bitsadmin /SetProxySettings myDownloadJob PRECONFIG
```

Di seguito sono riportati alcuni altri esempi.

```
bitsadmin /setproxysettings myDownloadJob NO_PROXY
bitsadmin /setproxysettings myDownloadJob OVERRIDE proxy1:80 ""
bitsadmin /setproxysettings myDownloadJob OVERRIDE proxy1,proxy2,proxy3 NULL
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)