---
title: setsecurityflags Bitsadmin
description: Argomento dei comandi di Windows per Bitsadmin setsecurityflags, che imposta i flag per HTTP che determinano se i bit devono controllare l'elenco di revoche di certificati, ignorare determinati errori del certificato e definire i criteri da usare quando un server reindirizza la richiesta HTTP.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0da5cbf5-5f7f-4833-bbbe-c4e8379a78ab
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8a7b857bb398e3061a3435a730bf9a751ee2c5e3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849144"
---
# <a name="bitsadmin-setsecurityflags"></a>setsecurityflags Bitsadmin

Imposta i flag per HTTP che determinano se i bit devono controllare l'elenco di revoche di certificati, ignorare determinati errori del certificato e definire i criteri da usare quando un server reindirizza la richiesta HTTP. Il valore è un Unsigned Integer.

## <a name="syntax"></a>Sintassi

```
bitsadmin /SetSecurityFlags <Job> <Value>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|
|Valore|Vedere la sezione Osservazioni|

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

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente vengono impostati i flag di sicurezza per abilitare un controllo CRL per il processo denominato *il mio lavoro*.
```
C:\>bitsadmin /SetSecurityFlags myJob 0x0001
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)