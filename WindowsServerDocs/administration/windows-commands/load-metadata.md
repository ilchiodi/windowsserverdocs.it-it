---
title: carica metadati
description: Argomento di riferimento per il comando Load Metadata, che carica un file con estensione cab dei metadati prima di importare una copia shadow trasportabile o carica i metadati del writer in caso di ripristino.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2c535487-668b-44fc-babb-ff59cf7d190e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c7dc967476412261e7afc228088566f74ec4208c
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820191"
---
# <a name="load-metadata"></a>Caricare i metadati

Carica un file con estensione cab metadati prima di importare una copia shadow trasportabili o carica i metadati del writer in caso di ripristino. Se utilizzata senza parametri, **caricare i metadati** Visualizza la Guida al prompt dei comandi.

## <a name="syntax"></a>Sintassi

```
load metadata [<drive>:][<path>]<metadata.cab>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `[<drive>:][<path>]` | Specifica il percorso del file di metadati. |
| Metadata. cab | Specifica il file CAB di metadati da caricare. |

## <a name="remarks"></a>Osservazioni

- È possibile utilizzare il **importare** comando per importare una copia shadow trasportabili in base ai metadati specificati da **caricare i metadati**.

- È necessario eseguire questo comando prima del comando **Begin Restore** per caricare i writer e i componenti selezionati per il ripristino.

## <a name="examples"></a>Esempi

Per caricare un file di metadati denominato metafile.cab dal percorso predefinito, digitare:

```
load metadata metafile.cab
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Import DiskShadow](import.md)

- [comando BEGIN Restore](begin-restore.md)
