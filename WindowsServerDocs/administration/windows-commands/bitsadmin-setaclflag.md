---
title: bitsadmin setaclflag
description: Argomento dei comandi di Windows per Bitsadmin setaclflag, che imposta i flag di propagazione dell'elenco di controllo di accesso.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6e3bcda0-827d-4dfd-8384-d1da018f3e10
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4ac47e554dde6a555e891d89668cd12fec3179d4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849674"
---
# <a name="bitsadmin-setaclflag"></a>bitsadmin setaclflag

Imposta i flag di propagazione dell'elenco di controllo di accesso (ACL) per il processo. I flag indicano che si desidera mantenere il proprietario e le informazioni ACL con il file da scaricare. Ad esempio, per mantenere il proprietario e il gruppo con il file, impostare **flags** su `OG`.

## <a name="syntax"></a>Sintassi

```
bitsadmin /SetAclFlags <Job> <Flags>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|
|Flag|Specificare uno o più dei valori di flag seguenti:</br>-O: Copiare informazioni sul proprietario con file.</br>-G: Copiare le informazioni di gruppo con file.</br>-D: Copiare informazioni DACL con file.</br>-S: copia SACL informazioni con file.|

## <a name="remarks"></a>Note

L'opzione SetAclFlags viene usata per mantenere le informazioni sull'elenco di controllo di accesso e proprietario quando un processo sta scaricando i dati da una condivisione Windows (SMB).

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente imposta il controllo di accesso flag di propagazione di elenco per il processo denominato *myDownloadJob* per mantenere le informazioni di gruppo e il proprietario con i file scaricati.
```
C:\>bitsadmin /setaclflags myDownloadJob OG
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)