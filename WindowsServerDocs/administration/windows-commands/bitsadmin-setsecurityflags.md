---
title: setsecurityflags Bitsadmin
description: Argomento dei comandi di Windows per **BITSAdmin setsecurityflags** -imposta i flag per http che determinano se i bit devono controllare l'elenco di revoche di certificati, ignorare determinati errori del certificato e definire i criteri da usare quando un server reindirizza la richiesta HTTP.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0da5cbf5-5f7f-4833-bbbe-c4e8379a78ab
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: acc5a64ef7c82b14e6815b6d51dda5ea4700dcad
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380413"
---
# <a name="bitsadmin-setsecurityflags"></a>setsecurityflags Bitsadmin



Imposta i flag per HTTP che determinano se i bit devono controllare l'elenco di revoche di certificati, ignorare determinati errori del certificato e definire i criteri da usare quando un server reindirizza la richiesta HTTP. Il valore è un Unsigned Integer.

## <a name="syntax"></a>Sintassi

```
bitsadmin /SetSecurityFlags <Job> <Value>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|
|Value|Vedere la sezione Osservazioni|

## <a name="remarks"></a>Note

Il parametro **value** può contenere uno o più dei flag di notifica seguenti.

|Azione|Rappresentazione binaria|
|------|---------------------|
|Abilita controllo CRL|Imposta il bit meno significativo|
|Ignora nome comune non valido nel certificato del server|Imposta il secondo bit da destra|
|Ignora data non valida nel certificato del server|Imposta il terzo bit da destra|
|Ignora autorità di certificazione non valida nel certificato del server|Imposta il quarto bit da destra|
|Ignora utilizzo non valido del certificato|Imposta il quinto bit da destra|
|Criteri di Reindirizzamento|Controllato da 9 a 11 bit a destra</br>0, 0, 0: i reindirizzamenti verranno consentiti automaticamente.</br>0, 0, 1: il nome remoto nell'interfaccia IBackgroundCopyFile verrà aggiornato se si verifica un reindirizzamento.</br>0, 1, 0-BITS non riuscirà a eseguire il processo se si verifica un reindirizzamento.|
|Consenti Reindirizzamento da HTTPS a HTTP|Imposta il dodicesimo bit da destra|

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente vengono impostati i flag di sicurezza per abilitare un controllo CRL per il processo denominato *il mio lavoro*.
```
C:\>bitsadmin /SetSecurityFlags myJob 0x0001
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)