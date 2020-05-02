---
title: bitsadmin setaclflag
description: Argomento di riferimento per il comando Bitsadmin setaclflag, che imposta i flag di propagazione dell'elenco di controllo di accesso (ACL).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6e3bcda0-827d-4dfd-8384-d1da018f3e10
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1852bd267fe22825d55f7522a81179e9290e2a00
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716982"
---
# <a name="bitsadmin-setaclflag"></a>bitsadmin setaclflag

Imposta i flag di propagazione dell'elenco di controllo di accesso (ACL) per il processo. I flag indicano che si desidera mantenere il proprietario e le informazioni ACL con il file da scaricare. Ad esempio, per mantenere il proprietario e il gruppo con il file, impostare il parametro **Flags** su `og`.

## <a name="syntax"></a>Sintassi

```
bitsadmin /setaclflag <job> <flags>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| processo | Nome visualizzato o GUID del processo. |
| flags | Specificare uno o più valori, tra cui:<ul><li>**o** -copia le informazioni sul proprietario con il file.</li><li>**g** -copia le informazioni sul gruppo con il file.</li><li>**d** : copia le informazioni dell'elenco di controllo di accesso discrezionale (DACL) con file.</li><li>**s** : copia le informazioni dell'elenco di controllo di accesso di sistema (SACL) con file.</li></ul> |

## <a name="examples"></a>Esempi

Per impostare i flag di propagazione dell'elenco di controllo di accesso per il processo denominato *myDownloadJob*, in modo che mantenga le informazioni sul proprietario e sul gruppo con i file scaricati.

```
bitsadmin /setaclflags myDownloadJob og
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
