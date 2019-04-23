---
title: nslookup set retry
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 615fdfa2-fa29-47a8-8c9e-a6c5b45b3b71
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 63d72a45c33da099c5936d625b27aa71ef002280
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857662"
---
# <a name="nslookup-set-retry"></a>nslookup set retry

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Imposta il numero di tentativi.
## <a name="syntax"></a>Sintassi
```
set retry=<Number>
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|<Number>|Specifica il nuovo valore per il numero di tentativi. Il numero di tentativi predefinito è 4.|
|{help &#124; ?}|Viene visualizzato un breve riepilogo di **nslookup** sottocomandi.|
## <a name="remarks"></a>Note
-   Quando una risposta a una richiesta non viene ricevuta entro un determinato periodo di tempo, il periodo di timeout è raddoppiato negli anni e la richiesta viene inviata di nuovo. Il valore di ripetizione dei tentativi determina il numero di volte una richiesta viene inviata di nuovo prima di rinunciare. È possibile modificare il periodo di timeout con i **impostare il timeout** sottocomando.
## <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave sintassi della riga di comando](command-line-syntax-key.md)
[nslookup impostare timeout](nslookup-set-timeout.md)
