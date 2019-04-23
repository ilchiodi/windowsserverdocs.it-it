---
title: bitsadmin setaclflag
description: Argomento i comandi di Windows per **bitsadmin setaclflag** -imposta il controllo di accesso contrassegni propagazioni dell'elenco.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6e3bcda0-827d-4dfd-8384-d1da018f3e10
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 89d825a4bc4512022fed98a3188537d3977fa3c3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867402"
---
# <a name="bitsadmin-setaclflag"></a>bitsadmin setaclflag

Imposta il controllo di accesso contrassegni propagazioni dell'elenco (ACL) per il processo. I flag indicano che si desidera mantenere il proprietario e le informazioni di ACL con il file in corso il download. Ad esempio, per mantenere il proprietario e il gruppo con il file, impostare **Flags** a `OG`.

## <a name="syntax"></a>Sintassi

```
bitsadmin /SetAclFlags <Job> <Flags>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|
|Flag|Specificare uno o più dei valori di flag seguenti:</br>-   O: Copiare informazioni sul proprietario con file.</br>-   G: Copiare le informazioni sui gruppi con file.</br>-   D: Copiare informazioni DACL con file.</br>-S: copia SACL informazioni con file.|

## <a name="remarks"></a>Note

Il commutatore SetAclFlags viene utilizzato per mantenere informazioni dell'elenco di controllo proprietario e l'accesso quando un processo di download dei dati da una condivisione di Windows (SMB).

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente imposta il controllo di accesso flag di propagazione di elenco per il processo denominato *myDownloadJob* per mantenere le informazioni di gruppo e il proprietario con i file scaricati.
```
C:\>bitsadmin /setaclflags myDownloadJob OG
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)