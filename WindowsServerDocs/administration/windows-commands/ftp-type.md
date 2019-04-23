---
title: tipo di FTP
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6e96dcd4-08f8-4e7b-90b7-1e1761fea4c7 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3524c7c772cdcd54a131d8a7e8c8714fad9ce563
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858902"
---
# <a name="ftp-type"></a>FTP: tipo

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Imposta o Visualizza il tipo di trasferimento di file.   
## <a name="syntax"></a>Sintassi  
```  
type [<typeName>]  
```  
### <a name="parameters"></a>Parametri  
|Parametro|Descrizione|  
|-------|--------|  
|[<typeName>]|Specifica il tipo di trasferimento di file.|  
## <a name="remarks"></a>Note  
-   Se *typeName* non viene specificato, viene visualizzato il tipo corrente.  
-   **FTP** supporta due tipi di trasferimento, ASCII e file binario del file.  
    Il tipo di trasferimento di file predefinito è ASCII.  Il **ascii** comando deve essere utilizzato durante il trasferimento dei file di testo. In QUESTA modalità, vengono eseguite le conversioni di caratteri da e verso il set di caratteri standard di rete. Ad esempio, caratteri di fine della riga vengono convertiti come obbligatorio, basata sul sistema operativo di destinazione.  
    Il **binario** comando deve essere utilizzato durante il trasferimento di file eseguibili. In modalità binaria, il file viene spostato in unità di misura a un byte.  
## <a name="BKMK_Examples"></a>Esempi  
Impostare il tipo di trasferimento di file ASCII.  
```  
type ascii  
```  
Impostare il trasferimento di tipo di file in formato binario.  
```  
type binary  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)  
