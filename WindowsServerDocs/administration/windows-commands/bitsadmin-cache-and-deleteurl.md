---
title: Bitsadmin cache e DeleteUrl
description: Argomento dei comandi di Windows per la **cache Bitsadmin e DeleteUrl**, che elimina tutte le voci della cache per l'URL specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e108b76b-fae9-4c16-bf4c-d74c9f025953
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 70099e795d0f05d0fcf75fbf6b82f5466d1c0c55
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850934"
---
# <a name="bitsadmin-cache-and-deleteurl"></a>Bitsadmin cache e DeleteUrl

Elimina tutte le voci della cache per l'URL specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /deleteURL url
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| url | Uniform Resource Locator che identifica un file remoto. |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente vengono eliminate tutte le voci della cache per `https://www.contoso.com/en/us/default.aspx`

```
C:\>bitsadmin /deleteURL https://www.contoso.com/en/us/default.aspx 
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)