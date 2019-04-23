---
title: ftp ascii
description: 'Argomento i comandi di Windows per ftp ascii '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 523be48e-eab0-4237-8fb5-ca222824f0b6 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b7bcca3f29cec8ff5c30256dfd123acc7fbb804d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849192"
---
# <a name="ftp-ascii"></a>ftp: ascii

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Imposta il tipo di trasferimento di file in formato ASCII.   
## <a name="syntax"></a>Sintassi  
```  
ascii  
```  
### <a name="parameters"></a>Parametri  
nessuno  
## <a name="remarks"></a>Note  
-   Il tipo di trasferimento di file predefinito è ASCII.  
-   In QUESTA modalità, vengono eseguite le conversioni di caratteri da e verso il set di caratteri standard di rete. Ad esempio, caratteri di fine della riga vengono convertiti in base alle esigenze, in base al sistema operativo di destinazione.  
-   **FTP** supporta ASCII e tipi di trasferimento di file di immagine binari. Utilizzare ASCII durante il trasferimento dei file di testo. Per altre informazioni sul trasferimento di file binari, vedere **ftp: binario** in riferimenti aggiuntivi.  
## <a name="BKMK_Examples"></a>Esempi  
Impostare il tipo di trasferimento di file ASCII.  
```  
ascii  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
-   [ftp: binary](ftp-binary.md)  
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)  
