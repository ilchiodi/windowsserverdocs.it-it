---
title: bitsadmin setaclflag
description: Argomento dei comandi di Windows per **BITSAdmin setaclflag** -imposta i flag di propagazione dell'elenco di controllo di accesso.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: fbdb12c29af7b4db8b25846d43ee1c93b2454ff2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380762"
---
# <a name="bitsadmin-setaclflag"></a>bitsadmin setaclflag

Imposta i flag di propagazione dell'elenco di controllo di accesso (ACL) per il processo. I flag indicano che si desidera mantenere il proprietario e le informazioni ACL con il file da scaricare. Ad esempio, per mantenere il proprietario e il gruppo con il file, impostare **flags** per `OG`.

## <a name="syntax"></a>Sintassi

```
bitsadmin /SetAclFlags <Job> <Flags>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|
|Flag|Specificare uno o più dei valori di flag seguenti:</br>O Copiare le informazioni sul proprietario con il file.</br>G Copia le informazioni sul gruppo con il file.</br>D Copiare le informazioni DACL con file.</br>-S: copia SACL informazioni con file.|

## <a name="remarks"></a>Note

L'opzione SetAclFlags viene usata per mantenere le informazioni sull'elenco di controllo di accesso e proprietario quando un processo sta scaricando i dati da una condivisione Windows (SMB).

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente imposta il controllo di accesso flag di propagazione di elenco per il processo denominato *myDownloadJob* per mantenere le informazioni di gruppo e il proprietario con i file scaricati.
```
C:\>bitsadmin /setaclflags myDownloadJob OG
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)