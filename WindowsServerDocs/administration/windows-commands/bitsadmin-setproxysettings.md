---
title: bitsadmin setproxysettings
description: Argomento dei comandi di Windows per **BITSAdmin setproxysettings** -imposta le impostazioni proxy per il processo specificato.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 38987c88bcfc93ea9251583b7914f982cf79e057
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380475"
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
|Utilizzo|Uno dei valori seguenti:</br>-Preconfig — usa le impostazioni predefinite di Internet Explorer del proprietario.</br>-NO_PROXY-non usare un server proxy.</br>-OVERRIDE: usare un elenco di proxy esplicito e un elenco di bypass. È necessario seguire un elenco proxy e bypass proxy.</br>-RILEVAMENTO automatico — rileva automaticamente le impostazioni del proxy.|
|List|Usato quando il parametro *Usage* è impostato su override — contiene un elenco delimitato da virgole di server proxy da usare.|
|Bypass|Usato quando il parametro *Usage* è impostato su override, contiene un elenco delimitato da spazi di nomi host o indirizzi IP, o entrambi, per cui i trasferimenti non devono essere indirizzati tramite un proxy. Questo può essere **\<local >** per fare riferimento a tutti i server nella stessa LAN. I valori null o "" può essere utilizzato per un elenco di esclusione proxy vuoto.|

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente vengono impostate le impostazioni proxy per il processo denominato *myDownloadJob*.

```
C:\>bitsadmin /SetProxySettings myDownloadJob PRECONFIG
```

Ecco alcuni altri esempi.

```
bitsadmin /setproxysettings myDownloadJob NO_PROXY
bitsadmin /setproxysettings myDownloadJob OVERRIDE proxy1:80 ""
bitsadmin /setproxysettings myDownloadJob OVERRIDE proxy1,proxy2,proxy3 NULL
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)