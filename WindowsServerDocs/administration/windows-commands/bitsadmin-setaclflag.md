---
title: bitsadmin setaclflag
description: Argomento dei comandi di Windows per **BITSAdmin setaclflag**, che imposta i flag di propagazione dell'elenco di controllo di accesso (ACL).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6e3bcda0-827d-4dfd-8384-d1da018f3e10
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f0aae550e94d04db518edccafb1d6bcf46d0320b
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123069"
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
| lavoro | Nome visualizzato o GUID del processo. |
| flags | Specificare uno o più valori, tra cui:<ul><li>**o** -copia le informazioni sul proprietario con il file.</li><li>**g** -copia le informazioni sul gruppo con il file.</li><li>**d** : copia le informazioni dell'elenco di controllo di accesso discrezionale (DACL) con file.</li><li>**s** : copia le informazioni dell'elenco di controllo di accesso di sistema (SACL) con file.</li></ul> |

## <a name="remarks"></a>Note

L'opzione/setaclflag viene usata per mantenere le informazioni sull'elenco di controllo di accesso e proprietario quando un processo sta scaricando i dati da una condivisione Windows (SMB).

## <a name="examples"></a>Esempi

Nell'esempio seguente imposta il controllo di accesso flag di propagazione di elenco per il processo denominato *myDownloadJob* per mantenere le informazioni di gruppo e il proprietario con i file scaricati.

```
C:\>bitsadmin /setaclflags myDownloadJob og
```

## <a name="additional-references"></a>Altre informazioni di riferimento

[Chiave della sintassi della riga di comando](command-line-syntax-key.md)&reg;'    