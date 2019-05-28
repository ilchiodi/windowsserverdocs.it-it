---
title: tpmtool
description: Argomento i comandi di Windows per tpmtool - Ottiene informazioni su Trusted Platform Module.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
author: ashleytqy
ms.author: ashleytqy
manager: ronaldai
ms.date: 05/07/2019
ms.openlocfilehash: d2939b693c3f9bd8d0d8c8e1fefdd5d8f892e4c9
ms.sourcegitcommit: 3582c38d87f23cc54467d63bf00c29ef07cdb7c8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/10/2019
ms.locfileid: "65517194"
---
>[!IMPORTANT]
>Alcune informazioni sono relative a un prodotto non definitivo che potrebbe subire modifiche sostanziali prima del rilascio sul mercato. Microsoft non riconosce alcuna garanzia, espressa o implicita, in merito alle informazioni qui fornite.

# <a name="tpmtool"></a>tpmtool

Questa utilità può essere utilizzata per ottenere le informazioni di [modulo TPM (Trusted Platform)](https://docs.microsoft.com/windows/security/information-protection/tpm/trusted-platform-module-overview).

Per esempi di utilizzo di questo comando, vedere [Esempi](#tpmtool_examples).

## <a name="syntax"></a>Sintassi

```
tpmtool /parameter [<arguments>]
```
## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|getdeviceinformation|Visualizza le informazioni di base del TPM.|
|stesse [percorso di directory di output]|Raccoglie i log di TPM e li inserisce nella directory specificata. Per impostazione predefinita, vengono inseriti nella directory corrente.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="tpmtool_examples"></a>Esempi

Per visualizzare le informazioni di base del TPM, digitare:
```
tpmtool getdeviceinformation
```
Per raccoglie i log di TPM e inserirli nella directory corrente, digitare:
```
tpmtool gatherlogs
```
Per raccoglie i log di TPM e inserirli in `C:\Users\Public`, tipo:
```
tpmtool gatherlogs C:\Users\Public
```
