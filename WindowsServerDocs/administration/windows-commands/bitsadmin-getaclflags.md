---
title: bitsadmin getaclflags
description: Argomento dei comandi di Windows per **BITSAdmin GETACLFLAGS**, che recupera i flag di propagazione dell'elenco di controllo di accesso (ACL).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 99266def-7479-4430-a61c-98ec433fa88b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d53018e2fa5c659c8cf4b0ec985beda848a8c1af
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850794"
---
# <a name="bitsadmin-getaclflags"></a>bitsadmin getaclflags

Recupera i flag di propagazione dell'elenco di controllo di accesso (ACL), che indicano se gli elementi vengono ereditati dagli oggetti figlio.

## <a name="syntax"></a>Sintassi

```
bitsadmin /getaclflags <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| lavoro | Nome visualizzato o GUID del processo. |

## <a name="remarks"></a>Note

Visualizza uno o più dei seguenti valori di flag:

- **o** -copia le informazioni sul proprietario con il file.

- **g** -copia le informazioni sul gruppo con il file.

- **d** : copia le informazioni dell'elenco di controllo di accesso discrezionale (DACL) con file.

- **s** : copia le informazioni dell'elenco di controllo di accesso di sistema (SACL) con file.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente vengono recuperati i flag di propagazione dell'elenco di controllo di accesso per il processo denominato *myDownloadJob*.

```
C:\>bitsadmin /getaclflags myDownloadJob
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)