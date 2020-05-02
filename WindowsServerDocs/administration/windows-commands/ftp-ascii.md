---
title: ASCII FTP
description: Argomento di riferimento per FTP ASCII
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 523be48e-eab0-4237-8fb5-ca222824f0b6 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: aa33637f43ef8d26635f36b40dbd21cfe2bff42e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725398"
---
# <a name="ftp-ascii"></a>FTP: ASCII

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Imposta il tipo di trasferimento di file in formato ASCII.   
## <a name="syntax"></a>Sintassi  
```  
ascii  
```  
#### <a name="parameters"></a>Parametri  
none  
## <a name="remarks"></a>Osservazioni  
- Il tipo di trasferimento di file predefinito è ASCII.  
- In QUESTA modalità, vengono eseguite le conversioni di caratteri da e verso il set di caratteri standard di rete. Ad esempio, caratteri di fine della riga vengono convertiti in base alle esigenze, in base al sistema operativo di destinazione.  
- **FTP** supporta i tipi di trasferimento di file di immagine binari e ASCII. Utilizzare ASCII durante il trasferimento dei file di testo. Per ulteriori informazioni sul trasferimento di file binari, vedere **FTP: Binary** in riferimenti aggiuntivi.  
  ## <a name="examples"></a>Esempi  
  Impostare il tipo di trasferimento di file ASCII.  
  ```  
  ascii  
  ```  
  ## <a name="additional-references"></a>Riferimenti aggiuntivi  
- [FTP: binario](ftp-binary.md)  
- - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
