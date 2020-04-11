---
title: bitsadmin wrap
description: Argomento comandi di Windows per **BITSAdmin wrap**, che esegue il wrapping di qualsiasi riga di testo di output che si estende oltre il bordo all'estrema destra della finestra di comando alla riga successiva.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 14e57522-539d-4621-ad15-09f7a44ccab7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e754a765d94661baf24190431b455584d29991ec
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122569"
---
# <a name="bitsadmin-wrap"></a>bitsadmin wrap

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esegue il wrapping di tutte le righe di output testo che si estende oltre il bordo all'estrema destra della finestra di comando nella riga successiva. È necessario specificare questa opzione prima di tutte le altre opzioni.

Per impostazione predefinita, tutte le opzioni, ad eccezione dell'opzione di [monitoraggio Bitsadmin](bitsadmin-monitor.md) , incapsulano il testo di output.

## <a name="syntax"></a>Sintassi

```
bitsadmin /wrap <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ---------- |
| Job | Nome visualizzato o GUID del processo. |

## <a name="examples"></a>Esempi

Nell'esempio seguente recupera le informazioni per il processo denominato *myDownloadJob* e include l'output.

```
C:\>bitsadmin /wrap /info myDownloadJob /verbose
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
