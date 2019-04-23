---
title: offerta di FTP
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4500a1d3-c091-42c7-a909-f61df7f2e993 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0b1aaf9eadfd9c51048ad41106ce6532f6f9588b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865882"
---
# <a name="ftp-quote"></a>ftp: quote

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Invia testualmente gli argomenti per il server ftp remoto. Viene restituito un codice di risposta ftp singolo.   
## <a name="syntax"></a>Sintassi  
```  
quote <Argument>[ ]  
```  
### <a name="parameters"></a>Parametri  
|Parametro|Descrizione|  
|-------|--------|  
|<Argument>|Specifica l'argomento da inviare al server ftp.|  
## <a name="remarks"></a>Note  
Il **virgoletta** è identico al comando il **letterale** comando.  
## <a name="BKMK_Examples"></a>Esempi  
Invio di un **quit** comando al server ftp remoto.  
```  
quote quit  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   [ftp: literal_1](ftp-literal_1.md)  
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)  
