---
title: Ridimensionamento vssadmin shadowstorage
description: Descrizione del comando vssadmin resize shadowstorage.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 03/05/2020
ms.localizationpriority: medium
ms.openlocfilehash: 05e58498a59a9e87fc773428d7e5956ce3285b67
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720236"
---
# <a name="vssadmin-resize-shadowstorage"></a>Ridimensionamento vssadmin shadowstorage

> Si applica a: Windows 10, Windows 8.1, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Ridimensiona la quantità massima di spazio di archiviazione che può essere utilizzata per l'archiviazione delle copie shadow.

La quantità minima di spazio di archiviazione che può essere usata per l'archiviazione delle copie shadow può essere specificata usando il valore del registro di sistema **MinDiffAreaFileSize** . Per ulteriori informazioni, vedere [MinDiffAreaFileSize](https://docs.microsoft.com/windows/win32/backup/registry-keys-for-backup-and-restore#mindiffareafilesize).

> [!WARNING]
> Il ridimensionamento dell'associazione di archiviazione può causare la scomparsa delle copie shadow.

## <a name="syntax"></a>Sintassi

```cmd
vssadmin resize shadowstorage /for=<ForVolumeSpec> /on=<OnVolumeSpec> [/maxsize=<MaxSizeSpec>]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---|---|
`/for=<ForVolumeSpec>`  | Specifica il volume per il quale deve essere ridimensionata la quantità massima di spazio di archiviazione.
`/on=<OnVolumeSpec>` | Specifica il volume di archiviazione.
`[/maxsize=<MaxSizeSpec>]` |  Specifica la quantità massima di spazio che può essere utilizzata per l'archiviazione delle copie shadow. Se non viene specificato alcun valore per/MaxSize, non è previsto alcun limite per la quantità di spazio di archiviazione che è possibile usare.  <br> <br> Il valore di MaxDimensione deve essere maggiore o uguale a 1 MB e deve essere espresso in una delle unità seguenti: KB, MB, GB, TB, PB o EB. Se non viene specificata alcuna unità, MaxDimensione usa i byte per impostazione predefinita.

## <a name="examples"></a>Esempi

```cmd
vssadmin Resize ShadowStorage /For=C: /On=D: /MaxSize=900MB
vssadmin Resize ShadowStorage /For=C: /On=D: /MaxSize=UNBOUNDED
vssadmin Resize ShadowStorage /For=C: /On=C: /MaxSize=20%
```

## <a name="additional-references"></a>Altri riferimenti

* [Chiave sintassi della riga di comando](https://docs.microsoft.com/windows-server/administration/windows-commands/command-line-syntax-key)
* [Vssadmin](vssadmin.md)
