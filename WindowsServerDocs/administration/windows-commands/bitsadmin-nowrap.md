---
title: bitsadmin nowrap
description: Argomento di riferimento per il comando Bitsadmin nowrap, che tronca qualsiasi riga di testo di output che si estende oltre il bordo all'estrema destra della finestra di comando.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 85a47b90-783a-41e4-96f2-81f26ae8ca93
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2aac604ec3e13026e322d7cb7a9364df46266a0c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717340"
---
# <a name="bitsadmin-nowrap"></a>bitsadmin nowrap

Tronca qualsiasi riga di testo di output che si estende oltre il bordo destro della finestra di comando. Per impostazione predefinita, tutte le opzioni, ad eccezione dell'opzione di **monitoraggio** , incapsulano l'output. Specificare l'opzione **nowrap** prima di altre opzioni.

## <a name="syntax"></a>Sintassi

```
bitsadmin /nowrap
```

## <a name="examples"></a>Esempi

Per recuperare lo stato per il processo denominato *myDownloadJob* senza eseguire il wrapping dell'output:

```
bitsadmin /nowrap /getstate myDownloadJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
