---
title: setsecurityflags Bitsadmin
description: Argomento i comandi di Windows per **bitsadmin setsecurityflags** -set di flag per il protocollo HTTP che determinano se BITS deve controllare l'elenco di revoche di certificati, ignorare determinati errori di certificato e definire i criteri da utilizzare quando un server Reindirizza la richiesta HTTP.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 2a7f74146a26314ddb4fa92f85e5a40267d0f0d9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858622"
---
# <a name="bitsadmin-setsecurityflags"></a>setsecurityflags Bitsadmin



Imposta i flag per il protocollo HTTP che determinano se BITS deve controllare l'elenco di revoche di certificati, ignorare determinati errori di certificato e definire i criteri da utilizzare quando un server reindirizza la richiesta HTTP. Il valore è un intero senza segno.

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

Il **valore** parametro può contenere uno o più dei seguenti flag di notifica.

|Azione|Rappresentazione binaria|
|------|---------------------|
|Abilita controllo di CRL|Impostare il bit meno significativo|
|Ignorare il nome comune non valido nel certificato server|Impostare il bit 2 da destra|
|Ignora la data non valida nel certificato server|Impostare il bit 3 ° da destra|
|Ignora le autorità di certificazione non valido nel certificato server|Impostare il bit 4th da destra|
|Ignora un utilizzo non valido del certificato|Impostare il 5 bit da destra|
|Criteri di reindirizzamento|Controllato da 9 a 11 bit da destra</br>0, 0,0 - reindirizzamenti saranno consentiti automaticamente.</br>0, 0,1 - nome remoto nell'interfaccia IBackgroundCopyFile verrà aggiornate se si verifica un reindirizzamento.</br>0, 1,0 - bit avrà esito negativo del processo se si verifica un reindirizzamento.|
|Consenti il reindirizzamento da HTTPS a HTTP|Impostare il 12 bit da destra|

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente imposta i flag di sicurezza per attivare la verifica CRL per il processo denominato *myJob*.
```
C:\>bitsadmin /SetSecurityFlags myJob 0x0001
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)